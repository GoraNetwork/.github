
#### This README document is undergoing changes! Please visit back soons (WIP)!


| <img src="https://avatars.githubusercontent.com/u/96357480?s=400&u=f54a2fab0e5faaf6bccf57b993e0a28ca2102001&v=4" width="100"> <br>[GoraNetwork](https://goa.io) is a decentralized oracle network powered by [$GORA](https://www.mexc.com/)  | <img src="https://uploads-ssl.webflow.com/646efe133ad1fe199a53f269/64e8a304831329ff7513f00b_poseNewWebsite01_2-p-800.png" width="170">|  |
| -------- | ------- | ------- |
| Devs start here 👉  | [Devs Start here](#👋-hey-developers)    ||
| Node Runners starter 👉  | [Node Runners Start here](#👋-hey-node-runner-lets-start-here)    ||
| Gora AlgoKit Template  | [Gora AlgoKit](https://github.com/GoraNetwork/algokit_default_template)     ||
| Gora example boilerplate consumer App  | [Gora Consumer Example](https://github.com/GoraNetwork/oracle_consumer_app)     ||
| Gora example boilerplate feed  | [Gora Feed Example](https://github.com/GoraNetwork/oracle_consumer_app)     ||
| Gora off-chain example  | [Gora Off-Chain Example](https://github.com/GoraNetwork/off-chain-starter)     ||
| Learn Gora Blog |[Gora Blog](https://www.gora.io/blog-post-categories/blog)      ||
| Read Gora Articles |[Gora Articles](https://goranetwork.medium.com/)      ||



### 👋 Hey developers, let's start here

#### 👨🏽‍💻 Work with Gora Data Feeds
There are three categories of data feeds in the GoraNetwork ecosystem: price beacon, aggregated, and custom.
- Price Beacon:
  
    GoraNetwork's Price beacons are similar to the Oracle industry standard of providing data feeds available on a regular interval (e.g. a 1% deviation), in a publicly accessible contract. The current list of crypto spot prices can viewed on the Price Beacon page. While the price beacons are limited to high volume, popular assets, consumers can make request to any asset or data type by calling aggregated or custome endpoints.
- Aggregated Data:
  
    Aggregated data is obtained from institutional data providers who generally  aggregate high quality sources, and as such, already have significant data pre-processing for accuracy and reliability. GoraNetwork aggregates this aggregated data, ensuring the most accurate and availalbe data of any oracle. 
- Custom Data:
  
    Custom data feeds allow consumers to obtain data from their own endpoints. Calling custom data endpoints is very similar to calling aggregated data endponts. The process is described further in the following sections.

🚀[Continue reading on Data feeds...](./INTEGRATEDATAFEEDS.md)

#### 👩🏽‍💻 Validator Setup
GoraNetwork has it's own validator network and registery process which is done through [Gora Validators Portal](https://sandbox-app.goracle.io/validators). This guide provides a step-by-step process to register your validator node via the GoraNetwork Validator Portal. Registering your validator in this manner enables the community to delegation to your node.

🚀[Continue reading on validators...](./VALIDATOR.md)

### 👋 Hey Node Runner, let's start here

Note: This guide contains instructions for Algorand Node Running. Arbitrum and Polygon nodes are currently in Alpha. The following steps will remain the same by upcoming network additions, with criteria of using the relevant RPC or Node endpoint.

#### Introduction to GNR
Gora Node Runner (GNR) is the sofware implementing Gora network node. It performs the following key tasks:
Monitoring of Algorand blockchain for new oracle requests
Fulfilling of oracle requests by querying data sources and aggregating results
Submission of results for certification to Gora smart contracts
The GNR is distributed as a Linux-based Docker image. It is designed to run on a docker-enabled customer host or in AWS cloud. Instalation and maintenance of GNR is conducted with Gora CLI tool - a self-contained command-line executable. A live Algorand node is required by GNR to interact with the blockchain. A Gora node operator must have access to a publically available Algorand node or run one of their own.
Any problems encountered using Gora software as described in this manual, or errors in the manual itself, should be reported here: https://validators.gora.io/bugreport

🚀[Continue reading on GNR...](./NODERUNNER.md)







