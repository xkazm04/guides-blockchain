# How to send a Bitcoin transaction

---

To send a Bitcoin transaction, you have two options with Tatum:
- You can enter a list of blockchain addresses from which the assets will be sent.
- You can enter specific Unspent Transaction Outputs (UTXO) which will be used to send the assets.

![how_to_send_transaction_opravena-07.png](https://stoplight.io/api/v1/projects/cHJqOjExNjE4OQ/images/wtzsmVq0MxE)


<!-- theme: info -->

>**More about Bitcoin**
>
>The Bitcoin blockchain is not account-based but transaction-based. You operate with a list of previously received transactions and the assets within them. This means that you can use multiple transactions in one transaction as a source of your assets, not just one specific address.
>
>To find out more about how Bitcoin works, check out [this article](https://www.investopedia.com/terms/u/utxo.asp).

When you enter a list of addresses, all incoming transactions that have not previously been used are used as a source. When you enter a list of UTXOs, only specific unspent transactions that belong to you are used.

![how_to_send_transaction_opravena-04.png](https://stoplight.io/api/v1/projects/cHJqOjExNjE4OQ/images/R0Q7CooFpf8)

<!-- theme: warning -->

>**Bitcoin transaction inputs**
>
>Please be aware that all of the assets that belong to the specific transaction are used as input, not only a fraction of them. To learn more about Bitcoin transactions, check out [this article](https://en.bitcoin.it/wiki/Transaction).

Finally, you have to know how much bitcoin you want to send to whom. You need to define a list of recipient addresses with the specific amounts that each address is to receive. The sum of the assets being sent must be bigger than the sum of assets being received. The difference is the transaction fee that must be paid to the network.

![how_to_send_transaction_opravena-06.png](https://stoplight.io/api/v1/projects/cHJqOjExNjE4OQ/images/TwUCjcHqSxY)

<!-- theme: info -->

>**Example**
>
>You want to use 10 BTC from 2 addresses as input and send it to 3 recipients, each of which will receive 3 BTC. The fee is 10 BTC - 9 BTC = 1 BTC.

## Sending a Bitcoin transaction

<!-- theme: warning -->

>**Securely signing transactions**
>
>In this guide, we are signing transactions with a private key via API. This is fine for testing and demo purposes, but should not be used for production purposes. 
>
>Your private keys and mnemonics should never leave your security perimeter. To correctly and securely sign a transaction you can use [Tatum JS](https://github.com/tatumio/tatum-js), the local [middleware API](https://github.com/tatumio/tatum-middleware), or our comprehensive key management system, [Tatum KMS](https://github.com/tatumio/tatum-kms). 

Enough of the theory. Let's send some bitcoin using this API endpoint.





```JavaScript
import {sendBitcoinTransaction} from '@tatumio/tatum';
/**
 * @param testnet - mainnet or testnet version
 * @param body - content of the transaction to send
 * @returns transaction Id
 */
 
const body = {
// If source = Wallet address
fromAddress: [{
      address: "mhuKgqf7SLRWPX8Sfrqn68i5YcdWsFMRXF",
      privateKey: "cNkXKkQ4YoNS2LmnvhtGqWBQKg7oo3BRaowPhSDPvTRjxXyGG6yr"
 }],
// If source = UTXO
fromUTXO: [
{
      txHash: "887dd5221800c65ada2a2081e65a14b5421b30600d4ab112421a44b17ded6ed4",
      index: 0,
      privateKey: "cNkXKkQ4YoNS2LmnvhtGqWBQKg7oo3BRaowPhSDPvTRjxXyGG6yr"
}
// Always fill recipient address and amount to send
],
to: [{
      address: "n1XD955iuticPuKoJJr8XfAjDw2gYadNrR",
      value: 0.15991455
  },{
      address: "mhuKgqf7SLRWPX8Sfrqn68i5YcdWsFMRXF",
      value: 0.4
  }]}
 
const transactionHash =  await sendBitcoinTransaction(false, body);
```
```cURL - List of addresses as a source
curl --location --request POST 'https://api-eu1.tatum.io/v3/bitcoin/transaction' \
--header 'x-api-key: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "fromAddress": [
        {
            "address": "mgSXLa5sJHvBpYTKZ62aW9z2YWQNTJ59Zm",
            "privateKey": "cNUHqqs67WVhMZP9hH3i9nLQ1BGEasA87y6N6SQMsfYaBaPi9SEs"
        }
    ],
    "to": [
        {
            "address": "mgSXLa5sJHvBpYTKZ62aW9z2YWQNTJ59Zm",
            "value": 0.000006
        },
        {
            "address": "mgSXLa5sJHvBpYTKZ62aW9z2YWQNTJ59Zm",
            "value": 0.000009
        }
    ]
}'
```
```cURL - List of UTXOs as a source
curl --location --request POST 'https://api-eu1.tatum.io/v3/bitcoin/transaction' \
--header 'x-api-key: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "fromUTXO": [
        {
            "txHash": "b2dc763cf75373ad14c984c07846f5425c20ddf34606debb0a148cd770528ef9",
            "index": 0,
            "privateKey": "cNUHqqs67WVhMZP9hH3i9nLQ1BGEasA87y6N6SQMsfYaBaPi9SEs"
        },
        {
            "txHash": "b2dc763cf75373ad14c984c07846f5425c20ddf34606debb0a148cd770528ef9",
            "index": 1,
            "privateKey": "cNUHqqs67WVhMZP9hH3i9nLQ1BGEasA87y6N6SQMsfYaBaPi9SEs"
        }
    ],
    "to": [
        {
            "address": "mgSXLa5sJHvBpYTKZ62aW9z2YWQNTJ59Zm",
            "value": 0.0000006
        },
        {
            "address": "mgSXLa5sJHvBpYTKZ62aW9z2YWQNTJ59Zm",
            "value": 0.000006
        }
    ]
}'
```

<!-- theme: warning -->

> When using Tatum KMS to securely sign transactions, you will use a **signatureId** instead of a **privateKey** parameter in the body of the API request.

The response will contain a transaction ID, from which we can get information about the transaction.

**Response:**
```json
{
    "txId": "962e4ad3781e7036ff3af6d880744fd3f06131c32d1085254da3cfa77b0e933f"
}
```

## Getting transaction details
To get information about the transaction, you can use the transaction ID from the response of the previous request with the Bitcoin/Get transaction endpoint.

```json
{
    "txId": "962e4ad3781e7036ff3af6d880744fd3f06131c32d1085254da3cfa77b0e933f"
}
```









































































