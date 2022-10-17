---
title: "Ethereum Gas, Fuel & Fees"
description: "Everything about Ethereum gas and fees is explained with examples, including Out of Gas exceptions, gas refunds, the block gas limit, and metering vs. fees."
permalink: ethereum-gas-fees
---

Ethereum is a platform for decentralized and truthful applications that run on a global, peer-to-peer network without any administrators or a single point of failure. These applications have zero downtime and anyone can create them: it is permissionless innovation. The applications are truthful, immutable and always interoperate as they are coded. From this perspective, the terminology of smart contracts is reasonable in that they are the ultimate in contracts that always follow the terms set at their creation.

The core of what makes this possible is effectively a World Computer. Technically called the Ethereum Virtual Machine (EVM), it includes operations for computation and data storage. A transaction represents a single session within the World Computer. It is the unit of interaction, similar to how a sentence is the unit of grammatical meaning, even though a single sentence can contain many words.

## **What is Gas?**

Gas is the metering unit for use of the World Computer. As an analogy, electricity is metered by kilowatt hours. Using more computation and storage in Ethereum means that more gas is used. One fundamental reason for metering is that it provides an incentive for people (miners) to operate the World Computer. These miners get a fee for processing transactions, which is determined by the metering scheme: gas.

Each operation in the EVM consumes gas. For example, a multiplication (MUL) consumes 5 gas and an addition (ADD) consumes 3 gas.  [Here is a spreadsheet of Ethereum’s operations and their gas consumption](https://docs.google.com/spreadsheets/d/1m89CVujrQe5LAFJ8-YAUCcNK950dUzMQPMJBxRtGCqs/edit){:rel="nofollow"}.

> Consider gas to be synonymous with fuel

Metering is different from fees and gas is different from Ether. To help clarify this, consider gas to be synonymous with fuel. A transaction must provide enough fuel, or startGas, to cover its entire use of the EVM’s computation and storage facilities. All remaining gas is refunded to the transaction’s originator: the user who initiated the transaction. A transaction that runs Out of Gas is reverted, but still included in a block and the associated fee is paid to the miner.

With an overview from the perspective of fuel, let’s turn to an overview of fees. While every operation in the EVM consumes a predefined amount of gas (for example, a MUL operation always consumes 5 gas), a user can specify a gas price in every transaction. The current gas price is 0.02µ Ethers, or 0.00000002 ETH. The fee an originator pays a miner is the transaction’s (startGas — remainingGas) × gas price.

Here is a summary of the influences of transaction fuel and transaction fee:

{% include pic.html img="fuel-fee.png" alt="Table of influences of transaction fuel and transaction fee" %}

At the start of a transaction, the Ether required for the startGas is set aside, and the remainingGas is set to startGas. With each operation of the transaction, gas is consumed and remainingGas is lowered. If there’s an Out of Gas exception, all operations are reverted and all the Ether that was initially set aside is given to the miner. If the transaction completes successfully, all the remainingGas is refunded to the originator and the rest is paid to the miner.

## **Simple example**

In the following mock scenario, assume that a STORE consumes 45 gas and an ADD consumes 10 gas. The scenario involves storing the number 31 in the EVM, summing 2 numbers, and then storing the sum. Let’s assume that the originator specified a startGas of 150 and a gas price of 0.02µETH. Below is an illustration as the transaction is processed by the EVM:

{% include pic.html img="example.png" alt="Simple example" %}

The originator pays the miner a fee of:

> (150–50) × 0.02µETH = 2µETH = 0.000002 ETH

## **Fuel vs. Fee**

There is a difference between an originator providing enough fuel and providing enough fees. Here are the likely effects on a transaction:

{% include pic.html img="fuel-vs-fee.png" alt="Fuel vs Fee effects on a transaction" %}

A transaction with too little fuel will not even reach miners, regardless of the fee supplied. If adequate fuel is provided for a transaction, but the fee is too low, even though the transaction may reach miners, upon seeing the fee miners will not perform any computation. Fees determine the order in which transactions will be included in the blockchain. The reason why providing high fuel can lead to a transaction taking longer to get mined is discussed below in [Potential delays with high startGas](#potential-delays-with-high-startgas).

## **startGas**

Let’s discuss the fuel needs at the start of a transaction. Abstractly, before allowing use of its computation and storage, the World Computer needs to know if the originator can pay for it. Concretely, before processing a transaction, a miner needs to know if it will get paid. A transaction must offer the maximum amount of fuel it is willing to consume. For this article’s purposes, this offer will be called  **startGas**. startGas is an essential part of a transaction. Unfortunately, different documentation has used different words to describe this critical component:

-   startGas is the term in the  [Ethereum White Paper](https://ethereum.org/669c9e2e2027310b6b3cdce6e1c52962/Ethereum_Whitepaper_-_Buterin_2014.pdf)
-   gasLimit is the term in the  [Ethereum Yellow Paper](https://github.com/ethereum/yellowpaper)
-   software, such as go-ethereum and web3.js, simply uses the term “gas”

startGas keeps things simple (away from say a pay as gas is consumed approach) because it is impossible for a miner, in the general case, to know how much computation is required without running the actual computation (related to the Halting problem). As a user of an Ethereum application, it may seem complicated to have to provide startGas, but developers have some tools for estimating startGas and hiding the details from the user.

## **Out of Gas exception**

A transaction offers the maximum amount of fuel it is willing to consume. This gas is consumed with each operation of the EVM. If all the gas is consumed without the transaction being completed, an Out of Gas exception occurs. The originator pays the miner for all the work that’s been performed and the transaction is included in a block, but all state changes (such as contracts created, values stored, [events and logs]({{ site.url }}{% post_url 2016-06-06-technical-introduction-to-events-and-logs-in-ethereum %})  written) are reverted.

Let’s use the same scenario as the first example, but this time the originator specifies a startGas of 90 (instead of 150). Here’s an illustration of the execution:

{% include pic.html img="out-of-gas.png" alt="Out of gas example" %}

At the start of the transaction, the originator must set aside funds for all the fuel: startGas × gas price = Ether placed in escrow. The amount of Ether in escrow is:

> 90 × 0.02µETH = 1.8µETH

Gas is consumed as each operation is performed and the operation to STORE sum is an Out of Gas exception. This causes all operations to be undone, meaning that the number 31 gets reverted to whatever value was previously stored, but the transaction is still included in the blockchain and the miner is paid the entire escrow amount: 1.8µETH.

## **Gas refund**

There are 2 operations in the EVM with negative gas:

-   Clearing a contract is -24,000
-   Clearing storage is -15,000

When the EVM executes such an operation, it is tallied in a separate refund counter. The gas refund is only provided at the end of the transaction. Also, the maximum refund equals half the gas consumed.

A key point is that a transaction’s fuel never increases. As the EVM performs operations, the fuel always decreases (with the refund counter increasing when some contract or storage is cleared). If the fuel reaches zero or negative, then an Out of Gas exception occurs immediately: it does not matter how much gas is in the refund counter. For a transaction to be able to use a gas refund, it must avoid an Out of Gas exception. Assuming that a transaction had enough gas, then it can make use of the gas in the refund counter.

The amount of gas refunded is at most half the gas used. For example, if a transaction used 60,000 gas and cleared 2 contracts for a refund of 48,000 (24,000 each above), the originator would still pay the miner for 30,000 gas. This incentivizes miners to run transactions with negative gas operations, as it gives them revenue and ensures that it is impossible for a miner to pay for someone else’s computation.

## **Block gas limit (BGL)**

Recall that startGas is the user-specified, maximum amount of fuel that a transaction will consume. So how many transactions can fit in a block? The answer is until the sum from each startGas reaches the  **block gas limit**  (BGL). The BGL is currently 4,712,388 (digits of 1.5π), meaning around 224 transactions that each have a startGas of 21000 can fit in 1 block (which is produced every 15 seconds on average). To prevent Bitcoin-like divisiveness over whether blocks should become larger, the Ethereum protocol allows the miner of a block to adjust the BGL by a factor of 1/1024 (0.0976%) in either direction. Separate from the protocol is a default mining strategy of a minimum BGL of 4,712,388.

## **Potential delays with high startGas**

Since an Out of Gas exception is practically a waste of money for an originator, it is  always  better to overestimate the startGas than to underestimate. So, why shouldn’t an originator always specify a startGas of 4 million?

The answer lies in the difference between a transaction’s startGas and the gas it actually consumes. Miners only get paid for the actual gas consumed by a transaction; all unused gas is refunded to the originator.

If there is a transaction with a startGas of 4M and another 100 transactions with a startGas of 40,000 each, a miner would probably choose the latter as the 100 transactions have more predictable revenue. If the transaction with 4M startGas actually only consumes 1M of gas, then the miner loses 3M gas of potential revenue. Thus, miners are likely to prioritize a set of “small” transactions over a single transaction with a very high startGas (unless intrinsic gas is also very high). This can lead to a delay before a high startGas transaction is eventually mined and explains why an excessive startGas can be detrimental.

## **Detour to Exchanges**

There are 2 types of accounts in Ethereum:

-   User accounts (controlled by private keys)
-   Contracts (controlled by code)

Sending Ether (ETH) to a user account has a fee of 21000 gas but sending ETH to a contract has a higher fee, which depends on the contract code and data being sent in the transaction. Some exchanges may have only been providing 21000 gas for all their transactions, which means that when a user wanted to send ETH from an exchange to their contract wallet in Mist (Ethereum Wallet), the transaction would run Out of Gas and the ETH would never make it to the user’s contract wallet.

## **Metering vs. Fee**

One more note on the difference between metering and fees. In Bitcoin, metering is done with bytes: the number of bytes in the transaction. In Ethereum, computation also needs to be metered because a small amount of code could still be a program that runs forever. Metering computation is one of the reasons for gas. But having gas doesn’t mean requiring fees.

For example, in a private chain each account could have X gas per day, or each account could have Y gas per transaction, or some other scheme. On the flip side, having fees doesn’t mean requiring gas: fees can be based on different metering, such as bytes. Security in a public blockchain requires both gas and fees, while the alternatives are more applicable to private chains (for example, a scheme where each account has X gas per day can be Sybil-attacked in a public chain where anyone can create an account).

## **Conclusion**

Gas is metering and fuel for using the World Computer and is different from the Ether fee that is charged for using the World Computer. There is a difference between providing enough fuel and providing enough fee. Running Out of Gas costs money and it is safer to offer more gas, since all unused gas is refunded. Gas is a core part of Ethereum and the majority of its topics have been discussed. There’s more to it, including [estimating gas](https://web3js.readthedocs.io/en/v1.8.0/web3-eth.html#estimategas){:rel="nofollow"}, [intrinsic gas](https://github.com/ethereum/pyethereum/blob/3841e9a406f4ca9452afa45035fb07b5ce6314d9/ethereum/transactions.py#L177){:rel="nofollow"}, [effects on transaction size](http://ethereum.stackexchange.com/questions/1106/is-there-a-limit-for-transaction-size), and further exploration and understanding can be built on the foundations provided by this article.
