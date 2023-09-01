---
description: Technical workflow
---

# Core mechanisms

#### Private Keys

Cryptography is at the core of any decentralized project, and that involves what we call keypairs, or in other words, public and private keys.&#x20;

Private keys are the keys to open safes of this new world, and therefore they need to be well guarded. In our case, private keys are the ones we need to delicately pass around based on one's predefined rules.

#### [Smart Contract](#user-content-fn-1)[^1]

Smart contract are executers of rules contained in a pre-defined contract. There is a smart contract owner initially, one that can be removed. While often regarded as a flaw, being able to change the rules of a smart contract can come really convenient in real life.&#x20;

For example, the owner of a Safe can decide to change some of the approvers if they have been compromised at any moment, and that is not a bug, but a feature. Similarly, one may want to change the specified heritance in the case of any status change (or relationship).

#### Main Scenario

Person A wants to provide access to the keys of W wallet to person B and person C in the case of passing.&#x20;

A knows that if it didn't connect to W for X months (variable defined by A), something major happened in life, likely reflecting own incapability to access. That's when A wants the computing will to kick in.

A heart-bit mechanism will essentially translate any detected incapability (X months of inactivity) to a trigger. The effective smart contract in place will therefore add B and C as authorized signers.&#x20;

#### Heart-bit

A function defined in the smart contract wallet that will check its activity on a regular basis, looking for any sign of life in the last X months at most. If there is no activity in the specified timeframe, a subsequent action will be triggered.&#x20;

**Notes:**

* Activity can be any of the following:&#x20;
  * Outgoing transaction, claim transaction, or voting transaction.
  * In general, activity here is defined as the owner manually utilizing the safe in any way.
* Frequency of the heart-bit can be set to daily, weekly, or monthly.

#### Conditional actions types

**Inheritance.** Provide the pre-designated account(s) with full wallet permissions.

**Forwarding**. Send all the assets to the pre-designated account(s).

**Destruction.** Send all the assets to Ethereum genesis address, essentially burning the value in the wallet.

**Legacy holding**. Lock the assets for another pre-specified amount of time, essentially restarting another cycle of Heart-bit. Other action types can kick in 2nd, 3rd, or a specified n numbers of cycles.

A combination of any two above can be used in conjunction for more advanced strategies. For example, someone with little kids might want to make sure that until a mature age, a 3rd party can manage some of the funds in case of emergency.&#x20;



#### Conditional pre-authorized public keys

This part might be one of the most challenging thus far. On one hand, we want to operate via secure and on-chain means only, namely, via Ethereum accounts. On the other hand, the most likely recipients one desires (mainstream) are not familiar enough with the Ethereum rails at this time (nature of any niche emerging technology industry). We discuss here options and ideas to best compromise. Current options from most secure to most friendly:

* An Ethereum public key (address).
* A social wallet address. Another smart contract accessible by guardians, which can be identified by a phone number providing the corresponding app to such social wallet is installed (e.g Argent).
* A QR generated code sent by email. The QR code represents the private key of such conditional pre-authorized public key.\


[^1]: 
