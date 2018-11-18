# Setup a Fabric network

This section will create a Fabric network. 

## Pre-requisites - Cloud9
We will use Cloud9 to provide a Linux shell.

1. Spin up a [Cloud9 IDE](https://us-east-1.console.aws.amazon.com/cloud9/home?region=us-east-1) from the AWS console.
In the Cloud9 console, click 'Create Environment'
2. Provide a name for your environment, e.g. fabric-c9, and click **Next Step**
3. Leave everything as default and click **Next Step**
4. Click **Create environment**. It would typically take 30-60s to create your Cloud9 IDE
5. In the Cloud9 terminal, in the home directory, clone this repo:

```
cd
git clone https://github.com/aws-samples/non-profit-blockchain.git
```

## Step 0 - in Cloud9
Configure your Fabric network name and other items.

The config for your Fabric network can be configured in the file `0-exports.sh`. This file
exports ENV vars used by the other scripts. If you exit your session and need to restart,
you can source this file again. Some statements in the file may fail, depending on how far along
the process your are of creating your Fabic network (I.e. some components may not exist yet), but
the script will export the values it can find.

You may need to edit this file and add in the `NETWORKID` and `NETWORKMEMBERID`, if you have
already created the Fabric network.

```
cd ~/non-profit-blockchain/ngo-fabric
vi 0-exports.sh
```

Execute the following script:

```
cd ~/non-profit-blockchain/ngo-fabric
source ./0-exports.sh
```

## Step 1 - in Cloud9
Create the Fabric network. 

Execute the following script:

```
cd ~/non-profit-blockchain/ngo-fabric
./1-fabric-network.sh
```

## Step 2 - in Cloud9
Create the Fabric client node, which will host the Fabric CLI. You will use the CLI to administer
the Fabric network. The Fabric client node will be created in its own VPC, with VPC endpoints 
pointing to the Fabric network you created in Step 1.

Execute the following script:

```
cd ~/non-profit-blockchain/ngo-fabric
./2-vpc-client-node.sh
```

Check the progress in the AWS CloudFormation console

## Step 3 - SSH into the EC2 Fabric client node
Setup the Fabric client node. This step installs the necessary packages.

SSH into the Fabric client node. The key should be in your home directory.

```
ssh ec2-user@<dns of EC2 instance> -i ~/<Fabric network name>-keypair.pem
```

Clone the repo:

```
cd
git clone https://github.com/aws-samples/non-profit-blockchain.git
```

Execute the following script:

```
cd ~/non-profit-blockchain/ngo-fabric
./3a-client-node-setup.sh
```

Now exit your SSH session and reconnect.

Execute the following script:

```
cd ~/non-profit-blockchain/ngo-fabric
./3b-client-node-setup.sh
```

Now exit your SSH session.

## Step 4 - in Cloud9
Make sure you are back in Cloud9.

Create the Fabric peer.

Execute the following script:

```
cd ~/non-profit-blockchain/ngo-fabric
./4-fabric-peer.sh
```

## Step 5 - in Fabric client node
Prior to executing any commands in the Fabric client node, execute the script to export the ENV values
we need to connect to the Fabric network. This will need to be done each time you exit the SSH session and reconnect.

```
./exports.sh
```

Generate the configtx channel configuration

Execute the following script:

```
cd ~/non-profit-blockchain/ngo-fabric
./5-configtx.sh
```

## Step 6 - in Fabric client node
Create a Fabric channel.

Execute the following script:

```
cd ~/non-profit-blockchain/ngo-fabric
./6-channel.sh
```

## Step 7 - in Fabric client node
Join peer to Fabric channel.

Execute the following script:

```
cd ~/non-profit-blockchain/ngo-fabric
./7-join.sh
```

## Step 8 - in Fabric client node
Install chaincode on Fabric peer.

Execute the following script:

```
cd ~/non-profit-blockchain/ngo-fabric
./8-install.sh
```

## Step 9 - in Fabric client node
Instantiate chaincode on Fabric channel.

Execute the following script:

```
cd ~/non-profit-blockchain/ngo-fabric
./9-instantiate.sh
```

## Step 10 - in Fabric client node
Query the chaincode on Fabric peer.

Execute the following script:

```
cd ~/non-profit-blockchain/ngo-fabric
./10-query.sh
```

## Step 11 - in Fabric client node
Invoke a Fabric transaction.

Execute the following script:

```
cd ~/non-profit-blockchain/ngo-fabric
./11-invoke.sh
```

## Step 10 - in Fabric client node
Query the chaincode on Fabric peer and check the change in value.

Execute the following script:

```
cd ~/non-profit-blockchain/ngo-fabric
./10-query.sh
```