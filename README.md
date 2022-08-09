# New-NFT

Here Are The steps To Compile It:

#Prerequisites

1.install Node.js/NPm
2.install hardhat
3.install ngrok
4.use vscode / sublime txt.
5.install metamask(head to Ropsten Etherium Project)
6.install Alchemy(Choose Ropsten)

#Initialize the project

In your terminal, run this command to make a new directory for your project:

mkdir nft-project
cd nft-project

Now, let's make another directory, ethereum/, inside nft-project/ and initialize it with Hardhat. Hardhat is a dev tool that makes it easy to deploy and test your Ethereum software.

mkdir ethereum
cd ethereum
npm init

Answer the questions however you want. Then, run those commands to make a Hardhat project:

npm install --save-dev hardhat
npx hardhat

You will see this prompt:

888    888                      888 888               888
888    888                      888 888               888
888    888                      888 888               888
8888888888  8888b.  888d888 .d88888 88888b.   8888b.  888888
888    888     "88b 888P"  d88" 888 888 "88b     "88b 888
888    888 .d888888 888    888  888 888  888 .d888888 888
888    888 888  888 888    Y88b 888 888  888 888  888 Y88b.
888    888 "Y888888 888     "Y88888 888  888 "Y888888  "Y888

Welcome to Hardhat v2.0.8

? What do you want to do? …
  Create a sample project
❯ Create an empty hardhat.config.js
  Quit
  
Select create an empty hardhat.config.js. This will generate an empty hardhat.config.js file that we will later update.

For the web app, we will use Next.js to initialize a fully-functional web app. Go back to the root directory nft-project/ and initialize a boilerplate Next.js app called web:

cd ..
mkdir web
cd web
npx create-next-app@latest

Your project now looks like this:

nft-project/
	ethereum/
	web/
  
#Define .env Variables

Run the following commands, make a file called .env inside your ethereum/ directory, and install dotenv. We will use them later.

cd ..
cd ethereum
touch .env
npm install dotenv --save

For your .env file, put the key you have exported from Alchemy and follow those instructions to grab your Metamask's private key.

Here's your .env file:

DEV_API_URL = YOUR_ALCHEMY_KEY
PRIVATE_KEY = YOUR_METAMASK_PRIVATE_KEY
PUBLIC_KEY = YOUR_METAMASK_ADDRESS

#The Smart Contract for NFTs

Go to the ethereum/ folder and create two more directories: contracts and scripts. A simple hardhat project contains those folders.

contracts/ contains the source files of your contracts
scripts/ contains the scripts to deploy and mint our NFTs
mkdir contracts
mkdir scripts

Then, install OpenZeppelin. OpenZeppelin Contract is an open-sourced library with pre-tested reusable code to make smart contract development easier.

npm install @openzeppelin/contracts

Finally, we will be writing the Smart Contract for our NFT. Navigate to your contracts directory and create a file titled EmotionalShapes.sol. You can name your NFTs however you see fit.

The .sol extension refers to the Solidity language, which is what we will use to program our Smart Contract. We will only be writing 14 lines of code with Solidity, so no worries if you haven't seen it before.

cd contracts
touch EmotionalShapes.sol

#Expose the Metadata for our NFT

Go to ngrok.com and complete the registration process
Unzip the downloaded package
In your terminal, make sure you cd into the folder where you unzipped your ngrok package
Follow the instruction on your dashboard and run

./ngrok authtoken YOUR_AUTH_TOKEN

5.  Then, run this command to create a tunnel to your web app hosted on localhost:3000

./ngrok http 3000

6.  You are almost there! On your terminal, you should see something like this:

ngrok by @inconshreveable                                                                            (Ctrl+C to quit)
                                                                                                                     
Session Status                online                                                                                 
Account                       YOUR_ACCOUNT (Plan: Free)                                                                       
Version                       2.3.40                                                                                 
Region                        United States (us)                                                                     
Web Interface                 http://127.0.0.1:4040                                                                  
Forwarding                    http://YOUR_NGROK_ADDRESS -> http://localhost:3000                             
Forwarding                    https://YOUR_NGROK_ADDRESS -> http://localhost:3000                             
Go to YOUR_NGROK_ADDRESS/api/erc721/1 to make sure your endpoint works correctly.

#Deploy our NFT

Change the _baseURI function in your ethreum/contracts/YOUR_NFT_NAME.sol file to return your ngrok address.

// ethereum/conrtacts/EmotionalShapes.sol

contract EmotionalShapes is ERC721 {
...
	function _baseURI() internal pure override returns (string memory) {
		return "https://YOUR_NGROK_ADDRESS/api/erc721/";
	}
...
}
To deploy our NFT, we will first need to compile it using Hardhat. To make the process easier, we will install ethers.js.

npm install @nomiclabs/hardhat-ethers --save-dev
Let's update our hardhat.config.js:

require("dotenv").config();
require("@nomiclabs/hardhat-ethers");

module.exports = {
  solidity: "0.8.0",
  defaultNetwork: "ropsten",
  networks: {
    hardhat: {},
    ropsten: {
      url: process.env.DEV_API_URL,
      accounts: [`0x${process.env.PRIVATE_KEY}`],
    },
  },
};

Finally, run:

npx hardhat compile

#view the NFT on the blockchain

Run the deployment script:

node ./scripts/deploy.js
You should see in your terminal EmotionalShapes deployed: SOME_ADDRESS. This is the address where your Smart Contract is deployed on the ropsten test network.

If you head over to https://ropsten.etherscan.io/address/SOME_ADDRESS, you should see your freshly deployed NFT.

#Mint your NFT

Now that you have deployed your NFT, it's time to mint it for yourself! Create a new file called mint.js in your scripts/ folder. We will be using ethers.js to help us.

Start by adding the ethers.js package:

npm install --save ethers

