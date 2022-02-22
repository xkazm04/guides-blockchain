# How to send an Ethereum transaction

---

Sending an Ethereum transaction is very straightforward, you only have to execute one API call to Tatum.


<!-- theme: success -->

>The Ethereum blockchain is account-based. Your address is the account from which you send and receive every transaction.

In order to send Ethereum to another blockchain address, you have to provide the recipient address, the amount to be transferred, and the private key of the account you want to send money from (or the signature ID from Tatum KMS). The fee for the transaction is calculated automatically based on the usage of the blockchain network, but can be entered manually.

<!-- theme: info -->
> **Estimating transaction fees**
>
>You can quickly estimate transaction fees using the Estimate fee for transaction endpoint.

---

## Sending an Ethereum transaction

Enough of the theory. Let's send some Ethereum using the Eth blockchain transfer endpoint. To send ERC-20 tokens, you'll also need to include the **contractAddress** of the ERC-20 token you're sending and **digits** (number of decimal points) the ERC-20 token has.


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
  contractAddress: '0xffb28c3c7a1b19380b7e9e5A7Bbe2afF1AA7A5Ef',
  digits: 10,
  amount: "100000",
  to: "0x687422eEA2cB73B5d3e242bA5456b782919AFc85"
});

```
```REST API call with Private key
curl --request POST \
  --url https://api-eu1.tatum.io/v3/ethereum/transaction \
  --header 'content-type: application/json' \
  --header 'x-api-key: REPLACE_KEY_VALUE' \
  --header 'x-testnet-type: SOME_STRING_VALUE' \
  --data '{
    "data": "My note to recipient.",
    "nonce": 0,
    "to": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
    "currency": "ETH",
    "contractAddress": "0xffb28c3c7a1b19380b7e9e5A7Bbe2afF1AA7A5Ef",
    "digits": 10,
    "fee": {
      "gasLimit": "40000",
      "gasPrice": "20"
      },
    "amount": "100000",
    "fromPrivateKey": "0x05e150c73f1920ec14caa1e0b6aa09940899678051a78542840c2668ce5080c2"
}}'

```
```REST API call with KMS
curl --request POST \
  --url https://api-eu1.tatum.io/v3/ethereum/transaction \
  --header 'content-type: application/json' \
  --header 'x-api-key: REPLACE_KEY_VALUE' \
  --header 'x-testnet-type: SOME_STRING_VALUE' \
  --data '{
    "data": "My note to recipient.",
    "nonce": 0,
    "to": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
    "currency": "ETH",
    "contractAddress": "0xffb28c3c7a1b19380b7e9e5A7Bbe2afF1AA7A5Ef",
    "digits": 10,
    "fee": {
      "gasLimit": "40000",
      "gasPrice": "20"
      },
    "amount": "100000",
    "signatureId": "26d3883e-4e17-48b3-a0ee-09a3e484ac83",
    "index": 0
}}'
```

The response will contain a transaction ID that you can use to get information about the transaction.

**Response**:
```json
{
    "txId": "0x7698f412a70385f3e1ab513a9ecf5edd7dc1c76f0e369b1c7b059d98892091c0"
}
```

<!-- theme: warning -->

>**Securely signing transactions**
>
>In this guide, we are signing transactions with a private key via API. This is fine for testing and demo purposes, but should not be used for production purposes. 
>
>Your private keys and mnemonics should never leave your security perimeter. To correctly and securely sign a transaction you can use [Tatum CLI](https://github.com/tatumio/tatum-cli) from the command line, a specific language library like [Tatum JS](https://github.com/tatumio/tatum-js), the local [middleware API](https://github.com/tatumio/tatum-middleware), or our comprehensive key management system, [Tatum KMS](https://github.com/tatumio/tatum-kms).

---

## Getting transaction detail

To get the details of the transaction, use the transaction ID from the response of the previous call with the Ethereum/Get transaction endpoint.

```SDK
import {ethGetTransaction } from '@tatumio/tatum';
/**
 * @param hash - transaction hash
 * @returns - transaction detail
 */
const transaction = await ethGetTransaction('962e4ad3781e7036ff3af6d880744fd3f06131c32d1085254da3cfa77b0e933f');
```
```REST API call
curl --location --request GET 'https://api-eu1.tatum.io/v3/ethereum/transaction/0x7698f412a70385f3e1ab513a9ecf5edd7dc1c76f0e369b1c7b059d98892091c0' \
--header 'x-api-key: YOUR_API_KEY' 
```
The response will contain the details of the transaction.

Response:
```json
{
    "blockHash": "0x51ba00b430d47ab623a8d4e516cb8a11dc5ee7e773bd9649ac2ad2573b9d3616",
    "blockNumber": 9115179,
    "contractAddress": null,
    "gas": 26000,
    "gasPrice": "60000000000",
    "gasUsed": 21000,
    "logs": [],
    "nonce": 0,
    "status": true,
    "transactionHash": "0x7698f412a70385f3e1ab513a9ecf5edd7dc1c76f0e369b1c7b059d98892091c0",
    "transactionIndex": 0,
    "value": "10000000000000000",
    "from": "0xfb99f8ae9b70a0c8cd96ae665bbaf85a7e01a2ef",
    "to": "0x687422eea2cb73b5d3e242ba5456b782919afc85"
}
```

And that's it. A piece of cake. Now you know how to send Eth and ERC-20 tokens on Ethereum and get the details of the transaction. To find out more about the endpoints you used today, read our API Reference.










































