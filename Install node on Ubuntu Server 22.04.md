Tutorial - Install node on Ubuntu Server 22.04
Install a node for your coin on Ubuntu Server 22.04 with the following tutorial.

Update your Ubuntu server with the following command:

sudo apt-get update && sudo apt-get upgrade -y

Download the Linux daemon for your wallet with the following command:

wget "https://dl.walletbuilders.com/download?customer=9eeba63d7b18c6386b82aaf322bd328fd3f2870f6d00c2a21e&filename=neiro-daemon-linux.tar.gz" -O neiro-daemon-linux.tar.gz

Extract the tar file with the following command:

tar -xzvf neiro-daemon-linux.tar.gz

Download the Linux tools for your wallet with the following command:

wget "https://dl.walletbuilders.com/download?customer=9eeba63d7b18c6386b82aaf322bd328fd3f2870f6d00c2a21e&filename=neiro-qt-linux.tar.gz" -O neiro-qt-linux.tar.gz

Extract the tar file with the following command:

tar -xzvf neiro-qt-linux.tar.gz

Type the following command to install the daemon and tools for your wallet:

sudo mv neirod neiro-cli neiro-tx /usr/bin/

Create the data directory for your coin with the following command:

mkdir $HOME/.neiro

Open nano.

nano $HOME/.neiro/neiro.conf -t

Paste the following into nano.

rpcuser=rpc_neiro
rpcpassword=dR2oBQ3K1zYMZQtJFZeAerhWxaJ5Lqeq9J2
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
listen=1
server=1
txindex=1
daemon=1

Save the file with the keyboard shortcut ctrl + x.

Type the following command to start your node:

neirod