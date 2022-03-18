# What are tokens and how to deploy token smart contracts? 

*Everything you need to know about different types of tokens and how to create them using Tatum.*

---

![whataretokens.png](https://stoplight.io/api/v1/projects/cHJqOjExNjE4OQ/images/zRCRwukL6OI)

## What are tokens?

Tokens are digital assets on blockchain networks. There are several different kinds and they’re all useful in their own ways. You’re probably already familiar with two of the most popular kinds of tokens: cryptocurrencies and NFTs. Cryptocurrencies are essentially digital money that you can use to buy stuff. Non-fungible tokens (NFTs) are one-of-a-kind digital assets.

What both types of assets have in common, is that they are both tokens created from programs on the blockchain called “smart contracts.” A smart contract is essentially the blueprint and the factory from which tokens are “minted” (created). It establishes the rules for the tokens that will be minted and is then used to mint them.

## What does “fungible” even mean?

Cryptocurrencies are fungible tokens, and the term “NFT” stands for “non-fungible token.” This “fungibility” just says whether every token is the same as the next, or whether each one is unique. Let’s take a look at the different types of tokens to better understand what this means for each one.

## What are the different types of tokens?

### Fungible tokens

Every fungible token is the same as the next and can be exchanged for any other token without changing anything. For example, if I have a dollar and you have a dollar, it doesn’t matter if we trade dollars… they’re exactly the same and any dollar can be exchanged for any other dollar. So dollars, like cryptocurrencies, are fungible. If I have 1 ETH and you have 1 ETH, I can send you my ETH and you can send me yours, and nothing has really changed... we both still have 1 ETH.

### Non-fungible tokens (NFTs)

If I have a highly valuable trading card, and you have a different, less valuable one, these cards cannot be exchanged 1-to-1. They are each unique. If we trade, I’m getting a bad deal, because I’m getting a less valuable card in exchange for my more valuable card. So in this way, these cards are non-fungible, just like NFTs. No NFT is the same as any other, and each NFT can have a different value depending on a variety of factors: rarity, demand, the quality of the metadata (image, audio file, video) included in the NFT. So basically, NFTs are similar to trading cards, and fungible tokens are more similar to traditional currencies.

### Semi-fungible tokens

There is also a third type of token, called “multi-tokens.” Multi-tokens are “semi-fungible.” Don’t worry, it’s really not as philosophical as it sounds. To understand multi-tokens, we have to understand what a token ID is:

**A token ID is a number that identifies each token minted from a smart contract.**

Since every fungible token is the same as the next, they don’t have token IDs. There’s no need for a unique identifier for each token if the tokens are all the same.

Since every NFT is a unique asset, each NFT has its own unique token ID. For example, every NFT in a collection would have a number associated with it (token ID 1, token ID 2, token ID 3, etc.), and no two NFTs created from the same smart contract can have the same token ID.

Multi-tokens, on the other hand, can have multiple tokens for a single, unique, token ID. We can mint say 30 tokens that all have the token ID 1, 30 with token ID 2, 30 with token ID 3.

### And why would you do something like that? 

Suppose you want to create an NFT of a valuable piece of artwork and sell fractional shares of it to multiple investors. If you create it as a multi-token, you could create thirty tokens that all have a token ID of “1”, and each investor could buy a few tokens to own a share in the piece of artwork. Investor 1 could own 15 tokens, investor 2 could own 5, and investor 3 could own 10. In this way, the split-ownership of any asset can be transparently and effectively represented on the blockchain, without any central authority to manage or organize it.

## How do I create tokens? 

### The old way...

Normally, to create tokens on any blockchain, you’d need to learn Solidity or another programming language to code your own smart contract. This takes quite a bit of time and effort. You’ve got to learn to code a smart contract for the token you want to create, debug and troubleshoot it to make sure it works and is secure, and deploy it onto the blockchain yourself.

The whole process involves at least 5 separate tools and could take weeks or months of your time to learn. And in the end, you’d just have a standard smart contract for minting one type of token on one blockchain. This is a huge barrier to entry for anyone who wants to simply create tokens to use in their apps.

### The Tatum way...

Since we’ve already done this many times for tons of blockchains, we asked ourselves, “Why should our users have to go through the trouble of coding smart contracts?”
You shouldn’t. That’s why we’ve created ready-to-go smart contracts on several blockchains to help you skip the whole process and just start making tokens. What this means, is that you don’t have to learn to code smart contracts using Solidity or any other programming language. Our experienced blockchain engineers have already done the hard work for you and created secure, audited, and standardized smart contracts that you can deploy instantly with 1 API call.

Not to mention, all of our token smart contracts APIs are unified across supported blockchains. This means that you can use the same API call to deploy tokens on Ethereum, Celo, Binance Smart Chain, Algorand, Flow, Polygon, or KCC. No need to code smart contracts for each blockchain, all you need to do is change the name of the chain in the call. Anyone can easily build multi-chain solutions without learning to code for each blockchain.

## How to deploy token smart contracts

### ERC-20

ERC-20 tokens are the most common type of fungible tokens. You might already be familiar with some, like USDC, BAT, or any other cryptocurrency. These tokens are not one-of-a-kind and do not have unique token IDs. Every token is the same as the next, and they can be used as currencies or any other use case that requires fungible tokens. If you want to create your own cryptocurrency, then you’ll want to use ERC-20 tokens.

You can deploy one of our prebuilt ERC-20 smart contracts with just 1 API call. The main difference between ERC-20 tokens and the other types we’ll discuss is that you also mint the initial supply of tokens when you deploy the smart contract in Tatum.


To deploy an ERC-20 smart contract on Celo, we’ll use our JavaScript SDK, which you can download and install [here](https://tatum.io/tatum-js.html).

<div class="toolbar-note">
To deploy smart contracts and mint tokens, you’ll need to pay gas fees using the private key of an address that holds crypto. For testing purposes, we recommend using [Celo testnet](https://docs.celo.org/getting-started/alfajores-testnet) and getting testnet funds from a [Celo faucet](https://celo.org/developers/faucet).
For production use, you should NEVER send private keys over the Internet. Instead, you should use [Tatum KMS to generate an encrypted signature ID and sign transactions locally](https://docs.tatum.io/tutorials/how-to-securely-store-private-keys).
</div>

Use the following API call to deploy your own ERC-20 token smart contract:

<div class='tabbed-code-blocks'>
``` Tatum JS
import {prepareDeployErc20SignedTransaction} from '@tatumio/tatum'
/**
 * Sign Ethereum deploy ERC20 transaction with private keys locally. Nothing is broadcast to the blockchain.
 * @param body content of the transaction to broadcast
 * @param provider url of the Ethereum Server to connect to. If not set, default public server will be used.
 * @returns transaction data to be broadcast to blockchain.
*/ 
const body = {
  chain: "CELO",
  symbol: "ERC_SYMBOL",
  name: "MyERC20",
  totalCap: "10000000",
  supply: "10000000",
  digits: 18,
  address: "0xa0Ca9FF38Bad06eBe64f0fDfF279cAE35129F5C6",
  fromPrivateKey: "0x05e150c73f1920ec14caa1e0b6aa09940899678051a78542840c2668ce5080c2",
  feeCurrency: "CELO"
}
const prepareDeploy = await prepareDeployErc20SignedTransaction(body)
```
</div>

The required parameters are:

- `chain` — the name of the blockchain (“CELO” in our case)
- `symbol` — the symbol of the ERC-20 token
- `name` — the name of the ERC-20 token
- `totalCap` — the total supply of the token that can be created
- `supply` — the initial supply of the token to be minted with the deployment of the contract. If the total supply is greater than the initial supply, more tokens can be minted at any point in the future.
- `digits` — the number of digits of the token
- `address` — the address to which the initial supply of the token will be minted
- `fromPrivateKey` — the private key of the address from which the gas fees will be paid. 
<div class="toolbar-warning">
NOTE: you should NEVER use private keys in productions use. They are fine for testing and demo purposes on testnets, but for production use you should use a secure key management system like [Tatum KMS](https://docs.tatum.io/tutorials/how-to-securely-store-private-keys).
</div>

- `feeCurrency` - the currency in which the gas fees will be paid (only for Celo).

For more on working with ERC-20 fungible tokens, check out our [full guide](https://docs.tatum.io/guides/blockchain/how-to-create-an-erc-20-token).

### ERC-721

This is the classic NFT standard. Each token minted from this type of smart contract is one-of-a-kind with a unique token ID. ERC-721 tokens are ideal for creating and selling digital collectibles.

Use the following API endpoint to deploy an NFT smart contract on Celo:

<div class='tabbed-code-blocks'>
```Request body
const transactionHash = await deployNFT(false, {
    body.chain: Currency.CELO,
    body.name: 'MY_ERC721',
    body.symbol: 'ERC_SYMBOL',
    body.fromPrivateKey: '0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb',
    body.feeCurrency: Currency.CUSD
});
```
</div>

The required parameters are:
- `chain` — the name of the blockchain (“CELO” in our case)
- `name` — the name of the NFT collection
- `symbol` — the symbol of the NFT collection
- `fromPrivateKey` — the private key of the address from which the gas fees will be paid. 
- `feeCurrency` — the currency in which the gas fees will be paid (only for Celo).

And that’s it: you’ve just deployed an ERC-721 smart contract on Celo. Be sure to check out our full guide on [how to work with NFTs in Tatum](https://docs.tatum.io/guides/blockchain/how-to-create-nft-token) for how to mint NFTs from your smart contract, transfer them, and more.

### ERC-1155

ERC-1155 is the main token standard used to create multi-tokens. As we mentioned, the main difference between these and ERC-721 tokens is that ERC-1155s can have multiple tokens for a single token ID. This allows you to create shared-ownership NFTs, perform batch functions, and opens doors for a variety of interesting use cases.

While their functionality is more complex than that of ERC-721 tokens, deploying them with Tatum is just as easy. Use the following endpoint to deploy an ERC-1155 smart contract:

<div class='tabbed-code-blocks'>
```TatumJS
import {Currency, deployMultiToken} from '@tatumio/tatum';
/**
 * Deploy MultiTokens (1155) contract.
 * @param testnet - testnet or mainnet
 * @param body - body of the request - https://docs.tatum.io/rest/smart-contracts/deploy-multi-token-smart-contract
 * @param provider - (optional) provider to broadcast tx
 */
const body = {
  fromPrivateKey: '0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb',
  chain: Currency.CELO,
  uri: "https://example.com/{id}",
  feeCurrency: Currency.CELO
}
const txId = deployMultiToken(false, body);
```
</div>

The required parameters are:

- `chain` — the name of the blockchain (“CELO” in our case)
- `uri` — the URL of the NFT metadata which will be included in tokens minted from the smart contract.
- `fromPrivateKey` — the private key of the address from which the gas fees will be paid. 
<div class="toolbar-note">
NOTE: you should NEVER use private keys in productions use. They are fine for testing and demo purposes on testnets, but for production use you should use a secure key management system like [Tatum KMS](https://docs.tatum.io/tutorials/how-to-securely-store-private-keys).
</div>

- `feeCurrency` — the currency in which the gas fees will be paid (only for Celo).
For more on how to mint multi-tokens from your smart contract and perform other operations, be sure to check out our [full guide](https://docs.tatum.io/guides/blockchain/how-to-create-erc-1155-multi-tokens).

## Ready to get started?

If you’re ready to jump right in and try it for yourself, go ahead and [sign up for an API key](https://dashboard.tatum.io/sign-up) to communicate with Tatum’s infrastructure, and dive into [our docs](https://docs.tatum.io/) to find out everything you can do with Tatum.

If you need any help, hop onto the Tatum [Discord](https://discord.com/invite/4TWtSP3vxU) and our devs will get back to you asap.

Happy coding!


