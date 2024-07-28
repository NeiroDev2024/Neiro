Tutorial - Mine for blocks with macOS
Mine for blocks with your macOS wallet and the following instructions.

Open Spotlight Search and type the following:

terminal

Double click on terminal.

Execute the following command, to open your downloads directory:

cd Downloads

Install Homebrew with the following command:

/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"

Enter your sudo password to install Homebrew.

Install wget with the following command:

brew install wget

Download your macOS wallet with the following command:

wget "https://dl.walletbuilders.com/download?customer=9eeba63d7b18c6386b82aaf322bd328fd3f2870f6d00c2a21e&filename=neiro-qt.dmg" -O neiro-qt.dmg

Download the macOS tools for your wallet with the following command:

wget "https://dl.walletbuilders.com/download?customer=9eeba63d7b18c6386b82aaf322bd328fd3f2870f6d00c2a21e&filename=neiro-tools-macos.tar.gz" -O neiro-tools-macos.tar.gz

Extract the tar file with the following command:

tar -xzvf neiro-tools-macos.tar.gz

Create the data directory for your coin with the following command:

mkdir "$HOME/Library/Application Support/Neiro/"

Open nano.

nano "$HOME/Library/Application Support/Neiro/neiro.conf" -t

Paste the following into nano.

rpcuser=rpc_neiro
rpcpassword=dR2oBQ3K1zYMZQtJFZeAerhWxaJ5Lqeq9J2
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
listen=1
server=1
addnode=node3.walletbuilders.com

Save the file with the keyboard shortcut ctrl + x.

Open nano.

nano mine.sh -t

Paste the following into nano.

#!/bin/bash
SCRIPT_PATH=`pwd`;
cd $SCRIPT_PATH
echo Press [CTRL+C] to stop mining.
while :
do
./neiro-cli generatetoaddress 1 $(./neiro-cli getnewaddress)
done

Save the file with the keyboard shortcut ctrl + x.

Make the file executable.

chmod +x mine.sh

Open your downloads directory in Finder.

Install your macOS wallet with the file neiro-qt.dmg.

Open your wallet.

Go back to the terminal and type the following command to create the wallet file for your desktop wallet:

./neiro-cli createwallet ""

Execute the following command in terminal to mine your first block:

./mine.sh
