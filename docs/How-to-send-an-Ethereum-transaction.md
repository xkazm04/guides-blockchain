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

## Sending an Ethereum transaction

Enough of the theory. Let's send some Ethereum using the Eth blockchain transfer endpoint. To send ERC-20 tokens, you'll also need to include the **contractAddress** of the ERC-20 token you're sending and **digits** (number of decimal points) the ERC-20 token has.


```JavaScript
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
```cURL + private key
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
```cURL + KMS

```






















































