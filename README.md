# Ritual-Infernet Devnettt
![image](https://github.com/user-attachments/assets/31e672e1-46c8-420a-9614-bba64febcb3e)

Ritual building an execution layer for crypto AI which allow devs to run ML model outputs in smart contracts while managing the computation offchain through network of nodes which would be get paid in ritual tokens in rewards for computation, and proof of computation they would generate. -@hmalviya9

![image](https://github.com/user-attachments/assets/40128c7c-5278-471a-822c-947e8f17f7dd)

### Node Specs
![image](https://github.com/user-attachments/assets/50bd2689-cc14-4056-8c07-1bbc5d317cfc)

### Pre-Requisite

#### Create Custom Base RPC url

Login to https://dashboard.alchemy.com/ and create Base mainnet endpoint

![image](https://github.com/user-attachments/assets/3e16122d-a24b-4dd2-98a5-0fc73715fc01)


Go to network tab and note down your BASE mainnet RPC

![image](https://github.com/user-attachments/assets/a2c58742-a752-4f5d-8fcd-d241304b545f)

#### Fund wallet with $5-10 worth of BASE_ETH for gas

#### Install Tools

Login to your Vm

```
sudo su -
sudo apt update && sudo apt upgrade -y
sudo apt -qy install curl git jq lz4 build-essential screen
```
#### Install Docker
```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```
sudo apt-get update
```
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

#### Install Docker Compose

```
sudo curl -L "https://github.com/docker/compose/releases/download/2.28.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
```
sudo chmod +x /usr/local/bin/docker-compose
```
```
docker compose version
```
### Preparing and deploying the Containers

```
git clone https://github.com/ritual-net/infernet-container-starter
cd infernet-container-starter
```
```
screen -S ritual
```
```
docker pull ritualnetwork/hello-world-infernet:latest
```
```
project=hello-world make deploy-container
```
ctrl+a+d to detach

### Node Configuration

Edit below files one by one and update these variables where ever applicable

```
RPC URL: <your alchemy RPC>
Private Key: Enter your private key. Add “0x” to your key if it does not start with 0x.
Registry: 0x3B1554f346DFe5c482Bb4BA31b880c1C18412170
```
```
"trail_head_blocks": 3
```
```
"sleep": 3,
"batch_size": 9500,
"starting_sub_id": 180000 ,
"sync_period": 30
```
![image](https://github.com/user-attachments/assets/ab3845c2-545a-42bd-8810-a93ea04f7465)

```
nano ~/infernet-container-starter/deploy/config.json
```
```
nano ~/infernet-container-starter/projects/hello-world/container/config.json
```
```
nano ~/infernet-container-starter/projects/hello-world/contracts/script/Deploy.s.sol
```
```
nano ~/infernet-container-starter/projects/hello-world/contracts/Makefile
```
#### Update Node Version

```
nano ~/infernet-container-starter/deploy/docker-compose.yaml
```
![image](https://github.com/user-attachments/assets/5fb08abd-58dd-4cda-8bf5-35d0b2fc224d)

#### Restart container

```
docker ps -a
```
![image](https://github.com/user-attachments/assets/24061ec0-a0e2-40e6-8795-a4a0110be88f)

```
docker restart hello-world infernet-node infernet-anvil infernet-redis infernet-fluentbit
```
```
docker logs -f -n 100 infernet-node
```
![image](https://github.com/user-attachments/assets/d4ee502e-cdc4-4870-8d8f-416f7d0be181)

### Install Foundry

```
cd
mkdir foundry && cd foundry
curl -L https://foundry.paradigm.xyz | bash
```
```
source ~/.bashrc
foundryup
```
```
cd ~/infernet-container-starter/projects/hello-world/contracts
```
```
rm -rf lib/forge-std
rm -rf lib/infernet-sdk
```
```
forge install --no-commit foundry-rs/forge-std
forge install --no-commit ritual-net/infernet-sdk
```
```
foundryup
```

```
cd ../../../
```
```
curl -L https://foundry.paradigm.xyz | bash
foundryup
```
```
screen -r ritual
```
### Deploy and call Contract

```
project=hello-world make deploy-contracts
```
![image](https://github.com/user-attachments/assets/58f70a79-6bca-4b31-9b15-1527f347ceb3)

Copy the contract address

Also you can find the contract address from https://basescan.org/address/<wallet>

```
nano ~/infernet-container-starter/projects/hello-world/contracts/script/CallContract.s.sol
```
Replace your contract address in SaysGM() function

```
project=hello-world make call-contract
```
![image](https://github.com/user-attachments/assets/63e8b641-c37d-486a-8218-048fd4bc6016)

Join discord https://discord.gg/4MPV7YvR and ask questions if you face any issue.

Best practices for freeing up disk space:

docker compose down/up to free up some space
recommended spec is 500GB SSD so make sure to upgrade your infra
docker container prune -f
docker image prune -f
docker volume prune -f (Please keep in mind that the last command docker volume prune -f will delete all stored data. You will loose all you users and scans.)
if all else fails, wipe server and install node again

Here's also the recommendation from the team:

The increasing storage is a result of the Anvil node. It's an Ethereum node which is only used for local development. 

It is not recommended to have long-running Infernet node deployments with the anvil node. If your node requires an on-chain connection, please use an RPC address to that chain and remove the anvil-node from the docker-compose file.

