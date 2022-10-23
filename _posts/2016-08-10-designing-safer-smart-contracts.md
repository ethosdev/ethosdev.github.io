---
title: "Designing Safer Smart Contracts"
description:
permalink: 
---

In a short time Ethereum has turned into the world’s second largest cryptocurrency, with a market cap of $1B and thousands of nodes around the world running its code. Developers and companies in a number of industries are exploring its potential to improve existing processes. Beyond metrics, Ethereum has pioneered the idea of easy to write smart contracts, where complex contract logic can be created on the blockchain without the need for centralized parties with a programming language similar to that used by non-blockchain developers.

Still, as The DAO attack and subsequent hard fork show, challenges abound for those writing smart contracts at this nascent stage for Ethereum. While The DAO had a substantial number of technical issues and a flawed rollout process that have both been heavily debated, it’s  **impossible**  for any developer to write bug free code that is impervious to attack. This is exacerbated by developing on a new platform where real money is at risk. This problem is even more challenging because blockchain application code is held on an append-only ledger with advanced techniques, planning, and design required to change code. Given how easy Solidity is to write, developers with experience in other fields of software may feel a false sense of comfort.

At ConsenSys, where we have a number of decentralized projects underway like SingularDTV and Gnosis, the challenges faced by The DAO underscore how important it is to ensure that all our projects meet the highest safety and security standards. After reaching out to a number of Ethereum community members, we assembled a document outlining  [Ethereum Smart Contract Best Practices](https://github.com/ConsenSys/smart-contract-best-practices)  that is now available for anyone to contribute to.

We collected a number of anti-patterns and sample attacks, like The DAO’s  [re-entrancy](https://github.com/ConsenSys/smart-contract-best-practices/blob/c5fca3b6f137ad8c294e4767c5f9bb24ec186154/docs/attacks/reentrancy.md){:rel="nofollow"} bug, an example of a broader set of “race conditions”. For most attacks, we show code examples, outline the risks, and recommend potential solutions. Over time, we hope this will be a primary resource for those writing Solidity — and encourage you to add examples of good code or smart contract attacks you’ve seen.

Second, we had a spirited debate amongst ourselves about what philosophies the community needs to reduce the likelihood of a future systemic risk to the entire Ethereum platform. After all, at this stage in Ethereum’s development, no amount of memorized code idioms will prevent unknown, often unknowable, mistakes. As such, we highlighted a number of key philosophies for every Solidity developer from rolling out contracts slowly over a period to staying up to date with security discoveries.

Some of our observations are counterintuitive and should encourage debate. For example, we think that despite an easy language, blockchain programming is not far off from programming for a NASA launch or designing a financial exchange with money at risk, and so needs a substantial mindset shift from traditional web programming, especially for  _fail fast_  type products like social networks.

We encourage you to add your own commentary and  [file an issue](https://github.com/ConsenSys/smart-contract-best-practices/issues){:rel="nofollow"}  if you’d like to debate something. As all developers, it’s likely we’ve made mistakes or not considered all possibilities, and so we welcome corrections.

The goal is for the  [best practices](https://github.com/ConsenSys/smart-contract-best-practices)  to be community owned and maintained, with a focus on facts and minimal opinions. A  [bibliography](https://github.com/ConsenSys/smart-contract-best-practices/blob/7ecf3b31440718512066eb660d8d04010041196d/docs/bibliography.md){:rel="nofollow"}  embraces links to all relevant resources, including blog posts that can be added by anyone in the community, including the authors of such posts themselves. For those that prefer a frictionless approach instead of a pull request, the  [Ethereum Wiki Safety](https://github.com/ethereum/wiki/wiki/Safety/2a579c1668d57fe63faee895a1b3e80d2e3eceba){:rel="nofollow"}  page is editable by anyone. So feel free to fix a typo, add an example, add something from your blog, or write an entire section!

Over the course of the next few weeks and months, we will be going more in depth about some of the tips and philosophies outlined in the document.

Despite the challenges, the work we all do at this stage is pioneering new approaches to decentralized systems and each of us can have substantial impact given how early it is for the ecosystem.
* * *
_Initially published on the [ConsenSys blog](https://media.consensys.net/designing-safer-smart-contracts-d8389ba10d81){:rel="nofollow"}. Links have been fixed with Github versions. Anchor text for "Ethereum Smart Contract Best Practices" was originally "Ethereum Contract Security Techniques and Tips". My main co-authors on launch of the smart contract best practices were Nemil Dalal, Simon de la Rouviere, and Peter Borah._
