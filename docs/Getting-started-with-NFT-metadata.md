# Getting started with NFT metadata

The term "metadata" usually refers to data that contains information about other data, such as features of a specific transaction, image, video, audio and more.

Storing it on IPFS can be expensive, and integrating it into your app or marketplace is difficult. Tatum offers free IPFS storage and native IPFS integration which makes it super easy to handle your metadata.

## NFT.Storage

With native IPFS integration already live in Tatum, we’re taking things one step further by offering NFT storage with NFT.Storage completely for free! Now you can store your images, videos, audio files, or any other metadata you would like to include in your NFT at no cost whatsoever.

Through the NFT.Storage service, your files are stored on IPFS and Filecoin, two powerful distributed file-storage networks that offer long-term, immutable storage of digital assets. IPFS enables content addressing, allowing NFTs to immutably reference files split into chunks across distributed networks and ensuring long-term, provable data resilience, even if NFT.storage is attacked or otherwise compromised. Files are resistant to tampering and censorship, so you can rest assured that your metadata will live on.

## Usage

It should come as no surprise that integrating free NFT storage into your apps is super easy with Tatum. Storing, retrieving, and deleting your data is just a matter of three simple API calls.
First off, to store metadata, use the following API call:

```json
curl --request POST \
  --url https://api-eu1.tatum.io/v3/ipfs \
  --header 'content-type: multipart/form-data' \
  --header 'x-api-key: REPLACE_KEY_VALUE'
  -F 'file=@\"C:/myfile.txt\"'
```

To retrieve it, use this one:
```json
curl --request GET \
  --url https://api-eu1.tatum.io/v3/ipfs \
  --header 'x-api-key: REPLACE_KEY_VALUE'

```

And that’s all there is to it! These API calls will integrate seamlessly with your app or NFT marketplace backend, with no need for external tools. If you’re using our SDK, the same commands are available to you in our various libraries with single lines of code.

If you have any questions or need any help, please drop us a line on our Discord channel or the Tatum subreddit, and we’ll get back to you as quickly as we can.
