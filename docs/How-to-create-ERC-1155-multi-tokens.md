# How to create ERC-1155 mult-tokens

*Create flexible multi tokens with both fungible and non-fungible properties*

---

<!-- theme: info -->

>The [ERC-1155](https://eips.ethereum.org/EIPS/eip-1155) is a standardized smart contract with a predefined set of features. These tokens have properties in common with both fungible and non-fungible tokens. Any single contract may include any combination of fungible, non-fungible, or semi-fungible tokens.

NFTs are typically made using ERC-721 smart contracts on Ethereum-compatible blockchains. While this type of contract gives them the unique properties that make them so desirable (scarcity, verifiable authenticity), it also has certain limitations.

ERC-721 tokens each have a unique ID, for which there can be only one token. This makes batch operations very costly in terms of gas fees and makes shared ownership impossible.

ERC-1155 tokens can have multiple tokens for a single smart contract ID. This opens up all sorts of possibilities for shared ownership, gaming applications, and a wide range of other use cases.

However, creating an ERC-1155 smart contract on an Ethereum-compatible blockchain is not easy at all. As a developer, you have to know a lot of things:
- Solidity - a new programming language for writing smart contracts
- How to use a compiler for Solidity
- How to develop using an IDE
- How to deploy the compiled smart contract

![ntf__-11.png](https://stoplight.io/api/v1/projects/cHJqOjExNjE4OQ/images/sDrExhIFg24)

It's a lot to know, and for developers who only need the basic features of ERC-1155 tokens, it would take weeks to figure it all out.

<!-- theme: success -->

>Tatum has a prebuilt ERC-1155 contract ready for deployment. The source code is available on [GitHub](https://github.com/tatumio/smart-contracts/blob/master/contracts/token/ERC1155/Tatum1155.sol).

Please refer to the guide below and the following workshop to learn how to create and work with ERC-1155 multi-tokens:

https://www.youtube.com/watch?v=YxB4oEn1C5g&feature=emb_title&ab_channel=Tatum

---

## Creating a multi-token smart contract

To create and deploy multi tokens using Tatum, all you need is one simple API call. This API call deploys a standard, validated multi-token smart contract to the blockchain of your choice. 

In this example, we are using [Celo](https://celo.org/) because it is fast, cheap and you can pay for transactions with a stable coin (cUSD). The required parameters are the name and symbol of the deployed token, the initial supply of the tokens, and the recipient address where the initial supply will be transferred.

<!-- theme: warning -->
>In this guide, we are signing transactions using a private key via API. This is fine for testing and demo purposes, but for production use, your private keys and mnemonics should never leave your security perimeter. To correctly and securely sign a transaction, you can use [Tatum CLI](https://github.com/tatumio/tatum-cli) from the command line, a specific language library like [Tatum JS](https://github.com/tatumio/tatum-js), the local [middleware API](https://github.com/tatumio/tatum-middleware), or our comprehensive key management system, [Tatum KMS](https://github.com/tatumio/tatum-kms).

Use the following API endpoint to deploy an ERC-1155 smart contract:

```SDK
import {Currency, deployMultiToken} from '@tatumio/tatum';
/**
 * Deploy MultiTokens (1155) contract.
 * @param testnet - testnet or mainnet
 * @param body - body of the request - https://tatum.io/apidoc#operation/DeployMultiToken
 * @param provider - (optional) provider to broadcast tx
 */
const body = {
  fromPrivateKey: '0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb',
  chain: Currency.CELO,
  uri: "https://example.com/{id}",
  fee: {
    gasLimit: "40000",
    gasPrice: "40"
  }
}
const txId = deployMultiToken(false, body);
```
```REST API call with Private Key
curl --location --request POST 'https://api-eu1.tatum.io/v3/multitoken/deploy/' \
--header 'Content-Type: application/json' \
--header 'x-api-key: 4966d428-9507-45cb-9f90-02cca00674bd' \
--data-raw '{
    "chain": "CELO",
    "fromPrivateKey": "0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb",
    "uri": "https://example.com/{id}",
    "fee": {
      "gasLimit": "40000",
      "gasPrice": "40"
    }
}'
```
```REST API call with KMS
curl --location --request POST 'https://api-eu1.tatum.io/v3/multitoken/deploy/' \
--header 'Content-Type: application/json' \
--header 'x-api-key: 4966d428-9507-45cb-9f90-02cca00674bd' \
--data-raw '{
    "chain": "CELO",
    "publicMint": true,
    "index": 0,
    "signatureId": "26d3883e-4e17-48b3-a0ee-09a3e484ac83",
    "uri": "https://example.com/{id}"
    "fee": {
      "gasLimit": "40000",
      "gasPrice": "40"
    }
}'
```
The response will contain a transaction ID from which we can obtain the smart contract's address.

```Response
{
    "txId": "0xd17dbf37a9b2467c69ccf9fab35af2f2e7f329f3c760f684373625f5af2331bd"
}
```

---

## Getting the address of the contract from a blockchain transaction

Use the following API call to get the address of the smart contract from the transaction ID.
The required parameters are:
- The **chain** you are working on
- The **transactionId** in the response to the previous request

```SDK
import {Currency, getNFTContractAddress} from '@tatumio/tatum';
const contractAddress = await getNFTContractAddress(Currency.CELO, '0xd17dbf37a9b2467c69ccf9fab35af2f2e7f329f3c760f684373625f5af2331bd');
```
```REST API call
curl --request GET \
  --url https://api-eu1.tatum.io/v3/blockchain/sc/address/CELO/0xd17dbf37a9b2467c69ccf9fab35af2f2e7f329f3c760f684373625f5af2331bd \
  --header 'x-api-key: REPLACE_KEY_VALUE'
```

The response will contain the address of the NFT smart contract you deployed in the previous step.

```Response
{
    "contractAddress": "0x7e8fada7108d0DBbE8C6aca50d0FFb13CC34Eb81"
}
```

---

## Minting new ERC-1155 tokens

When your smart contract is deployed and you know the contract address, you can issue new tokens with the mint multi-token endpoint. This endpoint will create a new token with a unique identifier. Every token should have [metadata as a JSON schema](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-721.md#specification) containing additional information about the token, like the image URL, description, etc.

```SDK
import {Currency, mintMultiToken} from '@tatumio/tatum';
/**
 * Mint MultiToken (1155) with Tatum KMS.
 * @param testnet - testnet or mainnet
 * @param body - body of the request - https://tatum.io/apidoc#operation/MintMultiToken
 * @param provider - (optional) provider to broadcast tx
 */
const body = {
  fromPrivateKey: '0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb',
  chain: Currency.CELO,
  tokenId: "1",
  to: "0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea",
  amount: "10",
  fee: {
    gasLimit: "40000",
    gasPrice: "40"
  }
}
const txId = mintMultiToken(false, body);
```
```REST API call with Private key
curl --location --request POST 'https://api-eu1.tatum.io/v3/multitoken/mint/' \
--header 'Content-Type: application/json' \
--header 'x-api-key: 4966d428-9507-45cb-9f90-02cca00674bd' \
--data-raw '{
    "to": "0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea",
    "contractAddress": "0x7e8fada7108d0DBbE8C6aca50d0FFb13CC34Eb81",
    "tokenId": "1",
    "amount": "10",
    "chain": "CELO",
    "fromPrivateKey": "0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb",
    "nonce": 0,
    "fee": {
      "gasLimit": "40000",
      "gasPrice": "40"
    }
}'
```
```REST API call with KMS
curl --location --request POST 'https://api-eu1.tatum.io/v3/multitoken/mint/' \
--header 'Content-Type: application/json' \
--header 'x-api-key: 4966d428-9507-45cb-9f90-02cca00674bd' \
--data-raw '{
    "to": "0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea",
    "contractAddress": "0x7e8fada7108d0DBbE8C6aca50d0FFb13CC34Eb81",
    "tokenId": "1",
    "amount": "10",
    "chain": "CELO",
    "index": 0,
    "signatureId": "26d3883e-4e17-48b3-a0ee-09a3e484ac83",
    "nonce": 0,
    "fee": {
      "gasLimit": "40000",
      "gasPrice": "40"
    }
}'
```

The response will contain a transaction ID from which you can obtain the details of the transaction.

```Response
{
    "txId": "0x67e5f725726c3bd7fc6689797be8ef41f59aa535ef33e4d7135874be4e820289"
}
```

Since ERC-1155 smart contracts can include multiple tokens for the same ID, you can also mint multiple tokens for multiple IDs using the mint multi-token batch endpoint.

```SDK
import {Currency, mintMultiTokenBatch} from '@tatumio/tatum';
/**
 * Mint batch of MultiTokens (1155) with Tatum KMS.
 * @param testnet - testnet or mainnet
 * @param body - body of the request - https://tatum.io/apidoc#operation/MintMultiToken
 * @param provider - (optional) provider to broadcast tx
 */
const body = {
  fromPrivateKey: '0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb',
  chain: Currency.CELO,
  tokenId: ["1","2"],
  to: ["0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea", "0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea"],
  amounts: ["10", "20"],
  fee: {
    gasLimit: "40000",
    gasPrice: "40"
  }
};
const txId = mintMultiTokenBatch(false, body);
```
```REST API call with Private key
curl --location --request POST 'https://api-eu1.tatum.io/v3/multitoken/mint/batch' \
--header 'Content-Type: application/json' \
--header 'x-api-key: 4966d428-9507-45cb-9f90-02cca00674bd' \
--data-raw '{
    "to": ["0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea", "0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea"],
    "contractAddress": "0x7e8fada7108d0DBbE8C6aca50d0FFb13CC34Eb81",
    "tokenId": [["1"], ["2"]],
    "amounts": [["10"], ["20"]],
    "chain": "CELO",
    "fromPrivateKey": "0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb",
    "fee": {
      "gasLimit": "40000",
      "gasPrice": "40"
    }
}'
```
```REST API call with Private key
curl --location --request POST 'https://api-eu1.tatum.io/v3/multitoken/mint/batch' \
--header 'Content-Type: application/json' \
--header 'x-api-key: 4966d428-9507-45cb-9f90-02cca00674bd' \
--data-raw '{
    "to": ["0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea", "0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea"],
    "contractAddress": "0x7e8fada7108d0DBbE8C6aca50d0FFb13CC34Eb81",
    "tokenId": [["1"], ["2"]],
    "amounts": [["10"], ["20"]],
    "chain": "CELO",
    "index": 0,
    "signatureId": "26d3883e-4e17-48b3-a0ee-09a3e484ac83",
    "nonce": 0,
    "fee": {
      "gasLimit": "40000",
      "gasPrice": "40"
    }
}'
```

The response will contain a transaction ID from which you can obtain the details of the transaction.

```Response
{
    "txId": "0x0b4e6281b7710e6d0b4fdbed463a265f87ab3ac41c8f3e8e75b7b56331e3e091"
}
```


---

## Transferring ERC-1155 tokens

To transfer tokens from the address where they were issued to another blockchain address, you can use the transfer multi-token endpoint. You need the private key of the address where the tokens are located (address from the first call where the initial supply is distributed).

```SDK
import {Currency, transferMultiToken} from '@tatumio/tatum';
/**
 * Transfer MultiTokens (1155)
 * @param testnet - testnet or mainnet
 * @param body - body of the request - https://tatum.io/apidoc.php#operation/TransferMultiToken
 * @param provider - (optional) provider to broadcast tx
 */
const body = {
  fromPrivateKey: '0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb',
  chain: Currency.CELO,
  contractAddress: "0x7e8fada7108d0DBbE8C6aca50d0FFb13CC34Eb81",
  tokenId: "1",
  to: "0xfb99F8aE9b70A0C8Cd96aE665BBaf85A7E01a2ef",
  amount: "1",
  fee: {
    gasLimit: "40000",
    gasPrice: "40"
  }
};
const txId = transferMultiToken(false, body);
```
```REST API call with Private key
curl --location --request POST 'https://api-eu1.tatum.io/v3/multitoken/transaction/' \
--header 'Content-Type: application/json' \
--header 'x-api-key: 4966d428-9507-45cb-9f90-02cca00674bd' \
--data-raw '{
    "to": "0xfb99F8aE9b70A0C8Cd96aE665BBaf85A7E01a2ef",
    "contractAddress": "0x7e8fada7108d0DBbE8C6aca50d0FFb13CC34Eb81",
    "tokenId": "1",
    "amount": "1",
    "chain": "CELO",
    "fromPrivateKey": "0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb",
    "fee": {
      "gasLimit": "40000",
      "gasPrice": "40"
    }
}'
```
```REST API call with KMS
curl --location --request POST 'https://api-eu1.tatum.io/v3/multitoken/transaction/' \
--header 'Content-Type: application/json' \
--header 'x-api-key: 4966d428-9507-45cb-9f90-02cca00674bd' \
--data-raw '{
    "to": "0xfb99F8aE9b70A0C8Cd96aE665BBaf85A7E01a2ef",
    "contractAddress": "0x7e8fada7108d0DBbE8C6aca50d0FFb13CC34Eb81",
    "tokenId": "1",
    "amount": "1",
    "chain": "CELO",
    "index": 0,
    "signatureId": "26d3883e-4e17-48b3-a0ee-09a3e484ac83",
    "nonce": 1,
    "fee": {
      "gasLimit": "40000",
      "gasPrice": "40"
    }
}'
```

The response will contain a transaction ID from which you can obtain the details of the transaction.

```Response
{
    "txId": "0x4907008b79c881a61db1af4bec792f822070c0cffed77c26151ec917d6b0c2fd"
}
```

You can also transfer multiple tokens at once using the transfer multi-token batch endpoint. This method significantly saves gas costs, which is especially useful on the Ethereum blockchain.

```SDK
import {Currency, transferMultiTokenBatch} from '@tatumio/tatum';
/**
 * Transfer MultiTokens (1155)
 * @param testnet - testnet or mainnet
 * @param body - body of the request - https://tatum.io/apidoc.php#operation/TransferMultiToken
 * @param provider - (optional) provider to broadcast tx
 */
const body = {
  fromPrivateKey: '0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb',
  chain: Currency.CELO,
  contractAddress: "0x7e8fada7108d0DBbE8C6aca50d0FFb13CC34Eb81",
  tokenId: ["1","2"],
  to: ["0xfb99F8aE9b70A0C8Cd96aE665BBaf85A7E01a2ef"],
  amounts: ["1", "1"],
  fee: {
    gasLimit: "40000",
    gasPrice: "40"
  }
};
const txId = transferMultiTokenBatch(false, body);
```
```REST API call with Private key
curl --location --request POST 'https://api-eu1.tatum.io/v3/multitoken/transaction/batch' \
--header 'Content-Type: application/json' \
--header 'x-api-key: 4966d428-9507-45cb-9f90-02cca00674bd' \
--data-raw '{
    "to": "0xfb99F8aE9b70A0C8Cd96aE665BBaf85A7E01a2ef",
    "contractAddress": "0x7e8fada7108d0DBbE8C6aca50d0FFb13CC34Eb81",
    "tokenId": ["1", "2"],
    "amounts": ["1", "1"],
    "chain": "CELO",
    "fromPrivateKey": "0x4874827a55d87f2309c55b835af509e3427aa4d52321eeb49a2b93b5c0f8edfb",
    "fee": {
      "gasLimit": "40000",
      "gasPrice": "40"
    }
}'
```
```REST API call with KMS
curl --location --request POST 'https://api-eu1.tatum.io/v3/multitoken/transaction/batch' \
--header 'Content-Type: application/json' \
--header 'x-api-key: 4966d428-9507-45cb-9f90-02cca00674bd' \
--data-raw '{
    "to": "0xfb99F8aE9b70A0C8Cd96aE665BBaf85A7E01a2ef",
    "contractAddress": "0x7e8fada7108d0DBbE8C6aca50d0FFb13CC34Eb81",
    "tokenId": ["1", "2"],
    "amounts": ["1", "1"],
    "chain": "CELO",
    "index": 0,
    "signatureId": "26d3883e-4e17-48b3-a0ee-09a3e484ac83",
    "nonce": 1,
    "fee": {
      "gasLimit": "40000",
      "gasPrice": "40"
    }
}'
```

The response will contain a transaction ID from which you can obtain the details of the transaction.

```Response
{
    "txId": "0x1d6f1fd6dcd3a552ae02badbe7a2de7085ee5155d3aa0321a4afbbefbbb9815e"
}
```

---

## Getting a list of tokens that belong to an address

If you want to display a list of tokens that someone possesses, you can use the get multi-token account balance endpoint. 
The required parameters for this call are:
- The **chain** you are working on
- The **contractAddress** from which the tokens were minted
- The **accountAddress** you'd like to get token balances for
- The **tokenId** of the token you'd like to obtain a balance for

```SDK
import {Currency, getMultiTokensBalance} from '@tatumio/tatum';
  /**
   * Get Multi Tokens on Account. Returns tokenIDs of tokens Account holds.
   * @param chain - blockchain to use function on
   * @param address - account address
   * @param contractAddress - multi Token contract address
   * @param tokenId - multi Token token unique identifier
   */
const balance = getMultiTokensBalance(Currency.CELO, 
"0x7e8fada7108d0DBbE8C6aca50d0FFb13CC34Eb81","0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea","1");
```
```REST API call
curl --location --request GET 'https://api-eu1.tatum.io/v3/multitoken/balance/CELO/0x7e8fada7108d0DBbE8C6aca50d0FFb13CC34Eb81/0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea/1' \
--header 'x-api-key: 4966d428-9507-45cb-9f90-02cca00674bd'
```

The response will contain the balance of the tokens for the specific token ID at the designated address.

```Response
{
    "data": "18"
}
```

You can also get the balance of multiple accounts at once using the get multi-token account balance batch endpoint.
The required parameters for this call are:
- The **chain** you are working on
- The **contractAddress** from which the tokens were minted
- Each **accountAddress** you'd like to get token balances for
- Each **tokenId** of the token you'd like to obtain a balance for

```SDK
import {Currency, getMultiTokensBatchBalance} from '@tatumio/tatum';
  /**
   * Get Multi Tokens on Account. Returns tokenIDs of tokens Account holds.
   * @param chain - blockchain to use function on
   * @param address - account address
   * @param contractAddress - multi Token contract address
   * @param tokenId - multi Token token unique identifier
   */
const balance = await getMultiTokensBatchBalance(
 Currency.CELO, 
 "0x7e8fada7108d0DBbE8C6aca50d0FFb13CC34Eb81",  // contract
 "0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea, 0x3223AEB8404C7525FcAA6C512f91e287AE9FfE7B", // addresses
 "1,2" // token ids
 );
```
```REST API call
curl --location --request GET 'https://api-eu1.tatum.io/v3/multitoken/balance/batch/CELO/0x7e8fada7108d0DBbE8C6aca50d0FFb13CC34Eb81/?tokenId=1,2&address=0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea,0x3223AEB8404C7525FcAA6C512f91e287AE9FfE7B' \
--header 'x-api-key: 4966d428-9507-45cb-9f90-02cca00674bd'
```

The response will contain the balances of each token ID at each address specified in the call.

```Response
{
    "data": [
        "18", 
        "24",
        "3",
        "15"
    ]
}
```

---

## Obtaining metadata for a multi-token

If you want to obtain a metadata URL for a token, you can use the get multi-token metadata endpoint. If there is `{id}` in the URL, it will be automatically replaced with the token ID.
The required parameters for this call are:
- The **chain** you are working on
- The **contractAddress** from which the tokens were minted
- The **tokenId** for which you'd like to obtain metadata

```SDK
import {Currency, getMultiTokenMetadata} from '@tatumio/tatum';
  /**
   * Get Multi Token Metadata
   * @param chain - blockchain to use function on
   * @param contractAddress contract address of the NFT token
   * @param tokenId ID of the token
   */
const metadata = getMultiTokenMetadata(Currency.CELO, "0x7e8fada7108d0DBbE8C6aca50d0FFb13CC34Eb81","1");
```
```REST API call
curl --location --request GET 'https://api-eu1.tatum.io/v3/multitoken/metadata/CELO/0x7e8fada7108d0DBbE8C6aca50d0FFb13CC34Eb81/1' \
--header 'x-api-key: 4966d428-9507-45cb-9f90-02cca00674bd'
```
```Response
{
    "data": "https://example.com/1"
}
```

So that's all there is to it! Just a few calls to accomplish just about everything you need to with ERC-1155 multi-tokens. No need to install anything or learn Solidity, and you can perform the exact same operations on any blockchain that supports ERC-1155 tokens in Tatum.

To find out more about the API calls we have just used, visit our API Reference.















































































































