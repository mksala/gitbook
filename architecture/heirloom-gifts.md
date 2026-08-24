---
description: >-
  How Heirloom seals gifts on the TimeLockRouter, why claim links keep the
  personal note off every server, and how gasless claims stay abuse-proof.
---

# Heirloom Gifts

Heirloom is a presentation layer over the deployed `TimeLockRouter`
contract: a sealed gift IS a timelock of the contract's `Gift` type,
created with `createTimelockedGift`. No new contract was deployed for the
feature; everything below runs on the audited router that also powers
beneficiary timelocks.

## Escrow: two lanes, one seal

- **ETH gifts** ride the router's storage-token swap lane. The sealed ETH
  is converted into a whitelisted storage token at creation (WETH
  preferred: a 1:1 wrap with no DEX dependency) and held by the router's
  vault. At claim time the withdrawal unwraps back to ETH, so the
  recipient receives ETH.
- **USDC gifts** ride the plain ERC-20 lane. `createTimelockedGift`
  performs `safeTransferFrom` into the `TimeLockERC20` vault at sealing,
  after an exact-amount approval. The recipient receives the tokens
  themselves.

Both lanes escrow at creation. The giver keeps no allowance, no custody,
and no recall path: the unlock date and recipient are immutable contract
state.

On-chain metadata is deliberately generic (`name: "Gift"`, empty
`giftName`): personal information never touches the chain.

## The claim link and the privacy model

A claim link is `/heirloom/{chainId}/{timelockId}#g=<base64url payload>`.

- The **path** carries only public on-chain identifiers. The gift page
  reads everything factual (recipient, unlock time, assets, claimed
  state) from the subgraph for that chain.
- The **fragment** (`#g=...`) carries the recipient name, giver name, and
  personal message. Fragments are never sent in HTTP requests, so no
  server (ours, an email provider's link scanner, anyone's) sees the
  note. The gift page decodes it locally.
- The sealing device also stores a **keepsake** (payload, occasion, card
  mode) in `localStorage`, keyed by chain and timelock id. This lets the
  gift's detail page rebuild the full link and the themed card later, on
  that device only. It is the same device-local posture as private legacy
  names: no server copy exists.

Whoever holds the link can view the gift. Only the on-chain recipient can
withdraw, regardless of who submits the transaction.

## Gasless claims: the sponsored withdrawal relay

The recipient signs an EIP-712 `WithdrawAuth` (free, no gas) and the
reminder-worker's relayer submits `TimeLockRouter.withdrawFor`, paying the
network fee.

```mermaid
sequenceDiagram
    participant R as Recipient (browser)
    participant W as Reminder worker
    participant C as TimeLockRouter

    R->>C: read sponsorNonce(recipient)
    R->>R: sign WithdrawAuth (recipient, timelockId, skipSwap, nonce, deadline)
    R->>W: POST /sponsor/gift-withdraw
    W->>W: rate limits, daily caps, payload checks
    W->>C: estimateGas withdrawFor(...)
    C-->>W: ok (reverts cost nothing)
    W->>C: withdrawFor(timelockId, skipSwap, auth)
    C->>C: verify signature, exact nonce, deadline
    C-->>R: pays out to the RECOVERED SIGNER only
    W-->>R: txHash
```

Why this cannot be drained:

- **The contract is the judge**: `withdrawFor` recovers the signer from
  the typed-data signature and pays out strictly to that recovered
  address. A forged or replayed request reverts; the relayer's
  `estimateGas` catches it before any gas is spent.
- **Exact sequential nonce** (`sponsorNonce`) and a deadline bound every
  authorization to one use, soon.
- **Worker caps** bound the spend: a global daily relay cap, a per-gift
  daily cap, a gas-price ceiling, and per-IP rate limits. The numbers
  live in the reminder-worker configuration; at current mainnet fees a
  claim costs roughly a dollar of gas, and the worst full-saturation day
  is bounded to tens of dollars.
- The realistic abuse is cap exhaustion (sealing tiny gifts to yourself
  and claiming them sponsored), which burns the day's free-claim budget
  but never other people's funds. Self-paid claiming always remains
  available.

Availability is reported by `GET /sponsor/status` as a `gift` field,
separate from the legacy sponsor's readiness.

## Notify emails

`POST /gift/notify` lets the GIVER register a recipient email for two
sends: "a gift is waiting" immediately and "your gift just unlocked" on
opening day.

- **Owner-signed EIP-712**: the signature covers the exact payload
  string, and the worker verifies the signer against the timelock's
  on-chain owner via the subgraph. Only the actual giver can email
  anyone; this is the anti-spam core.
- **Claim-link origins are allowlisted** so a malicious payload cannot
  point victims at a phishing domain wearing our email template.
- **Encrypted at rest**: the payload (email, names, the full claim link
  including its fragment) is AES-GCM encrypted with a per-row derived
  key.
- **Crypto-shredded at maturity**: the unlock-day pass deletes every
  matured row, silently when the gift is already claimed, right after
  the unlock send otherwise. Encrypted personal data never outlives
  opening day (plus a retry window if the email proxy is down).
- **Dedupe survives deletion**: send idempotency lives in a separate
  ledger keyed by a hash of the email, so a deleted row can never re-arm
  a duplicate send, while correcting a typo'd address still gets its
  copy.

## Printable artifacts

All three artifacts render client-side on a canvas (no server round
trip) at Letter size, 200 dpi:

- **Key sheet**: a wrap-fold three-panel packet. The flat sheet renders
  the secret on the top panel, the occasion cover on the middle, and the
  "how to open" back panel rotated 180 degrees so it reads upright after
  folding. QR codes are generated with margin 2 and error correction M;
  the secret panel is always pure white for scan reliability.
- **Gift card**: occasion-themed certificate carrying the claim link QR
  and the personal message. Never the key, never the amount.
- **Gift record**: an austere factual document for the giver, headlined
  by fair market value on the sealing date (historical price lookup for
  past gifts, live price for same-day ones, an honest "no price source"
  line offline), with a not-tax-advice disclaimer.

Occasion designs (palettes, canvas-drawn motifs, greetings, message
templates) are a reviewed content pack; religious occasions use only
symbols customary on greeting cards.

## Known operational notes

- Wallet reputation engines can flag young contracts. The create page
  explains the caution in place and points at address verification; the
  false-positive appeal is an operational task tracked in the app
  repository's ops checklist.
- A gift is also visible as a `Gift`-type row in the giver's Timelock
  tab; the shield score deliberately does not credit outgoing gifts as a
  protection layer.
