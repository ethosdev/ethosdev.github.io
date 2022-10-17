---
title: "Mainstream staking with Distributed Validator Technology"
description: "Distributed Validator Technology (DVT) would require multiple signatures to command a validator, allowing a secure and easy way for mainstream users to stake on Ethereum."
permalink: dvt-staking
---

TODO links

I dream of a market where mainstream users can stake ETH easily and with no fees. There's also a dream that staking ETH will be as easy as plugging in an Internet router. Until then, we can't escape reality that staking with an exchange or pool is easiest for mainstream users.

I dream that staking will be free. Currently, most staking companies and pools charge fees starting at 10%! There's hope from history. When buying stocks, for decades brokerages charged commission fees. Three years ago, in the USA the [fees were eliminated!](https://www.cnn.com/2019/10/01/investing/charles-schwab-eliminates-commissions)

Is there a catch to free staking?

## **Staking companies with the most validators can attack the network**

[Coinbase explains](http://web.archive.org/web/20200328023024/https://help.coinbase.com/en/coinbase/trading-and-funding/other/staking-on-coinbase.html){:rel="nofollow"}: "You retain full ownership of your crypto, but you’re delegating your staking power to Coinbase." It hints at the power that staking services amass, but does not educate the risks of that power.

Ownership and custody is relatively simple in Ethereum staking, because there are two staking keys.  One is a validator "hot" key, and one is a withdrawal "cold" key.  Basically, whoever has the withdrawal key, owns the funds.  You can give the validator key to a staking company, and keep the withdrawal key to yourself: a company cannot steal your stake (though they can lose the stake).

By giving your validator key to a company, you give them full power to control your validator.  This is the delegated staking power quoted above.  When many people give their validator keys to a company, that company can control enough validators to be a threat to the Ethereum network.  The company could knowingly or unknowingly command validators to attack the network.

Free staking increases risk of an attack on the network, especially if there's only one free staking service and it captures most of the stakers.  Existing proof-of-stake blockchains have no protocol defence against this.

You need to know this risk of free staking services. But there's also good news that the Ethereum protocol has been designed with fundamentals to eliminate risk from a single entity.

## **Dreaming of Distributed Validator Technology (DVT)**

Distributed Validator Technology (DVT) would require multiple signatures to command a validator, allowing a secure and easy way for mainstream users to stake on Ethereum.

DVT is like multisig for a validator.  Instead of a validator being controlled by a single signature, multiple signatures would be needed.  This means that you don't have to delegate your staking power to a single company.  I dream that staking companies and pools will interoperate with each other with [Distributed Validator Technology](https://github.com/ethereum/distributed-validator-specs).

Unlike other proof-of-stake blockchains, [DVT is possible in Ethereum](https://www.youtube.com/watch?v=Jtz9b7yWbLo){:rel="nofollow"}.  The Beacon Chain uses BLS signatures.  Since BLS signatures are additive, Shamir's Secret Sharing can be used to split the validator private key into multiple shares.  From the individual share signatures, the full BLS signature can be computed.  You can configure how many shares to split your validator private key, and how many share signatures are needed for the full BLS signature. This type of Distributed Validator Technology has also been called Secret Shared Validators (SSV).

For example, you could split your validator private key into three shares and give three companies, one share each.  The setup could be 2-of-3, so that a single signature is unable to command your validator.  Two of the companies would have to sign the same message to command your validator. A company would not have unilateral power to command all the validators of their customer base.

Distributed Validator Technology is critical.  There should be easy tools for you to split your validator private key into shares that you can provide to staking companies.  There needs to be methods for operating Distributed Validators so that staking operations of different companies can be combined.

Some may tell you that you should run your own validators.  That's fine if you're comfortable and enjoy the work.  But it's not the mainstream solution yet.  Even if you run your own validators, you can still benefit from DVT.  They allow you to spread validators across several machines and even different data centers.  This increases security and avoids single points of failure.

I dream that market forces and customer demand will push the staking industry to be interoperable with each other using Distributed Validator Technology.  We will also need to educate mainstream stakers to use multiple companies configured with DVT, so that they do not grant a single company full control of their validators.  Let's all make it happen!

* * *

_A 2020 version of this post in a different style and older SSV terminology is at [{% post_url 2020-05-04-free-staking-on-ethereum %}]({% post_url 2020-05-04-free-staking-on-ethereum %})_

