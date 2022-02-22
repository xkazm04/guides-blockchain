# How to create royalty NFTs with percentage cashback and provenance data

*Create NFTs that payout cashback as a percentage of every sale and record provenance data with every transaction.*

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

---

## What is provenance data?

While NFTs exist on the blockchain and their authenticity can be verified, there have still been attempts at forging high-value NFTs. To ensure the authenticity of NFTs, we have created NFT smart contracts that record metadata about each transaction every time the token is transferred. The token's entire transaction history is included in the token itself and its authenticity can be verified quickly and easily.

<!-- theme: info -->
>**Tatum royalty NFTs** are ERC-721 tokens with specialized smart contracts that pay creators cashback with each subsequent transaction. They can either pay a fixed amount of cashback or cashback percentages. 
>
>In this guide, we will learn how to use **NFTs that payout cashback as a percentage of each sale and record provenance data with each transaction.**
>
>For information on creating NFTs that payout cashback as a fixed value without provenance data, please see this guide.

<!-- theme: warning -->
>**Tatum royalty NFTs** are primarily intended for higher-value NFTs. The royalty functionality is currently incompatible with OpenSea and must be transferred using smart contract methods not currently supported within the platform.
>
>As soon as OpenSea allows this standard and method calls, our royalty NFTs will be fully compatible with the platform.

## Import required libraries

In this guide, we will be using the Tatum JavaScript SDK to create and work with NFTs. First, we'll need to download and import the required libraries for the commands we'll be using. Download the [Tatum JS SDK here](https://www.npmjs.com/package/@tatumio/tatum/v/1.16.17).

Now, import the required libraries:

```ImportSDK
import { deployNFT, getNFTContractAddress, mintNFTWithUri, transferNFT, getNFTsByAddress, getNFTMetadataURI, updateCashbackForAuthorNFT, getNFTProvenanceData } from '@tatumio/tatum';
```

---

## Creating an NFT smart contract

Normally, to create an NFT you'd need to learn Solidity and code your own smart contract. To speed things up, we've created prebuilt and validated smart contracts for you. You can instantly deploy them with just a few lines of code.

<!-- theme: warning -->
>In this guide, we are signing transactions using a private key via API. This is fine for testing and demo purposes, but for production use, your private keys and mnemonics should never leave your security perimeter. To correctly and securely sign a transaction, you can use [Tatum CLI](https://github.com/tatumio/tatum-cli) from the command line, a specific language library like [Tatum JS](https://github.com/tatumio/tatum-js), the local [middleware API](https://github.com/tatumio/tatum-middleware), or our comprehensive key management system, [Tatum KMS](https://github.com/tatumio/tatum-kms).

Use the following endpoint to deploy an NFT on Ethereum. 

```SDK
const transactionHash = await deployNFT(false, {
    name: 'MY_NFT',
    chain: Currency.ETH,
    symbol: 'NFT_SYMBOL',
    provenance: true
    fromPrivateKey:'0x05e150c73f1920ec14caa1e0b6aa09940899678051a78542840c2668ce5080c2'
});
```
```REST API call with Private key
curl --request POST \
  --url https://api-eu1.tatum.io/v3/nft/deploy \
  --header 'content-type: application/json' \
  --header 'x-api-key: REPLACE_KEY_VALUE' \
  --header 'x-testnet-type: SOME_STRING_VALUE' \
  --data {
  "chain": "ETH",
  "name": "My ERC721",
  "symbol": "ERC_SYMBOL",
  "fromPrivateKey": "0x05e150c73f1920ec14caa1e0b6aa09940899678051a78542840c2668ce5080c2",
  "provenance": true,
  "nonce": 0,
  "fee": {
    "gasLimit": "40000",
    "gasPrice": "20"
  }
}
```
```REST API call with KMS
curl --request POST \
  --url https://api-eu1.tatum.io/v3/nft/deploy \
  --header 'content-type: application/json' \
  --header 'x-api-key: REPLACE_KEY_VALUE' \
  --header 'x-testnet-type: SOME_STRING_VALUE' \
  --data {
  "chain": "ETH",
  "name": "My ERC721",
  "provenance": true,
  "symbol": "ERC_SYMBOL",
  "index": 0,
  "signatureId": "26d3883e-4e17-48b3-a0ee-09a3e484ac83",
  "nonce": 0,
  "fee": {
    "gasLimit": "40000",
    "gasPrice": "20"
  }
}
```

The required parameters are:
- **name** - the name of the NFT
- **chain** - the name of the blockchain we are working on
- **symbol** - the symbol of the NFT symbol
- **provenance: true** - Enabling provenance in this call also enables percentage royalty cashback.
- **fromPrivateKey** - the private key of the address that will be paying for the gas fees for the transaction
The response will contain a transaction ID, from which we can get the contract address.

```Response
{
  "txId": "0x7060694f5ce1feb5a255d06fdcf6e4f7a3507492",
  "failed": false
}
```

---

## Getting the address of the smart contract

To get the address of the smart contract, use the transaction ID in the response to the previous call with the Get NFT contract address endpoint:

```SDK
const contractAddress = await getNFTContractAddress(Currency.ETH, '0x7060694f5ce1f
```
```REST API call
curl --request GET \
  --url https://api-eu1.tatum.io/v3/blockchain/sc/address/ETH/0x7060694f5ce1feb5a255d06fdcf6e4f7a3507492 \
  --header 'x-api-key: REPLACE_KEY_VALUE'
```

The response will be the address of your NFT smart contract.

```Response
{
  "contractAddress": "0xd2A6B389eBE69208636B20b2abAcFC1eE0C3fF57"
}
```

---

## Uploading metadata

Before we mint NFTS, we have to upload the metadata (image, audio, video, etc.) that we will include in the NFT. Please check this guide to learn how to upload metadata to IPFS for free using Tatum.

---

## Minting a new unique ERC-721 token

Now that we have the address of our smart contract and have uploaded our metadata, we can mint NFTs.
Use the following endpoint to mint a new NFT:

```SDK
const transactionHash = await mintNFTWithUri(false, {
    to: '0x0ff64c166a462b31ed657c9d88c5ac4fef6b88b6',
    url: 'https://my-token-metadata-url',
    tokenId: '12',
    provenance: true,
    cashbackValues: ["100"],
    authorAddresses: ["0x0ff64c166a462b31ed657c9d88c5ac4fef6b88b6"],
    fixedValues: ["0.5"],
    chain: Currency.ETH,
    contractAddress: '0x7060694f5ce1feb5a255d06fdcf6e4f7a3507492',
    erc20: '0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48',
    fromPrivateKey:'0x05e150c73f1920ec14caa1e0b6aa09940899678051a78542840c2668ce5080c2'
});
```
```REST API call with private key
curl --request POST \
  --url https://api-eu1.tatum.io/v3/nft/mint \
  --header 'content-type: application/json' \
  --header 'x-api-key: REPLACE_KEY_VALUE' \
  --header 'x-testnet-type: SOME_STRING_VALUE' \
  --data '{
  "chain": "ETH",
  "tokenId": "100000",
  "to": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
  "contractAddress": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
  "url": "https://my_token_data.com",
  "provenance": true,
  "authorAddresses": [
    "0x687422eEA2cB73B5d3e242bA5456b782919AFc85"
  ],
  "cashbackValues": [
    "100"
  ],
  "fixedValues": [
    "0.5"
  ],
  "fromPrivateKey": "0x05e150c73f1920ec14caa1e0b6aa09940899678051a78542840c2668ce5080c2",
  "nonce": 0,
  "fee": {
    "gasLimit": "40000",
    "gasPrice": "20"
  }
}
```
```REST API call with KMS
curl --request POST \
  --url https://api-eu1.tatum.io/v3/nft/mint \
  --header 'content-type: application/json' \
  --header 'x-api-key: REPLACE_KEY_VALUE' \
  --header 'x-testnet-type: SOME_STRING_VALUE' \
  --data '{
  "chain": "ETH",
  "tokenId": "100000",
  "to": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
  "contractAddress": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
  "url": "https://my_token_data.com",
  "provenance": true,
  "authorAddresses": [
    "0x687422eEA2cB73B5d3e242bA5456b782919AFc85"
  ],
  "cashbackValues": [
    "100"
  ],
  "fixedValues": [
    "0.5"
  ],
  "index": 0,
  "signatureId": "26d3883e-4e17-48b3-a0ee-09a3e484ac83",
  "nonce": 0,
  "fee": {
    "gasLimit": "40000",
    "gasPrice": "20"
  }
}
```

The required fields for the API endpoint body are:
- **to** - he recipient's address to which the NFT will be minted
- **url** - the URL of of our JSON scheme on IPFS
- **tokenId** - The ID of the token to be minted. This can be any unique number and depends on your numbering convention. Some developers use chronological numbering, some do it by date and time, it's up to you to decide how you will organize your token IDs.
- **provenance: true** - Provenance must be enabled in this call as well.
- **authorAddresses** - the address or addresses to which cashback will be sent.
- **cashbackValues** - the percentage value(s) of the cashback to be sent. In the example above, the value "100" means that 1% of the sale price will be transferred to the author each time the NFT is transferred.
- **fixedValues** - the minimum fixed value(s) of the cashback. This value is the minimum cashback value that will be paid and is a fixed amount of the blockchain's native currency. If the cashback to be paid from the percentage of the sale is less than the fixed value, the fixed value will be paid to the authors instead.
- **chain** - The name of the blockchain
- **contractAddress** - The address of the NFT smart contract
- **erc20** -  address of the custom ERC-20 token smart contract the creators would like to receive cashback in. If this field is absent, cashback will be paid out in the native currency of the blockchain (in this case, ETH).
- **fromPrivateKey** - the private key of the address that will be paying for the gas fees for the transaction
The response will contain a transaction ID:

```Response
{
  "txId": "c83f8818db43d9ba4accfe454aa44fc33123d47a4f89d47b314d6748eb0e9bc9",
  "failed": false
}
```

---

## Transferring an NFT

To transfer an NFT, we must enter the price of the sale from which the percentage royalties will be calculated.

```SDK
const transactionHash = await transferNFT(false, {
    to: '0x0ff64c166a462b31ed657c9d88c5ac4fef6b88b6',
    tokenId: '12',
    chain: Currency.ETH,
    contractAddress: '0x7060694f5ce1feb5a255d06fdcf6e4f7a3507492',
    provenance: true,
    provenanceData: "test",
    tokenPrice: "1",
    fromPrivateKey:'0x05e150c73f1920ec14caa1e0b6aa09940899678051a78542840c2668ce5080c2'
});
```
```REST API call with Private key
curl --request POST \
  --url https://api-eu1.tatum.io/v3/nft/transaction \
  --header 'content-type: application/json' \
  --header 'x-api-key: REPLACE_KEY_VALUE' \
  --header 'x-testnet-type: SOME_STRING_VALUE' \
  --data '{
  "value": "1",
  "chain": "ETH",
  "to": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
  "tokenId": "100000",
  "contractAddress": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
  "provenance": true,
  "provenanceData": "test",
  "tokenPrice": "1",
  "fromPrivateKey": "0x05e150c73f1920ec14caa1e0b6aa09940899678051a78542840c2668ce5080c2",
  "nonce": 1,
  "fee": {
    "gasLimit": "40000",
    "gasPrice": "20"
  }
}
```
```REST API call with KMS
curl --request POST \
  --url https://api-eu1.tatum.io/v3/nft/transaction \
  --header 'content-type: application/json' \
  --header 'x-api-key: REPLACE_KEY_VALUE' \
  --header 'x-testnet-type: SOME_STRING_VALUE' \
  --data '{
  "value": "1",
  "chain": "ETH",
  "to": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
  "tokenId": "100000",
  "contractAddress": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
  "index": 0,
  "provenance": true,
  "provenanceData": "test",
  "tokenPrice": "1",
  "signatureId": "26d3883e-4e17-48b3-a0ee-09a3e484ac83",
  "nonce": 1,
  "fee": {
    "gasLimit": "40000",
    "gasPrice": "20"
  }
}
```

The required parameters to transfer the NFT are:
- **to** - the recipient address to which the NFT will be sent
- **tokenId** - the ID of the token to be transferred
- **chain** - the name of the blockchain
- **contractAddress** - The address of the NFT smart contract
- **provenance: true** - Provenance must be enabled to transfer the token.
- **provenanceData** - Any optional provenance data to be included in the transaction
- **tokenPrice** - The sale price of the token being transferred
- **fromPrivateKey** - the private key of the address holding the NFT that will be paying for the gas fees for the transaction
The response will contain a transaction ID.

```Response
{
  "txId": "c83f8818db43d9ba4accfe454aa44fc33123d47a4f89d47b314d6748eb0e9bc9",
  "failed": false
}
```

---

## Updating NFT cashback for authors

Now, we must designate the addresses to which the cashback percentages from subsequent transactions will be sent. You can update the cashback percentage for any authors of the NFT at any time. However, the minimum fixed values (in the "fixedValues" field of the mint call) cannot be changed.

To update the cashback values of an NFT, use the following endpoint:

```SDK
const transactionHash = await updateCashbackForAuthorNFT(false, {
    to: '0x0ff64c166a462b31ed657c9d88c5ac4fef6b88b6',
    tokenId: '12',
    chain: Currency.ETH,
    contractAddress: '0x7060694f5ce1feb5a255d06fdcf6e4f7a3507492',
    cashbackValue: '15',
    fromPrivateKey: '0x315ea1dae65c75f088542161a0777525a8de1153904c745cb8131a9e0c632204'
});
```
```REST API call with Private key
curl --request PUT \
  --url https://api-eu1.tatum.io/v3/nft/royalty \
  --header 'content-type: application/json' \
  --header 'x-api-key: REPLACE_KEY_VALUE' \
  --header 'x-testnet-type: SOME_STRING_VALUE' \
  --data '{
  "chain": "ETH",
  "tokenId": "100000",
  "cashbackValue": "0.1",
  "contractAddress": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
  "fromPrivateKey": "0x05e150c73f1920ec14caa1e0b6aa09940899678051a78542840c2668ce5080c2",
  "nonce": 0,
  "fee": {
    "gasLimit": "40000",
    "gasPrice": "20"
  }
}
```
```REST API call with KMS
curl --request PUT \
  --url https://api-eu1.tatum.io/v3/nft/royalty \
  --header 'content-type: application/json' \
  --header 'x-api-key: REPLACE_KEY_VALUE' \
  --header 'x-testnet-type: SOME_STRING_VALUE' \
  --data '{
  "chain": "ETH",
  "tokenId": "100000",
  "cashbackValue": "0.1",
  "contractAddress": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
  "index": 0,
  "signatureId": "26d3883e-4e17-48b3-a0ee-09a3e484ac83",
  "nonce": 0,
  "fee": {
    "gasLimit": "40000",
    "gasPrice": "20"
  }
}
```

To update the cashback percentage, the following parameters are required:
- **to** - the recipient's address
- **tokenId** - the ID of the token
- **chain** - the name of the blockchain
- **contractAddress** - The address of the NFT smart contract
- **cashbackValue** - The new cashback value as a percentage (i.e. a value of "15" = 0.15%)
- **fromPrivateKey** - the private key of the address that will be paying for the gas fees for the transaction
The response will contain a transaction ID.

```Response
{
  "txId": "c83f8818db43d9ba4accfe454aa44fc33123d47a4f89d47b314d6748eb0e9bc9",
  "failed": false
}
```

---

## Getting a list of NFTs by address

To find out which NFTs a specific address holds, use the following call.

```SDK
const nfts = await getNFTsByAddress(
    Currency.ETH,
    '0x7060694f5ce1feb5a255d06fdcf6e4f7a3507492',
    '0x0ff64c166a462b31ed657c9d88c5ac4fef6b88b6',
);
```
```REST API call
curl --request GET \
  --url https://api-eu1.tatum.io/v3/nft/balance/{chain}/{contractAddress}/{address} \
  --header 'x-api-key: REPLACE_KEY_VALUE' \
  --header 'x-testnet-type: SOME_STRING_VALUE'
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

## Getting NFT metadata

To get NFT metadata for a specific token, you need:
- The **chain** on which you are working
- The **smart contract address** from which the NFT was minted
- The **token ID** of the NFT

```SDK
const metadataURI = await getNFTMetadataURI(
    Currency.ETH,
    '0x7060694f5ce1feb5a255d06fdcf6e4f7a3507492',
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

## Get NFT provenance data

Since we've enabled provenance data in the NFTs we've minted, now we can use the get NFT provenance data endpoint to view a record of all of the transactions of the NFT since it was minted. The required parameters are the same as with the get metadata endpoint.

```SDK
const obj = await getNFTProvenanceData(
    Currency.ETH,
    '0x7060694f5ce1feb5a255d06fdcf6e4f7a3507492',
    '15',
);
```
```REST API call
curl --request GET \
  --url 'https://api-eu1.tatum.io/v3/nft/provenance/{chain}/{contractAddress}/{tokenId}?account=0xc1b45bc27b9c61c3' \
  --header 'x-api-key: REPLACE_KEY_VALUE' \
  --header 'x-testnet-type: SOME_STRING_VALUE'
```

The response will contain the provenance data of the NFT:
```Response
[
  {
    "provenanceData": "0x94Ce79B9F001E25BBEbE7C01998A78F7B27D1326",
    "tokenPrice": "123"
  }
]
```

---

## Well done!

Now you can create and work with NFTs that hold provenance data and payout royalties as percentages. Two in-demand features with just a few lines of code. For the full list of NFT operations available in Tatum, please refer to our API documentation. 



































































































































































































