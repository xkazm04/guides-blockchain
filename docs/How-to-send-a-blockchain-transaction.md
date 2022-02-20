# How to send a blockchain transaction

---
This guide describes sending a blockchain transaction,  specifically a Bitcoin transaction.

To send a Bitcoin transaction with Tatum, you have two options:
1. Entering blockchain addresses
You can enter a list of blockchain addresses from which the assets will be sent. All incoming transactions not previously used will be taken as a source.

2. Entering UTXOs
You can enter specific Unspent Transaction Outputs (UTXO) which will be used to send the assets. Only specific UTXOs that belong to you will be used.

<!-- theme: info -->

> #### More about Bitcoin
> The Bitcoin blockchain is transaction-based, not account-based. You operate with a list of previously received transactions and the assets within them. This means that in one transaction, you can use multiple transactions as a source of your assets, not just one specific address.
>To find out more about how Bitcoin works, check out [this article](https://www.investopedia.com/terms/u/utxo.asp).


<!-- theme: warning -->

>#### Bitcoin transaction inputs
>Keep in mind that all assets belonging to a specific transaction are used as input, not only a fraction of them. To learn more about Bitcoin transactions, check out [this article](https://en.bitcoin.it/wiki/Transaction).

To specify how much bitcoin you want to send and to whom, you need to define a list of recipient addresses with the specific amounts that each address is to receive. The sum of assets to be sent must be bigger than the sum of assets to be received. A transaction fee that must be paid to the network constitutes the difference.

![how_to_send_transaction_opravena-06.png](https://stoplight.io/api/v1/projects/cHJqOjk1MzAz/images/wQhvRxBtqj8)

<!-- theme: info -->

>#### Example
>You want to use 10 BTC from two addresses as input and send it to three recipients, each of whom will receive 3 BTC. The fee is 10 BTC - 9 BTC = 1 BTC.

---
## Sending a Bitcoin transaction

Enough theory; let's send some bitcoin using [this API endpoint](../blockchain/b3A6MjgzNjM1MjM-transfer-assets-between-adresses).

**Request example**
```json
curl --request POST \
  --url https://api-eu1.tatum.io/v4/blockchain/ETH/transaction \
  --header 'Content-Type: application/json' \
  --header 'env: ' \
  --header 'x-api-key: ' \
  --data '{
  "sender": {
    "privateKey": "0x05e150c73f1920ec14caa1e0b6aa09940899678051a78542840c2668ce5080c2",
    "secret": "snSFTHdvSYQKKkYntvEt8cnmZuPJB",
    "address": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
    "utxoIndex": 0
  },
  "receiver": [
    {
      "address": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
      "note": "Hello there",
      "chainId": "ETH",
      "token": {
        "address": "2MsM67NLa71fHvTUBqNENW15P68nHB2vVXb",
        "symbol": "BTC",
        "amount": 0
      }
    }
  ],
  "fee": {
    "price": 50,
    "limit": 50000,
    "currency": "CELO"
  }
}'
```
**Response example**
```json
{
  "signatureId": "26d3883e-4e17-48b3-a0ee-09a3e484ac83"
}
```
<!-- theme: warning -->
> #### Security
>
> Blockchain transactions are signed using a private key via API, which is not a secure way of signing transactions. Your private keys and mnemonics should never leave your security perimeter. To correctly and securely sign a transaction, you can use the [Tatum CLI](https://github.com/tatumio/tatum-cli); a specific language library like [Tatum JS](https://github.com/tatumio/tatum-js); local [middleware API](https://github.com/tatumio/tatum-middleware); or our complex key management system, [Tatum KMS](https://github.com/tatumio/tatum-kms).
---

## Getting transaction details

You can check the result via the [Get transaction by hash or address](../blockchain/b3A6MjgzNjM1MTY-get-transaction-by-hash-or-address) 
 endpoint.
 
**Request example**

```json
curl --request GET \
  --url https://api-eu1.tatum.io/v4/blockchain/chainId/transaction/id \
  --header 'Content-Type: application/json' \
  --header 'env: ' \
  --header 'x-api-key: '
```

**Response example**

```json
[
  {
    "block": {
      "hash": "000ca231a439a5c0a86a5a5dd6dc1918a8",
      "height": 500124
    },
    "hash": 1,
    "index": "d1c75a84e4bdf0dd9bf1bcd0ce4fb25f89e2ed3c5e9574dbca2760b52c428717",
    "fee": {
      "price": 50,
      "limit": 50000,
      "currency": "CELO"
    },
    "addressFrom": "2MsM67NLa71fHvTUBqNENW15P68nHB2vVXb",
    "addressTo": "2MsM67NLa71fHvTUBqNENW15P68nHB2vVXb",
    "token": {
      "contractAddress": "2MsM67NLa71fHvTUBqNENW15P68nHB2vVXb",
      "currencySymbol": "BTC",
      "amount": 0
    },
    "status": "true",
    "timestamp": "2021-21-10 07:36:05"
  }
]
```
