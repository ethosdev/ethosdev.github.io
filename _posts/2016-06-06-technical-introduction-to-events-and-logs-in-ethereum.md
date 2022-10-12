---
title: "Technical Introduction to Events and Logs in Ethereum"
description: "An introduction to use cases for events and logs on the Ethereum blockchain with sample code."
permalink: /events-and-logs-in-ethereum/
---

Events and logs are important in  [Ethereum](https://consensys.net/knowledge-base/about-ethereum-eth/)  because they facilitate communication between smart contracts and their user interfaces. In traditional web development, a server response is provided in a callback to the frontend. In Ethereum, when a transaction is mined, smart contracts can emit events and write logs to the  [blockchain](https://consensys.net/knowledge-base/about-blockchain-technology/)  that the frontend can then process. There are different ways to address events and logs. This technical introduction will explain some sources of confusion regarding events and some sample code for working with them.

Events can be confusing because they can be used in different ways. An event for one may not look like an event for another. There are 3 main use cases for events and logs:

1.  smart contract return values for the user interface
2.  asynchronous triggers with data
3.  a cheaper form of storage

The terminology between events and logs is another source of confusion and this will be explained in the third use case.

## 1) Smart contract return values for the user interface

The simplest use of an event is to pass along return values from contracts, to an app’s frontend. To illustrate, here is the problem.

```solidity
contract ExampleContract {  
  // some state variables ...  
  function foo(int256 _value) returns (int256) {  
    // manipulate state ...  
    return _value;  
  }  
}
```

Assuming exampleContract is an instance of ExampleContract, a frontend using web3.js, can obtain a return value by simulating the contract execution:

```js
var returnValue = exampleContract.foo.call(2);  
console.log(returnValue) // 2
```

However, when web3.js submits the contract call as a transaction, it cannot obtain the return value[^1]:

```js
var returnValue = exampleContract.foo.sendTransaction(2, {from: web3.eth.coinbase});  
console.log(returnValue) // transaction hash
```

The return value of a sendTransaction method is always the hash of the transaction that’s created. Transactions don’t return a contract value to the frontend because transactions are not immediately mined and included in the blockchain.

The recommended solution is to use an event, and this is one of the intended purposes for events.
```solidity
contract ExampleContract {  
  event ReturnValue(address indexed _from, int256 _value);function foo(int256 _value) returns (int256) {  
    ReturnValue(msg.sender, _value);  
    return _value;  
  }  
}  
```
A frontend can then obtain the return value:
```js
var exampleEvent = exampleContract.ReturnValue({_from: web3.eth.coinbase});  
exampleEvent.watch(function(err, result) {  
  if (err) {  
    console.log(err)  
    return;  
  }  
  console.log(result.args._value)  
  // check that result.args._from is web3.eth.coinbase then  
  // display result.args._value in the UI and call      
  // exampleEvent.stopWatching()  
})  
exampleContract.foo.sendTransaction(2, {from: web3.eth.coinbase})
```
When the transaction invoking foo is mined, the callback inside the watch will be triggered.  This effectively allows the frontend to obtain return values from foo.

## 2) asynchronous triggers with data

Return values are a minimal use case for events, and events can be generally considered as asynchronous triggers with data. When a contract wants to trigger the frontend, the contract emits an event. As, the frontend is watching for events, it can take actions, display a message, etc. An example of this is provided in the next section (a UI can be updated when a user makes a deposit.)

## 3) a cheaper form of storage

The third use case is quite different from what’s been covered, and that is using events as a significantly cheaper form of storage. In the Ethereum Virtual Machine (EVM) and Ethereum Yellow Paper[^2], events are referred to as logs (there are LOG opcodes). When speaking of storage, it would be technically more accurate to say that data can be stored in logs, as opposed to data being stored in events. However, when we go a level above the protocol, it is more accurate to say that contracts emit or trigger events which the frontend can react to. Whenever an event is emitted, the corresponding logs are written to the blockchain. The terminology between events and logs is another source of confusion, because the context dictates which term is more accurate.

Logs were designed to be a form of storage that costs significantly less gas than contract storage. Logs basically[^3] cost 8 gas per byte, whereas contract storage costs 20,000 gas per 32 bytes. Although logs offer gargantuan gas savings, logs are not accessible from any contracts[^4].

Nevertheless, there are use cases for using logs as cheap storage, instead of triggers for the frontend.  A suitable example for logs is storing historical data that can be rendered by the frontend.

A cryptocurrency exchange may want to show a user all the deposits that they have performed on the exchange. Instead of storing these deposit details in a contract, it is much cheaper to store them as logs. This is possible because an exchange needs the state of a user’s balance, which it stores in contract storage, but does not need to know about details of historical deposits.

```solidity
contract CryptoExchange {  
  event Deposit(uint256 indexed _market, address indexed _sender, uint256 _amount, uint256 _time);

  function deposit(uint256 _amount, uint256 _market) returns (int256) {  
    // perform deposit, update user’s balance, etc  
    Deposit(_market, msg.sender, _amount, now);  
}
```
  
Suppose we want to update a UI as the user makes deposits. Here is an example of using an event (Deposit) as an asynchronous trigger with data (_market, msg.sender, _amount, now). Assume cryptoExContract is an instance of CryptoExchange:

```js
var depositEvent = cryptoExContract.Deposit({_sender: userAddress});  
depositEvent.watch(function(err, result) {  
  if (err) {  
    console.log(err)  
    return;  
  }  
  
  // append details of result.args to UI  
})
```

Improving the efficiency of getting all events for a user is the reason why the `_sender` parameter to the event is indexed:

```solidity
event Deposit(uint256 indexed _market, address indexed _sender, uint256 _amount, uint256 _time)
```

By default, listening for events only starts at the point when the event is instantiated.  When the UI is first loading, there are no deposits to append to.  So we want to retrieve the events since block 0 and that is done by adding a `fromBlock` parameter to the event.

```solidity
var depositEventAll = cryptoExContract.Deposit({_sender: userAddress}, {fromBlock: 0, toBlock: 'latest'});  
depositEventAll.watch(function(err, result) {  
  if (err) {  
    console.log(err)  
    return;  
  }  
  // append details of result.args to UI  
})
```

When the UI is rendered `depositEventAll.stopWatching()` should be called.

## Aside — Indexed parameters

Up to 3 parameters can be indexed. For example, a proposed token standard has:

`event Transfer(address indexed _from, address indexed _to, uint256 _value)`

This means that a frontend can efficiently just watch for token transfers that are:

-   sent by an address `tokenContract.Transfer({_from: senderAddress})`
-   or received by an address `tokenContract.Transfer({_to: receiverAddress})`
-   or sent by an address to a specific address  
    `tokenContract.Transfer({_from: senderAddress, _to: receiverAddress})`

## Conclusion

Three use cases have been presented for events. First, using an event to simply get a return value from a contract function invoked with sendTransaction(). Second, using an event as an asynchronous trigger with data, that can notify an observer such as a UI. Third, using an event to write logs in the blockchain as a cheaper form of storage. 

This introduction has shown some of the APIs[^5] for working with events. There are other approaches to working with events, logs, and receipts[^6] and these topics can be covered in future articles.
* * *
_Thanks to Aaron Davis, Vincent Gariepy, and Joseph Lubin for feedback on this article._

[^1]: web3.js could watch for the transaction to be included the blockchain, then replay the transaction in an instance of the EVM, to get the return value, but this is a significant amount of logic to add to web3.js
[^2]: [https://github.com/ethereum/yellowpaper](https://github.com/ethereum/yellowpaper)
[^3]: There are gas costs of 375 for a LOG operation, and 375 gas per topic, but when many bytes are being stored, these costs represent an insignificant fraction of the total cost of the storage.
[^4]: Merkle proofs for logs are possible, so if an external entity supplies a contract with such a proof, a contract can verify that the log actually exists inside the blockchain.
[^5]: [https://github.com/ethereum/wiki/wiki/JavaScript-API#web3ethfilter](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3ethfilter)
[^6]: [http://ethereum.stackexchange.com/questions/1381/how-do-i-parse-the-transaction-receipt-log-with-web3-js](http://ethereum.stackexchange.com/questions/1381/how-do-i-parse-the-transaction-receipt-log-with-web3-js)

