---
description: >-
  Seal ETH or USDC as a gift that opens on a date you choose. The recipient
  opens it with a link, and claiming is free.
---

# Heirloom (sealed gifts)

Heirloom seals value for someone until a date that matters: a Bar or Bat
Mitzvah, a wedding, a birth, an 18th birthday. The gift is held by the same
audited timelock contract the rest of the app uses. Until the opening date
nobody can take it out, including you.

The thing you send is a gift. Heirloom is the product that seals it.

## Sending a sealed gift

Open **Heirloom** in the top menu (or the **Send a sealed gift** button on
the Timelock tab) and fill one card:

1. **Occasion**: Bar/Bat Mitzvah, Wedding, New baby, Birthday, Graduation,
   Hanukkah, Christmas, Eid, or Someday. The occasion sets the design of
   the printable sheets and suggests a message. It also proposes a fitting
   unlock horizon (five years for most occasions, eighteen for a new baby).
2. **To**: either the recipient's wallet address, or, when they have no
   wallet, **No wallet? Hand them a card**: the app generates a fresh
   wallet for the gift and you hand its key over on paper. The paper is
   the gift.
3. **Amount**: ETH, or USDC for stable value. Both are escrowed on
   sealing; the choice changes what the recipient receives, not how the
   seal works.
4. **Unlocks**: a preset horizon or an exact date. The date is final and
   can never be shortened.
5. **Message (optional)**: stays off the blockchain. It travels inside the
   claim link and prints on the gift card.

Sealing sends one transaction (two for USDC: an exact-amount approval,
then the seal). The gift cannot be recalled or changed after it confirms,
not even by you.

{% hint style="info" %}
Some wallets show a caution while our escrow contract is still young. This
is a reputation flag on a new contract, not a finding. Rather than
ignoring any warning, verify: the contract address your wallet shows must
match the published list in the public repository
(`contract-addresses.json`).
{% endhint %}

## The claim link is the gift

Sealing produces one link, shaped like
`app.10102.io/heirloom/1/42#g=...`. The part after `#` carries the names
and your message. Browsers never send that part to any server, ours
included, so the personal words stay between the two of you.

- Anyone with the link can SEE the gift: who it is from, what it holds,
  when it opens.
- Only the recipient's wallet can TAKE it, and taking it is free (see
  claiming below).
- Share it by copy, WhatsApp, email, or the printed card's QR code.

The seal panel can also email the link for you: enter the recipient's
email and sign one free message. They get "a gift is waiting" right away
and "your gift just unlocked" on opening day. The address is stored
encrypted and deleted once its job is done.

The names, message, and occasion are also kept on the device that sealed
the gift (never on a server), so the gift's detail page on that device can
rebuild the full link and the printed card later.

## The printables

Three artifacts, each with a distinct job:

- **The key sheet** (paper-key gifts only): a single sheet that folds into
  a three-panel packet. The occasion cover faces front, "How to open your
  gift" faces back, and the secret key is sealed inside the fold. Print
  single-sided and fold on the printed dashed lines, in the numbered
  order. Whoever holds this paper controls the gift: never photograph it
  open.
- **The gift card**: the greeting. Occasion design, who it is for, the
  opening date, your message in quotes, and a QR of the claim link. Safe
  to share and photograph; it contains no key material and never shows
  the amount.
- **The gift record**: a factual document for the giver, downloadable
  from the gift's detail page. It leads with the fair market value on the
  sealing date, then dates, amounts, shortened wallet addresses, and the
  escrow contract, with instructions for verifying everything on the
  public ledger. It is not tax advice and says so; hand it to your
  advisor.

## Claiming (free for the recipient)

On opening day the recipient opens the link, connects or imports their
wallet, and claims:

- Claiming costs the recipient nothing. They sign one free message and
  10102 submits the transaction and pays the network fee. The signature
  only proves who they are; the contract pays out strictly to the
  on-chain recipient.
- If the sponsored path is ever unavailable, the recipient can submit the
  withdrawal themselves from their own wallet instead.
- Before opening day, the gift page shows a countdown and can add the
  opening day to Google Calendar or any calendar app. Nothing is sent to
  our servers.
- After claiming, the page becomes a small keepsake: sealed date, opened
  date, and an invitation to seal one forward.

## Honest limits

- **Irrevocable**: once sealed, no one can cancel, shorten, or redirect a
  gift. Triple-check the recipient address and the date.
- **Paper is a bearer instrument**: for paper-key gifts, losing the key
  sheet before opening day loses the gift, and anyone who photographs the
  open sheet can take it. The sheet says this on the sheet.
- **The link shows the gift**: treat the claim link like a greeting card,
  not like a password. It cannot move funds, but it does reveal names,
  message, and amounts to whoever holds it.
- **Sponsored claims are capped**: gas sponsorship is bounded by daily
  caps to keep the feature abuse-proof. If the day's budget is exhausted,
  claiming still works self-paid, or free again the next day.
