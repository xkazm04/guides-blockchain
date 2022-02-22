# How to create NFTs on Flow

*Colectibles in the blockchain are just a few API calls away.*

---

NFTs have been making waves in the digital world recently and for good reason. These specialized tokens ensure verifiable authenticity and scarcity of digital assets, opening up a world of possibilities for a wide range of digital marketplaces.

From digital collectibles to in-game assets to one-of-a-kind releases of digital media, the possibilities are endless. Everyone wants to start implementing NFTs into their platforms, but they're running into a few obstacles.

Creating a new NFT, or "smart contract" on the Flow blockchain, is not easy at all. As a developer, you have to know a lot of things:
- Cadence - a new programming language for writing smart contracts
- How to use a compiler for Cadence
- How to develop using an IDE
- How to deploy the compiled smart contract
- Undergo a review process from DapperLabs
It's a lot to know, and for developers who only need the basic features of NFT tokens, it would take weeks to figure it all out. Not to mention, on Flow, once you've coded your own smart contract, you need it to be audited and approved by the Flow team. While this service is very useful for ensuring that your smart contract is secure, it can takes weeks or months to get approval. We have already gotten our NFT smart contract audited and approved on Flow, and you can register a section of it for your app or marketplace to mint as many NFTs as you would like.


 <div class="toolbar-tip">

Tatum has a prebuilt FLOW NFT contract ready for deployment. The source code is available on GitHub.
</div>

 <div class="toolbar-warning">

In this guide, we are signing blockchain transactions using a private key via API. This is fine for testing and demo purposes, but for production use, it is not a secure way of signing transactions. 

Your private keys and mnemonics should never leave your security perimeter. To correctly and securely sign a transaction, you can use [Tatum CLI](https://github.com/tatumio/tatum-cli) from the command line, a specific language library like [Tatum JS](https://github.com/tatumio/tatum-js), or our comprehensive key management system, [Tatum KMS](https://github.com/tatumio/tatum-kms).
</div>
---

## Generate a Flow wallet, address, and private key

The first step is to generate a wallet on the Flow blockchain. The wallet will be used to generate and hold account addresses and private keys.
<div class='tabbed-code-blocks'>
```SDK
import {generateFlowWallet} from '@tatumio/tatum';
/**
 * Generate Flow wallet
 * @param mnem mnemonic seed to use
 */
 const mnemonic = "fabric recipe physical attend foam also inspire swear risk outside monkey palm mutual park civil peasant virtual angle curve tennis follow gym put transfer"
 const wallet = await generateFlowWallet(mnemonic);
```
```REST API call
curl --location --request GET 'https://api-eu1.tatum.io/v3/flow/wallet' \
--header 'x-api-key: YOUR_API_KEY'
```
</div>

The response will contain the wallet's extended public key (xpub) and mnemonic phrase which you will then use to generate addresses and private keys. If you enter a custom mnemonic in the generate wallet API request, this mnemonic will be included in the response. If no custom mnemonic is entered, a mnemonic will be generated for you.

```Response
{
    "mnemonic": "fabric recipe physical attend foam also inspire swear risk outside monkey palm mutual park civil peasant virtual angle curve tennis follow gym put transfer",
    "xpub": "xpub6EWe9RRoaXxhWR6K1boerSprD2Q84756t8PMoqWS2WCAYmMVxYRXqj5c4PboamiUmxrvKpcoZw2z714kviYL5fQ7VNiktHz3XeBxHqu5Xdh"
}
```

From the xpub generated in the previous call, you can now create an account address on the Flow blockchain. The address you generate will be used as a minter address - it will only be possible to mint new tokens from this account address.
<div class='tabbed-code-blocks'>
```SDK
import {Currency, generateAddressFromXPub} from '@tatumio/tatum';
/**
 * Generate address
 * @param currency type of blockchain
 * @param testnet testnet or mainnet version of address
 * @param xpub extended public key to generate address from
 * @param i derivation index of address to generate. Up to 2^31 addresses can be generated.
 * @returns blockchain address
 */
 const xpub = 'bafybeihrumg5hfzqj6x47q63azflcpf6nkgcvhzzm6f6nhic2eg6uvozlq'
 const address = await generateAddressFromXPub(Currency.FLOW, false, xpub, 1);
```
```REST API call
curl --location --request GET 'https://api-eu1.tatum.io/v3/flow/address/xpub6EWe9RRoaXxhWR6K1boerSprD2Q84756t8PMoqWS2WCAYmMVxYRXqj5c4PboamiUmxrvKpcoZw2z714kviYL5fQ7VNiktHz3XeBxHqu5Xdh' \
--header 'x-api-key: YOUR_API_KEY'
```
</div>
The response will contain the address you have generated.

```Response
{
    "address": "0x3dd16e9a72762818"
}
```

---

## Creating an NFT Smart Contract

To deploy an NFT smart contract using Tatum, all you need is one simple API call. For most supported blockchains, this API call deploys a standard, validated NFT smart contract to the blockchain of your choice. 

In the case of Flow, it registers a section of a pre-deployed smart contract for you to mint NFTs from. This is because deploying a new smart contract on Flow requires an audit from the Flow team, which can take weeks or months to complete. Our Flow smart contract has already been audited and deployed, and you can mint as many NFTs from it as you would like.

**The signing address in the request is the address of the minter. It will only be possible to mint new tokens from this address.**

 <div class="toolbar-tip">

A Flow address can be used only once to deploy a new NFT contract. If you want to register multiple sections of our smart contract for use, you need to use multiple addresses as deployer/minter addresses.
</div>
<div class='tabbed-code-blocks'>
```SDK
import {Currency, deployNFT} from '@tatumio/tatum';
const transactionHash = await deployNFT(false, {
    body.chain: Currency.FLOW,
    body.fromPrivateKey: 'e08f2ef94365e1696698f1df3dbfdecf54e255fd51c5db1040ddacf219b79edd',
    body.account: '0x3dd16e9a72762818'
});
```
```REST API call with Private key
curl --location --request POST 'https://api-eu1.tatum.io/v3/nft/deploy/' \
--header 'Content-Type: application/json' \
--header 'x-api-key: YOUR_API_KEY' \
--data-raw '{
    "chain": "FLOW",
    "privateKey": "e08f2ef94365e1696698f1df3dbfdecf54e255fd51c5db1040ddacf219b79edd",
    "account": "0x3dd16e9a72762818"
}'
```
```REST API call with KMS
curl --location --request POST 'https://api-eu1.tatum.io/v3/nft/deploy/' \
--header 'Content-Type: application/json' \
--header 'x-api-key: YOUR_API_KEY' \
--data-raw '{
    "chain": "FLOW",
    "signatureId": "26d3883e-4e17-48b3-a0ee-09a3e484ac83",
    "index": 0,
    "account": "0x3dd16e9a72762818"
}'
```
</div>
The response will contain a transaction ID, which we will use to get the address of the section of the NFT smart contract registered to you.

```Response
{
    "txId": "df09b27ca2b4655c4c4d5d4e0a402e6a01c04a0c08f4d9e17d31df0b6358bab7"
}
```

---

## Getting the NFT smart contract address

Using the transaction ID from the response of the previous call, you can now get the address of the section of the NFT smart contract on the Flow blockchain registered to you.

<div class='tabbed-code-blocks'>
```SDK
import {Currency, getNFTContractAddress} from '@tatumio/tatum';
const contractAddress = await getNFTContractAddress(Currency.FLOW, 'df09b27ca2b4655c4c4d5d4e0a402e6a01c04a0c08f4d9e17d31df0b6358bab7');
```
```REST API call
curl --request GET \
  --url https://api-eu1.tatum.io/v3/blockchain/sc/address/FLOW/df09b27ca2b4655c4c4d5d4e0a402e6a01c04a0c08f4d9e17d31df0b6358bab7 \
  --header 'x-api-key: REPLACE_KEY_VALUE'
```
</div>

The response will contain the address of the section of the pre-deployed NFT smart contract on the Flow blockchain which is registered for you to use.

```Response
{
    "contractAddress": "0182e912-329c-4f98-a481-b7515f8a8c51"
}
```

---

## Minting a new unique NFT token

When your NFT smart contract is deployed and you know the contract address, you can issue new unique tokens with the Mint NFT endpoint. This endpoint will create a new token with a unique, automatically generated token ID.

Every token should have [metadata as a JSON schema](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-721.md#specification) containing additional information about the token, like the image URL, description, etc. With Tatum, you can upload your NFT metadata to IPFS for free with our native IPFS integration. For more on how to do it, please refer to this guide.

Use the following API endpoint to mint NFTs on Flow:

<div class='tabbed-code-blocks'>
```SDK
import {Currency, mintNFTWithUri} from '@tatumio/tatum';
const transactionHash = await mintNFTWithUri(false, {
    to: '0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea',
    url: 'https://my_token_data.com',
    chain: Currency.FLOW,
    contractAddress: '0182e912-329c-4f98-a481-b7515f8a8c51',
    fromPrivateKey: 'e08f2ef94365e1696698f1df3dbfdecf54e255fd51c5db1040ddacf219b79edd',
    account: '0x3dd16e9a72762818'
});
```
```REST API call with Private key
curl --location --request POST 'https://api-eu1.tatum.io/v3/nft/mint/' \
--header 'Content-Type: application/json' \
--header 'x-api-key: YOUR_AP_KEY' \
--data-raw '{
    "to": "0x3dd16e9a72762818",
    "url": "https://my_token_data.com",
    "contractAddress": "0182e912-329c-4f98-a481-b7515f8a8c51",
    "chain": "FLOW",
    "privateKey": "e08f2ef94365e1696698f1df3dbfdecf54e255fd51c5db1040ddacf219b79edd",
    "account": "0x3dd16e9a72762818"
}'
```
```REST API call with KMS
curl --location --request POST 'https://api-eu1.tatum.io/v3/nft/mint/' \
--header 'Content-Type: application/json' \
--header 'x-api-key: YOUR_AP_KEY' \
--data-raw '{
    "to": "0x3dd16e9a72762818",
    "url": "https://my_token_data.com",
    "contractAddress": "0182e912-329c-4f98-a481-b7515f8a8c51",
    "chain": "FLOW",
    "signatureId": "26d3883e-4e17-48b3-a0ee-09a3e484ac83",
    "index": 0,
    "account": "0x3dd16e9a72762818"
}'
```
</div>
The response will contain the ID of the minted token and the ID of the transaction.

```Response
{
    "txId": "1e28c9eea5750fccc2db5c34c51d01f040a4ad7e51ffa6e6476d7e69088d7afc",
    "tokenId": 12
}
```

---

## Transferring a specific NFT token

To transfer tokens from the address where they were issued to another blockchain address, you can use the Transfer NFT token endpoint. You'll need the private key of the address where the tokens are located (the address from the first call where the initial supply was distributed), or the signature ID of a transaction signed using Tatum KMS.

<div class='tabbed-code-blocks'>
```SDK
import {Currency, transferNFT} from '@tatumio/tatum';
const transactionHash = await transferNFT(false, {
    to: '0x2d0d7b39db4e3a08',
    tokenId: '12',
    chain: Currency.FLOW,
    contractAddress: '27320939-3087-490e-a65e-a53c8b06fcd9',
    fromPrivateKey: 'e08f2ef94365e1696698f1df3dbfdecf54e255fd51c5db1040ddacf219b79edd',
    account: '0x3dd16e9a72762818'
});
```
```REST API call with Private key
curl --location --request POST 'https://api-eu1.tatum.io/v3/nft/transaction/' \
--header 'Content-Type: application/json' \
--header 'x-api-key: YOUR_API_KEY' \
--data-raw '{
    "tokenId": "12",
    "chain": "FLOW",
    "to": "0x2d0d7b39db4e3a08",
    "contractAddress": "27320939-3087-490e-a65e-a53c8b06fcd9",
    "privateKey": "e08f2ef94365e1696698f1df3dbfdecf54e255fd51c5db1040ddacf219b79edd",
    "account": "0x3dd16e9a72762818"
}'
```
```REST API call with KMS
curl --location --request POST 'https://api-eu1.tatum.io/v3/nft/transaction/' \
--header 'Content-Type: application/json' \
--header 'x-api-key: YOUR_API_KEY' \
--data-raw '{
    "tokenId": "12",
    "chain": "FLOW",
    "to": "0x2d0d7b39db4e3a08",
    "contractAddress": "27320939-3087-490e-a65e-a53c8b06fcd9",
    "signatureId": "26d3883e-4e17-48b3-a0ee-09a3e484ac83",
    "index": 0,
    "account": "0x3dd16e9a72762818"
}'
```
</div>
The response will contain a transaction ID, from which you can obtain the details of the transaction.

```Response
{
    "txId": "d7d42e8bb3e5335e8db6ff19c7658004ecc0d7b24c120b9fbe60daaee33e7fdf"
}
```

---

## Getting a list of tokens that belong to the address

If you want to display a list of tokens that someone possesses, you can use the Get NFT Account balance endpoint.
Simply enter:
- The **name** of the blockchain
- The **contractAddress** of the NFT smart contract.
- The **address** for which you would like to obtain a list of token IDs

<div class='tabbed-code-blocks'>
```SDK
import {getNFTsByAddress} from '@tatumio/tatum';
const nfts = await getNFTsByAddress(
    Currency.FLOW,
    '27320939-3087-490e-a65e-a53c8b06fcd9/0x3dd16e9a72762818',
);
```
```REST API call
curl --location --request GET 'https://api-eu1.tatum.io/v3/nft/balance/FLOW/27320939-3087-490e-a65e-a53c8b06fcd9/0x3dd16e9a72762818' \
--header 'x-api-key: YOUR_API_KEY'
```
</div>
The response will contain the token IDs held by the address entered in the request.

```Response
[
  10,
  11
]
```

---

## Obtaining metadata for a specific token

If you want to obtain a metadata URL for a token, you can use the Get NFT Token metadata endpoint.
The required parameters are:
- The **name** of the blockchain
- The **contractAddress** of the NFT smart contract
- The **tokenId** of the NFT
- The **account** holding the NFT

<div class='tabbed-code-blocks'>
```SDK
import {Currency, getNFTMetadataURI} from '@tatumio/tatum'
const metadataURI = await getNFTMetadataURI(
    Currency.FLOW,
    contractAddress: '27320939-3087-490e-a65e-a53c8b06fcd9',
    tokenId: '8',
    account: '0x2d0d7b39db4e3a08', 
)
```
```REST API call
curl --location --request GET 'https://api-eu1.tatum.io/v3/nft/metadata/FLOW/27320939-3087-490e-a65e-a53c8b06fcd9/8?account=0x2d0d7b39db4e3a08' \
--header 'x-api-key: YOUR_API_KEY'
```
</div>
The response will contain the URL of the NFT's metadata.

```Response
{
  "data": "https://my_token_data.com"
}
```

Pretty simple, right? You only had to make three calls to Tatum to deploy your own token, issue a unique one, and transfer it. And just two more for getting all its useful information. No headache with learning Cadence or waiting for your smart contract to be approved, and you can get minting on Flow now with a paid Tatum API key. 

Flow is not available with a free plan, but there's a good reason for that: **we pay for all of your gas fees**. So you've got to pay for a plan, but then you won't be paying anything else on top of that, nor will you have to buy Flow and hold onto it to pay for your transactions!

To find out more about the API calls we have just used, visit our API Reference.

