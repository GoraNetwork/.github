
#### This README document is undergoing changes! (WIP)!
|<img src="https://avatars.githubusercontent.com/u/96357480?s=400&u=f54a2fab0e5faaf6bccf57b993e0a28ca2102001&v=4" width="100">  |  GoraNetwork|
| -------- | ------- |

|   GoraNetwork is a decentralized oracle network powered by $GORA  |  <img src="https://uploads-ssl.webflow.com/646efe133ad1fe199a53f269/64e8a304831329ff7513f00b_poseNewWebsite01_2-p-800.png" width="170"> |
| -------- | ------- |
| Devs start here 👉  | [Devs Start here](#👋-hey-developers)    |
| Node Runners starter 👉  | [Node Runners Start here](#👋-hey-node-runner-lets-start-here)    |
| Technical docs |[Gora Technical Docs](https://docs.goracle.io/technical-documentation/)      |
| Gora AlgoKit Template  | [Gora AlgoKit](https://github.com/GoraNetwork/algokit_default_template)     |
| Gora example boilerplate consumer App  | [Gora Consumer Example](https://github.com/GoraNetwork/oracle_consumer_app)     |
| Gora example boilerplate feed  | [Gora Feed Example](https://github.com/GoraNetwork/oracle_consumer_app)     |
| Gora off-chain example  | [Gora Off-Chain Example](https://github.com/GoraNetwork/off-chain-starter)     |
| Learn Gora Blog |[Gora Blog](https://www.gora.io/blog-post-categories/blog)      |
| Read Gora Articles |[Gora Articles](https://goranetwork.medium.com/)      |



### 👋 Hey developers, let's start here


### 👋 Hey Node Runner, let's start here

Note: This guide contains instructions for Algorand Node Running. Arbitrum and Polygon nodes are currently in Alpha. The following steps will remain the same by upcoming network additions, with criteria of using the relevant RPC or Node endpoint.

#### Introduction
Gora Node Runner (GNR) is the sofware implementing Gora network node. It performs the following key tasks:
Monitoring of Algorand blockchain for new oracle requests
Fulfilling of oracle requests by querying data sources and aggregating results
Submission of results for certification to Gora smart contracts
The GNR is distributed as a Linux-based Docker image. It is designed to run on a docker-enabled customer host or in AWS cloud. Instalation and maintenance of GNR is conducted with Gora CLI tool - a self-contained command-line executable. A live Algorand node is required by GNR to interact with the blockchain. A Gora node operator must have access to a publically available Algorand node or run one of their own.
Any problems encountered using Gora software as described in this manual, or errors in the manual itself, should be reported here: https://validators.goracle.io/bugreport
#### Prerequisites
This manual is written for someone able to work with command-line (CLI) tools and Linux OS in general. Advanced customization may require editing of text files in JSON format. If you are not comfortable with the above, it is recommended that you enlist professional help or get up to speed using resources widely available online.
For the Node Runner, 64-bit x86 hardware is currently officially supported. Mac, ARM, older 32-bit or any other non-amd86 hardware platforms are not. Most of these have no inherent incompatibilities with Gora software and will likely be supported in future releases. For the CLI tool, Mac platform is supported as well as Linux.
#### Permissions
At no time should you run Gora software as root. If hacked, software executed as root becomes gateway to complete control of the system it is running on. This makes such software a high-priority target for hackers, which is neither in Gora's interest nor yours. No support will be provided for such usage scenarios.
You should be able to run Gora software as a normal user with no additional setup. If you run into a permissions issue, do not use sudo to force your way through. If the error is docker-related, make sure you have added yourself to the docker user group. If adding priveleges to your normal user is undesirable, create a new user gora with useradd command, give it the necessary permissions and perform all operations as them.
#### Running the node
1) Install the required software
Install Docker engine if you want to run your node locally, or AWS CLI if you prefer to run it on AWS. When using docker, ensure that you are able to use docker as a normal, non-root user; usually this requires adding yourself to the docker group, then logging out and back in.
Download Gora CLI tool. To do it with wget run: https://download.goracle.io/latest-release/linux/goracle -O gora
Make the downloaded binary executable by running chmod u+x ./gora

2) Initialize your installation
Initialization of a new Gora node involves the following key steps:
Creating your Gora participation account on Algorand
Initializing it by linking it to your main Algorand account and supplying it with tokens and Algo.
Creating a Gora config file ~/.goracle with info on these accounts as well as connection details for the Algorand node that your Gora node will use.

To start the process, run ./gora init. You will be guided through the steps with text prompts and messages. You can abort the process at any time by pressing Ctrl-C. To start over, rename or delete the produced config file ~/.gora.
Make sure that your participation account balance is at least 10 ALGOs. It should have received them during initialization via Gora web app. If it has not, simply send 10 ALGOs to the participation account address using your Algorand wallet.

3) Run Node runner
   - Run Node Runner on AWS:
    AWS functionality is currently experimental, new users are encouraged to skip to the section 3b and run their node locally. If you are willing to try running on AWS, run export GORA_EXPERIMENTAL_MODE=1 and proceed with caution.
    Download and install AWS CLI. To start your node, run ./goracle aws-start. If it is the first time, several AWS configuration items will be set up for you, producing an output like:

    ```
    Creating security group "gora-nr-sg"
    Creating log group "gora-nr-logs"
    Registering task definition "gora-nr-task"
    ```

    Then you should see the kind of output that would appear every time you start your node up:
    Startup initiated, task ID: "2468d56dff884c9ca536fb2e537f8928"
    This means the node is starting up and should be online shortly. You can check its status by executing ./gora aws-status, which should eventually produce an output like:

    ```
    State: Running
    Started at: 2022-07-04T17:33:08.803Z
    Uptime: 2 min.
    Task ID: "2468d56dff884c9ca536fb2e537f8928"
    IP: 3.68.119.121
    ```

    The IP address above is the public address of your node. Node log messages can be inspected using AWS web UI or by running ./gora aws-log. To bring the node down, execute ./gora aws-stop. 

- Run Node Runner on any Docker-enabled host:
    Running your Gora node outside of cloud environments gives you more control and can be more economical. Before continuing, make sure that you have installed Docker and added yourself to the docker group on your host. To start your node, execute: ./gora docker-start. When run for the first time, it will pull docker image from the registry. Then Node Runner will be launched, logging its messages to standard output, e.g.:
    ```
    2023-03-30T14:20:45.540Z INFO  Starting Gora Node Runner
    2023-03-30T14:20:45.733Z INFO  Version: "1.0.18"
    2023-03-30T14:20:45.733Z INFO  Built on: "Thu, 30 Mar 2023 14:20:31 GMT"
    2023-03-30T14:20:45.909Z INFO  Revision: "2c8f95ff6de45ef9218739fbc590fcb8bb4d7105"
    2023-03-30T14:20:45.909Z INFO  Smart contracts revision: "6e83988464446e288f5cb750581f061c29eb49c3"
    2023-03-30T14:20:45.909Z INFO  Docker image: "docker.example.com/goracle-nr:latest-release"
    2023-03-30T14:20:45.909Z INFO  Docker image hash: "e16ed958aa5ec11c772951e899a98a2cab95de07b9997ddd53304fcf6b9dd042"
    2023-03-30T14:20:46.042Z INFO  Gora network ID: 10000
    2023-03-30T14:20:46.144Z INFO  Blockchain server: http://algorand.example.com:4001/
    2023-03-30T14:20:46.147Z INFO  Main address: SZH4LXMWE3VROWHH6NC6UERXPHAVM5VFBAVUSGOA5SAQU4KDTM3DPR5T64
    2023-03-30T14:20:46.147Z INFO  Participation address: TOSGXN36JZMJ246BJ6BJQERGDWU7HRVQWTJJCZ44NUFZ4OZK7OVUJDQSYM
    2023-03-30T14:20:47.144Z INFO  Last blockchain round: 28782146
    2023-03-30T14:20:47.275Z INFO  Staked amount: 50000000000 microGORA
    2023-03-30T14:20:47.275Z INFO  Deposits: 8455700 microALGO, 62770000 microGORA
    2023-03-30T14:20:47.340Z INFO  Oracle sources set up: 25
    2023-03-30T14:20:47.343Z INFO  Waiting for next oracle request
    ....
    ```
    To make it switch into background upon startup, add --background to the command. To quickly check instance status, use gora docker-status. To inspect instance's logs, use docker log command, e.g.: docker logs -fn 100 gora-nr.

4) Stop a Node Runner instance:
To stop an AWS-deployed Node Runner instance, run: ./gora aws-stop. For stopping a locally run instance, hit Ctrl-C if it is running in the foreground, or execute ./gora docker-stop to stop a background instance.

#### Updating Gora Software
Gora CLI tool is updated with gora update command. It checks whether there is a more recent version than the one being run, and if so, offers to upgrade it by downloading and replacing gora binary. Current binary will be backed up.
Gora Node Runner is distributed as a docker image, so it will be automatically updated whenever your Gora node is started. To ensure that you are running the latest version, simply stop and start your node again.
#### Background information
- Gora Algorand accounts:
    To ensure security of operator's staked assets, a Gora node uses not one, but two Algorand accounts:
    Main account stakes operator's GORA tokens and receives rewards earned by the node. Its private key is never exposed to the node software.
    Participation account is created specifically for the use by Gora node and linked to the main account with a record the blockchain. Participation account has no access to operator's stake or rewards, except the small amount accrued since the last time rewards were claimed.
    This keeps staked tokens and most of the rewards safe in the unlikely case that your Node Runner instance is compromised.
- Gora config file:
    Gora config file contains settings specific for your Gora node in JSON format. When deploying a node to the cloud or launching it locally, Gora CLI tool reads this file and passes its contents to the Node Runner via Docker container environment settings. The default location of Gora config file is ~/.gora. It should contain at least the following settings:
    ```
    {
    "blockchain": {
        "server": "<your Algorand node API URL>",
        "perNetworkConfig": {
        "wGHE2Pwdvd7S12BL5FaOP20EGYesN73ktiC1qzkkit8=": {
            "mnemonic": "<Algorand mnemonic for your participation account>",
            "mainAddr": "<Algorand address of your main account>",
        }
        }
    }
    }
    ```
    The server variable contains URL of an Algorand node via which your Gora node interacts with the blockchain. This URL must contain the protocol prefix as well as port number, e.g.: "http://example.com:4001" When using standard Algorand node token-based authentication with the default token value of aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa, nothing needs to be added. To set a custom authentication token, under the "blockchain" subtree, add:
        "token": "<your Algorand node access token>",
    Connecting to Purestake instead requires the following setting:
        "authKey": "<Your Purestake access key>",
    Important! Config file access permissions must be set to deny all access to anyone but the user. On Linux, this is done by running: chmod 0600 ~/.gora. To protect your account, Gora CLI tool will fail to work otherwise.






