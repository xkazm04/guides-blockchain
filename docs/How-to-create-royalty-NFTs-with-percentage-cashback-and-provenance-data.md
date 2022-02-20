# How to create royalty NFTs with percentage cashback and provenance data

*Create NFTs that payout cashback as a percentage of every sale and record provenance data with every transaction.*

---

## NFT 

**Tatum royalty NFT**s are [ERC-721](http://erc721.org/) tokens with specialized smart contracts that pay creators cashback with each subsequent transaction. They can either pay a fixed amount of cashback or cashback percentages. This guide shows how to use NFTs that pay out cashback as a percentage of each sale and record provenance data with each transaction. For information on creating NFTs that pay out cashback as a fixed value without provenance data, see [this guide](../token/ZG9jOjI4OTA4NjI2-how-to-create-royalty-nf-ts-with-fixed-cashbach).

<!-- theme: info -->
>The ERC-721 token is a standardized smart contract with a predefined set of features. The tokens are non-fungible, which means that each one is unique. You can think of them as one-of-a-kind collectibles.

## How can creators keep getting paid?

NFTs have surged in popularity, and many NFT marketplaces have implemented functionality to allow creators to receive a portion of every subsequent sale of their work. This is great for creators, as it allows them to continue getting paid each time their NFT changes hands.

However, there's one critical flaw. Once the NFT leaves the marketplace  it was originally sold on, the creator has no way of collecting any money ever again. This is because the mechanism for collecting portions of subsequent sales is only implemented at application level, not at blockchain level.

For this reason, we have created instantly deployable NFT smart contracts that implement this functionality at blockchain level. Payouts for subsequent sales to the original creators happen automatically, and forever. Creators will keep getting paid for as long as their NFT exists.

<!-- theme: warning -->
>**Tatum royalty NFT**s are primarily intended for higher-value NFTs. The royalty functionality is currently incompatible with OpenSea and must be transferred using smart contract methods not currently supported within the platform. As soon as OpenSea allows this standard and method calls, our royalty NFTs will be fully compatible with the platform.

Tatum royalty NFTs can currently be created on:
- Ethereum
- Celo
- Binance Smart Chain
- Polygon
- Harmony.ONE

---
## What is provenance data?

While NFTs exist on the blockchain and their authenticity can be verified, there have still been attempts at forging high-value NFTs. To ensure the authenticity of NFTs, we have created NFT smart contracts that record metadata about each transaction every time the token is transferred. The entire transaction history of a token is included in the token itself, and its authenticity can be verified quickly and easily.

---

## Creating an NFT smart contract

In this guide, we will be using the Tatum JavaScript SDK to create NFTs and work with them. 

## Import required libraries

First, we'll need to download and import libraries required for the commands we'll be using. You can download the [Tatum JS SDK](https://www.npmjs.com/package/@tatumio/tatum/v/1.16.17) here.
Then import the required libraries:
```json
import { deployNFT, mintNFTWithUri, mintMultipleNFTWithUri } from '@tatumio/tatum';
```

---
## Creating an NFT smart contract

To create an NFT, you would normally need to learn Solidity and code your own smart contract. To speed things up, we've created prebuilt and validated smart contracts for you. You can instantly deploy them with just a few lines of code.

In this example, we will be deploying an NFT on Ethereum. The required parameters are:
- `name`: NFT name
- `chain`: The name of the blockchain
- `symbol`: The NFT symbol
- `provenance`: Enabling provenance in this call also enables percentage royalty cashback.

```json
const transactionHash = await deployNFT(false, {
    name: 'MY_NFT',
    chain: Currency.ETH,
    symbol: 'NFT_SYMBOL',
    provenance: true
});
```
And just like that, you have deployed an NFT smart contract. 