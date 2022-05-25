## NFT market
non-fungible token
* Ethereum, Tezos, Polkadot, Solana, Wax, and Tron
* NFT market: Open Sea, Rarible
* Market joiner sometimes can be DAO, pleasrDAO, actions: Christie's

famous things:
crypto punks, beeple NFT , Dapper Labs (nba top shots),  Animoca Brands(HK) Sandbox (land); Sorare (football)
games: Sandbox, Axie Infinity


**ERC-721** IS THE FIRST NFT in ETHEREUM BLOCKCHAIN. It has a game called CryptoKitties

NFT generally uses the Ethereum ERC-721 or ERC-1155

ETH is free to read, but charge write

## ERC-721

* ERC-20 for fungibale token, ERC-721 for NFT
* ECR-721 
  - token ID, each token has a different token ID
  - JSON metadata can be attached to token, like the name, description, image URL etc

## ERC-1155
introduced in 2019
- 帮助方便购买一个collection内的多个NFT，使用一个transaction
- 解决ERC20和ECR0721 incompatability
- 使得NFT 可以和货币token在一个网络内运行

##  NFT usage
blockchain domain names, like xxx.eth instead of long account names
decentraland

## deploy NFT
* metamask
* ipfs desktop
* etf remix https://remix.ethereum.org

## invest in NFT
some forms:
* artist create the physical work and tokenize it with NFT
* digitize the artwork and destroy the physical work
* treat the physical artwork as just a tool for the tokenization process

## how to find early stage NFT projects
1. rarity.tools (early stage projects)
2. https://opensea.io/

https://dappradar.com/nft

https://icy.tools/

https://rarity.tools/

https://moby.gg/

https://beta.traitsniper.com/

https://raritysniffer.com/

https://golom.io/


apricot : for music

## mint NFT
可以带盲盒效果，产生随机的图片，曾家珍藏性

## NFT 制作
× 设计主题背景
× 设计元素，纹身，眼镜，表情
× 随机生成

## NFT 美术馆
https://decentraland.org/

## Wallet
* ledger
* traezor

## NFT minting
1. own the right to tokenize asset
2. prepare the asset, need to prepare the high resolution file for the owner of NFT
3. select the market place to mint the NFT
https://testnets.opensea.io/

Step-by-step NFT minting using IPFS
Now you understand the potential of the NFT market and you want to create your first NFT? Fear no more! Let’s turn digital art into something unique with NFTs!!!
The NFT industry has become the last couple of years a multi-billion dollar industry. It still sometimes a bit hard to navigate it due to the lack of standard but there are some good practices that you can follow.


NFT’s are tokens that have fungibility, meaning that each token is unique and irreplaceable. They generally use the Ethereum ERC-721 standard that was introduced in the Ethereum network in January 2018 and revolutionized an entire industry.

If you are here, you probably want to understand how to create and mint a digital NFT so… let’s get into it!

Using IPFS to store the NFT asset
Now let’s break down the NFT creation into two sections. First, there is the blockchain which handles the minting and bookkeeping of the NFT. Blockchain is excellent to ensure that the metadata of the NFT is immutable and secure by replicating it across thousands of computers/nodes across the world. However, blockchains cannot handle storing large amounts of data because it becomes extremely expensive to replicate large amounts of data across those thousands of nodes. This is where the second section comes into place: storing the NFT data.

To store an image on the Ethereum blockchain would cost probably tens of thousands of dollars. For this reason, the majority of the NFTs data needs to be stored off-chain, and we need to secure this data too.

We can solve this issue with IPFS — The InterPlanetary File System, a peer-to-peer protocol to share and store files. IPFS uses content-addressing to uniquely identify each file in a global namespace that is important for our NFTs to link the NFT metadata to the place where the asset or artwork is stored. IPFS can be seen as more persistent with data pinning when compared to centralized services such as Dropbox or Google Drive.

When creating an NFT, we need to use an URL link that references an asset. This URL will be included in the metadata of the NFT. As you know by now, the NFT data is immutable, and it will live forever in the blockchain, so it’s also essential to find a nice home for the asset or image related to the NFT.


Pinata is one of the well-known IPFS services: https://pinata.cloud/

IPFS works with content identifiers called CID, which references content as hashes. These CID are part of the URL, and the URL won’t change if the content doesn’t change. The image behind a certain CID and respective URL will always be the same image, giving us a certain degree of immutability to the NFT data stored off-chain.\

In the section “Step by step minting” we will see how to create the IFPS CID/URL with Pinata and associate it with the NFT that we will be minting.

Step by step minting NFTs
Disclosure: the NFT market is new at it’s evolving at a very fast pace. I cannot guarantee that by the time you read this, the steps to create an NFT will be the same, nor can I guarantee that this content will be updated.

Step 0 — Ownership of the asset

Before creating an NFT, you need to make sure that you are either the creator or the owner of the asset/art piece you will tokenize. You must have a way to prove that you are the owner or creator.

Step 1 — Prepare the assets

Make sure that you have the file for the image. You can simply tokenize a JPEG/PNG, but it’s better to also have the source file or a good quality file. If you are dealing with digital art, the TIFF, AI/EPS can also be shared in the sale process.

Step 2 — Select the marketplace and authenticate

Now we need to mint the NFT token. The token can be minted directly on OpenSea marketplace when you want to sell it, or you can mint it first on Rarible because on Rarible you can mint a token without actually having to sell it. Up to you to decide.

In this step by step, I assume that you already have your Metamask browser plugin installed and have an Ethereum wallet with some Ether.

On OpenSea, click create and connect your Metamask wallet (check wallets section). Login to your Metamask wallet by clicking the Metamask icon and click connect. Later you will also need Ether to pay the transaction fee to the network during the minting process, but for now, you don’t need to spend money.

Once your wallet is connected, you are authenticated and identified on the website with your public key. It’s similar to when you login using your Google or Facebook identity (also known as SAML/SSO — Single Sign-On).

Step 3 — Start to create the NFT by uploading a file

To create a new item, go ahead and click create. You will have to create a collection, and your NFTs can be part of a collection. You can make more collections in the future — for example, the 2D collection, 3D collection, etc.

Once you have created a collection, you are able to “add new item” to the collection. Click “add new item”. You will be able to upload a file, and you will find a number of formats available: PNG, GIF, WEBP, MP4, MP3 and much more. You can select and upload your file here.

Step 4 — Create an IPFS link

It’s important to highlight that the image itself it’s not stored on the blockchain. What is stored on the blockchain is only metadata about the image, i.e. the hash of the file, name, timestamp and a link to the place where the file is stored. Blockchains are not good to store big files, and the files will always need to be stored somewhere else. In the case of OpenSea, they will take care of storing the image.

If you want the buyer to receive the high-resolution file or source file, you can also store this file in a storage service (IPFS, Google Drive, S3 or Dropbox) and share the link to the file in the field “Unlockable content”. This file will be shared with the buyer once the purchase is completed.

To keep things more decentralized and keep the blockchain spirit here, instead of using a centralized storage service like Google Drive or Dropbox, let’s use IPFS — Interplanetary File System. IPFS is not a blockchain, but it’s a distributed peer-to-peer file system (similar to BitTorrent) that will allow us to store and share files.

The easiest way to use UPFS is Pinata. Go to Pinata.cloud and sign up if you didn’t sign up yet. Once you have a Pinata account, go to your dashboard, and click upload. Select the file and upload it.

Once the file is uploaded, you will find a CID hash (content identifier), something like Qma4Jse7V6tZ7k3756iPv39tsMG6DhxUQrc42cKoAVVsbR.

This is the hash the will be linked to the image. Copy also the link for the image, go back to the OpenSea website, and paste it in the “Unlockable content” field. The link should look something like this:

https://gateway.pinata.cloud/ipfs/Qma4Jse7V6tZ7k3756iPv39tsMG6DhxUQrc42cKoAVVsbR

Step 5 — NFT properties

Complete the additional properties and tags.

Finally, click create. 🖼🚀😍

You will now have created the asset on the OpenSea website but it’s still not listed for sale.

Step 6 - Sell the NFT

Go to your item page and click “Sell”.

You can also set a “Set price”. This is similar to Ebay’s “Buy it now”, and it’s the price that you are willing to receive to sell your item immediately. The price can be listed in different cryptocurrencies, but the most common one is Ether (ETH, Ethereum’s native currency).

You can also select “Highest bid”. This is the auction option, and in this option, you can select the minimum bid, reserve price and auction expiration date.

Finally, click “Post your Listing”.

Once you click, follow the steps to mint the token. Your Metamask window will prompt (if not, you need to click the Metamask icon) and click sign. OpenSea doesn’t charge any fees, but whenever you mint a new NFT, you will be writing data to the blockchain, and you will incur gas fees (i.e. fees to the Ethereum network).

Once you click “approve” it will prompt your Metamask wallet so that you pay for the fees. On your Metamask wallet, you can click “edit” to edit the fees and select slow or fast. Slow means that you will pay fewer gas fees, but the transaction can take longer to be settled in the blockchain (usually less than 1 hour).

The cost to mint a new NFT may be sometimes high considering that the Ethereum may be congested, but it will probably tend to be lower in the future.

Your NFT is now listed, and people will be able to bid for it or buy it.


## NFT scams
* check if it is verified account
* go to author website check account address

## NFT lingo and slang
