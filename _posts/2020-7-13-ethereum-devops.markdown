---
layout: post
title:  "Ethereum Devops"
date:   2020-7-13 15:53:00 -0500
---

I wanted to share a basic outline I've settled on after playing a devops role on a few ethereum dApps.

Using docker-compose for local development allows developers to easily test their app against a test blockchain (such as ganache) or connect to a public blockchain (like mainnet or a test network) to test client changes against real contracts.

Running against a test blockchain also allows you to make certain deterministic assumptions about the way that the smart contracts are set up - such as the ethereum address where the contract will be deployed, which simplifies connecting to those contracts from your clients and apis.

The setup I present below assumes a directory structure with a folder for the contracts and a folder for the client.
```
/client
/ethereum
```


## Docker Compose
A `/docker-compose.yml` file sets up a few services - a client, a test blockchain (ganache), a service to deploy the contracts (ganache-deploy) and a block explorer.

```
version: '2.1'
services:
  client:
    build:
      context: ./client
      dockerfile: Dockerfile
    ports:
      - '3000:80'
    environment:
      BLOCK_EXPLORER_URL: http://localhost:7000
      CONTRACT_ADDRESS: 0xDETERMINISTICALLY_GENERATED_BY_DEPLOY
  ganache:
    image: trufflesuite/ganache-cli:latest
    ports:
      - "8545:8545"
    command: ganache-cli --blockTime=2 --account="0xYOUR_ACCOUNT_HERE,100000000000000000000" --mnemonic "set your own damn mnemonic"
  ganache-deploy:
    build:
      context: ./ethereum
      dockerfile: Dockerfile
    healthcheck:
      test: curl -sf -X POST --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}' http://ganache:8545
      interval: 5s
      timeout: 5s
      retries: 5
    volumes:
      - ./ethereum/build:/usr/src/app/build
  block-explorer:
    image: alethio/ethereum-lite-explorer
    environment:
      APP_NODE_URL: http://localhost:8545
    ports:
      - '7000:80'
```

## Client

For the sake of this example, let's assume that the client is a react/redux javascript app - which will be available on port 3000.

It's `/client/Dockerfile` may look something like this

```
# The Builder
FROM node:10.16.3-alpine AS builder
MAINTAINER Greg Taschuk
WORKDIR /usr/src/app
RUN apk add --no-cache git 
COPY package.json .
COPY yarn.lock .
RUN yarn install
COPY . .
RUN npm run build

# The App
FROM nginx:1.17.4-alpine AS app
COPY nginx-site.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /usr/src/app/build /usr/share/nginx/html
```

This is an example of how you might deploy the app, you may opt for a different dockerfile that runs a development server and shares folders with the host for easy debugging.

## Contracts


For the ethereum folder, we will create a truffle project using `truffle init` giving us a file structure like so

```
/ethereum/contracts
/ethereum/migrations
/ethereum/Dockerfile
/ethereum/truffle-config.js
/ethereum/package.json
/etherum/yarn.lock
/etherum/build
/etherum/node_modules
```

and a `/ethereum/Dockerfile`that deploys the contracts to our test blockchain

```
FROM node:10.16.3-alpine
MAINTAINER Greg Taschuk
WORKDIR /usr/src/app
RUN apk add --no-cache git python make g++

COPY package.json .
COPY yarn.lock .
RUN yarn install

COPY . .

CMD ["npm", "run", "deploy-contracts-from-docker"]
```

as long as the package.json has a script


```
  "scripts": {
    "test": "truffle test",
    "compile-contracts": "truffle compile",
    "migrate-contracts": "truffle migrate",
    "deploy-contracts": "truffle deploy",
    "deploy-contracts-from-docker": "truffle deploy --network dockerGanache",
    "lint": "eslint scripts",
    "lint:fix": "npm run lint -- --fix",
    "lint:sol": "solium -d ./contracts",
    "console": "truffle console",
    "networks": "truffle networks"
  },
```

and a truffle config

```
const HDWalletProvider = require("@truffle/hdwallet-provider");
const infuraKey = process.env.INFURA_KEY;
const fs = require("fs");
const path = require("path");
const ganacheMnemonic =
  "set your own damn mnemonic"

function walletProvider(filepath) {
  if (fs.existsSync(filepath)) {
    return () => {
      const file = fs.readFileSync(path.join(__dirname, filepath), "utf8");
      let { mnemonic, providerUrl } = JSON.parse(file);
      var HDWalletProvider = require("@truffle/hdwallet-provider");

      return new HDWalletProvider(mnemonic, providerUrl, 0, 3);
    };
  } else {
    return () => {
      throw "uh oh, you don't have that mnemonic";
    };
  }
}

const gas = 6250000;
const gasPrice = 3000000000;

module.exports = {
  networks: {
    development: {
      host: "127.0.0.1", // Localhost (default: none)
      port: 8545, // Standard Ethereum port (default: none)
      network_id: "*" // Any network (default: none)
    },
    dockerGanache: {
      provider: new HDWalletProvider(
        ganacheMnemonic,
        "http://ganache:8545",
        0,
        3
      ),
      network_id: "*" // Any network (default: none)
    },
    rinkeby: {
      confirmations: 2,
      provider: walletProvider(".secret.json"),
      network_id: 4,
      gas,
      gasPrice
    }
  },
  mocha: {},
  compilers: {
    solc: {
      version: "0.5.10" // Fetch exact version from solc-bin (default: truffle's version)
    }
  }
};

```

The beauty of this setup is that if you import the mnemonic you set into metamask, the accounts you create will be derived off the same key that is used for ganache, so they will have eth to interact with contracts, and the proper ownership permissions to interact with them.

I recommend sharing a mnemonic among developers so that everyone has the same values for important environment variables like contract addressses.  Just be careful that you only use these accounts locally.  Better to go ahead and share this key and be upfront that it isn't secure for any real-world deployments than to all have your own secrets.

## Metamask

It's best to create a metamask installation for the exclusive purpose of development.  An easy way to enforce this separation is to create a chrome profile, so that you have a separate installation of the extension.


## Real world examples

A few open-source projects I have set up with this pattern:

https://github.com/gtaschuk/ERC809
https://github.com/gtaschuk/saml-eth
https://github.com/TruSet/bitmask-rbac
https://github.com/teamsempo/SempoBlockchain

## Shameless Plug

If this all sounds a tad confusing or you want to save time, I'm happy to set it up for you as a freelancer - just contact me!
