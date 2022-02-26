# How to create a blockchain wallet

*Generating a Bitcoin wallet in three API calls*


#### Getting started

<div class="toolbar-note">
To use Tatum, you have to sign up for an API key in the [Dashboard](https://dashboard.tatum.io/login). This API Key is used in every HTTP request to Tatum as an HTTP header.
</div>

Do you know that many developers do not know how to handle private keys securely and store them somewhere in their infrastructure? For 2 million users, they store 2 million private keys. This is a really bad way to do it. Let's look at how to create a wallet, a much more secure and efficient way of handling private keys.

Today, you will be working with Bitcoin to generate a BIP44 compatible wallet. The most important feature of this type of wallet is that you only need to store one secret mnemonic for more than 2 billion private keys and addresses.

<div class="toolbar-note">
[BIP 44](http://aaronjaramillo.org/bip-44-hierarchical-deterministic-wallets) is originally a Bitcoin proposal to generate multiple addresses for different blockchains from a single mnemonic seed.
</div>

## Creating a blockchain wallet

To generate a Bitcoin wallet, you need to call a request to the [Bitcoin/wallet](https://developer.tatum.io/rest/blockchain/generate-bitcoin-wallet) endpoint.

<div class='tabbed-code-blocks'>
```SDK
// https://github.com/tatumio/tatum-js
const {generateWallet, Currency} = require("@tatumio/tatum");
const btcWallet = async generateWallet(Currency.BTC, false);
console.log(btcWallet);
```
```REST API call
curl --request GET \
  --url 'https://api-eu1.tatum.io/v3/bitcoin/wallet' \
  --header 'x-api-key: YOUR_API_KEY'
```
</div>

The response contains two fields - a mnemonic and extended public key (xpub).

**Response:**
```json
{
  "mnemonic": "zebra parent avocado margin ready heart space orchard police junior travel today bag action rough system novel large rain detail route spare add mail",
  "xpub": "xpub6FPGLmppWEemTJ56aq6wcSkjeZN4iEw1CBvQzkusgbJpqyoiPPJASLpbduzKrNF54i348moHyoVGkyz1H2TC3iEPLfacjPFEfTENkD6YzzZ"
}
```

#### Working with private keys

<div class="toolbar-note">
In this guide, we are using our API to create blockchain wallets and private keys, which is fine for testing and demo purposes.
</div>

However, for production use, your private keys and mnemonics should never leave your security perimeter. To correctly and securely generate wallets and work with private keys, we recommend using [Tatum CLI](https://github.com/tatumio/tatum-cli) from the command line or our complex key management system, [Tatum KMS](https://github.com/tatumio/tatum-kms).

---

## Generating an address from the wallet

A mnemonic is a 24 word-long string that you can understand as a password to our wallet. Every address and the private key can be generated from a mnemonic. It is sensitive information and should not be revealed. Otherwise, you risk losing your funds.

"Xpub" is an abbreviation for "extended public key." It is essentially a generator of addresses. Only publicly visible addresses can be generated with an xpub, not private keys.

Every wallet generated from Tatum holds more than 2 billion addresses. To generate a specific address, you can use an endpoint to generate the address - [Bitcoin/address](https://developer.tatum.io/rest/blockchain/generate-bitcoin-deposit-address-from-extended-public-key), where you enter your xpub and the index of the address you want to generate.

<div class='tabbed-code-blocks'>
```SDK
// Replace <<xpub>> with xpub you've got from step 2
const {generateAddressFromXPub, Currency} = require("@tatumio/tatum");

const btcAddress = generateAddressFromXPub(Currency.BTC, false, "<<xpub>>", 1);
console.log(btcAddress);
```
```REST API call
curl --request GET \
  --url https://api-eu1.tatum.io/v3/bitcoin/address/xpub6FPGLmppWEemTJ56aq6wcSkjeZN4iEw1CBvQzkusgbJpqyoiPPJASLpbduzKrNF54i348moHyoVGkyz1H2TC3iEPLfacjPFEfTENkD6YzzZ/1 \
  --header 'x-api-key: YOUR_API_KEY'
```
</div>
The response contains the address for index number 1.

**Response:**
```json
{
  "address": "1BZWZ89vWpb8htzpBmPMwcWPxt5f6usdZN"
}
```

---

## Generating a private key from the wallet

To [generate a private key](https://developer.tatum.io/rest/blockchain/generate-bitcoin-private-key), the same mechanism is used. The only difference is that in this case, private keys are from the wallet's mnemonic, not the xpub.

<div class='tabbed-code-blocks'>
```SDK
// Replace <<mnemonic>> with xpub you've got from step 2
const {generatePrivateKeyFromMnemonic, Currency} = require("@tatumio/tatum");

const btcPrivateKey = await generatePrivateKeyFromMnemonic(Currency.BTC, false, "<<mnemonic>>", 1);
console.log(btcPrivateKey);
```
```REST API call
curl --request POST \
  --url https://api-eu1.tatum.io/v3/bitcoin/wallet/priv \
  --header 'content-type: application/json' \
  --header 'x-api-key: YOUR_API_KEY' \
  --data '{"index":1,"mnemonic":"zebra parent avocado margin ready heart space orchard police junior travel today bag action rough system novel large rain detail route spare add mail"}'
```
</div>

When you execute a https://api-eu1.tatum.io/v3/bitcoin/wallet/priv endpoint request with the mnemonic and index entered as the request body, you can see the generated private key for index number 1.

**Response:**
```json
{
  "key": "L2A9CvxKQ26LudMRN1air5JpRNZU6Wadno1shxkfq3X3kLtTVQ9z"
}
```

That was super easy. You made only three calls to Tatum, and you were able to generate a wallet and receive bitcoin at the blockchain addresses. The same principle applies to other blockchains supported by Tatum, such as Ethereum, Litecoin, Bitcoin Cash, and others...

To find out more about the API calls we have just used, [visit our API reference](https://developer.tatum.io/rest/blockchain).

