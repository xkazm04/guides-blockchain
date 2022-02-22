# How to store metadata to IPFS and include it in an NFT

*Use Tatum's native IPFS integration and free storage to easily upload NFT metadata.*

---

Making NFTs with Tatum is just a matter of a few simple API calls. One very important step in the process is uploading your metadata (image, video, audio file, etc.) to IPFS and including it in your NFT. In this guide, you’ll learn how to do exactly that.

---

## Why IPFS?

[The InterPlanetary File System (IPFS)](https://ipfs.io/) is a distributed file storage system that is perfect for storing NFT metadata. It ensures long-term data resilience and security, and now that Tatum has partnered with [NFT.storage](https://nft.storage/), you can store your files on it for free!

---

## How to do it

We’ve built native IPFS integration into our platform, so you just need a couple of API calls to store your metadata.

1. First, upload your metadata (image, video, audio file, etc.) to IPFS using the [following API call](../docs/storage/b3A6Mjc2Mjk0NzI):

```SDK
import {ipfsGet, ipfsGet} from '@tatumio/tatum';
/**
 * Gets data from the IPFS
 * @param id - IPFS CID of the file
 */
  const ipfsId = await ipfsGet('QmXJJ6UF5WkF4WTJvsdhiA1etGwBLfpva7Vr9AudGMe3pj');
 
/**
 * Upload file to the IPFS storage.
 * @param file - data buffer of the file. Content Type: multipart/form-data
 * @returns ipfsHash - IPFS hash of the file
 */
  const ipfsHash = await ipfsUpload('logo.jpg', 'fileName');
```
```REST API call
curl --request POST \
  --url https://api-eu1.tatum.io/v3/ipfs \
  --header 'content-type: multipart/form-data' \
  --header 'x-api-key: REPLACE_KEY_VALUE' \
  -F upload=@local_path_to_your_file_e_q_test-356.jpg
```
2. A successful call will return a response that includes an IPFS hash.

```Response
{
  "ipfsHash": "bafybeihrumg5hfzqj6x47q63azflcpf6nkgcvhzzm6f6nhic2eg6uvozlq/test-356.jpg"
}
```

3. Using this hash, create a JSON metadata scheme in your development environment and save it. The required fields are:
- Name of NFT
- Description of NFT
- IPFS hash

4. Here is an example [JSON metadata scheme](https://gateway.pinata.cloud/ipfs/bafybeidi7xixphrxar6humruz4mn6ul7nzmres7j4triakpfabiezll4ti/metadata.json) which is connected to [this NFT](https://testnets.opensea.io/assets/0x0ff74c54dcca05ba1c0a8c3c00d96b53296fc220/559114) for reference:

```Response
{
  "name": "Lorem Ipsum Dolor Test",
  "description": "Friendly Lorem OpenSea Creature that enjoys long swims in the ocean.",
  "image": "ipfs://bafybeihrumg5hfzqj6x47q63azflcpf6nkgcvhzzm6f6nhic2eg6uvozlq/test-356.jpg"
}
```

5. Now, upload your saved JSON metadata scheme to IPFS using the [same API call as above](../docs/storage/b3A6Mjc2Mjk0NzI).
6. Again, a successful upload will return an IPFS hash in the response.
7. Include this hash in the “URL” field of the [Mint NFT API call](../docs/v3smartContracts/b3A6MzA3NjA4OTQ) when you mint a new NFT.

```SDK
import {Currency, mintNFTWithUri} from '@tatumio/tatum';
const transactionHash = await mintNFTWithUri(false, {
    to: '0x687422eEA2cB73B5d3e242bA5456b782919AFc85',
    url: 'ipfs://bafybeidi7xixphrxar6humruz4mn6ul7nzmres7j4triakpfabiezll4ti/metadata.json',
    tokenId: '100000',
    chain: Currency.MATIC,
    contractAddress: '0x687422eEA2cB73B5d3e242bA5456b782919AFc85',
    fromPrivateKey: '0x05e150c73f1920ec14caa1e0b6aa09940899678051a78542840c2668ce5080c2'
});
```
```REST API call with Private key
curl --request POST \
  --url https://api-eu1.tatum.io/v3/nft/mint \
  --header 'content-type: application/json' \
  --header 'x-api-key: REPLACE_KEY_VALUE' \
  --data '{
      "chain":"MATIC",
      "tokenId":"100000",
      "to":"0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
      "contractAddress":"0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
      "url":"ipfs://bafybeidi7xixphrxar6humruz4mn6ul7nzmres7j4triakpfabiezll4ti/metadata.json",
      "fromPrivateKey":"0x05e150c73f1920ec14caa1e0b6aa09940899678051a78542840c2668ce5080c2"
  }'
```
```REST API call with KMS
curl --request POST \
  --url https://api-eu1.tatum.io/v3/nft/mint \
  --header 'content-type: application/json' \
  --header 'x-api-key: REPLACE_KEY_VALUE' \
  --data '{
      "chain":"MATIC",
      "tokenId":"100000",
      "to":"0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
      "contractAddress":"0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
      "url":"ipfs://bafybeidi7xixphrxar6humruz4mn6ul7nzmres7j4triakpfabiezll4ti/metadata.json",
      "index": 0,
      "signatureId": "26d3883e-4e17-48b3-a0ee-09a3e484ac83",
      "nonce": 0
  }'
```

And there you have it! You've successfully uploaded your metadata to IPFS, created a JSON metadata scheme containing the URL of the file, uploaded the scheme, and minted an NFT connected to it.



