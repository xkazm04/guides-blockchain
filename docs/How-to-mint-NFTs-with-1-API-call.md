# How to mint NFTs with 1 API call

*Mint NFTs without coding or deploying smart contracts, sending private keys, or holding crypto to pay for gas fees.*

---

In Tatum, we’ve already made it super easy to create your own NFTs on multiple blockchains without having to learn Solidity or create your own smart contracts. Anyone can deploy our prebuilt smart contracts with just a few lines of code and mint NFTs in minutes.
However, there are still several potential obstacles to consider:
1. You need to buy and hold crypto to pay for the gas fees, which can lead to regulatory difficulties for enterprises. Some banks will even freeze your account if you try to buy crypto.
2. You need to hold crypto on every blockchain they would like to mint NFTs on, which can be a logistical challenge when minting NFTs on a mass scale. 
3. You need to deploy smart contracts to mint NFTs on each blockchain, which can be both complicated and expensive. 
4. You need to deal with security issues with private keys to mint NFTs.
**If you're facing any of these issues, you'll be glad to know we've taken things a step further to allow you to mint NFTs even more easily with Tatum.**

---

## NFT Express

To make things as simple and straightforward as possible, we’ve created a feature called **NFT Express**. Here’s how it works:
- You can mint as many NFTs as you want on 5 blockchains using Tatum’s smart contracts. 
- Your gas fees are deducted from the monthly credit allowance of your paid plan.
You don’t need to deploy your own smart contracts, worry about holding crypto, work with private keys, or keep addresses topped up with crypto to pay for gas fees on each blockchain. With NFT Express, you can just mint NFTs with one simple API call, on the following blockchains:
- **Ethereum** 
- **Polygon** 
- **Binance Smart Chain (BSC)**
- **Celo** 
- **Harmony**

---

## How to mint NFTs with 1 API call

With NFT Express, you can use the same simple API endpoint you would to mint NFTs for your own smart contracts with Tatum. **The only difference is you no longer need to enter a private key, smart contract address, or token ID.** 

<!-- theme: warning -->
> 
>**NFT Express only works with paid Tatum plans.** This is because the gas fees to mint NFTs are deducted from your monthly credit allowance. With a free Tatum plan, you would have to pay for the gas fees to mint NFTs from a blockchain address holding crypto. To create NFTs this way, please refer to our full guide on creating NFTs.

1. First, you'll need to get an API key with a **paid Tatum plan**. Please visit the [Tatum Dashboard](https://dashboard.tatum.io/login) to sign up for one. To mint NFTs on Celo and Harmony, a **$9/month Start Plan** should be enough to mint plenty of NFTs. For Polygon and BSC, we'd recommend a **$49/month Basic Plan**. Since gas fees on Ethereum are very high, please contact us to get an **Enterprise Plan** that will suit your needs.
2. Next, enter the API key in the environment variables in your IDE or into the header of the direct API request. To use JavaScript, please download and install the [Tatum JavaScript SDK](https://tatum.io/tatum-js.html).
3. And now, all you need is to send the following API request to mint an NFT:

```SDK
const transactionHash = await mintNFTWithUri(false, {
    chain: Currency.CELO
    to: '0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea',
    url: 'https://my_token_data.com',
    feeCurrency: Currency.CELO
});
```
```REST API call
curl --location --request POST 'https://api-eu1.tatum.io/v3/nft/mint' \
--header 'x-api-key: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
   "chain": "CELO",
   "to": "0x8cb76aed9c5e336ef961265c6079c14e9cd3d2ea",
   "url": "https://my_token_data.com",
   "feeCurrency": "CELO"
}'
```

There are just three necessary parameters for the request:
- **chain** - The blockchain on which the NFT will be minted. In the example above, we have used Celo (CELO), but you can also mint NFTs on Ethereum (ETH), Polygon (MATIC), Harmony (ONE), and Binance Smart Chain (BSC).
- **to** - The recipient’s address to which the NFT will be sent
- **url** - The URL of the metadata (image, audio, video, etc.) to be included in the NFT. For more on how to upload metadata to IPFS for free using Tatum, please refer to our full guide.

<!-- theme: info -->
> The feeCurrency parameter is only necessary when using Celo, with any other blockchain it can be omitted. 

---

## And that’s it!

With just 1 API call, you can instantly mint an NFT on Celo to the recipient’s address. 

The **private key** and **contract address** will by default be set to Tatum’s pre-deployed smart contract and our private key on the blockchain you have specified, and the **token ID** will be generated automatically. The gas fees will be deducted as credits from your monthly credit allowance from the paid Tatum plan you have selected.

You can mint as many NFTs as you want and you don’t need to worry about any of the tedious backend logic of setting up your own NFT solution. And what's more, you'll never have to worry about whether or not your bank will allow you to purchase crypto from your account or worry about the technicalities of buying and trading crypto. 

Happy minting!




















































