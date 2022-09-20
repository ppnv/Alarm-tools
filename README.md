# TenderDuty v2 - monitoring and alerting
![](https://user-images.githubusercontent.com/83868103/190860486-4dc9c7ac-8884-4e85-a643-7c9777e29536.png)
## Server preparation
```
sudo apt update && sudo apt upgrade -y
sudo apt install curl build-essential git wget jq make gcc tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```
## Docker installation
```
. <(wget -qO- https://raw.githubusercontent.com/SecorD0/utils/main/installers/docker.sh)
```
## Install tenderduty
```
mkdir tenderduty && cd tenderduty
docker run --rm ghcr.io/blockpane/tenderduty:latest -example-config >config.yml
```
Download config and edit it
```
wget -O $HOME/tenderduty/config.yml "https://raw.githubusercontent.com/lesnikutsa/lesnik_utsa/main/monitoring/TenderDuty(ru)/config.yml"
```
## Monitoring setup (commands are executed on the server with the node)
```
NODE_FOLDER=.haqq
TIKER=haqqd
WALLET=<wallet>
```
## Getting data to edit config.yml
```
echo -e "\033[0;32mprometheus_listen_port$(grep -A 8 "\[instrumentation\]" ~/$NODE_FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"
echo chain-id:" "$($TIKER status 2>&1 | jq ."NodeInfo" | jq ."network")
echo valoper_address:$($TIKER keys show $WALLET --bech val -a)
echo -e "\033[0;32mhttp://$(wget -qO- eth0.me)$(grep -A 3 "\[rpc\]" ~/$NODE_FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"
```
## On the Server with TenderDuty, you need to enter your data
```
nano $HOME/tenderduty/config.yml
```
## After setting up the config, run
```
docker run -d --name tenderduty -p "8888:8888" -p "28686:28686" --restart unless-stopped -v $(pwd)/config.yml:/var/lib/tenderduty/config.yml ghcr.io/blockpane/tenderduty:latest
```
## Get dashboard link
```
echo -e "\033[0;32mhttp://$(wget -qO- eth0.me):8888/\033[0m"
```
## Check logs
```
docker logs -f --tail 20 tenderduty
```
# 
