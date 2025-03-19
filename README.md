# Overview

The project is a simplistic web application with the goal of distributing small amounts of Test token in test networks.
 or 

The page include two parts: 
- Faucet: Users need to post their SWC addresses to claim test token, it requests the user's wallet must hold at least
  0.01 ETH on Ethereum mainnet. After a funding round, the faucet prevents the same address requesting again for a 
  pre-configured amount of time (e.g. 1 day). 
- Swap: Use SepoliaEth on the Sepolia to swap Test QKC on the SWC (1 SepoliaEth = 10,000 Test QKC). Users need to connect 
  to their metamask wallet before swap and the minimum exchange amount is 100 Test QKC.
 
## Running Front-End Page
The front-end page can be deploy to w3link.io through the [ethstorage-sdk](https://github.com/ethstorage/ethstorage-sdk).

It can also run the following command to set up the front-end locally.
```bash
python -m http.server 8000
```

To integrate the page with the local backend server, you can replace the following in the faucet.html


`server = new WebSocket("wss://faucet.beta.testnet.l2.quarkchain.io:81/api");` 

to

`server = new WebSocket("ws://127.0.0.1:81/api");`


## Running Backend Service
The `faucet` is a single binary app (everything included) with all configurations set via command line flags and a few files.
Building `faucet` requires both a Go (version 1.23 or later), you can run the following command to build it:

```bash
go build
```

Run the following command to start the backend service
```bash
./faucet --ethrpc XXX --sepwsrpc ws://88.99.30.186:8546 --wsrpc ws://5.9.87.214:8546 --account.json ./youraccount.json --account.pass ./youraccount.pass
```

### Operation

The `faucet` needs to connect to Ethereum mainnet, Sepolia testnet and SWC test network fetch related info or submit transaction. 
So each of the following flags must be set:
- `--ethrpc`   is Ethereum mainnet rpc URL for ethclient to get address balance 
- `--sepwsrpc` is Ethereum Sepolia websocket URL for ethclient to get swap transaction 
- `--wsrpc`    is SWC websocket URL for ethclient to submit faucet or swap transactions to SWC


### Funding

To be able to distribute funds, the `faucet` needs access to an already funded SWC account. This can be configured via:

- `--account.json` is a path to the SWC account's JSON key file
- `--account.pass` is a path to a text file with the decryption passphrase

The faucet is able to distribute various amounts of Test Token in exchange for various timeouts. These can be configured via:

- `--faucet.amount` is the number of QKC to send by default
- `--faucet.minutes` is the number of minutes to wait between funding rounds


### Miscellaneous

Beside the above - mostly essential - CLI flags, there are a number that can be used to fine tune the `faucet`'s operation. 
Please see `faucet --help` for a full list.
