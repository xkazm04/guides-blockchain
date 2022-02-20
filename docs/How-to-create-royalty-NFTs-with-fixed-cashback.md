# How to create royalty NFTs with fixed cashback

*Create specialized NFTs that pay out a set amount to the creator for each subsequent transaction. Forever.*


---

## NFT 

Tatum royalty NFTs are [ERC-721](http://erc721.org/) tokens with specialized smart contracts that pay creators cashback with each subsequent transaction. They can either pay a fixed amount of cashback or cashback percentages. In this guide, we are dealing with fixed cashback.
For information on creating NFTs with percentage cashback and provenance data, please see [this guide](../token/ZG9jOjI4OTE4MTc2-how-to-create-royalty-nf-ts-with-percentage-cashback-and-provenane-data). 

<!-- theme: info -->
>The ERC-721 token is a standardized smart contract with a predefined set of features. The tokens are non-fungible, which means that each one is unique. You can think of them as one-of-a-kind collectibles.

## How can creators keep getting paid?

NFTs have surged in popularity, and many NFT marketplaces have implemented functionality to allow creators to receive a portion of every subsequent sale of their work. This is great for creators, as it allows them to continue getting paid each time their NFT changes hands.

However, there's one critical flaw. Once the NFT leaves the marketplace it was originally sold on, the creator has no way of collecting any money ever again. This is because the mechanism for collecting portions of subsequent sales is only implemented at application level, not at blockchain level.

For this reason, we have created instantly deployable NFT smart contracts that implement this functionality at blockchain level. Payouts for subsequent sales to the original creators happen automatically, and forever. Creators will keep getting paid for as long as their NFT exists.


<!-- theme: warning -->
>**Tatum royalty NFT**s are primarily intended for higher-value NFTs. The royalty functionality is currently incompatible with OpenSea and must be transferred using smart contract methods not currently supported within the platform. As soon as OpenSea allows this standard and method calls, our royalty NFTs will be fully compatible with the platform.

Tatum royalty NFTs can currently be created on:
- Ethereum
- Celo
- Binance Smart Chain

---

## Creating an NFT smart contract

​To [create and deploy royalty NFT](../token/b3A6MzA3Mjc3MzI-deploy-nft-smart-contract)s using Tatum, all you need is one simple API call. This API call deploys a standard, validated NFT smart contract to the blockchain of your choice. 

In this example, we are using [Celo](https://celo.org/) because it is fast, cheap and you can pay for transactions with a stable coin (cUSD). The required parameters are the name and symbol of the deployed token, the initial supply of the tokens, and the recipient address where the initial supply will be transferred.

<!-- theme: warning -->
> #### Security
>
> Blockchain transactions are signed using a private key via API, which is not a secure way of signing transactions. Your private keys and mnemonics should never leave your security perimeter. To correctly and securely sign a transaction, you can use the [Tatum CLI](https://github.com/tatumio/tatum-cli); a specific language library like [Tatum JS](https://github.com/tatumio/tatum-js); local [middleware API](https://github.com/tatumio/tatum-middleware); or our complex key management system, [Tatum KMS](https://github.com/tatumio/tatum-kms).

**Request example**
```json
curl --request POST \
  --url https://api-eu1.tatum.io/v4/token/nft/ETH/deploy \
  --header 'Content-Type: application/json' \
  --header 'x-api-key: ' \
  --header 'x-testnet-type: ' \
  --data '{
  "name": "My ERC721",
  "symbol": "ERC_SYMBOL",
  "fromPrivateKey": "0x05e150c73f1920ec14caa1e0b6aa09940899678051a78542840c2668ce5080c2",
  "nonce": 0,
  "feeCurrency": "CELO"
}'
```
**Response example**
```json
{
  "txId": "c83f8818db43d9ba4accfe454aa44fc33123d47a4f89d47b314d6748eb0e9bc9",
  "failed": false
}
```
#### Getting the address of the smart contract

The response includes the `txId` property from which the address of the created token can be obtained; this is shown in the following call. The property `contractAddress` in the response represents the address of the NFT.

**Request example**
```json
curl --request GET \
  --url https://api-eu1.tatum.io/v4/token/nft/address/CELO/hash \
  --header 'Content-Type: application/json' \
  --header 'x-api-key: ' \
  --header 'x-testnet-type: '
```
**Response example**
```json
{
  "contractAddress": "0xc21C81ef03f98898Fb155E00C364e8a7b9D158b8"
}
```
---
## Minting a new unique ERC-721 token

When your NFT smart contract has been deployed and you know the contract address, you can issue new unique tokens with the [Mint NFT](../token/b3A6MzA3Mjc3Mzc-mint-nft) endpoint. This endpoint will create a new token with a unique identifier. Every token should include metadata in the JSON schema form containing additional information about the token, like the image URL, description, etc.

If you want to enable the royalty feature, you need to enter two new properties: `authorAddresses` and `cashbackValues`. You can define a list of recipients of the cashback for every subsequent transfer of the token. These values are absolute and are in the currency of the underlying blockchain.

<!-- theme: info -->
>In addition, if the NFT creators intend to receive cashback in a custom ERC-20 token, an `erc20` field must be present with the address of the smart contract of the ERC-20 token.

**Request example**
```json
curl --request POST \
  --url https://api-eu1.tatum.io/v4/token//nft/ETH/mint \
  --header 'Content-Type: application/json' \
  --header 'x-api-key: ' \
  --header 'x-testnet-type: ' \
  --data '{
  "tokenId": "100000",
  "to": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
  "contractAddress": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
  "url": "https://my_token_data.com",
  "authorAddresses": [
    "0x687422eEA2cB73B5d3e242bA5456b782919AFc85"
  ],
  "cashbackValues": [
    "0.5"
  ],
  "fromPrivateKey": "0x05e150c73f1920ec14caa1e0b6aa09940899678051a78542840c2668ce5080c2",
  "nonce": 0,
  "feeCurrency": "CELO"
}'
```
**Response example**
```json
{
  "txId": "c83f8818db43d9ba4accfe454aa44fc33123d47a4f89d47b314d6748eb0e9bc9",
  "failed": false
}
```
---
## Transferring a specific ERC-721 token

If you want to transfer tokens from the address they were issued at to another blockchain address, you can use the [Transfer NFT token](../token/b3A6MzA3Mjc3Mzg-transfer-nft-token) endpoint. You need the private key of the address where the tokens are located (this is the address from the first call where the initial supply is distributed). You need to provide a transfer transaction value - this is the sum of all the royalties that should be paid to the authors.

**Request example**
```json
curl --request POST \
  --url https://api-eu1.tatum.io/v4/token/nft/ETH/transaction \
  --header 'Content-Type: application/json' \
  --header 'x-api-key: ' \
  --header 'x-testnet-type: ' \
  --data '{
  "value": "1",
  "to": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
  "tokenId": "100000",
  "contractAddress": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
  "fromPrivateKey": "0x05e150c73f1920ec14caa1e0b6aa09940899678051a78542840c2668ce5080c2",
  "nonce": 1,
  "fee": {
    "gasLimit": "40000",
    "gasPrice": "20"
  }
}'
```
**Response example**
```json
{
  "txId": "c83f8818db43d9ba4accfe454aa44fc33123d47a4f89d47b314d6748eb0e9bc9",
  "failed": false
}
```
---
## Getting a list of tokens that belong to the address

If you want to display a list of tokens in someone's possession, you can use the [Get NFT account balance](../token/b3A6MzEwNjgyNzY-get-nft-account-balance) endpoint.

**Request example**
```json
curl --request GET \
  --url https://api-eu1.tatum.io/v4/token/nft/balance/ETH/contractAddress/address \
  --header 'Content-Type: application/json' \
  --header 'x-api-key: ' \
  --header 'x-testnet-type: '
```
**Response example**
```json
[
  "10"
]
```
---
## Obtaining metadata for a specific token

If you want to obtain a metadata URL for a token, you can use the [Get NFT Token metadata](../token/b3A6MzEwNjgyNzc-get-nft-token-metadata) endpoint.

**Request example**
```json
curl --request GET \
  --url https://api-eu1.tatum.io/v4/token/nft/metadata/ETH/contractAddress/token \
  --header 'Content-Type: application/json' \
  --header 'x-api-key: ' \
  --header 'x-testnet-type: '
```
**Response example**
```json
{
  "data": "https://my_token_data.com"
}
```
---
## Obtaining royalty details for a specific token

If you want to obtain royalty details for a token, you can use the [Get NFT Royalty Data](../token/b3A6MzEwNjgyNzg-get-nft-token-royalty-information) endpoint.

**Request example**
```json
curl --request GET \
  --url https://api-eu1.tatum.io/v4/token/nft/royalty/ETH/contractAddress/token \
  --header 'Content-Type: application/json' \
  --header 'x-api-key: ' \
  --header 'x-testnet-type: '
```
**Response example**
```json
{
  "addresses": [
    "0x94Ce79B9F001E25BBEbE7C01998A78F7B27D1326"
  ],
  "values": [
    "0.2"
  ]
}
```
Tatum royalty NFTs are the only tokens in existence that pay out to their creators forever. They are easy to deploy and work with and they provide enormous benefits for users. We look forward to the marketplaces you will create with them!

<!-- theme: info -->

>To **find out more** about the API calls we have just used, visit our API Reference.