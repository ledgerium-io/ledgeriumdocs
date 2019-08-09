# **Ledgerium Setup**
=====================
It **One-click Setup**
ledgerium setup repository is one click setup contains scripts which can be used to setup full or masternode setup. It contains 

install_depdendcies.sh is a unix bash file that downloads and deploys one Ledgerium node (consists of Geth, Tessera, and Governance Docker containers) in a single click.

To download the installer go to the console and type

git clone https://github.com/ledgerium-io/ledgeriumsetup.git
cd ledgeriumsetup
./install_dependencies.sh
This script does the following:

Install prerequisite softwares (Docker and NodeJS) to run ledgerium tools. Add $USER to docker group, to avoid using sudo before docker commands
Creates a docker network
Once the dependencies are done installing go ahead and run the node

```
./ledgerium_setup.sh
The script will prompt user for some input, example:

MODE: masternode
Number of Nodes: 1
Enter Mnemonic 0: xxxxxxxxxxxxxxxxxxxxxxxx
Enter Password 0: anyPassword
The script then creates a docker-compose file and brings up the containers
```

Confirm Masternode is Running
Go to testnet.ledgerium.net:3000