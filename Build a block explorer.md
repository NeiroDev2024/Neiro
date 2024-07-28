Tutorial - Install a block explorer on Ubuntu Server 22.04
Install a block explorer on Ubuntu Server 22.04 with the following tutorial.

Update your Ubuntu server with the following command:

sudo apt-get update && sudo apt-get upgrade -y

Install the dependencies with the following command:

sudo apt-get install gnupg2 nodejs npm git nano cmake screen unzip -y

Import the MongoDB GPG key:

wget -nc https://www.mongodb.org/static/pgp/server-6.0.asc
cat server-6.0.asc | gpg --dearmor | sudo tee /etc/apt/keyrings/mongodb.gpg >/dev/null

Install the MongoDB repository with the following command:

sudo sh -c 'echo "deb [ arch=amd64,arm64 signed-by=/etc/apt/keyrings/mongodb.gpg] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" >> /etc/apt/sources.list.d/mongo.list'

Update your Ubuntu server with the following command:

sudo apt-get update -y

Install MongoDB with the following command:

sudo apt install mongodb-org -y

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

Type the following command to open your home directory:

cd $HOME

Create the data directory for your coin with the following command:

mkdir $HOME/.neiro

Open nano.

nano $HOME/.neiro/neiro.conf -t

Paste the following text into nano.

rpcuser=rpc_neiro
rpcpassword=dR2oBQ3K1zYMZQtJFZeAerhWxaJ5Lqeq9J2
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
listen=1
server=1
txindex=1
daemon=1
addnode=node3.walletbuilders.com

Save the file with the keyboard shortcut ctrl + x.

Type the following command to start your daemon:

neirod

Type the following command to start MongoDB:

sudo systemctl start mongod

Type the following command to open MongoDB:

mongosh

Type the following command to create a MongoDB database named “explorerdb”:

use explorerdb

Type the following command to create a MongoDB user named “iquidus”:

db.createUser( { user: "iquidus", pwd: "414uq3EhKDNX76f7DZIMszvHrDMytCnzFevRgtAv", roles: [ "readWrite" ] } )

Type the following command to close MongoDB:

exit

Type the following command to clone iquidus-explorer:

git clone https://github.com/walletbuilders/explorer.git explorer

Type the following command to install iquidus-explorer:

cd explorer && npm install --production

Type the following command to create the file settings.json:

cp ./settings.json.template ./settings.json

Open nano.

nano settings.json -t

Modify the following values in the file settings.json

title - “IQUIDUS” -> “Neiro”.

address - Change the value “127.0.0.1” with the IPv4 address of your server.

coin - “Darkcoin” -> “Neiro”.

symbol - “DRK” -> “NEIRO”.

password - “3xp!0reR” -> “414uq3EhKDNX76f7DZIMszvHrDMytCnzFevRgtAv”.

port - “9332” -> “18615”.

user - “darkcoinrpc” -> “rpc_neiro”.

pass - 123gfjk3R3pCCVjHtbRde2s5kzdf233sa” -> “dR2oBQ3K1zYMZQtJFZeAerhWxaJ5Lqeq9J2”.

confirmations - “40” -> “20”.

api - “true” -> “false”.

markets - “true” -> “false”.

twitter - “true” -> “false”.

Save the file with the keyboard shortcut ctrl + x.

Type the following command to open a screen session:

screen

Type the following commands to start your block explorer:

cd $HOME/explorer
npm start

Press the keyboard shortcut ctrl + a + d to disconnect from your screen session.

Type the following command to open crontab:

crontab -e

Press the Page Down key on your keyboard PgDown.

Paste the following text into crontab.

@reboot neirod
*/1 * * * * cd $HOME/explorer && /usr/bin/nodejs scripts/sync.js index update > /dev/null 2>&1
*/5 * * * * cd $HOME/explorer && /usr/bin/nodejs scripts/peers.js > /dev/null 2>&1

Save the crontab with the keyboard shortcut ctrl + x

Confirm that you want to save the crontab with the keyboard shortcut y + enter

The block explorer is accessible on http://replace_with_your_ip:3001
