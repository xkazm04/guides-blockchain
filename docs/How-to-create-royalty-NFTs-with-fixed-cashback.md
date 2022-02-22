# How to create royalty NFTs with fixed cashback

*Create specialized NFTs that pay out a set amount to the creator for each subsequent transaction. Forever.*


---
 

<!-- theme: info -->
>The [ERC-721 token](http://erc721.org/) is a standardized smart contract with a predefined set of features. The tokens are non-fungible, which means that each one is unique. You can think of them as one-of-a-kind collectibles.
![ntf__-10.png](https://stoplight.io/api/v1/projects/cHJqOjExNjE4OQ/images/fRN3fCiot40)

---

## How can creators keep getting paid?

NFTs have surged in popularity, and many NFT marketplaces have implemented functionality to allow creators to receive a portion of every subsequent sale of their work. This is great for creators, as it allows them to continue getting paid each time their NFT changes hands.

However, there's one critical flaw. Once the NFT leaves the marketplace it was originally sold on, the creator has no way of collecting any money ever again. This is because the mechanism for collecting portions of subsequent sales is only implemented at application level, not at blockchain level.

For this reason, we have created instantly deployable NFT smart contracts that implement this functionality at blockchain level. Payouts for subsequent sales to the original creators happen automatically, and forever. Creators will keep getting paid for as long as their NFT exists.

Tatum royalty NFTs can currently be created on:
- **Ethereum**
- **Celo**
- **Binance Smart Chain**
- **Polygon**
- **Harmony.ONE**

<!-- theme: info -->
>**Tatum royalty NFTs** are ERC-721 tokens with specialized smart contracts that pay creators cashback with each subsequent transaction. They can either pay a fixed amount of cashback or cashback percentages. In this guide, we are dealing with fixed cashback.
>
>For information on creating NFTs with percentage cashback and provenance data, please see this guide.

<!-- theme: warning -->
>**Tatum royalty NFTs** are primarily intended for higher-value NFTs. The royalty functionality is currently incompatible with OpenSea and must be transferred using smart contract methods not currently supported within the platform. As soon as OpenSea allows this standard and method calls, our royalty NFTs will be fully compatible with the platform.

---

## Import required libraries

In this guide, we will be using the Tatum JavaScript SDK to create and work with NFTs. First, we'll need to download and import the required libraries for the commands we'll be using. Download the [Tatum JS SDK here](https://www.npmjs.com/package/@tatumio/tatum/v/1.16.17).
Now, import the required libraries:

```ImportSDK
import { deployNFT, getNFTContractAddress, mintNFTWithUri, transferNFT, getNFTsByAddress, getNFTMetadataURI, getNFTRoyalty } from '@tatumio/tatum'
```

---

## Creating an NFT smart contract

​To [create and deploy royalty NFT]()s using Tatum, all you need is one simple API call. This API call deploys a standard, validated NFT smart contract to the blockchain of your choice. 

In this example, we are using [Celo](https://celo.org/) because it is fast, cheap and you can pay for transactions with a stable coin (cUSD). The required parameters are the name and symbol of the deployed token, the initial supply of the tokens, and the recipient address where the initial supply will be transferred.

<!-- theme: warning -->
> #### Security
>
> In this guide, we are signing transactions using a private key via API. This is fine for testing and demo purposes, but for production use, your private keys and mnemonics should never leave your security perimeter. To correctly and securely sign a transaction, you can use the [Tatum CLI](https://github.com/tatumio/tatum-cli); a specific language library like [Tatum JS](https://github.com/tatumio/tatum-js); local [middleware API](https://github.com/tatumio/tatum-middleware); or our complex key management system, [Tatum KMS](https://github.com/tatumio/tatum-kms).

```SDK
const transactionHash = await deployNFT(false, {
    name: 'CollectibleToken',
    chain: Currency.CELO,
    symbol: 'COLLECTIBLE',
    fromPrivateKey:'0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb',
    feeCurrency: Currency.CUSD
});
```
```REST API call with Private key
curl --location --request POST 'https://api-eu1.tatum.io/v3/nft/deploy' \
--header 'x-api-key: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
  "symbol": "COLLECTIBLE",
  "name": "CollectibleToken",
  "chain": "CELO",
  "fromPrivateKey": "0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb",
  "feeCurrency": "CUSD"
}'
```
```REST API call with KMS
curl --location --request POST 'https://api-eu1.tatum.io/v3/nft/deploy' \
--header 'x-api-key: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
  "symbol": "COLLECTIBLE",
  "name": "CollectibleToken",
  "chain": "CELO",
  "fromPrivateKey": "0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb",
  "feeCurrency": "CUSD"
}'
```
The required parameters are:
- **name** - the name of the NFT
- **chain** - the name of the blockchain we are working on
- **symbol** - the symbol of the NFT symbol
- **fromPrivateKey** - the private key of the address that will be paying for the gas fees for the transaction
- **feeCurrency** - the currency in which the fees will be paid (only for Celo)
The response is the transaction ID from which the created token's address can be obtained.

```Response
{
  "txId": "0xe9d62746dba8d39035142420eeb25dcaed717e615e06ed797118dc55aa5863f9"
}
```

---

## Getting the address of the smart contract

To get the address of the smart contract, use the transaction ID in the response to the previous call with the Get NFT contract address endpoint:

```SDK
const contractAddress = await getNFTContractAddress(Currency.CELO, '0xe9d62746dba8d39035142420eeb25dcaed717e615e06ed797118dc55aa5863f9');
```
```REST API call
curl --request GET \
  --url https://api-eu1.tatum.io/v3/blockchain/sc/address/CELO/0xe9d62746dba8d39035142420eeb25dcaed717e615e06ed797118dc55aa5863f9 \
  --header 'x-api-key: REPLACE_KEY_VALUE'
```

The response will contain the address of your NFT smart contract in the contractAddress field:

```Response
{
  "contractAddress": "0xd2A6B389eBE69208636B20b2abAcFC1eE0C3fF57"
}
```

---

## Uploading metadata

Before we mint NFTs, we have to upload the metadata (image, audio, video, etc.) that we will include in the NFT. Please check this guide to learn how to upload metadata to IPFS for free using Tatum.

---

## Minting a new unique ERC-721 token

When your NFT smart contract is deployed and you know the contract address, you can issue new unique tokens with the Mint NFT endpoint. This endpoint will create a new token with a unique identifier. Every token should have [metadata as a JSON schema](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-721.md#specification) containing additional information about the token, like the image URL, description, etc. If you want to enable the royalty feature, you need to enter two new properties - **authorAddresses** and **cashbackValues**. You can define a list of recipients of the cashback for every subsequent transfer of the token. These values are absolute and are in the currency of the underlying blockchain.

In addition, if the creators of the NFT would like to receive cashback in a custom ERC-20 token, the "erc20" field must be present with the address of the smart contract of the ERC-20 token.

<!-- theme: info -->
>You want to set up a cashback token for 1 author 0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea with value 0.1. This address will receive the cashback of 0.1 CELO for every token transfer.

```SDK
const transactionHash = await mintNFTWithUri(false, {
    to: '0x0ff64c166a462b31ed657c9d88c5ac4fef6b88b6',
    url: 'https://my-token-metadata-url',
    tokenId: '12',
    chain: Currency.CELO,
    contractAddress: '0xd2A6B389eBE69208636B20b2abAcFC1eE0C3fF57',
    authorAddresses: ["0x687422eEA2cB73B5d3e242bA5456b782919AFc85"],
    cashbackValues: ["0.5"],
    fromPrivateKey:'0x05e150c73f1920ec14caa1e0b6aa09940899678051a78542840c2668ce5080c2',
    feeCurrency: Currency.CUSD
});
```
```REST API call with Private key
curl --location --request POST 'https://api-eu1.tatum.io/v3/nft/mint' \
--header 'x-api-key: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
  "tokenId": "1",
  "to": "0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea",
  "chain": "CELO",
  "contractAddress": "0xd2A6B389eBE69208636B20b2abAcFC1eE0C3fF57",
  "cashbackValues": ["1"],
  "authorAddresses": ["0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea"],
  "url": "https://my_token_data.com",
  "fromPrivateKey": "0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb",
  "feeCurrency": "CUSD",
  "erc20": "0x1410d4ec3d276c0ebbf16ccbe88a4383ae734ed0"
}'
```
```REST API call with KMS
curl --location --request POST 'https://api-eu1.tatum.io/v3/nft/mint' \
--header 'x-api-key: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
  "tokenId": "1",
  "to": "0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea",
  "chain": "CELO",
  "contractAddress": "0xd2A6B389eBE69208636B20b2abAcFC1eE0C3fF57",
  "cashbackValues": ["1"],
  "authorAddresses": ["0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea"],
  "url": "https://my_token_data.com",
  "index": 0,
  "signatureId": "26d3883e-4e17-48b3-a0ee-09a3e484ac83",
  "nonce": 0,
  "feeCurrency": "CUSD",
  "erc20": "0x1410d4ec3d276c0ebbf16ccbe88a4383ae734ed0"
}'
```

The required fields for the API endpoint body are:
- **to** - he recipient's address to which the NFT will be minted
- **url** - the URL of of our JSON scheme on IPFS
- **tokenId** - The ID of the token to be minted. This can be any unique number and depends on your numbering convention. Some developers use chronological numbering, some do it by date and time, it's up to you to decide how you will organize your token IDs.
- **authorAddresses** - the address or addresses to which cashback will be sent.
- **cashbackValues** - the fixed values of cashback in the native currency of the blockchain or ERC-20 token entered in the smart contract address below.
- **chain** - The name of the blockchain
- **contractAddress** - The address of the NFT smart contract
- **erc20** -  address of the custom ERC-20 token smart contract the creators would like to receive cashback in. If this field is absent, cashback will be paid out in the native currency of the blockchain (in this case, ETH).
- **fromPrivateKey** - the private key of the address that will be paying for the gas fees for the transaction
- **feeCurrency** - the currency in which the fees will be paid (only for Celo)
The response will be a transaction ID:

```Response
{
  "txId": "0xc13b89f8f270e7848d2a8897582a01f1db60bb30918044d5fa3a0744b455a5a6"
}
```

---

## Transferring a specific ERC-721 token

To transfer tokens from the address where they were issued to another blockchain address, you can use the Transfer NFT token endpoint. You need the private key of the address where the tokens are located (address from the first call where the initial supply is distributed). You need to provide the value for the transfer transaction - it's the sum of all the royalties that should be paid to the authors.


```SDK
const transactionHash = await transferNFT(false, {
    to: '0xfb99F8aE9b70A0C8Cd96aE665BBaf85A7E01a2ef',
    tokenId: '12',
    chain: Currency.CELO,
    tokenPrice: '1',
    contractAddress: '0xd2A6B389eBE69208636B20b2abAcFC1eE0C3fF57',
    fromPrivateKey:'0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb',
    feeCurrency: Currency.CUSD
});
```
```REST API call with Private key
curl --location --request POST 'https://api-eu1.tatum.io/v3/nft/transaction' \
--header 'x-api-key: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
  "tokenId": "1",
  "chain": "CELO",
  "tokenPrice": "1",
  "to": "0xfb99F8aE9b70A0C8Cd96aE665BBaf85A7E01a2ef",
  "contractAddress": "0xd2A6B389eBE69208636B20b2abAcFC1eE0C3fF57",
  "fromPrivateKey": "0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb",
  "feeCurrency": "CUSD"
}'
```
```REST API call with KMS
curl --location --request POST 'https://api-eu1.tatum.io/v3/nft/transaction' \
--header 'x-api-key: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
  "tokenId": "1",
  "chain": "CELO",
  "tokenPrice": "1",
  "to": "0xfb99F8aE9b70A0C8Cd96aE665BBaf85A7E01a2ef",
  "contractAddress": "0xd2A6B389eBE69208636B20b2abAcFC1eE0C3fF57",
  "index": 0,
  "signatureId": "26d3883e-4e17-48b3-a0ee-09a3e484ac83",
  "nonce": 1,
  "feeCurrency": "CUSD"
}'
```

The required parameters to transfer the NFT are:
- **to** - the recipient address to which the NFT will be sent
- **tokenId** - the ID of the token to be transferred
- **chain** - the name of the blockchain
- **contractAddress** - The address of the NFT smart contract
- **tokenPrice** - The sale price of the token being transferred
- **fromPrivateKey** - the private key of the address holding the NFT that will be paying for the gas fees for the transaction
- **feeCurrency** - the currency in which the fees will be paid (only for Celo)
Again, the result is a blockchain transaction with an ID:

```Response
{
  "txId": "0x06d2f647c52f7a71b7f157dda4b3e9bd4beb7dff1f1910fa32ed5dc479fd5183"
}
```

If we use the get NFT transaction endpoint, you can see that the token was transferred to another recipient and the royalty cashback was paid to the author.

```REST API request
curl --location --request GET 'https://api-eu1.tatum.io/v3/nft/transaction/CELO/0xe9d62746dba8d39035142420eeb25dcaed717e615e06ed797118dc55aa5863f9' \
--header 'x-api-key: YOUR_API_KEY' 
```
```REST API response
{
    "blockHash": "0xb67611ef02119da1082883acd6453ebc64655ed31c3b345d184050a808c3bebc",
    "blockNumber": 9814820,
    "contractAddress": "0x1860Cf5A199892BC527A0698e7be08a7C6Bc6064",
    "gas": 4500000,
    "gasPrice": "119000000000",
    "gasUsed": 4446222,
    ...
    ...
}
```

---

## Getting a list of tokens that belong to the address

To find out which NFTs a specific address holds, use the following call.

```SDK
const nfts = await getNFTsByAddress(
    Currency.CELO,
    '0x7060694f5ce1feb5a255d06fdcf6e4f7a3507492',
    '0xd2A6B389eBE69208636B20b2abAcFC1eE0C3fF57',
);
```
```REST API call 
curl --location --request GET 'https://api-eu1.tatum.io/v3/nft/balance/CELO/0xd2A6B389eBE69208636B20b2abAcFC1eE0C3fF57/0xfb99F8aE9b70A0C8Cd96aE665BBaf85A7E01a2ef' \
--header 'x-api-key: YOUR_API_KEY'
```

The required parameters are:
- The **name of the blockchain**
- The **address** for which we would like to obtain a list of NFTs
- The **address of the smart contract** that minted the NFTs
The response will return a list of token IDs held by the address:

```Response
[
  "10"
]
```

---

## Obtaining metadata for a specific token

To get NFT metadata for a specific token, you need:
- The **chain** on which you are working
- The **smart contract address** from which the NFT was minted
- The **token ID** of the NFT

```SDK
const metadataURI = await getNFTMetadataURI(
    Currency.CELO,
    '0xd2A6B389eBE69208636B20b2abAcFC1eE0C3fF57',
    '15',
);
```
```REST API call
curl --request GET \
  --url 'https://api-eu1.tatum.io/v3/nft/metadata/{chain}/{contractAddress}/{token}?account=0xc1b45bc27b9c61c3' \
  --header 'x-api-key: REPLACE_KEY_VALUE' \
  --header 'x-testnet-type: SOME_STRING_VALUE'
```

The response will contain the location of the NFT's metadata:

```Response
{
  "data": "https://my_token_data.com"
}
```

---

## Obtaining royalty details for a specific token

If you want to obtain royalty details for a token, you can use the Get NFT Royalty data endpoint. The required parameters are the same as for getting the token's metadata.

```SDK
const nftRoyalty = await getNFTRoyalty(
    Currency.CELO,
    '0xd2A6B389eBE69208636B20b2abAcFC1eE0C3fF57',
    '15',
);
```
```REST API call
curl --request GET \
  --url https://api-eu1.tatum.io/v3/nft/royalty/{chain}/{contractAddress}/{token} \
  --header 'x-api-key: REPLACE_KEY_VALUE' \
  --header 'x-testnet-type: SOME_STRING_VALUE'
```

The response will contain the addresses of the authors to be paid, and the cashback values for each author:

```Response
{
    "addresses": [
        "0x8cb76aEd9C5e336ef961265c6079C14e9cD3D2eA"
    ],
    "values": [
        "1"
    ]
}
```
Tatum royalty NFTs are the only tokens in existence that payout to creators forever. They're easy to deploy, easy to work with, and provide enormous benefits for users. We look forward to the marketplaces you will create with them!

To find out more about the API calls we have just used, visit our API Reference.

To learn how to create and work with provenance NFTs with percentage royalties, please check out this guide.

















































