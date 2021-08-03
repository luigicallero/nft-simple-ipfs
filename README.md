# NFT on IPFS

Following the simple steps in this image:
https://www.quicknode.com/guides/solidity/how-to-create-and-deploy-an-erc-721-nft

- Adding Files to IPFS
Before writing our NFT contract, we need to host our art for NFT and create a metadata file; for this, we’ll use IPFS - a peer-to-peer file storing and sharing distributed system. Download and install IPFS CLI  based on your Operating system by following the installation guide in IPFS docs: https://ipfs.io/#install. 
 
## Hosting the image and metadata file.

Step 1: Creating IPFS repo.
Start the IPFS repo by typing the following in a terminal/cmd window.
```bash
ipfs init
```
Step 2: Starting the IPFS daemon.
Start IPFS daemon, open a separate terminal/cmd window and type the following.
```bash
ipfs daemon
```
Step 3: Adding an image to IPFS. 
Go to the first terminal window and in the images folder, add the image to IPFS:
```bash
cd images
ipfs add bondimat_mainhtml.png
```
Copy the hash starting from Qm (ipfs v0) and add the “https://ipfs.io/ipfs/” prefix to it:
https://ipfs.io/ipfs/QmaHtfxCPQ95XggCJisBEStygG3oD2uDbYvkmhkqwGoDyv

Step 4: Adding JSON file to IPFS
Create a JSON file nft.json and save it in the same directory as the image. 
JSON file format: 
```bash
{
    "name": "NFT Art: Bondimat",
    "description": "This image shows one of our first Dapps.",
    "image": "https://ipfs.io/ipfs/QmaHtfxCPQ95XggCJisBEStygG3oD2uDbYvkmhkqwGoDyv",
}
```
Now add the JSON file.
```bash
ipfs add nft.json
```
Copy the hash starting from Qm and add the “https://ipfs.io/ipfs/” prefix to it:
https://ipfs.io/ipfs/QmUGhQrUAsnvJNq19ukxsvkkVaBtFS9rdi5M39Tv9Zrrqj
Save this URL. We'll need this to mint our NFT.

Creating our own token.
For ease and security, we’ll use the 0xcert/ethereum-erc721 contract to create our NFT. With 0xcert/ethereum-erc721, we don’t need to write the whole ERC-721 interface. Instead, we can import the library contract and use its functions.
 
Head over to the Ethereum Remix IDE and make a new Solidity file, for example - nft.sol
 
Paste the following code into your new Solidity script:


Explanation of the code above: 
 
Line 1: Specifying SPDX license type, which is an addition after Solidity ^0.6.8. Whenever the source code of a smart contract is made available to the public, these licenses can help resolve/avoid copyright issues. If you do not wish to specify any license type, you can use a special value UNLICENSED or simply skip the whole comment (it won’t result in an error, just a warning).
 
Line 2: Declaring the solidity version.

Line 4-5: Importing 0xcert/ethereum-erc721 contracts.

Line 7: Starting our Contract named newNFT and mentioning it’s extending NFTokenMetadata and Ownable contracts.

Line 9-12: Initializing the constructor and setting name, a symbol of our token.

Line 14: Declaring function mint with three arguments, variable _to of type address which will store the address of the receiver of NFT token, variable _tokenId of uint256 type which will hold the token id, variable _uri of type string which will store the URI of the JSON file. Declaring mint as external means, it can be accessed from other smart contracts and outside the self scope.

Line 15: Minting token using the address of the receiver and token id.

Line 16: Setting token URI using token id and URI of JSON file.

Compile the smart-contract and deploy it using injected Web3 (make sure to select Ropsten testnet on Metamask before compiling the contract). Approve the transaction from metamask.


If you receive an error message before deployment, “This contract may be abstract,” make sure to select the appropriate contract under the Contract tab.
Confirm the transaction in Metamask



Now go to the “Deployed Contracts” section in Remix and expand the deployed contract. You’ll see a bunch of functions/methods. Expand the mint function and add the following details:

    Add your Ropsten address in the _to the field.
    Enter any Big number value in the _tokenid field (we suggest 1 since it’s the first).
    Add URI of JSON file in the _uri field, which we obtained in the previous section. 


Click on transact and confirm the transaction from metamask. Now you have the token on the Ropsten chain.

You can check other details like name, symbol, owner, or tokenuri by entering the token id we mentioned earlier.

```bash
```
