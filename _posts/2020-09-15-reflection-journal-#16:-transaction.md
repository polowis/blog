---
date: 2020-09-15 01:33:20
layout: post
title: "Reflection journal #16: Transaction"
subtitle:
description: semester 2, week 10
image: https://i.ytimg.com/vi/_rFqYgt7jxM/maxresdefault.jpg
optimized_image:
category: blog
tags:
  - python
  - blockchain
  - crytocurrency
  - dapp
author: polowis
paginate: false
---

## The Blockchain technology
In the blockchain era, many traditional applications are slowly moving towards decentralization.

These applications are called decentralized applications, or **dapp** for short.

**Dapps** are now mostly built as web apps, because the environment and interactive tools are pretty well developed, like **metamask**, or **dapp browsers**.

Theoretically, it's also possible to build dapps on mobile, but right now there is still a big hurdle when traditional app stores like ```google play``` or ```app store``` have not yet accepted such apps. Partly for security reasons, partly because it will affect the in-app purchase system of those stores.

Hopefully in the future with the rising of blockchain, the stores will have more open mechanisms to the **dapp**.

To build the dapp we can use pure javascript, or reactjs, or vuejs, unity, or anything.

## JS Example
I will show how to build a decentralized application (DApp) as well as build a prototype combining **Contract** and **Frontend** using JavaScript.

There will be a post about Truffle but for now, you need to install it in order to build. Just simply run:

```sh
$ npm install -g truffle
```

Then you need to unbox it
```sh
mkdir EthCodeBased && cd EthCodeBased
```

Unbox **Webpack**
```sh
$ truffle unbox webpack
```
Then init npm

```sh
$ npm init
```
Your package.json file should now look like this:
```json
{
  "name": "testeth",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "directories": {
    "test": "test"
  },
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "dotenv": "^8.1.0",
    "solidity-coverage": "^0.6.4",
    "truffle": "^5.0.12",
    "truffle-assertions": "^0.9.1",
    "truffle-hdwallet-provider": "^1.0.6"
  }
}
```
You can use **yarn** or **npm** to install dependencies

```sh
$ yarn install
```

The **webpack** box contains default **MetaCoin** contract, and this will be the contract for us to do the basic demo. This contract will basically be a super shortened version of **ERC20**. This contract will issue 10,000 coins, of which each coin will correspond to a price of 2 ETH

```js
function getBalanceInEth(address addr) {
        return ConvertLib.convert(getBalance(addr),2);
}

function convert(uint amount,uint conversionRate) {
        return amount * conversionRate;
}
```


To deploy we will use the **ropsten** network, so we will need to reconfigure the **index.js** file

```js
const path = require('path');
var HDWalletProvider = require('truffle-hdwallet-provider');
MNENOMIC = process.env.MNENOMIC;
INFURA_API_KEY = process.env.INFURA_API_KEY;
require('dotenv').config();

module.exports = {
  networks: {
    development: {
      host: '127.0.0.1', // Localhost (default: none)
      port: 8545, // Standard Ethereum port (default: none)
      network_id: '*' // Any network (default: none)
    },
    development: {
      host: '127.0.0.1',
      port: 8545,
      network_id: '*'
    },
    ropsten: {
      provider: () =>
        new HDWalletProvider(MNENOMIC, 'https://ropsten.infura.io/v3/' + INFURA_API_KEY),
      network_id: 3,
      gas: 4612388
    },
    kovan: {
      provider: () =>
        new HDWalletProvider(MNENOMIC, 'https://kovan.infura.io/v3/' + INFURA_API_KEY),
      network_id: 42,
      gas: 470000,
      gasPrice: 21
    },
    rinkeby: {
      provider: () =>
        new HDWalletProvider(MNENOMIC, 'https://rinkeby.infura.io/v3/' + INFURA_API_KEY),
      network_id: 4,
      gas: 470000,
      gasPrice: 21
    }
  }
};
```

There are 2 things need to be understood are **Mnemonic** and **Infura** key:

**Mnemonic**: Taken from **Metamask** itself, 12 characters of seed words, when taking, remember the account with **ETH** because later you will use this account to deploy.

**Infura key**: Obtained simply by creating a project on https://infura.io/dashboard will automatically generate a PROJECT ID. Infura can be understood as a fullnode

Then you need to run this command to deploy:
```sh
truffle migrate --network ropsten
```
After deploying is complete, a new folder will appear to store the compiled file of the contract:

The **Metacoin** file will contain 2 important parts that are **ABI** and **Address**:

**ABI** can understand that it contains the description of the functions and variables of the smart contract that have just been deployed

**Address**: address of the contract that has just been deployed, look in the **Metacoin** file with the keyword networks, there will appear an object with key 3 - corresponding to the chainID of **ropsten** (each network will have its own chainID, mainnet of ETH has chainID of 1 )

So basically the contract is finished, the important things to achieve in this part are the ABI and Address of the smart cotnract. In addition to using Truffle, you can also use Remix (Using remix will save more gas than the truffle because you do not have to spend more gas to deploy contract Migration

## Flask

The question is how would I able to achieve this with python?

We can use **Web3** to interact with blockchain network of **Ethereum**. You can simply install ```web3.py``` to start with.

In your config file, you should have list the keys
```py
PORT=888
PUBLIC_PORT=9999
PROJECT_ID="" #<YOUR_INFURA_PROJECT_ID>
ADDRESS="" #<YOUR_ACCOUNT>
PRIVATE_KEY="" #<PRIVATE_KEY>
IPC_PROVIDER=None #example: ./path/to/geth.ipc
HTTP_PROVIDER="https://ropsten.infura.io/v3/" #example: http://127.0.0.1:8545 make sure to specify the port number
WS_PROVIDER=None #example: ws://127.0.0.1:8546 
```
It is important to keep the keys secure. So don't export them to the public

I'm going to use the structure of the project so it looks easier to read

```python

from web3 import Web3

address_server="address_server"
balance="balance_server"

class Transaction(Controller):
    ...

    
    @route('/')
    def index(self):
        return render_template('index.html', title='Build Dapp with Python', address=address_server, balance=balance)
```

Then you can just build a HTML file however you like.

We need to write a ```send``` function to transfer money. First we need to set ```private_key``` to sign the send and receive orders, the next parameter is the ```nonce``` to set into the transaction when sending. Next we need to get the parameters from the form side using ```request``` or ```json``` if you are using a frontend framework such as Vue or React. After you have the ```address_des``` destination address and the ```value``` to send ```value```, we will create a transaction with parameters like the ```tx``` variable below. ```signed_tx``` is the use of the private key to sign the transaction. Finally, we do transaction with ```sendRawTransaction```, the value it returns is a hash code we can use on the web blockchain to check transaction with this hash. After having the hash code we will go to the website to check the transaction. 

Here is the full code from my project 
```py
from flask import jsonify
import json
from app import app
from app.framework.controller import *
from app.framework.requests.request import request
from app.configuration.web3_config import *


def get_provider():
    return IPC_PROVIDER or HTTP_PROVIDER or WS_PROVIDER

infura_url = HTTP_PROVIDER + PROJECT_ID
web3 = Web3(Web3.HTTPProvider(infura_url))

address_server=ADDRESS
balance = web3.fromWei(web3.eth.getBalance(address_server), 'ether')

class TransactionController(Controller):
    def construct(cls):
        TransactionController.register(app)

    
    @route('/api/ethereum/send')
    def send_ethereum_transaction(self):
        private_key = PRIVATE_KEY
        nonce = web3.eth.getTransactionCount(address_server)

        address_destination = data['address_destination']
        value = data['value']
        tx = {
            'nonce': nonce,
            'to': address_destination,
            'value': web3.toWei(value, 'ether'),
            'gas': 2000000,
            'gasPrice': web3.toWei('50', 'gwei'),
        }
        signed_tx = web3.eth.account.signTransaction(tx, private_key)
        tx_hash = web3.toHex(web3.eth.sendRawTransaction(signed_tx.rawTransaction))

        balance_server = web3.fromWei(web3.eth.getBalance(address_server), 'ether')
        return jsonify(message="Success")
```

If you have trouble installing web3
You can use Docker instead:

```dockerfile
    
    FROM python:3.7

    WORKDIR /app

    COPY . .

    RUN pip install -r requirements.txt

    CMD ["python", "app.py"]
```

Make sure to run ```$ docker --version``` to check if docker is installed

Requirements.txt
```js
Flask==1.1.1
web3==4.9.1
```

Docker-compose.yml

```yml
version: '3.7'
    
    services:
      app:
        image: dapp-python
        build:
          context: .
          dockerfile: Dockerfile
        volumes:
          - .:/app
        ports:
          - '${PUBLIC_PORT}:${PORT}'
        restart: unless-stopped
        environment:
          PORT: ${PORT}
          PROJECT_ID: ${PROJECT_ID}
          ADDRESS: ${ADDRESS}
          PRIVATE_KEY: ${PRIVATE_KEY}
```

And then simply run ```docker-compose up --build``` to load images and build containers for us.