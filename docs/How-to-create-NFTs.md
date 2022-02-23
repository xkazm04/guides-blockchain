# How to create NFTs

*Collectibles in the blockchain are just a few API calls away.*

---

<div class="toolbar-note">
The [ERC-721 token](http://erc721.org/) is a standardized smart contract with a predefined set of features. The tokens are non-fungible, which means that each one is unique. You can think of them as one-of-a-kind collectibles.
</div>


NFTs have been making waves in the digital world recently and for good reason. These specialized tokens ensure verifiable authenticity and scarcity of digital assets, opening up a world of possibilities for a wide range of digital marketplaces.

From digital collectibles to in-game assets to one-of-a-kind releases of digital media, the possibilities are endless. Everyone wants to start implementing NFTs into their platforms, but they're running into a few obstacles.

Creating a new NFT, or "smart contract" on an Ethereum-compatible blockchain, is not easy at all. As a developer, you have to know a lot of things:
- Solidity - a new programming language for writing smart contracts
- How to use a compiler for Solidity
- How to develop using an IDE
- How to deploy the compiled smart contract


![ntf__-11.png](https://stoplight.io/api/v1/projects/cHJqOjExNjE4OQ/images/81dJ6yOpTlg)

It's a lot to know, and for developers who only need the basic features of ERC-721 tokens, it would take weeks to figure it all out.

This is why we've made prebuilt ERC-721 smart contracts that are ready to deploy with one API. You can easily create NFTs without learning Solidity and can deploy them on your blockchain of choice.

<div class="toolbar-tip">

Check out the source code to our **ERC-721** smart contract on [GitHub](https://github.com/tatumio/tatum-middleware/blob/master/src/contracts/erc721/ERC721Full.sol).
</div>
![ntf_-12.png](https://stoplight.io/api/v1/projects/cHJqOjExNjE4OQ/images/NyL8ch1C7Hk)

In this guide, you will learn how to deploy an ERC-721 smart contract, and to mint, transfer, and burn tokens. Please also check out our workshop on working with NFTs for a live demo of the workflow in Postman:

https://www.youtube.com/watch?time_continue=3&v=i86Nh-Srfsk&feature=emb_title&ab_channel=Tatum

You can find all of the API calls we will be using in our [API documentation](https://developer.tatum.io/rest/smart-contracts/non-fungible-token-nft) or download our Postman collection for this guide [here](https://www.postman.com/blockchainnow/workspace/tatum-io/folder/10167180-22217e73-44f0-414d-b865-2098a72c2050?ctx=documentation).

Tatum currently supports the following blockchains for deploying our prebuilt NFTs:
- **Ethereum**
- **Polygon (Matic)**
- **Celo**
- **Harmony.ONE**
- **Tron**
- **Flow**
- **Binance Smart Chain**

To create and deploy NFTs using Tatum, all you need is one simple API call. This API call deploys a standard, validated NFT smart contract to the blockchain of your choice. 

In this example, we are using [Celo](https://celo.org/) because it is fast, cheap and you can pay for transactions with a stable coin (cUSD). However, you can deploy an NFT smart contract on any other supported blockchain with the same API call by changing the name of the blockchain in the "chain" field of the call.



**Securely signing transactions**
<div class="toolbar-warning">
In this guide, we are signing transactions with a private key via API. This is fine for testing and demo purposes, but should not be used for production purposes. 
</div>

Your private keys and mnemonics should never leave your security perimeter. To correctly and securely sign a transaction you can use [Tatum CLI](https://github.com/tatumio/tatum-cli) from the command line, a specific language library like [Tatum JS](https://github.com/tatumio/tatum-js), the local [middleware API](https://github.com/tatumio/tatum-middleware), or our comprehensive key management system, [Tatum KMS](https://github.com/tatumio/tatum-kms).


The following API call will [deploy an NFT smart contract](https://developer.tatum.io/rest/smart-contracts/deploy-nft-smart-contract) on Celo:

<div class='tabbed-code-blocks'>
```SDK
const transactionHash = await deployNFT(false, {
    body.chain: Currency.CELO,
    body.name: 'MY_ERC721',
    body.publicMint: true,
    body.symbol: 'ERC_SYMBOL',
    body.fromPrivateKey: '0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb',
    body.provenance: true,
    body.feeCurrency: Currency.CUSD
});
```
```REST API call with Private key
curl --location --request POST 'https://api-eu1.tatum.io/v3/nft/deploy' \
--header 'x-api-key: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
  "chain": "CELO",
  "name": "My ERC721",
  "publicMint": true,
  "symbol": "ERC_SYMBOL",
  "fromPrivateKey": "0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb",
  "provenance": true,
  "feeCurrency": "CUSD"
}'
```
```REST API call with KMS
curl --location --request POST 'https://api-eu1.tatum.io/v3/nft/deploy' \
--header 'x-api-key: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
  "chain": "CELO",
  "name": "My ERC721",
  "publicMint": true,
  "symbol": "ERC_SYMBOL",
  "index": 0,
  "signatureId": "26d3883e-4e17-48b3-a0ee-09a3e484ac83",
  "nonce": 0,
  "provenance": true,
  "feeCurrency": "CELO"
}'
```
</div>

The following parameters should be present in the API request body:
- **chain** - The blockchain on which you are deploying the NFT smart contract.
- **name** - The name of the NFTs minted from the contract.
- **publicMint** - If set to "true" any user will be able to mint NFTs from this smart contract. If set to "false" only the private key that created the contract or other private IDs added using the [add NFT minter](https://developer.tatum.io/rest/smart-contracts#/b3A6MzA3NjA4OTg-add-nft-minter) endpoint will be able to mint NFTs from the contract.
- **symbol** - The symbol of the NFTs to be minted from the contract.
- **fromPrivateKey** - The private key which will pay for the gas fees to deploy the contract and will automatically be able to mint NFTs from it.
- **signatureId** - The ID of the signed transaction from KMS. This is used in place of a private key to sign transactions securely and locally. 
- **feeCurrency** - The currency in which the gas fees for the operation will be paid (only for Celo).

**Response**:
```json
{
  "txId": "0xa61f21f5c33996f7e8a6d209513fb446ea345565f10e614e40a7978e3373b919"
}
```

---

## Getting the contract address

The response to the above call is a transaction ID . Using the transaction ID, we can use the [Get contract address from transaction](https://developer.tatum.io/rest/smart-contracts/get-contract-address-from-transaction) endpoint to obtain the address of the NFT smart contract.

<div class='tabbed-code-blocks'>
```SDK
const contractAddress = await getNFTContractAddress(Currency.CELO, '0xa61f21f5c33996f7e8a6d209513fb446ea345565f10e614e40a7978e3373b919'
```
```REST API call
curl --request GET \
  --url https://api-eu1.tatum.io/v3/blockchain/sc/address/CELO/0xa61f21f5c33996f7e8a6d209513fb446ea345565f10e614e40a7978e3373b919 \
  --header 'x-api-key: REPLACE_KEY_VALUE'
```
</div>

The required parameters to enter are:
- The **chain** you are working on.
- The **transaction ID** from the previous call.

**Response**:
```json
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

In the response, you can see a field called "contractAddress," which contains the address of the smart contract.

---

## Minting a new, unique ERC-721 token 

When your NFT smart contract is deployed and you know the contract address, you can issue new unique tokens with the [Mint NFT](https://developer.tatum.io/rest/smart-contracts/mint-nft) endpoint. This endpoint will create a new token with a unique identifier. 

Every token should have metadata as a JSON schema containing additional information about the token, like the image URL, description, etc. This metadata is the "content" of the NFT and usually links to a picture, video, audio file, or another type of digital asset on IPFS or another hosting service. To upload and retrieve data to IPFS natively using Tatum, please have a look at our [Storage/IPFS](https://developer.tatum.io/rest/storage/store-data-to-ipfs) endpoints, and refer to our full guide on how to upload data to IPFS.

<div class='tabbed-code-blocks'>
```SDK
const transactionHash = await mintNFTWithUri(false, {
    to: '0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea',
    url: 'https://my_token_data.com',
    tokenId: '1',
    provenance: true,
    cashbackValues: ["0.5"],
    authorAddresses: ["0x0ff64c166a462b31ed657c9d88c5ac4fef6b88b6"],
    fixedValues: ["0.5"],
    chain: Currency.CELO,
    contractAddress: '0x7060694f5ce1feb5a255d06fdcf6e4f7a3507492',
    fromPrivateKey: '0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb',
    erc20: '0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48',
    feeCurrency: Currency.CUSD
});
```
```REST API call with Private key
curl --location --request POST 'https://api-eu1.tatum.io/v3/nft/mint' \
--header 'x-api-key: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
   "chain": "CELO",
   "tokenId": "1",
   "to": "0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea",
   "erc20": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
   "contractAddress": "0x1860Cf5A199892BC527A0698e7be08a7C6Bc6064",
   "url": "https://my_token_data.com",
   "authorAddresses": [
      "0x0ff64c166a462b31ed657c9d88c5ac4fef6b88b6"
      ],
   "provenance": true,
   "cashbackValues": [
      "0.5"
      ],
   "fixedValues": [
      "0.5"
      ],
   "fromPrivateKey": "0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb",
   "nonce": 0,
   "feeCurrency": "CUSD"
}'
```
```REST API call with KMS
curl --location --request POST 'https://api-eu1.tatum.io/v3/nft/mint' \
--header 'x-api-key: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
   "chain": "CELO",
   "tokenId": "1",
   "to": "0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea",
   "erc20": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
   "contractAddress": "0x1860Cf5A199892BC527A0698e7be08a7C6Bc6064",
   "url": "https://my_token_data.com",
   "authorAddresses": [
      "0x0ff64c166a462b31ed657c9d88c5ac4fef6b88b6"
      ],
   "provenance": true,
   "cashbackValues": [
      "0.5"
      ],
   "fixedValues": [
      "0.5"
      ],
   "index": 0,
   "signatureId": "26d3883e-4e17-48b3-a0ee-09a3e484ac83",
   "nonce": 0,
   "feeCurrency": "CUSD"
}'
```
</div>
The API request body to [mint a new NFT](https://developer.tatum.io/rest/smart-contracts/mint-nft) should contain the following values. The authorAddresses, cashbackValues, and fixedValues fields are only required if you are minting provenance NFTs that pay royalties in percentages. For more on working with provenance NFTs please refer to this guide.

- **chain** - The blockchain on which you are minting the NFT.
- **tokenId** - The ID of the token to be minted. This can be any unique number and depends on your numbering convention. Some developers use chronological numbering, some do it by date and time, it's up to you to decide how to organize your token IDs.
- **to** - The recipient's address.
- **erc20** - The address of the custom ERC-20 token smart contract the creators would like to receive cashback in. If this field is absent, cashback will be paid out in the native currency of the blockchain (in this case, CELO).
- **contractAddress** - The address of the NFT smart contract from which the NFT will be minted.
- **url** - The URL of your JSON scheme on IPFS that points the metadata to be included in the minted NFT.
- **provenance** - If set to "true" the NFT minted will be record provenance data and be able to pay out percentage royalties.
- **authorAddresses** - The address or addresses of the creators to whom cashback will be sent.
- **cashbackValues** - The percentage value or values of the cashback that will be sent to the NFT's creators. In the example above, the value "0.5" means that 0.005% of the sale price will be transferred to the author each time the NFT is transferred.
- **fixedValues** - This value is the minimum cashback value that will be paid and is a fixed amount of the blockchain's native currency. If the cashback to be paid from the percentage of the sale is less than the fixed value, the fixed value will be paid to the authors instead.
- **fromPrivateKey** - The private key from which the gas fees for the mint operation will be paid.
- **signatureId** - The ID of the signed transaction from KMS. This is used in place of a private key to sign transactions securely and locally. For more info on KMS, please refer to this guide.
- **feeCurrency** - The currency in which the gas fees for the operation will be paid (only for Celo).

**Response**:
```json
{
  "txId": "0x35c8a88f194a62732b8ba3377a5f4e4801994976dbfbcad4c86b15a990a4b73f"
}
```

The response will contain a transaction ID which can be fetched using the get NFT transaction API endpoint to see details about the transaction.

---

## Transferring a specific ERC-721 token

To transfer tokens from the address where they were issued to another blockchain address, you can use the [Transfer NFT token](https://developer.tatum.io/rest/smart-contracts/transfer-nft-token) endpoint. You need the private key of the address where the tokens are located (the contract address from the first call where the initial supply is distributed).

<div class='tabbed-code-blocks'>
```SDK
const transactionHash = await transferNFT(false, {
    to: '0xfb99F8aE9b70A0C8Cd96aE665BBaf85A7E01a2ef',
    tokenId: '1',
    chain: Currency.CELO,
    contractAddress: '0x1860Cf5A199892BC527A0698e7be08a7C6Bc6064',
    fromPrivateKey: '0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb',
    provenance: true,
    provenanceData: "test",
    tokenPrice: "1",
    feeCurrency: Currency.CUSD
});
```
```REST API call with Private key
curl --location --request POST 'https://api-eu1.tatum.io/v3/nft/transaction' \
--header 'x-api-key: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
  "chain": "CELO",
  "tokenId": "1",
  "to": "0xfb99F8aE9b70A0C8Cd96aE665BBaf85A7E01a2ef",
  "contractAddress": "0x1860Cf5A199892BC527A0698e7be08a7C6Bc6064",
  "fromPrivateKey": "0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb",
  "provenance": true,
  "provenanceData": "test",
  "tokenPrice": "1",
  "feeCurrency": "CUSD"
}'
```
```REST API call with KMS
curl --location --request POST 'https://api-eu1.tatum.io/v3/nft/transaction' \
--header 'x-api-key: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
  "chain": "CELO",
  "tokenId": "1",
  "to": "0xfb99F8aE9b70A0C8Cd96aE665BBaf85A7E01a2ef",
  "contractAddress": "0x1860Cf5A199892BC527A0698e7be08a7C6Bc6064",
  "signatureId": "26d3883e-4e17-48b3-a0ee-09a3e484ac83",
  "provenance": true,
  "provenanceData": "test",
  "tokenPrice": "1",
  "feeCurrency": "CUSD"
}'
```
</div>
The API request body should contain the following parameters.  
`provenanceData` and `tokenPrice` parameters are only required when transferring a provenance NFT that will record provenance data and/or pay cashback.

- **chain** - The blockchain on which you are transferring the NFT.
- **tokenId** - The ID of the token to be transferred.
- **to** - The recipient's address.
- **contractAddress** - The address of the NFT smart contract of the NFT to be transferred.
- **fromPrivateKey** - The private key of the address holding the NFT and from which the gas fees for the transfer operation will be paid.
- **signatureId** - The ID of the signed transaction from KMS. This is used in place of a private key to sign transactions securely and locally. 
- **provenance** - If set to "true" provenance data will be recorded in the operation
- **provenanceData** - Optional data to be included in the operation.
- **tokenPrice** - The purchase price of the NFT.
- **feeCurrency** - The currency in which the gas fees for the operation will be paid (only for Celo).

**Response**:
```json
{
  "txId": "0x21ec859ec3d859b3cf47d0a99660e242af4f124f514996b042339d42d5a44d90"
}
```

Again, the response contains a transaction ID from which we can get details of the transaction using the [get NFT transaction endpoint](https://developer.tatum.io/rest/smart-contracts/get-contract-address-from-transaction).

---

## Getting a list of tokens that belong to an address

If you want to display a list of tokens that someone possesses, you can use the Get NFT Account balance endpoint.

<div class='tabbed-code-blocks'>
```JavaScript
const nfts = await getNFTsByAddress(
    Currency.CELO,
    '860Cf5A199892BC527A0698e7be08a7C6Bc6064/0xfb99F8aE9b70A0C8Cd96aE665BBaf85A7E01a2ef',
);
```
```cURL
curl --location --request GET 'https://api-eu1.tatum.io/v3/nft/balance/CELO/0x1860Cf5A199892BC527A0698e7be08a7C6Bc6064/0xfb99F8aE9b70A0C8Cd96aE665BBaf85A7E01a2ef' \
--header 'x-api-key: YOUR_API_KEY'
```
</div>
**Response**:
```json
{
  "data": ["1"]
}
```

The response contains the token IDs of the NFTs at the specified address.

---

## Obtaining metadata for a specific token

If you want to obtain a metadata URL for a token, you can use the [Get NFT Token metadata](../docs/v3smartContracts/b3A6MzA3NjA5MDY-get-nft-token-metadata) endpoint.
<div class='tabbed-code-blocks'>
```SDK
const metadataURI = await getNFTMetadataURI(
    Currency.CELO,
    '0x1860Cf5A199892BC527A0698e7be08a7C6Bc6064',
    '1',
)
```
```REST API call
curl --location --request GET 'https://api-eu1.tatum.io/v3/nft/metadata/CELO/0x1860Cf5A199892BC527A0698e7be08a7C6Bc6064/1' \
--header 'x-api-key: YOUR_API_KEY'
```
</div>
The required parameters are:
- The **address** of the smart contract from which the NFT was minted
- The **token ID** of the NFT

**Response**:
```json
{
  "data": "https://my_token_data.com"
}
```

The response will contain the URL of the NFT's metadata.

---

## And that's it!

Pretty simple, right? You only had to make three calls to Tatum to deploy your own token, issue a unique one, and transfer it. Another two for getting useful information. You don't have to install anything, learn Solidity, etc.
To find out more about the API calls we have just used, visit our [API Reference](https://developer.tatum.io/rest/smart-contracts/non-fungible-token-nft).
Also, be sure to watch through our workshops on How to create NFT marketplaces:

**Frontend and user journey:**

https://www.youtube.com/watch?v=5OpFvUoP5Qc&feature=emb_title&ab_channel=Tatum

**Creating NFTs, working with metadata:**

https://www.youtube.com/watch?v=-eysAYS7l3o&feature=emb_title&ab_channel=Tatum

**Working with ERC-1155 multi-tokens:**

https://www.youtube.com/watch?v=YxB4oEn1C5g&feature=emb_title






