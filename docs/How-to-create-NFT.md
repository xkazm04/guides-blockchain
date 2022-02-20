# How to create NFT
*This guide describes how to create your own ERC-721 token using a single TATUM API call.*


---
## ERC-721

The [ERC-721](http://erc721.org/) token is a standardized smart contract with a predefined set of features. The tokens are non-fungible, which means that each one is unique. You can think of them as one-of-a-kind collectibles.

NFTs have been making waves in the digital world recently, and that for a good reason. These specialized tokens ensure verifiable authenticity and scarcity of digital assets, opening up a world of possibilities for a wide range of digital marketplaces.

From digital collectibles to in-game assets to one-of-a-kind releases of digital media, the possibilities are endless. Everyone wants to start implementing NFTs into their platforms, but they are bound to run into a few obstacles.
Creating a new NFT, or "smart contract", on an Ethereum-compatible blockchain is not easy at all. As a developer, you have to know a lot of things:
- Solidity - a new programming language for writing smart contracts
- How to use a compiler for Solidity
- How to develop using an IDE
- How to deploy the compiled smart contract

That is a lot of information; for developers who only need the basic features of ERC-721 tokens, it would take weeks to figure it all out.

This is why we've made prebuilt ERC-721 smart contracts that are ready to deploy with a single API. You can easily create NFTs without learning Solidity and deploy them on your blockchain of choice.

---
## TATUM unified API call

You can create your own ERC721 token using the Unified API call. Currently, the feature is supported for the following blockchains:
- Ethereum
- Polygon (Matic)
- Celo
- Harmony.ONE
- Flow
- Tron
- Binance Smart Chain

Check the API call documentation for information about all parameters. 

The parameters `chainId` and `x-testnet-type` are worth noting:
- The `chainId` parameter is used to select a specific blockchain for deploying our prebuilt NFTs.
- The `x-testnet-type` parameter is used to select which environment you want to call the API in. Valid for Ethereum invocations only.

In this example, we are using Celo because it is fast, cheap and you can pay for transactions with a stable coin (cUSD). However, you can deploy an NFT smart contract on any other supported blockchain with the same API call by changing the `chainId` parameter in the API call header.

<!-- theme: warning -->
> #### Security
>
> Blockchain transactions are signed using a private key via API, which is not a secure way of signing transactions. Your private keys and mnemonics should never leave your security perimeter. To correctly and securely sign a transaction, you can use the [Tatum CLI](https://github.com/tatumio/tatum-cli); a specific language library like [Tatum JS](https://github.com/tatumio/tatum-js); local [middleware API](https://github.com/tatumio/tatum-middleware); or our complex key management system, [Tatum KMS](https://github.com/tatumio/tatum-kms).

**Request example**

```json
{
  "name": "My ERC721",
  "symbol": "ERC_SYMBOL",
  "fromPrivateKey": "0x05e150c73f1920ec14caa1e0b6aa09940899678051a78542840c2668ce5080c2",
  "nonce": 0,
  "feeCurrency": "CELO"
}
```
**Response example**
```json
{
  "txId": "c83f8818db43d9ba4accfe454aa44fc33123d47a4f89d47b314d6748eb0e9bc9",
  "failed": false
}
```
---
<!-- theme: info -->

> #### Learn more
>
> If you want to learn more, we strongly recommend you go through the video tutorials below.

<!-- theme: danger -->

> #### Warning!
>
> The video tutorials do not reflect the latest version of the API calls; they merely serve the purpose of inspiration and a deeper understanding of the ERC-721 problematics.


***Frontend and user journey:***

https://www.youtube.com/watch?v=5OpFvUoP5Qc&ab_channel=Tatum

***Creating NFTs, working with metadata:***

https://www.youtube.com/watch?v=-eysAYS7l3o&ab_channel=Tatum

***Working with ERC-1155 multi-tokens:***

https://www.youtube.com/watch?v=YxB4oEn1C5g&feature=emb_title&ab_channel=Tatum