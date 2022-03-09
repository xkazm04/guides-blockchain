# How to set up webhook notifications on Solana

![1_rZFW-_p03HoOBXdjjYKRFw.png](https://stoplight.io/api/v1/projects/cHJqOjExNjE4OQ/images/quu02rTcS4E)

[Solana](https://solana.com/) is one of the fastest and most in-demand blockchains as of late, and Tatum’s framework helps developers build apps on it more quickly and easily than ever. Now, with Tatum, you can instantly receive real-time notifications for any account address, any type of transaction, and any type of token on Solana.

## Why use Solana?

A lot of blockchains have been hyped as the next big thing, but Solana is truly delivering. With 400 millisecond block times, dirt-cheap gas fees, and super-scalable architecture, Solana is a developer’s dream.

As opposed to older blockchains like Ethereum, which can only perform 15 transactions per second (tps), Solana can handle up to 65,000 tps. This means gas fees are low and building scalable apps for thousands or millions of end-users is a breeze. Solana also maintains a single global state, which means you don’t have to deal with fragmented L2 solutions or sharded chains. It’s a truly next-gen network for building super-scalable apps.

## Why notify?

Your app’s end-users are used to push notifications, and to make your Solana app as user-friendly as possible, you’ll need to implement real-time alerts.

Nobody wants to have to hit “refresh” in their PayPal account to know if they’ve received funds, and your app’s end-users shouldn’t have to either. They will want to be notified instantly when they send or receive SOL, SPL tokens, or NFTs. To enable this functionality, you’ll need to set up webhook notifications.

Until now, there was no easy way to do it on Solana. No other service or development platform offers Solana webhooks, and building the implementation from scratch yourself would be very difficult. But now, with Tatum, you can monitor any address and receive real-time notifications for any kind of asset and any kind of transaction on Solana in minutes.

## How to set up webhook notifications on Solana

1. First, sign up for an API key in the [Tatum dashboard](https://dashboard.tatum.io/login).
2. Next, paste your API key into your environment variables in your IDE if you’re using the [Tatum JavaScript SDK](https://tatum.io/tatum-js.html), or [into your environment in Postman](https://blog.tatum.io/getting-started-with-tatum-in-postman-71487f56e9c9). If you’re using direct REST API calls, you will need to include your API key in the header of each request.
3. Create a Solana wallet using this [API request](https://docs.tatum.io/rest/blockchain/generate-solana-wallet). The following code snippet will generate a wallet using Tatum JS. If you already have a Solana wallet, or would like to monitor any other Solana address, you can skip this step.

```json
import {generateWallet} from '@tatumio/tatum-solana'
const wallet = await generateWallet()
```
4. Finally,[ create a subscription](https://docs.tatum.io/rest/subscriptions/create-new-subscription) to be notified about any events at the Solana address of the wallet you have just created. The following code will create a subscription using Tatum JS.

```json
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

The required parameters are:
- **address** — the blockchain address for which the subscription will be created. This can be any address on the blockchain.
- **chain** — the blockchain on which you are creating the subscription
- **url** — the URL to which the notification will be sent
- **type** — the type of the subscription. To be notified about all transactions, the type should set to `ADDRESS_TRANSACTION`.

Now, any time any token is sent to or from the address you have specified, you will receive a real-time notification at the webhook listener URL you have specified in the call above.

For more, check out our [full guide](https://docs.tatum.io/guides/blockchain/how-to-monitor-transactions-at-any-address) on monitoring blockchain addresses.

## Welcome aboard!

Now that you’re all ready to set up notifications on Solana, we’d highly recommend you check out our [docs](https://docs.tatum.io/) to learn more about our 300+ other developer features.

Also, be sure to hop onto our [Discord](https://discord.com/invite/4TWtSP3vxU) to join our thriving developer community. It’s the perfect place to discuss all things Tatum and get help with any questions you might have.

Happy coding!























