# How to create an ERC-20 token

*Create your own token in one simple step.*

---

<!-- theme: info -->

>An [ERC-20](https://www.investopedia.com/news/what-erc20-and-what-does-it-mean-ethereum/) token is a standardized smart contract with a predefined set of features. It represents fungible tokens, which can be interchanged. The ERC-20 token is used as a blockchain representation of the currency.

To create your own ERC-20 token, you need to deploy a smart contract. Creating your own smart contract is not easy at all. As a developer, you must do a few things:

- Solidity - you have to be able to write smart contracts in Solidity or the smart contract programming language used by the given blockchain.
- Run the Solidity compiler
- Run the Solidity development IDE
- Deploy the compiled Smart Contract

It is a lot to know. For developers who only need the basic features of ERC-20 tokens, Tatum offers ready-to-go, standardized ERC-20 smart contracts that can be deployed on the following blockchains:

- **Ethereum**
- **Polygon (Matic)**
- **Celo**
- **Harmony.ONE**
- **Flow**
- **Tron**
- **Binance Smart Chain**

<!-- theme: success -->

>Feel free to check out the source code to our ERC-20 smart contract on [GitHub](https://github.com/tatumio/tatum-middleware/blob/master/src/contracts/token.sol).

## Deploying an ERC-20 Smart Contract

To create and deploy an ERC-20 token using Tatum, all you need is one simple API call. This API call deploys a standard, validated ERC-20 smart contract to the blockchain designated in the chain field. The required parameters are the name and symbol of the deployed token, the initial supply of the tokens, and the recipient address where the initial supply will be transferred.

<!-- theme: warning -->

>**Securely signing transactions**
>
>In this guide, we are signing transactions with a private key via API. This is fine for testing and demo purposes, but should not be used for production purposes. 
>
>Your private keys and mnemonics should never leave your security perimeter. To correctly and securely sign a transaction you can use [Tatum CLI](https://github.com/tatumio/tatum-cli) from the command line, a specific language library like [Tatum JS](https://github.com/tatumio/tatum-js), the local [middleware API](https://github.com/tatumio/tatum-middleware), or our comprehensive key management system, [Tatum KMS](https://github.com/tatumio/tatum-kms).

```SDK
import {prepareDeployErc20SignedTransaction} from '@tatumio/tatum'
/**
 * Sign Ethereum deploy ERC20 transaction with private keys locally. Nothing is broadcast to the blockchain.
 * @param body content of the transaction to broadcast
 * @param provider url of the Ethereum Server to connect to. If not set, default public server will be used.
 * @returns transaction data to be broadcast to blockchain.
 */
 
const body = {
  chain: "ETH",
  symbol: "ERC_SYMBOL",
  name: "MyERC20",
  totalCap: "10000000",
  supply: "10000000",
  digits: 18,
  address: "0xa0Ca9FF38Bad06eBe64f0fDfF279cAE35129F5C6",
  fromPrivateKey: "0x05e150c73f1920ec14caa1e0b6aa09940899678051a78542840c2668ce5080c2",
  nonce: 0,
  fee: {
  gasLimit: "40000",
  gasPrice: "20"
  }
}
const prepareDeploy = await prepareDeployErc20SignedTransaction(body)
```
```REST API call with Private key
curl --request POST \
  --url https://api-eu1.tatum.io/v3/blockchain/token/deploy \
  --header 'content-type: application/json' \
  --header 'x-api-key: REPLACE_KEY_VALUE' \
  --header 'x-testnet-type: SOME_STRING_VALUE' \
  --data '{
    "chain": "ETH",
    "symbol": "ERC_SYMBOL",
    "name": "MyERC20",
    "totalCap": "10000000",
    "supply": "10000000",
    "digits": 18,
    "address": "0xa0Ca9FF38Bad06eBe64f0fDfF279cAE35129F5C6",
    "fromPrivateKey": "0x05e150c73f1920ec14caa1e0b6aa09940899678051a78542840c2668ce5080c2",
    "nonce": 0,
    "fee": {
      "gasLimit": "40000",
      "gasPrice": "20"
      }
}'
```
```REST API call with KMS
curl --request POST \
  --url https://api-eu1.tatum.io/v3/blockchain/token/deploy \
  --header 'content-type: application/json' \
  --header 'x-api-key: REPLACE_KEY_VALUE' \
  --header 'x-testnet-type: SOME_STRING_VALUE' \
  --data '{
    "chain": "ETH",
    "symbol": "ERC_SYMBOL",
    "name": "MyERC20",
    "totalCap": "10000000",
    "supply": "10000000",
    "digits": 18,
    "address": "0xa0Ca9FF38Bad06eBe64f0fDfF279cAE35129F5C6",
    "signatureId": "26d3883e-4e17-48b3-a0ee-09a3e484ac83",
    "nonce": 0,
    "fee": {
      "gasLimit": "40000",
      "gasPrice": "20"
      }
}'
```
The response will contain a transaction ID, from which you can obtain the address of the smart contract you have deployed.

**Response**:

```json
{
  "txId": "0x75808516abfb2fa4e44fd2fc85863d15e11bdc3f90769962e3f34d7a17df4f0f"
}
```

## Getting the address of the smart contract


To obtain the address of the smart contract you've deployed, use the following API request:

```SDK
const contractAddress = await getNFTContractAddress(Currency.ETH, '0x75808516abfb2fa4e44fd2fc85863d15e11bdc3f90769962e3f34d7a17df4f0f');
```
```REST API call 
curl --location --request GET 'https://api-eu1.tatum.io/v3/blockchain/sc/address/ETH/0x75808516abfb2fa4e44fd2fc85863d15e11bdc3f90769962e3f34d7a17df4f0f' \
--header 'x-api-key: YOUR_API_KEY'
```

The response will contain the address of the smart contract you've deployed.

**Response**:
```json
{
  "contractAddress": "0x316e52b83c66E8FB3936bdc2C8Ed8437441774cE"
}
```

## Transferring ERC-20 tokens to another blockchain address

To transfer the tokens from the address where they were issued to another blockchain address, you can use the Transfer ERC20 token endpoint from the Fungible Token section of the API documentation. You need the private key of the address where tokens are located (address from the first call where the initial supply is distributed) or the signature ID of the signed transaction from Tatum KMS.

```SDK
import {sendEthOrErc20Transaction} from '@tatumio/tatum';
/**
 * Send Ethereum or supported ERC20 transaction to the blockchain.
 * @param chain - chain to work with
 * @param fromPrivateKey - private key of sender address. Private key, or signature Id must be present.
 * @param contractAddress - address of ERC20 token
 * @param digits - number of decimal points that ERC20 token has
 * @param amount - amount to be sent
 * @param to - blockchain address to send ERC20 token to
 * @returns transaction id of the transaction in the blockchain
 */
  const transaction = await sendEthOrErc20Transaction({
  chain: Currency.ETH,
  fromPrivateKey: '0x05e150c73f1920ec14caa1e0b6aa09940899678051a78542840c2668ce5080c2',
  contractAddress: '0x316e52b83c66E8FB3936bdc2C8Ed8437441774cE',
  digits: 18,
  amount: "100000",
  to: "0x687422eEA2cB73B5d3e242bA5456b782919AFc85"
});
```
```REST API call with Private key
curl --request POST \
  --url https://api-eu1.tatum.io/v3/blockchain/token/transaction \
  --header 'content-type: application/json' \
  --header 'x-api-key: REPLACE_KEY_VALUE' \
  --header 'x-testnet-type: SOME_STRING_VALUE' \
  --data '{
    "chain": "ETH",
    "to": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
    "amount": "100000",
    "contractAddress": "0x316e52b83c66E8FB3936bdc2C8Ed8437441774cE",
    "digits": 18,
    "fromPrivateKey": "0x05e150c73f1920ec14caa1e0b6aa09940899678051a78542840c2668ce5080c2",
    "nonce": 0,
    "fee": {
      "gasLimit": "40000",
      "gasPrice": "20"
      }
}'
```
```REST API call with KMS
curl --request POST \
  --url https://api-eu1.tatum.io/v3/blockchain/token/transaction \
  --header 'content-type: application/json' \
  --header 'x-api-key: REPLACE_KEY_VALUE' \
  --header 'x-testnet-type: SOME_STRING_VALUE' \
  --data '{
    "chain": "ETH",
    "to": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
    "amount": "100000",
    "contractAddress": "0x316e52b83c66E8FB3936bdc2C8Ed8437441774cE",
    "digits": 18,
    "signatureId": "26d3883e-4e17-48b3-a0ee-09a3e484ac83",
    "nonce": 0,
    "fee": {
      "gasLimit": "40000",
      "gasPrice": "20"
      }
}'
```

The response will contain the transaction's ID from which you can get the details of the transaction.

**Response**:
```json
{
    "txId": "0xc330005e40aa36b1256f6629ebbf8c0a1b73bf1b517ed4fecf862d422b190ec3"
}
```

That wasn't too bad at all, right? With just three calls to Tatum, you were able to deploy your own token and transfer it. You don't have to install anything, learn Solidity, etc.

To find out more about the API calls we have just used, visit our API Reference.





















