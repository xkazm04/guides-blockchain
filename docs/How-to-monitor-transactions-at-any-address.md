# How to monitor transactions at any address 

*Get notified for any type of token, any type of transaction, at any address, on any supported blockchain.*

---

Webhook notifications in Tatum are streamlined into 1 API call: for any blockchain, any token, and for both incoming and outgoing transactions. This means NFTs, ERC-20s and ERC-1155s (or equivalent for the given blockchain), and native blockchain assets are all covered. You don’t need to set up different webhooks for different types of tokens or types of transactions, you just need to enable them all at once for each address you’d like to monitor.

**Tatum Notification Station** is currently available on:
- **Ethereum**
- **Polygon** 
- **Celo** 
- **Solana**
- **Bitcoin**
- **Litecoin**
- **Bitcoin Cash**
- **Dogecoin**

---

## How to set up webhook notifications for blockchain addresses

In Tatum, webhook notifications work as subscriptions to blockchain addresses. When you set up a subscription, the address specified will be monitored for any transactions for any type of token. Every time an incoming or outgoing transaction occurs, a webhook notification will be fired to the webhook service URL you have specified.

### Create a subscription

To create a subscription to monitor a blockchain address, you just need to send the following API call:

```JavaScript
import { createNewSubscription, CreateSubscription, SubscriptionType } from '@tatumio/tatum'

export const createAddressNotificationTransactionSubscription = async () => {
  const data: CreateSubscription = {
    "type": SubscriptionType.ADDRESS_TRANSACTION,
    "attr": {
        "address": "FykfMwA9WNShzPJbbb9DNXsfgDgS3XZzWiFgrVXfWoPJ",
        "chain": "SOL",
        "url": "https://webhook.site/d7eb36b5-a6ed-458a-b3be-836a6b7f0918"
    }
  }
  return await createNewSubscription(data)
}

```
```cURL
curl --location --request POST 'https://api-eu1.tatum.io/v3/subscription' \
--header 'x-api-key: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "attr": {
        "address": "FykfMwA9WNShzPJbbb9DNXsfgDgS3XZzWiFgrVXfWoPJ",
        "chain": "SOL",
        "url": "https://webhook.site/d7eb36b5-a6ed-458a-b3be-836a6b7f0918"
    },
    "type": "ADDRESS_TRANSACTION"
}'
```

The required parameters for the body of the call are:
- **address** - the blockchain address for which the subscription will be created 
- **chain** - the blockchain on which you are creating the subscription 
- **url** - the URL to which the notification will be sent 
- **type** - the type of the subscription. To be notified about all transactions, the type should be set to “ADDRESS_TRANSACTION”.

A successful request will return a transaction ID.

**Response**:

```json
{
  "id": "6204ef187d4be442c4765501"
}
```

Now, any incoming and outgoing transactions will trigger webhook notifications.

### Types of webhook responses

HTTP POST webhook responses will differ slightly for native assets and other tokens.

**Native asset transfer response:**

```json
{
  "address": "FykfMwA9WNShzPJbbb9DNXsfgDgS3XZzWiFgrVXfWoPJ",
  "type": "native",
  "txId": "2rdy3YCZHSwvpWtuDom1d4Jjy5UU9STLxF3ffXau6GToReDkfw8wEgX541fvzvh6btVC5D8iNapcKTXfPsoDBk7A",
  "blockNumber": 110827114,
  "asset": "SOL",
  "amount": -0.00204428
}
```

The parameters included are:
- **address** - the address from which the assets were sent or to which they were received
- **type** -  the type of asset that was transferred
- **txId** - the ID of the transaction
- **blockNumber** - the block in which the transaction was recorded
- **asset** - the name of the native blockchain asset
- **amount** - how much of the asset was transferred. Positive amounts represent incoming transactions, negative amounts represent outgoing transactions.

**Token transfer response:**

```json
{
  "address": "FykfMwA9WNShzPJbbb9DNXsfgDgS3XZzWiFgrVXfWoPJ",
  "txId": "2rdy3YCZHSwvpWtuDom1d4Jjy5UU9STLxF3ffXau6GToReDkfw8wEgX541fvzvh6btVC5D8iNapcKTXfPsoDBk7A",
  "blockNumber": 110827114,
  "asset": "3gUeeR3BfVhukYJMwtHownRtRkGcf1bvwiV8TbKMZBVz",
  "amount": 1,
  "type": "token",
  "counterAddress": "ET7gwtm6QZfjRQboBLjxZ4PSHDAH7y6AAiAJE8sPaWvv"
}
```

The parameters included are:
- **address** - the address from which the assets were sent or to which they were received
- **txId** - the ID of the transaction
- **blockNumber** - the block in which the transaction was recorded
- **asset** - the address of the smart contract from which the asset was created
- **amount** - how much of the asset was transferred. Positive amounts represent incoming transactions, negative amounts represent outgoing transactions.
- **type** - the type of asset that was transferred
counterAddress - the address from which the token was transferred (for incoming transactions) or to which the token was transferred (for outgoing transactions).

---

## Get notified!

So there you have it. Just 1 API call to setup notifications for any type of token on any supported blockchain. We're going to be adding A LOT of new supported blockchains to our **Notification Station** API, so please check back frequently, or follow us on [LinkedIn](https://www.linkedin.com/company/tatumio) or [Twitter](https://twitter.com/tatum_io) to stay up to date.


























