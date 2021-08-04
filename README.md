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
https://ipfs.io/ipfs/QmSLq26d7iFS6Qu8SGQaDxrJn4s2cgGfKtLNyAuBUb1XJ7
Save this URL. We'll need this to mint our NFT.

## Creating our own token
For ease and security, we’ll use the 0xcert/ethereum-erc721 contract to create our NFT. With 0xcert/ethereum-erc721, we don’t need to write the whole ERC-721 interface. Instead, we can import the library contract and use its functions.
 
Head over to the Ethereum Remix IDE and make a new Solidity file, for example - nft.sol
 
Paste the following code into your new Solidity script:

```bash
// SPDX-License-Identifier: MIT
// Specifying SPDX license type, which is an addition after Solidity ^0.6.8. Whenever the source code of a smart contract is made available to the public, these licenses can help resolve/avoid copyright issues. If you do not wish to specify any license type, you can use a special value UNLICENSED or simply skip the whole comment (it won’t result in an error, just a warning)
pragma solidity 0.8.0;

// Importing 0xcert/ethereum-erc721 contracts
import "https://github.com/0xcert/ethereum-erc721/src/contracts/tokens/nf-token-metadata.sol";
import "https://github.com/0xcert/ethereum-erc721/src/contracts/ownership/ownable.sol";

// newNFT is extending NFTokenMetadata and Ownable contracts
contract newNFT is NFTokenMetadata, Ownable {

// Initializing the constructor and setting name, a symbol of our token
  constructor() {
    nftName = "Bondi-Mat NFT";
    nftSymbol = "BONDI";
  }

// Declaring function mint with three arguments, variable _to of type address which will store the address of the receiver of NFT token, variable _tokenId of uint256 type which will hold the token id, variable _uri of type string which will store the URI of the JSON file. Declaring mint as external means, it can be accessed from other smart contracts and outside the self scope
  function mint(address _to, uint256 _tokenId, string calldata _uri) external onlyOwner {
    // Minting token using the address of the receiver and token id
    super._mint(_to, _tokenId);
    // Setting token URI using token id and URI of JSON file
    super._setTokenUri(_tokenId, _uri);
  }
 
}
```

Compile the smart-contract and deploy it using injected Web3 (make sure to select Rinkeby testnet on Metamask before compiling the contract). Approve the transaction from metamask.

If you receive an error message before deployment, “This contract may be abstract,” make sure to select the appropriate contract under the Contract tab.
Confirm the transaction in Metamask

Now go to the “Deployed Contracts” section in Remix and expand the deployed contract. You’ll see a bunch of functions/methods. Expand the mint function and add the following details:

    Add your Rinkeby address in the _to the field.
    Enter any number value in the _tokenid field.
    Add URI of JSON file in the _uri field, which we obtained in the previous section. 

Click on transact and confirm the transaction from metamask. Now you have the token on the Rinkeby chain.

Now you can see the Bondi-Mat NFT in Rinkeby Testnet:
https://rinkeby.etherscan.io/token/0x5aa22d9b219c875fc11e8af1deba287d787cd3f4?a=1550

You can check other details like name, symbol, owner, or tokenuri by entering the token id we mentioned earlier.


## ToDos:

- Usage of ipfs V1
- Pinning with the use of nft.storage commands
- Truffle/Brownie tests