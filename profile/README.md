



| <img src="https://avatars.githubusercontent.com/u/96357480?s=400&u=f54a2fab0e5faaf6bccf57b993e0a28ca2102001&v=4" width="100"> <br>[GoraNetwork](https://goa.io) is a decentralized blockchain oracle network powered by [$GORA](https://www.mexc.com/)  | <img src="https://uploads-ssl.webflow.com/646efe133ad1fe199a53f269/64e8a304831329ff7513f00b_poseNewWebsite01_2-p-800.png" width="170">|  
| -------- | ------- | 
| Devs start here 👉  | [Developers](#-hey-developers)    |
| Node Runners starter 👉  | [Node Runners](#-hey-node-runner-lets-start-here)    |
| Node Installtion Guide 👉  | [Node Devs](../profile/NODEINSTALLTION.md)    |
| Gora AlgoKit Template 🔥 | [AlgoKit Template](https://github.com/GoraNetwork/algokit_default_template)     |
| Gora example boilerplate consumer App 🦾 | [Consumer Example](https://github.com/GoraNetwork/oracle_consumer_app)     |
| Gora example boilerplate feed 🦾 | [Feed Example](https://github.com/GoraNetwork/oracle_consumer_app)     |
| Gora off-chain example 🦾 | [Off-Chain Example](https://github.com/GoraNetwork/off-chain-starter)     |
| Learn Gora Blog |[Gora Blog](https://www.gora.io/blog-post-categories/blog)      |
| Read Gora Articles |[Gora Articles](https://goranetwork.medium.com/)      |



### 👋 Hey developers, let's start here

#### Quick resource for BuildABull:
# [Gora developer quick start package](https://github.com/GoraNetwork/developer-quick-start)

The [Quick Start repository](https://github.com/GoraNetwork/developer-quick-start) is intended to help developers write their first blockchain
application using Gora decentralized oracle. This guide provides:

 * Example application that can be used as a template
 * Step-by-step instructions on how to deploy and test it
 * Info on commands and tools for troubleshooting your Gora applications

All instructions here are written and tested for Linux. Mac users have reported
success with most of the tools used here and are welcome to follow this guide at
their own risk. The reader must be comfortable with using command-line tools,
the Algorand blockchain, and Python programming.

## Prerequisites and environment
The general development environment includes:
*   [Python 3.10](https://www.python.org/downloads/) or above installed and `python` or `python3` commands available on terminal! Check it with `python --version` or `python3 --version`! 
* [PIP package](https://pypi.org/project/pip/) installed and `pip` command available! Check it by `pip --version` or `pip3 --version`

There are four essential pieces to form a Gora development environment:

 * An Algorand node, providing a locally simulated Algorand network (sandbox devnet)
 * Algorand Python toolset for smart contracts and external blockchain interaction (Beaker framework)
 * Gora smart contracts deployed to this network
 * A Gora node runner instance, running and connected to the above

### Algorand software

The following Algorand software must be installed and functioning:

 * [Algorand Sandbox](https://github.com/algorand/sandbox "Algorand Sandbox GitHub page").
 * [Algorand Beaker framework](https://github.com/algorand-devrel/beaker "Algorand Beaker GitHub page"): Install by `pip install beaker-pyteal` or `pip3 install beaker-pyteal`

Refer to the documentation at the above links for download and detailed installation
instructions. Alternatively, you may use Algokit, Algorand's simplified
environment setup toolkit that will handle that for you. Algorand Sandbox must
run a local Algorand network which is the default, but make sure not to start it
on TESTNET or devnet Algorand networks unintentionally.

**Warning!*** By default, the Algorand Sandbox runs its local network,
automatically confirming new transactions on time period basis. This is
currently the recommended mode for Gora development. The "dev" mode of Algorand
Sandbox which confirms every transaction instantly and places it in its own
round is currently not supported. It is incompatible with the security mechanisms
of production Gora smart contracts.

To check that an Algorand development node is up and running on your host, execute:
`curl http://localhost:4001/versions`. You should get a JSON response with
version information fields, including: `"genesis_id":"sandnet-v1"`.

### Gora software
- First update your Python packages if they are already installed! On nix systems it is recommended to `sudo apt update && sudo apt upgrade -y` first!
- Start by cloning the QuickStart repository by `git clone git@github.com:GoraNetwork/developer-quick-start.git`!

- Get Gora CLI tool : Both Gora smart contracts and Gora node are managed with Gora CLI tool.
 [Download it here](https://download.goracle.io/latest-dev/linux/goracle "Gora CLI tool Linux binary")!

- Make it executable by running `chmod +x ./goracle`.
- Running the CLI tool: Running without arguments will list available commands.
- To get help on a command, run
`./goracle help <command name>`, for example: `./goracle help docker-start`.

**Warning!** Do NOT follow normal Gora node setup process for live network
operators when setting up a development node.

- Node setup: To set up your development node, run: `GORACLE_CONFIG_FILE= ./goracle dev-init`.
This would clone Gora smart contracts from testnet to your local Algorand
Sandbox network and create a config file for your development node. By default,
this file is called `~/.goracle_dev`.
- Node start: Now you should be ready to start your development node as: `GORACLE_CONFIG_FILE=~/.goracle_dev ./goracle docker-start`.
This will form a single-node Gora network for local end-to-end testing of your
applications. This node will pick up your local Gora requests and process them
like a production network would, logging various debugging information to
standard output.

Done! Now you are ready to run the example APP!

**Warning!** do not stop the node. You must have your development node up and
running to process requests from the example app or from any Gora-enabled apps
which you will be developing locally.

## Example app

The example app [example_const.py](https://github.com/GoraNetwork/developer-quick-start/blob/main/example_const.py "Example app on Github")
demonstrates the use of Gora with a test oracle source. That source is built
into Gora and always returns the value of `1`, allowing for reliable testing and
minimal support code. Once the user understands this example app and can execute
it successfully in their development environment, they should be all set for
extending it to query other Gora sources in their own custom apps.

The example app is built with Algorand's [Beaker framework](https://algorand-devrel.github.io/beaker/html/index.html "Official Beaker documentation")
and is extensively commented to make it accessible for novice developers.

Be advised that the way you build your applications with Beaker changed at one
point, replacing Python subclassing with decorators as means of adding custom
functionality. If you are using additional Beaker documentation or examples,
make sure that they are current.

To run the example app, you must to point it to your local Gora network via
environment variables. Check output of your Gora development node for messages
like: `Main smart contract: "<number>"`, `Token asset ID: "<number>"`.
These contain values defining your local Gora development network. Now you can
use them to execute the example app:
`GORA_TOKEN_ASSET_ID=<token asset ID> GORA_MAIN_APP_ID=<main app ID> python example_const.py`

Once the app compiles and executes, the node should pick up its request, showing
a message like `Processing oracle request "<request ID>"`. When a message starting
with `Submitted <number> vote(s) on request "<request ID>"`appears, it means
that the request has been processed and the destination method app should have
been called with the response.

Algorand [Dapp Flow](https://app.dappflow.org/explorer/home) web app can be used
to trace applicable transactions and confirm that the destination call has been
made and values updated. **Warning!** You may get an error message from Dapp Flow about "disabled parameter:
application-id". This is a minor issue and should not affect operation.




#### 👨🏽‍💻 Work with Gora Data Feeds
There are three categories of data feeds in the GoraNetwork ecosystem: price beacon, aggregated, and custom.
- Price Beacon:
  
    GoraNetwork's Price beacons are similar to the Oracle industry standard of providing data feeds available on a regular interval (e.g. a 1% deviation), in a publicly accessible contract. The current list of crypto spot prices can viewed on the Price Beacon page. While the price beacons are limited to high volume, popular assets, consumers can make request to any asset or data type by calling aggregated or custome endpoints.
- Aggregated Data:
  
    Aggregated data is obtained from institutional data providers who generally  aggregate high quality sources, and as such, already have significant data pre-processing for accuracy and reliability. GoraNetwork aggregates this aggregated data, ensuring the most accurate and availalbe data of any oracle. 
- Custom Data:
  
    Custom data feeds allow consumers to obtain data from their own endpoints. Calling custom data endpoints is very similar to calling aggregated data endponts. The process is described further in the following sections.

🚀[Continue reading on Data feeds...](./profile/INTEGRATEDATAFEEDS.md)

#### 👩🏽‍💻 Validator Setup
GoraNetwork has it's own validator network and registery process which is done through [Gora Validators Portal](https://sandbox-app.goracle.io/validators). This guide provides a step-by-step process to register your validator node via the GoraNetwork Validator Portal. Registering your validator in this manner enables the community to delegation to your node.

🚀[Continue reading on validators...](./profile/VALIDATOR.md)

### 👋 Hey Node Runner, let's start here

Note: This guide contains instructions for Algorand Node Running. Arbitrum and Polygon nodes are currently in Alpha. The following steps will remain the same by upcoming network additions, with criteria of using the relevant RPC or Node endpoint.

#### Introduction to GNR
Gora Node Runner (GNR) is the sofware implementing Gora network node. It performs the following key tasks:
Monitoring of Algorand blockchain for new oracle requests
Fulfilling of oracle requests by querying data sources and aggregating results
Submission of results for certification to Gora smart contracts
The GNR is distributed as a Linux-based Docker image. It is designed to run on a docker-enabled customer host or in AWS cloud. Instalation and maintenance of GNR is conducted with Gora CLI tool - a self-contained command-line executable. A live Algorand node is required by GNR to interact with the blockchain. A Gora node operator must have access to a publically available Algorand node or run one of their own.
Any problems encountered using Gora software as described in this manual, or errors in the manual itself, should be reported here: https://validators.gora.io/bugreport

🚀[Continue reading on GNR...](./profile/NODERUNNER.md)







