---
title: "Free staking on Ethereum and Distributed Validator Technology"
date: "2020-05-04"
permalink: /:title/
---

Did you know that one of the largest exchanges already provides free staking?

Here's three secrets you should know about Ethereum staking and what to do about them.

You'll want to know about free staking, but more important is doing it with Distributed Validator Technology (DVT) to help maintain network security.

But first, by reporting some facts it can be perceived that there's businesses the author biases.  In truth, there's little bias and don't shoot the messenger :)  The author's bias is for the ecosystem which thrives with all the entities in it, and thrives even better when you, the customer, does well.

## **Secret #3: You should demand free staking**

You need to know that free staking is possible.

As [reported last month](https://www.theblockcrypto.com/linked/60916/coinbase-custody-is-the-biggest-tezos-staking-service-despite-charging-higher-fees):

> "Coinbase charges the highest fee –25% – for XTZ staking, while Kraken charges 7.25% and Binance currently charges no fee."

You may have thought that free staking is too cheap.  Don't, because it's already possible and sustainable by some.  Binance's business model includes free staking, a bit like how Internet business models give you a free email account. 

Free staking is good for you.  You should demand free staking from the providers charging a fee.  Now, is there a catch to free staking?  Only if secret #1 doesn't happen.  #1 is Distributed Validator Technology (DVT) and it also needs your voice.  But next, let's discuss the risk to free staking without DVT.

## **Secret #2: Staking companies with the most validators can attack the network**

[Coinbase explains](http://web.archive.org/web/20200328023024/https://help.coinbase.com/en/coinbase/trading-and-funding/other/staking-on-coinbase.html): "You retain full ownership of your crypto, but you’re delegating your staking power to Coinbase." It hints at the power that staking services amass, but does not educate the risks of that power.

Ownership and custody is relatively simple in Ethereum staking, because there are two staking keys.  One is a validator "hot" key, and one is a withdrawal "cold" key.  Basically, whoever has the withdrawal key, owns the funds.  You can give the validator key to a staking company, and keep the withdrawal key to yourself: a company cannot steal your stake (though they can lose the stake).

By giving your validator key to a company, you give them full power to control your validator.  This is the delegated staking power quoted above.  When many people give their validator keys to a company, that company can control enough validators to be a threat to the Ethereum network.  The company could knowingly or unknowingly command validators to attack the network.

Free staking increases risk of an attack on the network, especially if there's only one free staking service and it captures most of the stakers.  Existing Proof of Stake blockchains have no protocol defence against this.

You need to know this dark side of free staking services, but also the good news that the Ethereum protocol has been designed with fundamentals to eliminate risk from a single entity.

The #1 secret is that Distributed Validator Technology (DVT) allows you to stake with a company like Binance, without giving them full control of your validators.

## **Secret #1: You should demand adoption of Distributed Validator Technology (DVT)**

Distributed Validator Technology (DVT) is like multisig for a validator.  Instead of a validator being controlled by a single signature, multiple signatures would be required to command a validator.  This means that you don't have to delegate your staking power to a single company.  You should demand that staking companies prepare and adopt the [Ethereum Distributed Validator Specifications](https://github.com/ethereum/distributed-validator-specs).

Unlike other Proof of Stake blockchains, [DVT is possible in Ethereum](https://www.youtube.com/watch?v=Jtz9b7yWbLo).  The Beacon Chain uses BLS signatures.  Since BLS signatures are additive, Shamir's Secret Sharing can be used to split the validator private key into multiple shares.  From the individual share signatures, the full BLS signature can be computed.  You can configure how many shares to split your validator private key, and how many share signatures are needed for the full BLS signature. This type of Distributed Validator Technology has also been called Secret Shared Validators (SSV).

For example, if Prysmatic Labs and Sigma Prime (or others) have free staking for (retail) members of the community, then you could split your validator private key into three shares and give each a share.  You give the third share to Binance.  The setup could be 2-of-3, so that a single signature is unable to command your validator.  Two of the companies would have to sign the same message to command your validator. A company would not have unilateral power to command all the validators of their customer base.

Some may tell you that you should run your own validators.  That's fine if you're comfortable and enjoy the work.  But it's not the mainstream solution yet.

> **Secret Shared Validators are critical.** **There should be easy tools for you to split your validator private key into shares that you can provide to staking companies.  There needs to be a standard for how to operate Secret Shared Validators so that staking operations of different companies can be combined.**

Even if you run your own validators, you can still benefit from Secret Shared Validators.  They allow you to spread validators across several machines and even different data centers.  This increases security and avoids single points of failure.

There's a chance that Binance provides free staking but doesn't allow Secret Shared Validators.  Let's be clear, **you should demand that a company supports Secret Shared Validators before signing up for their free staking**.  If Binance offers 0% fees, you want Secret Shared Validators.  If Binance offers 0.5% fees, you still want Secret Shared Validators: paying fees is not a substitute.

## **Priority of Secret Shared Validators**

Prioritizing Secret Shared Validators depends on one's views about the likelihood of free staking and the threat of a single company obtaining many validators.  There's no alarm for Secret Shared Validators yet.  Currently, Ethereum 2.0 client teams are correctly focused on hardening and productionizing their clients.  More will soon be written about Secret Shared Validators, to kickstart the specification and implementation of _**Secret Shared Validator clients**_, as well as a standard for Secret Shared Validators that staking companies should adopt.

There is related work around Proof of Custody for eth2 Phase 1, and trustless staking pools. Such work (and maintaining focus on it) is important.  However, we should not be confused that it covers what's needed for Secret Shared Validators.  Such work is not explicitly leading to _Secret Shared Validator clients_ that can be used in production.  Nor does the work lead to a standard for Secret Shared Validators so that staking operations of different companies can be combined.

## **A dream or will it play out?**

Is it a good dream to have Binance and a couple of others provide free staking with Secret Shared Validators?  Will Coinbase join?  We'll need to drive towards ensuring:

1. a **specification for a Secret Shared Validator client**
2. there's **easy-to-use tools** (and UIs) that allow you to split your validator private key into multiple shares
3. at least **one implementation of a Secret Shared Validator client**
4. a **standard for how to operate Secret Shared Validators** (so that staking operations of different companies can be combined)
5. for retail customers, **multiple companies offer free staking with Secret Shared Validators**
6. you sign up with two or more such companies and **enjoy free staking more securely**

The first four items are needed so that Secret Shared Validators can be tested on testnets and released on Mainnet.  For the fifth item, you can prod exchanges and [prod Binance to **use Secret Shared Validators** and enter the Ethereum staking market sooner](https://twitter.com/cz_binance).

## **Secrets for you to spread**

You should know that free staking is viable (even maybe inevitable).  But, there's a risk to the Ethereum 2.0 network if there's only one free staking company that everyone joins.  Secret Shared Validators would prevent a company from unilateral control of their customers' validators.  We need a spec and implementation of a Secret Shared Validator client and contact @dankrad on Twitter or email dankrad@ethereum.org if you'd like to work on this.

Most important, you should demand a standard for operating Secret Shared Validators that staking companies should comply with.  Let's hope that you can enjoy free staking on Ethereum with Secret Shared Validators to maintain network security.  Let's all make it happen!

* * *

_Thank you to Dankrad Feist for reviewing a draft and feedback, including Secret Shared Validator terminology (I capitalized for readability).  As always, reviewers don't imply agreement with my opinions._

