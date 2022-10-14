---
title: "Trustless exchange and pegging of Bitcoin in Ethereum"
description: "TODO"
permalink: bitcoin-ethereum
---

Ethereum wouldn’t be where it is today without its older cousin, Bitcoin. Ever since it launched, there’s been critique about the requirement of launching a completely separate blockchain, with its own cryptographic token securing its network. For various reasons, it was decided it must be separate and after a year of development, Ethereum went live on 30 July 2015. It doesn’t mean these cousins won’t ever to get to hang out again! There’s been thoughts of getting bitcoin (the token) over to Ethereum or adding Ethereum (virtual machine) support to Bitcoin. A thought-provoking whitepaper, published last week by Rootstock, spurred discussion about this interplay and extending smart contracts to Bitcoin.

## **Overview of a Typical 2-way Peg**

The typical example of interoperability with Bitcoin and Ethereum is via a 2-way peg.

Ethereum smart contracts are powerful enough so that a 2-way peg with Bitcoin can be done in Ethereum. A 2-way peg with Bitcoin is exciting to most involved in Bitcoin, as it allows bitcoins to be “deposited” to another blockchain, and then “withdrawn” back to the Bitcoin blockchain. As bitcoins can be deposited in another blockchain, a 2-way peg can enable users to make use of functionality that exists on another blockchain that is not available on the Bitcoin platform. Or this mechanism can serve as an upgrade and migration path for Bitcoin: when people believe in the upgrade, they simply keep their bitcoins in the other blockchain instead of withdrawing it back.

Here is a general description of a 2-way peg with Bitcoin. Bitcoins are BTC, and let’s call the bitcoins on the other blockchain E-BTC. To keep things simple, we’ll use Ethereum as the other blockchain.

When a user “deposits” the BTC, the BTC is locked on the Bitcoin blockchain. A proof of this Bitcoin transaction is sent to an Ethereum contract, call it PegContract. PegContract verifies the transaction and issues E-BTC to an Ethereum address controlled by the user, such that 1 BTC on the Bitcoin system becomes 1 E-BTC on the Ethereum platform.

Later, the user decides they are finished with E-BTC and wants the BTC back. The user burns the E-BTC (or returns it to the smart contract automatically overseeing the whole process) and provides a proof of this to the Bitcoin blockchain. The Bitcoin blockchain verifies that the E-BTC are destroyed, and unlocks the original BTC.

The locking and unlocking of BTC is a simple way to explain a 2-way peg. The PegContract contract can be written today and can do everything it needs to trustlessly: it can verify that Bitcoins were sent to an address that locks them up; it can issue E-BTC; and it can destroy E-BTC and provide a proof of the destruction. But the mechanisms for verifying the proof and unlocking (and possibly locking) on the Bitcoin blockchain, need new opcodes to be added to Bitcoin.

{% include fig.html img="Bitcoin-Ethereum-2way-peg.png" alt="2-way peg of Bitcoin and Ethereum" caption="New Bitcoin opcodes are needed to verify the proof from PegContract. (No opcodes are needed in Ethereum to verify the proof from Bitcoin)" %}

Currently, Bitcoin can’t verify information from other chains.

Fortunately, there are ways of interacting with Bitcoin inside of Ethereum. From the spectrum of what’s possible now, to what will be possible in the future, the following can be built:

1) The currencies BTC and ETH can be exchanged trustlessly.

2) Trustlessly pegging BTC in Ethereum without requiring any new opcodes in Bitcoin: enabling an Ethereum-resident bitcoin proxy token that tracks the value of Bitcoin exactly.

3) Enabling BTC payments on the BTC blockchain for the execution of Ethereum contracts.

The Ethereum Virtual Machine (EVM) was designed to be able to write Ethereum smart contracts that *do* integrate with Bitcoin. One such contract is BTC Relay.

[BTC Relay](http://btcrelay.org/)  implements Bitcoin SPV (simplified payment verification) to verify whether any Bitcoin transaction has been confirmed (sufficiently) on the Bitcoin blockchain. So any transactions in the Bitcoin system, from payments to the locking of BTC, can be verified by an Ethereum contract.

**1) Decentralized exchange of BTC & ETH (works currently)**

Currently, BTC Relay allows a smart contract to automatically dispense ETH once the relay has verified that BTC were sent to a specific address. This provides a trustless way for a user that has BTC, to obtain ETH. Alternatively, using the same ETH-BTC swap mechanism, if a user wants BTC, they can deposit ETH into the contract which monitors whether a certain Bitcoin address receives sufficient funds. When an ETH purchaser sends BTC, the relay then verifies this Bitcoin transaction, upon which the ETH is then transferred. If the transaction does not occur in Bitcoin by a certain point in time, then the original owner of the ETH can send BTC to themselves to withdraw their ETH from the contract.

{% include fig.html img="BTCRelay-swap.png" alt="Swapping BTC and ETH with BTC Relay" caption="After a Seller has offered their ETH by sending it to a smart contract, a Buyer can buy the ETH using BTC" %}

Currently Litecoin is being used to move “bitcoin” between exchange platforms faster than the Bitcoin system can currently move it on average. Traders buy LTC with BTC, transfer to another exchange, then convert LTC back to BTC. One application of the ETH-BTC swap mechanism enabled by BTC Relay is even faster movement of BTC between exchanges. This will of course require BTC and ETH to be traded on the same exchange and will also depend on the trustless exchange attracting sufficient users to create acceptable liquidity.

With a liquid decentralized exchange, people with bitcoin will be able to buy whatever tokens they need for the Ethereum dApps they want to use. Whenever they want to get BTC back, they can sell their tokens on the exchange. There is less need to peg 1:1 BTC to E-BTC. However, for those that desire pegging, it is possible in Ethereum, without requiring additional opcodes in Bitcoin.

**2) Trustless Bonded Peg of Bitcoin in Ethereum (works currently)**

A problem with 2-way pegging, is that currently, Bitcoin can’t automatically unlock BTC: it can’t verify that E-BTC have been destroyed and then unlock BTC. To solve unlocking BTC, Ethereum will provide incentives, so that humans will unlock the BTC.

A user’s BTC will be locked in a Bitcoin multisig address, where each signer must deposit a bond to PegContract on Ethereum. This bond can be a large amount of ETH. A user can check PegContract that signers have a bond, before locking up their BTC. When BTC is locked, PegContract will issue E-BTC as usual. When E-BTC is sent back to PegContract, it is destroyed and a proof generated by PegContract. PegContract starts a countdown, to give signers enough time to verify the proof and sign the multisig to unlock the BTC. The bond incentivizes the signers to unlock the BTC when E-BTC is destroyed. If any of the signers do not sign the multisig, they forfeit their bond: the large amount of ETH could be sent to the user, who can then sell the ETH and get their BTC (plus more) back. (The user’s original BTC are still locked).

This is an overview of what’s possible now.

{% include fig.html img="Trustless-Bonded-Peg.png" alt="Trustless bonded peg of Bitcoin and Ethereum" %}

**3) Currency & Crypto abstraction (requires Bitcoin & Ethereum upgrades)**

Ethereum, in an attempt to further modularize and generalize the protocol, plans to implement an abstraction of the cryptography and tokens required in the chain. In this future, ETH deposits will be required to sign new blocks and collect transaction fees, but all other operations can be done by any other token if users so wish. Miners/stakers can accept fees in any Ethereum-based token. In this future, BTC itself, as a BTC-token-on-Ethereum, can also then be used to pay for transactions (alongside Ether and even  TysonCoins).

**Conclusion**

Taking stock, it seems there are several ways in which Bitcoin can interact with Ethereum currently and into the future. Adding functionality to Bitcoin, through a sidechain like Rootstock, or trustlessly pegging Bitcoin in Ethereum, both point to a desire to make use of the EVM’s current, more flexible, Turing-complete language. Ethereum is currently live and allows for various dApps to be deployed.

Many of the examples listed in the Rootstock whitepaper already exist (active development or launched already) in Ethereum, including:

-   Prediction Markets (groupgnosis.com and augur.net)
-   Decentralized exchange (etherex.org)
-   Crowdfunding (wei.fund)
-   Governance (boardroom.to)
-   Supply Chain ​Traceability (provenance.org)
-   Online Reputation & Digital Identity (uPort)
-   Micropayment channels and Hub-and-Spoke networks (Raiden)

Start building your dApps, write your smart contracts, try BTC Relay, and implement your peg of Bitcoin today!
* * *
_Co-authored with Simon de la Rouviere. Graphics by Bogdan Burcea and Eva Shon._
