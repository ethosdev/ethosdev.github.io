---
title: "Audit the Deployed Smart Contract, Not Github!"
description: "Best practice smart contract audits should be performed on a release candidate, which should be deployed on Ethereum mainnet before an audit is performed."
permalink: audit-deployed-smart-contract
---

_Best practice smart contract audits should be performed on a release candidate, which should be deployed on Ethereum mainnet before an audit is performed._

Suppose Mallory is looking to have her smart contract audited so she gives  [Bob](https://en.wikipedia.org/wiki/Alice_and_Bob), her auditor, some git hash, or (gasp!) a zip file of the source code. Mallory has taken care to follow the first rule of a responsible token sale by implementing a staggered release of funds and vesting contracts. Bob clones the repo, audits and reports issues, checks the fixes, and proclaims the audit is completed. Mallory announces the audit results, launches her ICO, and collects millions. The next day, however, all the funds, which were supposedly locked, have been sent to mysterious addresses and Mallory is never to be found again. A simple post-mortem finds that the smart contract Mallory deployed had a subtle backdoor that was added to the source code Bob had audited.

In a software release life cycle, a  [release candidate](https://en.wikipedia.org/wiki/Software_release_life_cycle#Release_candidate)  (RC) is the final stage before software is launched. To minimize code churn, an audit should be performed on an RC. An RC does not change, and it eventually  _becomes the release itself_, unless there are  showstopper bugs, which then get fixed and another RC is generated. Historically, an RC is managed by a version control system, but where does a smart contract RC belong? On the blockchain!

An auditor's responsibility to the community should include ensuring that what they audit is what is released to the community to use. Auditors can audit the RC that's actually on the blockchain, not some malleable code on Github that might get deployed. Auditors should ask their clients for the Ethereum mainnet addresses of the RC contracts. The community should request that bug bounty programs refer to Ethereum mainnet addresses. This methodology has four main benefits. Though we'll be speaking in terms of a single smart contract, the methodology is also applicable to a system of smart contracts.

## Four Benefits

First, auditing the deployed smart contract eliminates scenarios where the audited source code is accidentally or maliciously changed then deployed to mainnet. The smart contract is already on mainnet and its code cannot be changed.

Second, using a mainnet address  **signifies readiness**  for an audit and reduces code churn. Let's take Alice, who thinks her code is ready for an audit. Is it really ready? Or is there one more change or tweak left? Not having a mainnet address suggests that the contract isn't quite ready. By actually deploying the contract, Alice makes a distinct decision that the code is as ready as can be at that point in time. Even though it only costs $1 to pay the gas for deploying her contracts, Alice is unlikely to make code commits and redeploy several times. Bob is relieved because he can't audit a moving target.

Third, by actually deploying the contracts to mainnet, Alice  **increases her readiness**  for launch. In the ideal case that there are no showstoppers, the RC becomes the release and Alice can simply announce her launch! Note that before a mainnet deploy, any respectable development team will have addressed security as early as possible in development and have done rigorous testing, including testnets.

Fourth,  **ecosystem synergies**  can emerge. Since Bob has audited a contract with an address, he can have a registry of all the contracts he's audited. Charlie, another auditor can have a uPort attestation to Bob's registry as well as his own registry. User interfaces can be built in which someone can input an address and see who's audited the contract. Tools like MetaMask and Etherscan can tap into these, and further creative tools and DApps can leverage the ecosystem and expand it.

## How the audit process might look

1.  Alice deploys her wallet contract and gets the address 0x851b7f3ab81bd8df354f0d7640efcd7288553419.
2.  Alice uploads and verifies her source code on etherscan.
3.  Bob gets the source code at https://etherscan.io/address/0x851b7f3ab81bd8df354f0d7640efcd7288553419#code
4.  Bob runs  [verifier](https://www.npmjs.com/package/eth-bytecode-verifier){:rel="nofollow"} to ensure that the source code compiled bytecode matches the bytecode on the blockchain.
5.  Bob audits the source code (splitting it into smaller pieces if it helps him).
6.  Alice makes required fixes and repeats the process (typically only one more loop). The process can end immediately here if it's deemed that there are no showstoppers that need to be fixed.

## Verifying the source code from the blockchain

Perhaps people have been trusting contract source code on Etherscan too much. Alex Luoyuan has built a tool to help verify that the deployed bytecode at a given address matches the compiled output from source code:

[https://www.npmjs.com/package/eth-bytecode-verifier](https://www.npmjs.com/package/eth-bytecode-verifier){:rel="nofollow"}

Source code verification is only available on Etherscan for now, but stay tuned for further developments.

## Smart Contract Design

Alice may need to design or refactor her contract to support at least two states:

1.  ContractDeployed
2.  ContractReady

For example, if Alice had a token sale contract that started as soon as it was deployed, it would need to be modified so that auditors, bug bounty programs, and the public could examine the contract while it is in a ContractDeployed state and before the sale has commenced. The act of launching would then involve sending a transaction to configure and transition the contract to a ContractReady state where the token sale can accept contributions.

In practice, we don't really see such contracts that support two states breaking the functionality of the contract. If there are exceptions, they can be examined. Also, less interactive contracts such as a wallet don't need to necessarily support two states.

## What do you think?

Auditing deployed smart contracts is the closest thing to auditing what users will actually interact with. If you're an auditor, ask your clients for their mainnet contract addresses: it will save everyone time, make the audit process more secure, and help tap into ecosystem synergies. Learnings from this methodology will be described in a future post.
* * *
_Thanks to Maurelian, Gonçalo Sá, Alex Luoyuan, Eva Shon, and Bernhard Mueller for feedback._
