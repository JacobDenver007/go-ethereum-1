[![Build Status](https://travis-ci.org/ethereumproject/go-ethereum.svg?branch=master)](https://travis-ci.org/ethereumproject/go-ethereum)
[![Windows Build Status](https://ci.appveyor.com/api/projects/status/github/ethereumproject/go-ethereum?svg=true)](https://ci.appveyor.com/project/splix/go-ethereum)
[![API Reference](https://camo.githubusercontent.com/915b7be44ada53c290eb157634330494ebe3e30a/68747470733a2f2f676f646f632e6f72672f6769746875622e636f6d2f676f6c616e672f6764646f3f7374617475732e737667
)](https://godoc.org/github.com/ethereumproject/go-ethereum)
[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/ethereumproject/go-ethereum?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)
[![Download](https://api.bintray.com/packages/ethereumproject/GoEthereumClassic/go-ethereum/images/download.svg)](https://bintray.com/ethereumproject/GoEthereumClassic/go-ethereum/_latestVersion)

## Ethereum Go (Ethereum Classic Blockchain)

Official Go language implementation of the Ethereum protocol supporting the
_original_ chain. Ethereum Classic (ETC) offers a censorship-resistant and powerful application platform for developers in parallel to Ethereum (ETHF), while differentially rejecting the DAO bailout.

## Install

### From a release binary
The simplest way to get started running a node is to visit our [Releases page](https://github.com/ethereumproject/go-ethereum/releases) and download a zipped executable binary (matching your operating system, of course), then moving the unzipped file `geth` to somewhere in your `$PATH`. Now you should be able to open a terminal and run `$ geth --help` to make sure it's working. For additional installation instructions please check out the [Installation Wiki](https://github.com/ethereumproject/go-ethereum/wiki/Building-Ethereum). For common useage, just scroll down a little.

### Building the source

If your heart is set on the bleeding edge, install from source. However, please be advised that you may encounter some strange things, and we can't prioritize support beyond the release versions. Recommended for developers only.

#### Dependencies
Building geth requires both a Go and a C compiler. Visit our [Installation Wiki](https://github.com/ethereumproject/go-ethereum/wiki/Building-Ethereum) for instructions.

#### Installing command executables

To install...
- the full suite of utilities: `$ go install github.com/ethereumproject/go-ethereum/cmd/...`
- just __geth__: `$ go install github.com/ethereumproject/go-ethereum/cmd/geth`

Executables built from source will, by default, be installed in `$GOPATH/bin/`

## Executables

This repository includes several wrappers/executables found in the `cmd` directory. There's a lot going on here!

| Command    | Description |
|:----------:|-------------|
| **`geth`** | The main Ethereum CLI client. It is the entry point into the Ethereum network (main-, test-, or private net), capable of running as a full node (default) archive node (retaining all historical state) or a light node (retrieving data live). It can be used by other processes as a gateway into the Ethereum network via JSON RPC endpoints exposed on top of HTTP, WebSocket and/or IPC transports. Please see our [Command Line Options](https://github.com/ethereumproject/go-ethereum/wiki/Command-Line-Options) wiki page for details. |
| `abigen` | Source code generator to convert Ethereum contract definitions into easy to use, compile-time type-safe Go packages. It operates on plain [Ethereum contract ABIs](https://github.com/ethereumproject/wiki/wiki/Ethereum-Contract-ABI) with expanded functionality if the contract bytecode is also available. However it also accepts Solidity source files, making development much more streamlined. Please see our [Native DApps](https://github.com/ethereumproject/go-ethereum/wiki/Native-DApps:-Go-bindings-to-Ethereum-contracts) wiki page for details. |
| `bootnode` | Stripped down version of our Ethereum client implementation that only takes part in the network node discovery protocol, but does not run any of the higher level application protocols. It can be used as a lightweight bootstrap node to aid in finding peers in private networks. |
| `disasm` | Bytecode disassembler to convert EVM (Ethereum Virtual Machine) bytecode into more user friendly assembly-like opcodes (e.g. `echo "6001" | disasm`). For details on the individual opcodes, please see pages 22-30 of the [Ethereum Yellow Paper](http://gavwood.com/paper.pdf). |
| `evm` | Developer utility version of the EVM (Ethereum Virtual Machine) that is capable of running bytecode snippets within a configurable environment and execution mode. Its purpose is to allow insolated, fine graned debugging of EVM opcodes (e.g. `evm --code 60ff60ff --debug`). |
| `gethrpctest` | Developer utility tool to support our [ethereum/rpc-test](https://github.com/ethereumproject/rpc-tests) test suite which validates baseline conformity to the [Ethereum JSON RPC](https://github.com/ethereumproject/wiki/wiki/JSON-RPC) specs. Please see the [test suite's readme](https://github.com/ethereumproject/rpc-tests/blob/master/README.md) for details. |
| `rlpdump` | Developer utility tool to convert binary RLP ([Recursive Length Prefix](https://github.com/ethereumproject/wiki/wiki/RLP)) dumps (data encoding used by the Ethereum protocol both network as well as consensus wise) to user friendlier hierarchical representation (e.g. `rlpdump --hex CE0183FFFFFFC4C304050583616263`). |

## Running _geth_: the basics

By default, geth will store relevant node data in a __parent directory__ depending on your OS: 
- Linux: `$HOME/.ethereum-classic/`
- Mac: `$HOME/Library/EthereumClassic/`
- Windows: `$HOME/AppData/Roaming/`

You can specify this directory on the command line by using the `--datadir` flag.

Within this parent directory, geth will use a __subdirectory__ to hold data for each network you run. The defaults are `/mainnet` for the Mainnet, and `/morden` for the Testnet. 

You can specify the subdirectory with the `--chain` flag.

> _If you have existing data created prior to the [3.4 Release](), geth will attempt to migrate your existing ETC data to this structure. To learn more about managing this migration please read our [3.4 release notes on our Releases page]()._

### Full node on the main Ethereum network

By far the most common scenario is people wanting to simply interact with the Ethereum network: create accounts; transfer funds; deploy and interact with contracts. For this particular use-case the user doesn't care about years-old historical data, so we can fast-sync quickly to the current state of the network. To do so:

```
$ geth --fast --cache=512
```

This command will:

 * Start geth in fast sync mode (`--fast`), causing it to download more data in exchange for avoiding processing the entire history of the Ethereum network, which is very CPU intensive.
 * Bump the memory allowance of the database to 512MB (`--cache=512`), which can help significantly in sync times especially for HDD users. This flag is optional and you can set it as high or as low as you'd like, though we'd recommend the 512MB - 2GB range.

----
### Create or manage account(s)
```
$ geth account new
```

This command will create a new account and prompt you to enter a passphrase to protect your account. 

Other `account` subcommands include:
```
SUBCOMMANDS:

        list    print account addresses
        new     create a new account
        update  update an existing account
        import  import a private key into a new account

```

To learn more about creating, importing, and managing accounts please visit the [Accounts Wiki Page](https://github.com/ethereumproject/go-ethereum/wiki/Managing-your-accounts). And, as always: _do not lose, forget, confuse, conflate, misuse, misunderstand, or misremember your password._

----
### Interact with the Javascript console
```
$ geth console
```

This command will start up Geth's built-in interactive [JavaScript console](https://github.com/ethereumproject/go-ethereum/wiki/JavaScript-Console), through which you can invoke all official [`web3` methods](https://github.com/ethereumproject/wiki/wiki/JavaScript-API) as well as Geth's own [management APIs](https://github.com/ethereumproject/go-ethereum/wiki/Management-APIs). This too is optional and if you leave it out you can always attach to an already running Geth instance with `geth --attach`.

----

> Going through all the possible command line flags would be overwhelming here, but below you'll find a few common parameter combos to get you up to speed quickly on how you can run your own Geth instance. 
> 
> For a comprehensive list of command line options, please consult our [CLI Wiki page](https://github.com/ethereumproject/go-ethereum/wiki/Command-Line-Options).

## Running _geth_: developing and advanced useage

If you'd like to play around with creating Ethereum contracts, you
almost certainly would like to do that without any real money involved until you get the hang of the entire system. In other words, instead of attaching to the main network, you want to join the **test** network with your node, which is fully equivalent to the main network, but with play-Ether only.

```
$ geth --testnet --fast --cache=512 console
```

The `--fast`, `--cache` flags and `console` subcommand have the exact same meaning as above and they are equally useful on the testnet too. Please see above for their explanations if you've skipped to here.

Specifying the `--testnet` flag will reconfigure your Geth instance a bit:

 -  Instead of using the default data directory (`~/.ethereum-classic/mainnet` on Linux for example), Geth will host its data in a a `morden` subfolder (`~/.ethereum-classic/morden`).
 - Instead of connecting the main Ethereum network, the client will connect to the test network, which uses different P2P bootnodes, different network IDs and genesis states.

> *Note: Although there are some internal protective measures to prevent transactions from crossing over between the main network and test network (different starting nonces), you should make sure to always use separate accounts for play-money and real-money. Unless you manually move accounts, Geth
will by default correctly separate the two networks and will not make any accounts available between them.*

### Programatically interfacing Geth nodes

As a developer, sooner rather than later you'll want to start interacting with Geth and the Ethereum
network via your own programs and not manually through the console. To aid this, Geth has built in
support for a JSON-RPC based APIs ([standard APIs](https://github.com/ethereumproject/wiki/wiki/JSON-RPC) and
[Geth specific APIs](https://github.com/ethereumproject/go-ethereum/wiki/Management-APIs)). These can be
exposed via HTTP, WebSockets and IPC (unix sockets on unix based platroms, and named pipes on Windows).

The IPC interface is enabled by default and exposes all the APIs supported by Geth, whereas the HTTP
and WS interfaces need to manually be enabled and only expose a subset of APIs due to security reasons.
These can be turned on/off and configured as you'd expect.

HTTP based JSON-RPC API options:

  * `--rpc` Enable the HTTP-RPC server
  * `--rpcaddr` HTTP-RPC server listening interface (default: "localhost")
  * `--rpcport` HTTP-RPC server listening port (default: 8545)
  * `--rpcapi` API's offered over the HTTP-RPC interface (default: "eth,net,web3")
  * `--rpccorsdomain` Comma separated list of domains from which to accept cross origin requests (browser enforced)
  * `--ws` Enable the WS-RPC server
  * `--wsaddr` WS-RPC server listening interface (default: "localhost")
  * `--wsport` WS-RPC server listening port (default: 8546)
  * `--wsapi` API's offered over the WS-RPC interface (default: "eth,net,web3")
  * `--wsorigins` Origins from which to accept websockets requests
  * `--ipcdisable` Disable the IPC-RPC server
  * `--ipcapi` API's offered over the IPC-RPC interface (default: "admin,debug,eth,miner,net,personal,shh,txpool,web3")
  * `--ipcpath` Filename for IPC socket/pipe within the datadir (explicit paths escape it)

You'll need to use your own programming environments' capabilities (libraries, tools, etc) to connect via HTTP, WS or IPC to a Geth node configured with the above flags and you'll need to speak [JSON-RPC](http://www.jsonrpc.org/specification) on all transports. You can reuse the same connection for multiple requests!

> Note: Please understand the security implications of opening up an HTTP/WS based transport before doing so! Hackers on the internet are actively trying to subvert Ethereum nodes with exposed APIs! Further, all browser tabs can access locally running webservers, so malicious webpages could try to subvert locally available APIs!*

### Operating a private/custom network

Maintaining your own private network is more involved as several options taken for granted in the official networks need to be manually configured.

As of [Geth 3.4]() you are now able to configure a private chain via:
1. initializing nodes with a custom __Genesis state__.
2. specifying an __external chain configuration__ file, which includes necessary Genesis fields _as well as feature configurations for protocol forks, bootnodes, and chainID_. 

Both usages rely on a specified external JSON file. In both cases, __the same JSON file may be used__. However, the `init` command will not require any JSON fields besides the _genesis object_, which means that when using `init`, the external file may be an abbreviated version of the provided `mainnet.json` example.

Please find full [exemplary  external configuration files representing the Mainnet and Morden Testnet default specs in the /config subdirectory of this repo](). You can use either of these files as a starting point for your own customizations.

__Usages 1__ and __2__ are outlined below. The following instructions __Create the rendezvous point__ and __Starting up your member nodes__ will remain the same for both configuration implementations.

#### Usage 1: Initialize identical genesis states

> The "genesis state" of a blockchain defines _Block 0_. Since a blockchain deterministically depends on the ordering and individual characteristics of every block, the configuration of a genesis state uniquely influences the "fingerprint" at any point following on the chain; different _Block 0_, different _Block n_. 

The `init` command allows you to write the genesis state specifications from a JSON file to a chain database as the genesis block, which you'll need to initialize for **every** Geth node prior to starting it up to ensure congruent blockchain parameters:

```
$ geth init path/to/customnet.json
```

Where `customnet.json` must contain:

```json
  {
    "nonce": "0x0000fd6f72646561",
    "timestamp": "",
    "parentHash": "",
    "extraData": "",
    "gasLimit": "0x2FEFD8",
    "difficulty": "0x020000",
    "mixhash": "0x00000000000000000000000000000000000000647572616c65787365646c6578",
    "coinbase": "",
    "alloc": {}
  }

```

The above fields should be fine for most purposes, although we'd recommend changing the `nonce` to some random value so you prevent unknown remote nodes from being able to connect to you. 

If you'd like to pre-fund some accounts for easier testing, you can populate the `alloc` field with account balances:

```json
"alloc": {
      "0x3030303861636137636530353865656161303936": {
        "balance": "100000000000000000000000"
      },
      "0x3030306164613834383336326436613033393261": {
        "balance": "22100000000000000000000"
      },
      "0x3030306433393066623866386536353865616565": {
        "balance": "1000000000000000000000"
      },
      "0x3030313464396162393061303264373863326130": {
        "balance": "2000000000000000000000"
      }
    }
```
*Addresses may be either `0x`-prefixed or not. Testnet configuration default examples are unprefixed to signify difference from conventionally prefixed addresses.*


#### Usage 2: Define external chain configuration

Specifying an external chain configuration file will allow even finer-grained control over a custom blockchain/network configuration, _including the genesis state_ and extending through bootnodes and fork-based protocol upgrades. 

```
$ geth --chainconfig=/path/to/customnet.json [--flags] [command]
```

The external chain configuration file specifies valid settings for the following top-level fields:

| key | notes |
--- | --- | ---
| `chainID` |  Determines local __/subdir__ for chain data. It is required, but must not be identical for each node. Please note that this is _not_ the chainID validation introduced in `EIP-155`; that is configured within `forks.features`. |
| `name` | Human readable name, ie _Ethereum Classic Mainnet_, _Morden Testnet._ |
| `genesis` | Determines __genesis state__. If running the node for the first time, it will write the genesis block (just like `init`). The data required is identical to __Usage 1__. |
| `forks` | Determines configuration for fork-based __protocol upgrades__, ie _EIP-150_, _EIP-155_, _EIP-160_, _ECIP-1010_, etc ;-). |
| `bootstrap` | Determines __bootstrap nodes__ in [enode format](https://github.com/ethereumproject/wiki/wiki/enode-url-format). |

*Only the `name` field is optional. Geth will panic if any required field is missing, invalid, or in conflict with another flag. This renders `--chainconfig` incompatible with `--chain`, `--bootnodes`, `--testnet`.* 

To learn more about external chain configuration, please visit the [External Chain Configuration Wiki page]().

##### Create the rendezvous point

Once all participating nodes have been initialized to the desired genesis state, you'll need to start a __bootstrap node__ that others can use to find each other in your network and/or over the internet. The clean way is to configure and run a dedicated bootnode:

```
$ bootnode --genkey=boot.key
$ bootnode --nodekey=boot.key
```

With the bootnode online, it will display an [`enode` URL](https://github.com/ethereumproject/wiki/wiki/enode-url-format) that other nodes can use to connect to it and exchange peer information. Make sure to replace the
displayed IP address information (most probably `[::]`) with your externally accessible IP to get the actual `enode` URL.

*Note: You could also use a full fledged Geth node as a bootnode, but it's the less recommended way.*

##### Starting up your member nodes

With the bootnode operational and externally reachable (you can try `telnet <ip> <port>` to ensure it's indeed reachable), start every subsequent Geth node pointed to the bootnode for peer discovery via the `--bootnodes` flag. It will probably be desirable to keep private network data separate from defaults; to do so, specify a custom `--datadir` and/or `--chain` flag.

```
$ geth --datadir=path/to/custom/data/folder \
       --chain=kittynet \
       --bootnodes=<bootnode-enode-url-from-above>
```

*Note: Since your network will be completely cut off from the main and test networks, you'll also need to configure a miner to process transactions and create new blocks for you.*

#### Running a private miner

Mining on the public Ethereum network is a complex task as it's only feasible using GPUs, requiring an OpenCL or CUDA enabled `ethminer` instance. For information on such a setup, please consult the [EtherMining subreddit](https://www.reddit.com/r/EtherMining/) and the [Genoil miner](https://github.com/Genoil/cpp-ethereum) repository.

In a private network setting however, a single CPU miner instance is more than enough for practical purposes as it can produce a stable stream of blocks at the correct intervals without needing heavy resources (consider running on a single thread, no need for multiple ones either). To start a Geth instance for mining, run it with all your usual flags, extended by:

```
$ geth <usual-flags> --mine --minerthreads=1 --etherbase=0x0000000000000000000000000000000000000000
```

Which will start mining blocks and transactions on a single CPU thread, crediting all proceedings to the account specified by `--etherbase`. You can further tune the mining by changing the default gas limit blocks converge to (`--targetgaslimit`) and the price transactions are accepted at (`--gasprice`).

## Contribution

Thank you for considering to help out with the source code!

The core values of democratic engagement, transparency, and integrity run deep with us. We welcome contributions from everyone, and are grateful for even the smallest of fixes.  :clap:

This project is migrated from the now hard-forked [Ethereum (ETHF) Github project](https://github.com/ethereum), and we will need to slowly migrate pieces of the infrastructure required to maintain the project. We will apply all upstream patches unrelated to the DAO HF while organizing development.

Comments are being made in the codebase with the tag `!EPROJECT`
recommending actions that must be taken to help complete the migration.

If you'd like to contribute to go-ethereum, please fork, fix, commit and send a pull request for the maintainers to review and merge into the main code base. If you wish to submit more complex changes, please check up with the core devs first on [our gitter channel](https://gitter.im/ethereumproject/go-ethereum) to ensure those changes are in line with the general philosophy of the project and/or get some early feedback which can make both your efforts much lighter as well as our review and merge procedures quick and simple.

Please see the [Wiki](https://github.com/ethereumproject/go-ethereum/wiki) for more details on configuring your environment, managing project dependencies, and testing procedures.

## License

The go-ethereum library (i.e. all code outside of the `cmd` directory) is licensed under the [GNU Lesser General Public License v3.0](http://www.gnu.org/licenses/lgpl-3.0.en.html), also included in our repository in the `COPYING.LESSER` file.

The go-ethereum binaries (i.e. all code inside of the `cmd` directory) is licensed under the [GNU General Public License v3.0](http://www.gnu.org/licenses/gpl-3.0.en.html), also included in our repository in the `COPYING` file.
