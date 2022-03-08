# How to create NFTs on Solana

*Mint and transfer NFTs on Solana with just 2 API call*

---

[Solana](https://solana.com/) is one of the fastest and most in-demand blockchains for creating NFTs, and now you can mint them with just 1 API call in Tatum. Since Solana’s architecture is significantly different than those of Ethereum-compatible blockchains, the way it handles minting NFTs is also different. However, within Tatum, there are just a few minor workflow changes that you need to keep in mind.

With most blockchains, you need to deploy a smart contract from which you will mint NFTs. However, Solana uses the [Metaplex Protocol](https://www.metaplex.com/) - a smart contract and metadata standard for creating and working with NFTs. Tatum’s API endpoints leverage the Metaplex protocol, and as a result, you do not need to deploy your own smart contract to mint NFTs.

Usually, the JSON metadata scheme that points to the metadata included in your NFT must be stored on [IPFS](https://ipfs.io/). However, on Solana, this metadata is stored on the blockchain itself, so you do not need to create a JSON metadata scheme and upload it to IPFS. All you need to do is include a URL that points to the metadata in the uri field of the Mint NFT API call below.

We’ll get into a couple of other small differences specific to the Mint and Transfer NFT API endpoints below, but for now, let’s get minting, shall we?

---

## Minting NFTs

To mint an NFT on Solana, you can use the same Mint NFT endpoint in Tatum that you would with any other blockchain. Most of the parameters are exactly the same, but the royalty information in the `metadata` section has a couple of key differences:

The total percentage royalty of each transaction to be paid out to the creators is entered in the `sellerFeeBasisPoints` field of the Mint NFT endpoint. For example, if the `sellerFeeBasisPoints*` value is “10”, this means 10% of the purchase price of the NFT will be transferred to creators each time it is sold. 

The `share` field contains the different percentages of the total `sellerFeeBasisPoints` to be divided among the creators. If there are 2 creators, one with a share value of “30” and one with a `share` value of “70”, 30% and 70% of the total `sellerFeeBasisPoints` royalty will be paid to each respective creator.

Use the following API endpoint to mint an NFT on Solana:

<div class='tabbed-code-blocks'>
```REST API  
curl --location --request POST 'https://api-eu1.tatum.io/v3/nft/mint/' \
--header 'x-api-key: REPLACE_KEY_VALUE' \
--header 'Content-Type: application/json' \
--data-raw '{
   "from": "FykfMwA9WNShzPJbbb9DNXsfgDgS3XZzWiFgrVXfWoPJ",
   "chain": "SOL",
   "fromPrivateKey": "3abc79a31093e4cfa4a724e94a44906cbbc3a32e2f75f985a28616676a5dbaf1de8d82a7e1d0561bb0e1b729c7a9b9b1708cf2803ad0ca928a332587ace391ad",
   "to": "FykfMwA9WNShzPJbbb9DNXsfgDgS3XZzWiFgrVXfWoPJ",
   "metadata": {
       "name": "Tatum API",
       "symbol": "TAPI",
       "sellerFeeBasisPoints": 0,
       "uri": "https://www.nba.com/celtics/sites/celtics/files/tatum_27.jpg",
       "creators": [
           {
               "address": "FykfMwA9WNShzPJbbb9DNXsfgDgS3XZzWiFgrVXfWoPJ",
               "verified": true,
               "share": 100
           }
       ]
   }
}
```
```TatumJS
import { mintNft, SolanaNftMetadata } from '@tatumio/tatum-solana'
  await mintNft(
    {
      from: 'FykfMwA9WNShzPJbbb9DNXsfgDgS3XZzWiFgrVXfWoPJ',
      fromPrivateKey:
        '3abc79a31093e4cfa4a724e94a44906cbbc3a32e2f75f985a28616676a5dbaf1de8d82a7e1d0561bb0e1b729c7a9b9b1708cf2803ad0ca928a332587ace391ad',
      to: 'FykfMwA9WNShzPJbbb9DNXsfgDgS3XZzWiFgrVXfWoPJ',
      metadata: new SolanaNftMetadata('Tatum', 'TTM', 'https://tatum.io/images/logo/logo.svg', 0),
    }
)
```
</div>

The required fields in the API endpoint body are:
- **from** - the address of the creator of the NFT from which the gas fees will be paid 
- **chain** - the blockchain on which the NFT is being minted (in this case, “SOL”) 
- **fromPrivateKey** - the private key of the address from which the NFT is being minted 
- **to** - the recipient address to which the NFT will be minted 
- **metadata** - the data included in the NFT, including the royalty payment information to each co-creator 
- **name** - the name of the NFT 
- **symbol** - the symbol of the NFT 
- **sellerFeeBasisPoints** - the total percentage of the sale price to be paid out to co-creators based on their individual share (see below) 
- **uri** - the location of the metadata (image, video file, audio file, etc.). Usually, metadata json schemes are stored on IPFS. However, on Solana, the metadata scheme is stored on the Solana blockchain itself. 
- **creators** - information about each co-creator who will receive a percentage royalty share each time the NFT is sold address - the account address of the creator 
- **verified** - whether or not the account has been verified 
- **share** - the percentage share of the total sellerFeeBasisPoints percentage designated above to be paid out to each individual creator

The response will contain a transaction ID, and two additional parameters that are specific to Solana.

- **txId** - the transaction ID. This ID can be used to get information about the transaction using the Get transaction endpoint or within the Solana Explorer. 
- **nftAddress** - the address of the NFT on the blockchain. This is the address that is used to transfer the NFT below in the contractAddress field of the Transfer NFT API endpoint.
- **nftAccountAddress** - The address of the account that owns the NFT. This address is owned by the recipient address, and a new nftAccountAddress is created each time a new NFT is minted. The private key of the nftAccountAddress is the same as the private key of the account address it is owned by. Each time an NFT is transferred on Solana, a new nftAccountAddress is created to receive the token under the recipient’s account address.

```Response
{
    "txId": "5GAwhRcMR7h5Dx71KCQax4CAcCFA6FcrVE5fwNedq7wXDYS1qpZaGd4Bj2zh8dCUfAz7fqVSXhdZzXRigsNoTZsb",
    "nftAddress": "7dQWANaodDyttJNz3seaXoAe6VA8cLpPV1bM4cPGuNhG",
    "nftAccountAddress": "3z2bPwKzfTAFDKPNKTM4vvffBSDvcBvkuRJeME5iALs3"
}
```

## Transferring NFTs

The way the Solana blockchain works with minting and transferring NFTs uses **nftAccountAddresses**. For practical use of Tatum’s API, this makes very little difference, but it is important to know that for transferring NFTs, the **nftAddress** of the NFT is used, NOT the **nftAccountAddress**.

Use the following API endpoint to transfer an NFT on Solana:

<div class='tabbed-code-blocks'>
```REST API
curl --request POST \
  --url https://api-eu1.tatum.io/v3/nft/transaction \
  --header 'content-type: application/json' \
  --header 'x-api-key: REPLACE_KEY_VALUE' \
  --data '{
      "chain": "SOL",
      "from": "FykfMwA9WNShzPJbbb9DNXsfgDgS3XZzWiFgrVXfWoPJ",
      "to": "FykfMwA9WNShzPJbbb9DNXsfgDgS3XZzWiFgrVXfWoPJ",
      "contractAddress": "7dQWANaodDyttJNz3seaXoAe6VA8cLpPV1bM4cPGuNhG",
      "fromPrivateKey": "3abc79a31093e4cfa4a724e94a44906cbbc3a32e2f75f985a28616676a5dbaf1de8d82a7e1d0561bb0e1b729c7a9b9b1708cf2803ad0ca928a332587ace391ad"
}
```
```TatumJs
import { mintNft, SolanaNftMetadata } from '@tatumio/tatum-solana'
console.log(
  await transferNft(
    {
      from: 'FykfMwA9WNShzPJbbb9DNXsfgDgS3XZzWiFgrVXfWoPJ',
      fromPrivateKey:
        '3abc79a31093e4cfa4a724e94a44906cbbc3a32e2f75f985a28616676a5dbaf1de8d82a7e1d0561bb0e1b729c7a9b9b1708cf2803ad0ca928a332587ace391ad',
      to: 'FZAS4mtPvswgVxbpc117SqfNgCDLTCtk5CoeAtt58FWU',
      contractAddress: '7dQWANaodDyttJNz3seaXoAe6VA8cLpPV1bM4cPGuNhG'
    }
  )
)
```
</div>

The required parameters for the API endpoint body are:
- **chain** - the chain on which you are transferring the NFT (in this case, “SOL”) from - the sender’s account address 
- **to** - the recipient’s account address 
- **contractAddress** - the address of the NFT from the nftAddress field in the response to the Mint NFT call above. 
- **fromPrivateKey** - the private key of the sender’s address
The response will contain a transaction ID:

```Response
{
    "txId": "c83f8818db43d9ba4accfe454aa44fc33123d47a4f89d47b314d6748eb0e9bc9"
}
```

And that’s it! Two API calls and you can start minting and transferring NFTs on Solana like a pro. To see the rest of the API endpoints available to you for Solana in Tatum, check out our API documentation.


