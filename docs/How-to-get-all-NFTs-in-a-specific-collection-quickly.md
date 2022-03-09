# How to get all NFTs in a specific collection quickly

---

![1_BKXAqUcfF0CrFF873Ghdrg.png](https://stoplight.io/api/v1/projects/cHJqOjExNjE4OQ/images/BEfVne7q6mY)


Lots of developers have been asking us if they can look up all of the NFTs that belong to a specific collection with Tatum, and we’re very happy to say that YES, now you can! All it takes is 1 API call to view all of the NFTs belonging to a collection and all of their associated metadata.

We’ll show you how to do it in just a sec, but first, let’s go over exactly what this means.


## NFTs are hot right now!

Non-fungible tokens, or NFTs, are one-of-a-kind digital collectibles that are created on blockchains. They are created using smart contracts, or a type of executable program on the blockchain. You can think of smart contracts as both the blueprint and factory that creates NFTs.

NFTs have taken the digital world by storm over the last year, with everyone from the NBA to Snoop Dog riding the wave. There are many super popular NFT marketplaces and apps that allow users to create, buy, and sell NFTs, and their user bases are growing exponentially by the day.


## Developers want to build NFT apps fast.

Many developers want to learn how to make NFT apps fast, which is exactly what Tatum is superb for. Our [NFT](https://docs.tatum.io/guides/blockchain/how-to-create-nft-token) and [NFT marketplace](https://docs.tatum.io/tutorials/how-to-create-a-peer-to-peer-nft-marketplace) smart contracts allow you to build NFT apps quickly and easily without any blockchain experience.

And once you’ve built your NFT app, your app’s end-users will want to be able to view the NFTs from within your app. We’ve already got quite a few API endpoints to [view NFT metadata](https://docs.tatum.io/rest/smart-contracts/get-nft-token-metadata), [token](https://docs.tatum.io/rest/smart-contracts/get-nft-tokens-for-address), and [transaction](https://docs.tatum.io/rest/smart-contracts/get-nft-transactions-by-token) information, and you can even allow them to view entire NFT collections with just a few lines of code.


## What’s an NFT collection?

As we mentioned above, NFTs are created from executable programs on the blockchain called “smart contracts.” **An NFT collection simply refers to all of the NFTs that have been minted from a specific smart contract.** Some examples of popular NFT collections include CryptoPunks, Bored Ape Yacht Club, and Doodles.

## How to get NFTs by collection

You can easily get the NFTs from a specific collection with our new [**Get NFTs by collection** endpoint](https://docs.tatum.io/rest/smart-contracts/get-nft-tokens-in-collection). Here’s an example code snippet that will display the Majestic Apes collection on Polygon:

```json
curl --location --request GET 'https://api-us-west1.tatum.io/v3/nft/collection/MATIC/0x489b79f1bE574da9FAaFCF60DCd24823f73023F7?pageSize=50' \
--header 'x-api-key: cf5ad61b-40a3-4ac2-a72c-dab397e25e3e_100'
```
In the call, you simply need to enter:
- **chain** — the name of the blockchain
- **contractAddress** — the address of the smart contract from which the NFTs were minted
- **pageSize** — the number of items included per page in the response (max 50 items per page).
- **offset** (optional) — you can enter an offset value, which means the response will be offset by the number of pages you specify.

The response will contain the token IDs of every token in the collection, as well as all of the metadata associated with the token. Here is one page of data from the response to the example call above:

```json
[
   {
       "tokenId": "9001",
       "metadata": {
           "url": "ipfs://QmUgSR2c21Y6YtjXB4htRmbqNC93tSzf4zgKp8S6n6dnjF/9001.json",
           "metadata": {
               "name": "Majestic Apes #9001",
               "description": "Majestic Apes is a collection of Apes with Traditional Chlothes And Items NFTs. Collect these unique digital collectibles living on the Polygon blockchain.",
               "file_url": "ipfs://QmUCcr8vRA8GdeDuhFz4Hcr3srWSWe9z67aN1ehWkKA29f/9001.png",
               "custom_fields": {
                   "dna": "4d2da11b1fdab0f98cd40c1f27d4f9482a01c4c0",
                   "edition": 9001,
                   "date": 1645101286608,
                   "compiler": "HashLips Art Engine"
               },
               "attributes": [
                   {
                       "trait_type": "Backgrounds",
                       "value": "Blue Gradient"
                   },
                   {
                       "trait_type": "Weapons",
                       "value": "Tanto"
                   },
                   {
                       "trait_type": "Crown Tails",
                       "value": "Golden Crown Tails"
                   },
                   {
                       "trait_type": "Furs",
                       "value": "Bluesky Fur"
                   },
                   {
                       "trait_type": "Bangles",
                       "value": "Floralion Arm Bangle"
                   },
                   {
                       "trait_type": "Armors",
                       "value": "The Eternity Shroud"
                   },
                   {
                       "trait_type": "Necklaces",
                       "value": "The Eye of Etlich Necklace"
                   },
                   {
                       "trait_type": "Amulets",
                       "value": "Onyx Amulet Amulet"
                   },
                   {
                       "trait_type": "Earrings",
                       "value": "Red Pearl Earring"
                   },
                   {
                       "trait_type": "Eyes",
                       "value": "Ugly Eyes"
                   },
                   {
                       "trait_type": "Mouths",
                       "value": "Bitting Mouth"
                   },
                   {
                       "trait_type": "Crowns",
                       "value": "Keurvors Crown"
                   },
                   {
                       "trait_type": "Glasses",
                       "value": "Scouter"
                   },
                   {
                       "trait_type": "Noses",
                       "value": "Nose Color"
                   }
               ]
           },
           "tokenId": "9001"
       }
   }]
```

## And that’s it!

By integrating this API call into your app, you can allow your app’s end-users to instantly view all of the NFTs that belong to any collection on any supported blockchain.

If you’d like to give it a try for yourself, sign up for a free Tatum API key in the [dashboard](https://dashboard.tatum.io/login), and head on over to our [documentation](https://docs.tatum.io/) to find out more about what you can do with Tatum.

If you need any help or have any questions, please hop onto our [Discord](https://discord.com/invite/4TWtSP3vxU) and one of our developers will get back to you asap.

Happy coding!




