# How to send a Bitcoin transaction

---

To send a Bitcoin transaction, you have two options with Tatum:
- You can enter a list of blockchain addresses from which the assets will be sent.
- You can enter specific Unspent Transaction Outputs (UTXO) which will be used to send the assets.

![how_to_send_transaction_opravena-07.png](https://stoplight.io/api/v1/projects/cHJqOjExNjE4OQ/images/wtzsmVq0MxE)


<div class="toolbar-note">
**More about Bitcoin**
The Bitcoin blockchain is not account-based but transaction-based. You operate with a list of previously received transactions and the assets within them. This means that you can use multiple transactions in one transaction as a source of your assets, not just one specific address.
To find out more about how Bitcoin works, check out [this article](https://www.investopedia.com/terms/u/utxo.asp).
</div>

When you enter a list of addresses, all incoming transactions that have not previously been used are used as a source. When you enter a list of UTXOs, only specific unspent transactions that belong to you are used.

![how_to_send_transaction_opravena-04.png](https://stoplight.io/api/v1/projects/cHJqOjExNjE4OQ/images/R0Q7CooFpf8)

<div class="toolbar-warning">
**Bitcoin transaction inputs**
Please be aware that all of the assets that belong to the specific transaction are used as input, not only a fraction of them. To learn more about Bitcoin transactions, check out [this article](https://en.bitcoin.it/wiki/Transaction).
</div>

Finally, you have to know how much bitcoin you want to send to whom. You need to define a list of recipient addresses with the specific amounts that each address is to receive. The sum of the assets being sent must be bigger than the sum of assets being received. The difference is the transaction fee that must be paid to the network.

![how_to_send_transaction_opravena-06.png](https://stoplight.io/api/v1/projects/cHJqOjExNjE4OQ/images/TwUCjcHqSxY)

<div class="toolbar-tip">
**Example**
You want to use 10 BTC from 2 addresses as input and send it to 3 recipients, each of which will receive 3 BTC. The fee is 10 BTC - 9 BTC = 1 BTC.
</div>
---

## Sending a Bitcoin transaction

<div class="toolbar-warning">
**Securely signing transactions**
In this guide, we are signing transactions with a private key via API. This is fine for testing and demo purposes, but should not be used for production purposes. 
Your private keys and mnemonics should never leave your security perimeter. To correctly and securely sign a transaction you can use [Tatum JS](https://github.com/tatumio/tatum-js), the local [middleware API](https://github.com/tatumio/tatum-middleware), or our comprehensive key management system, [Tatum KMS](https://github.com/tatumio/tatum-kms). 
</div>

Enough of the theory. Let's send some bitcoin [using this API endpoint](https://docs.tatum.io/rest/blockchain/send-bitcoin-to-blockchain-addresses).

<div class='tabbed-code-blocks'>
```SDK
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
```REST - List of addresses as a source
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
```REST - List of UTXOs as a source
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
</div>

<div class="toolbar-note">
When using Tatum KMS to securely sign transactions, you will use a **signatureId** instead of a **privateKey** parameter in the body of the API request.
</div>

The response will contain a transaction ID, from which we can get information about the transaction.

**Response:**
```json
{
    "txId": "962e4ad3781e7036ff3af6d880744fd3f06131c32d1085254da3cfa77b0e933f"
}
```

---

## Getting transaction details
To get information about the transaction, you can use the transaction ID from the response of the previous request with the [Bitcoin/Get transaction endpoint](https://docs.tatum.io/rest/blockchain/get-transaction-by-hash).

<div class='tabbed-code-blocks'>
```SDK
import {btcGetTransaction} from '@tatumio/tatum';
/**
 * @param hash - transaction hash
 * @returns - transaction detail
 */
const transaction = await abtcGetTransaction('962e4ad3781e7036ff3af6d880744fd3f06131c32d1085254da3cfa77b0e933f');
```
```REST API call
curl --location --request GET 'https://api-eu1.tatum.io/v3/bitcoin/transaction/962e4ad3781e7036ff3af6d880744fd3f06131c32d1085254da3cfa77b0e933f' \
--header 'x-api-key: YOUR_API_KEY' 
```
</div>

The response will contain the details of the transaction you have performed.

**Response:**
```json
{
    "txid": "962e4ad3781e7036ff3af6d880744fd3f06131c32d1085254da3cfa77b0e933f",
    "hash": "962e4ad3781e7036ff3af6d880744fd3f06131c32d1085254da3cfa77b0e933f",
    "size": 374,
    "vsize": 374,
    "version": 2,
    "locktime": 0,
    "vin": [
        {
            "txid": "b2dc763cf75373ad14c984c07846f5425c20ddf34606debb0a148cd770528ef9",
            "scriptSig": {
                "asm": "3045022100e3ae860d446af1199e2799361abf8cc4cae3648a029fed3ad926fb3206ae13770220797889b261ec643c0c1dca38855bd58b6b367a9a98cda43fa220d15a919cc89201 0313f828eb023cde7a5924add3f2bd9c3e228b4e49ec6f550e8e1c5439572c238a",
                "hex": "483045022100e3ae860d446af1199e2799361abf8cc4cae3648a029fed3ad926fb3206ae13770220797889b261ec643c0c1dca38855bd58b6b367a9a98cda43fa220d15a919cc89201210313f828eb023cde7a5924add3f2bd9c3e228b4e49ec6f550e8e1c5439572c238a"
            },
            "sequence": 4294967295,
            "vout": 0
        },
        {
            "txid": "b2dc763cf75373ad14c984c07846f5425c20ddf34606debb0a148cd770528ef9",
            "scriptSig": {
                "asm": "304502210081baa63650e42672c8dde6d5b3b414e037102620b9154168c49abd4a90cc491c02204e5808826457bff853aa68c2f9ddaebf46baa3b77dfaee5c467b66eaa773690e01 0313f828eb023cde7a5924add3f2bd9c3e228b4e49ec6f550e8e1c5439572c238a",
                "hex": "48304502210081baa63650e42672c8dde6d5b3b414e037102620b9154168c49abd4a90cc491c02204e5808826457bff853aa68c2f9ddaebf46baa3b77dfaee5c467b66eaa773690e01210313f828eb023cde7a5924add3f2bd9c3e228b4e49ec6f550e8e1c5439572c238a"
            },
            "sequence": 4294967295,
            "vout": 1
        }
    ],
    "vout": [
        {
            "value": 6e-7,
            "n": 0,
            "scriptPubKey": {
                "asm": "OP_DUP OP_HASH160 0a21a1144fd4f6892c111c00141932608d17d3a9 OP_EQUALVERIFY OP_CHECKSIG",
                "hex": "76a9140a21a1144fd4f6892c111c00141932608d17d3a988ac",
                "type": "PUBKEYHASH",
                "reqSigs": 1,
                "addresses": [
                    "mgSXLa5sJHvBpYTKZ62aW9z2YWQNTJ59Zm"
                ]
            }
        },
        {
            "value": 0.000006,
            "n": 1,
            "scriptPubKey": {
                "asm": "OP_DUP OP_HASH160 0a21a1144fd4f6892c111c00141932608d17d3a9 OP_EQUALVERIFY OP_CHECKSIG",
                "hex": "76a9140a21a1144fd4f6892c111c00141932608d17d3a988ac",
                "type": "PUBKEYHASH",
                "reqSigs": 1,
                "addresses": [
                    "mgSXLa5sJHvBpYTKZ62aW9z2YWQNTJ59Zm"
                ]
            }
        }
    ],
    "blockhash": null,
    "confirmations": 0,
    "time": 1605903228,
    "blocktime": 0,
    "hex": "0200000002f98e5270d78c140abbde0646f3dd205c42f54678c084c914ad7353f73c76dcb2000000006b483045022100e3ae860d446af1199e2799361abf8cc4cae3648a029fed3ad926fb3206ae13770220797889b261ec643c0c1dca38855bd58b6b367a9a98cda43fa220d15a919cc89201210313f828eb023cde7a5924add3f2bd9c3e228b4e49ec6f550e8e1c5439572c238afffffffff98e5270d78c140abbde0646f3dd205c42f54678c084c914ad7353f73c76dcb2010000006b48304502210081baa63650e42672c8dde6d5b3b414e037102620b9154168c49abd4a90cc491c02204e5808826457bff853aa68c2f9ddaebf46baa3b77dfaee5c467b66eaa773690e01210313f828eb023cde7a5924add3f2bd9c3e228b4e49ec6f550e8e1c5439572c238affffffff023c000000000000001976a9140a21a1144fd4f6892c111c00141932608d17d3a988ac58020000000000001976a9140a21a1144fd4f6892c111c00141932608d17d3a988ac00000000"
}
```

And that's it. Super easy. Now you know how to create your wallet, receive assets, and send them somewhere else. To find out more about the endpoints you used today, read our [API Reference](https://docs.tatum.io/rest/blockchain/supported-services).



