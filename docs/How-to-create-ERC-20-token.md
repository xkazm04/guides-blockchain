---
tags: [ERC20]
---

# How to create ERC-20 token

*This article describes how to create your own ERC-20 token using a single TATUM API call.*



---

## ERC-20

An ERC-20 token is a standardized smart contract with a predefined set of features. It represents fungible tokens which can be interchanged. The ERC-20 token is used as a blockchain representation of the currency.

To create your own ERC-20 token, you need to deploy a smart contract. Creating your own smart contract is not at all easy. As a developer, you must do a few things:
- Be able to write smart contracts in Solidity (or any other smart contract programming language used by a given blockchain) 
- Run the Solidity compiler
- Run the Solidity IDE
- Deploy the compiled Smart Contract
It is a lot to know. For developers who only need the basic features of ERC-20 tokens, Tatum offers ready-to-go, standardized ERC-20 smart contracts that can be deployed on the following blockchains:
- Ethereum
- Polygon (Matic)
- Celo
- Harmony.ONE
- Flow
- Tron
- Binance Smart Chain

---

## Tatum unified API call

This Unified API method can register any supported fungible token. For all fungible tokens, we use the same request body parametrization. All mandatory and optional parameters can be seen below.

Request body is divided into two groups: **ERC20** and **ERC20Address**.

The difference is that in **ERC20**, `derivationIndex` and `xPub` parameters are mandatory, while for **ERC20Address**, only `address` is mandatory. This is the only difference between the methods.

A request for the fungible token **TRON TRC-10/20** has an additional `type` parameter that distinguishes between **TRC10** and **TRC20**. This call only has one request body, which contains all the parameters.

Check the API call documentation for information about all parameters. 

The parameter `chainId` and `x-testnet-type` are worth noting.

The `chainId` parameter is used to select a specific blockchain for deploying our prebuilt ERC-20 tokens.

The `x-testnet-type` parameter is used to select which environment you want to call the API in. Valid for Ethereum invocations only.

<!-- theme: warning -->
> #### Security
>
> Blockchain transactions are signed using a private key via API, which is not a secure way of signing transactions. Your private keys and mnemonics should never leave your security perimeter. To correctly and securely sign a transaction, you can use the [Tatum CLI](https://github.com/tatumio/tatum-cli); a specific language library like [Tatum JS](https://github.com/tatumio/tatum-js); local [middleware API](https://github.com/tatumio/tatum-middleware); or our complex key management system, [Tatum KMS](https://github.com/tatumio/tatum-kms).


Here you can see an example for TRC-10 TRON API calls:


**Request body example**
```json
{
  "symbol": "MY_TOKEN",
  "supply": "1000000.0",
  "decimals": 8,
  "type": "TRC10",
  "description": "My Public Token",
  "basePair": "EUR",
  "baseRate": 1,
  "customer": {
    "accountingCurrency": "USD",
    "customerCountry": "US",
    "externalId": "123456",
    "providerCountry": "US"
  },
  "accountingCurrency": "USD",
  "derivationIndex": 0,
  "xpub": "xpub6EsCk1uU6cJzqvP9CdsTiJwT2rF748YkPnhv5Qo8q44DG7nn2vbyt48YRsNSUYS44jFCW9gwvD9kLQu9AuqXpTpM1c5hgg9PsuBLdeNncid",
  "address": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85"
}
```

**Response example**

```json
{
  "accountId": "5e68c66581f2ee32bc354087",
  "address": "0xa7673161CbfE0116A4De9E341f8465940c2211d4"
}
```

---
<!-- theme: info -->

>#### Source code
> Check out the source code of our ERC-20 smart contract on [Github](https://github.com/tatumio/tatum-middleware/blob/master/src/contracts/token.sol).

> #### Learn more
>If you want to learn more about [ERC-20](https://www.investopedia.com/news/what-erc20-and-what-does-it-mean-ethereum/), this might be useful. 