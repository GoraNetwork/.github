<img src="https://avatars.githubusercontent.com/u/96357480?s=400&u=f54a2fab0e5faaf6bccf57b993e0a28ca2102001&v=4" width="100">  |  [◀️ GoraNetwork](https://github.com/GoraNetwork)|
| -------- | ------- |
## Gora Node Installation

1) Install AWS CLI tools:

Since Goracle uses a private Amazon ECR repository to host docker images, AWS CLI tools are necessary for local installs as well as cloud deployments. Refer to Amazon Getting started guide on how do download and install them.

2) Install Goracle CLI tool:
   
Goracle CLI tool is available as a self-contained executable for Windows or Linux:

- [Goracle CLI tool for Linux](https://github.com/GoracleNetwork/goracle-nr/raw/latest-release/bin/goracle)
- [Goracle CLI tool for Windows](https://github.com/GoracleNetwork/goracle-nr/raw/latest-release/bin/goracle.exe)

3) Configure Goracle Algorand accounts:
   
To ensure security of operator's staked assets, node Runner uses not one, but two Algorand accounts. Main account stakes operator's GORA tokens and receives all rewards earned by the node. Main account's private key is not exposed to the Node Runner. Participation account is auto-created by the Node Runner, then linked to the main account using Goracle CLI tool. Participation account has no access to the operator's stake or rewards, except the small amount accrued since the last time rewards were claimed. This keeps staked tokens and most of rewards safe in case the Node Runner instance is compromised.
To create and initialize participation and main accounts use Goracle Web app. This should leave you with an Algorand mnemonic and address for each account.

4) Create Goracle configuration file:
   
Goracle configuration file contains essential information specific to the node. When deploying a node to the cloud or launching it locally, Goracle CLI tool reads this file and passes its contents to the Node Runner via Docker container environment settings. The default location of this file is ~/.goracle. Now create and fill it as follows:
{
  "blockchain": {
    "mnemonic": "<Algorand mnemonic for your participation account>",
    "mainAddr": "<Algorand address of your main account>",
    "server": "<your Algorand node address>",
    "port": <your Algorand node port>,
    "token": "<your Algorand node access token>"
  }
}
Algorand node address must contain the protocol prefix, for example: "http://35.10.0.2". If the node uses a non-standard port, its number needs to be appended at the end after the usual ":".
After creating the file, set its access permissions to deny all access to anyone but yourself. On Linux, this is done by running: chmod 0600 ~/.goracle.

5a) Deploy and run Node Runner on AWS
If you prefer to run your node locally, skip to section 5b.
The deployment step is only necessary for AWS. When performed for the first time, it creates the AWS objects necessary to run the Node Runner as a service: a Fargate task definition, an ECS cluster and an ECS service. Subsequent deployments only update these objects.
To perform a deployment, run goracle deploy. If it is the first time, you should see an output like:
Using the only subnet "subnet-0caa1e1a9d9993a70"
Querying account ID
Registering task definition
Creating a new service
Warning! To prevent unintended service starts, the "deploy" command always leaves the service in a disabled state by setting "desiredCount" AWS parameter to 0, even if it was previously enabled (set to 1).
You must always enable the service after deployment by runnning goracle enable. This kicks off the AWS service startup process which can sometimes take up to a few minutes. You can keep an eye on it by running goracle status. Eventually, you should see an output like:
State: Running
Started at: 2022-07-04T17:33:08.803Z
Uptime: 2 min.
Task ID: edbfc8f709de4a299d0e781d207c1a01
IP: 3.122.97.201
The IP address above is the public address of your Goracle node. To check node status, point your browser to http://YOUR_NODE_IP:8000/ or inspect the logs using AWS CloudWatch logging service.

5b) Run Node Runner on any Docker-enabled host
When running a node for the first time or when updating it to the latest version, the Node Runner docker image needs to be pulled from Goracle ECR private registry. Run goracle docker-login to enable that. You should see an output like:
Logging into ECR with configured AWS credentials...
WARNING! Your password will be stored unencrypted in /home/smbdy/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store
Login Succeeded
This step needs not be repeated on subsequent runs.
If your Algorand node runs on the same host, the Node Runner docker process may not be able to connect to its local address (127.0.0.1 or similar). To work around that, specify Docker machine IP instead, which can be found out by running ifconfig docker0 on Linux.
To start Node Runner, execute goracle docker-start. When run for the first time, it will pull docker image from the registry. Then Node Runner will be launched, logging its messages to standard output, like:
2022-07-03T09:44:35.259Z INFO  Main address: P73FI2POGZOP7YBURWG3ODLKNDYZE5I4NU7TMWEFBLSXT4Q3ZTGM3HNJD4
2022-07-03T09:44:35.260Z INFO  Participation address: BHH6VTS2KSI3Q7KBTL55VZWDOQXUTLLGE2OL62HRHFJVJHSEZJPS6PHCBQ
2022-07-03T09:44:36.043Z INFO  Checking for blockchain events every 1000 ms.
2022-07-03T09:44:36.043Z INFO  Listening on 127.0.0.0:5001, network: 1
2022-07-03T09:44:36.352Z INFO  Starting web interface on port 8000
....

5) Stop a Node Runner instance
To stop an AWS-deployed Node Runner instance, run goracle disable. For stopping a locally run instance, hit Ctrl-C.
