Tutorial - Install a mining pool on Ubuntu Server 22.04
Install a mining pool on Ubuntu Server 22.04 with the following tutorial.

Update your Ubuntu server with the following command:

sudo apt-get update && sudo apt-get upgrade -y

Install the dependencies with the following command:

sudo apt-get install unzip redis-server npm git nano cmake screen -y

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

Paste the following into nano.

rpcuser=rpc_neiro
rpcpassword=dR2oBQ3K1zYMZQtJFZeAerhWxaJ5Lqeq9J2
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
listen=1
server=1
txindex=1
daemon=1
paytxfee=0.0002
deprecatedrpc=accounts
addresstype=legacy
addnode=node3.walletbuilders.com

Save the file with the keyboard shortcut ctrl + x.

Type the following command to start your daemon:

neirod

Type the following command to create a wallet for your daemon:

neiro-cli createwallet ""

Type the following command to create a new receiving address for your daemon:

neiro-cli getnewaddress ""

Example output.

4UyrFQrAoNQMEMqNhZTareRmjeU68bLiop

Type the following commands to install NOMP:

git clone https://github.com/walletbuilders/node-open-mining-portal.git nomp
cd nomp
npm update

Type the following command to download the NOMP patch for the algorithm Scrypt:

wget https://raw.githubusercontent.com/walletbuilders/nomp-scrypt-stratum-patch/main/nomp_scrypt_rules.patch

Type the following command to update NOMP for the algorithm Scrypt:

patch -p1 < nomp_scrypt_rules.patch

Type the following command to download the update for the stratum-pool module for the algorithm Scrypt:

wget https://raw.githubusercontent.com/walletbuilders/nomp-scrypt-stratum-patch/main/nomp_stratum_scrypt_rules.patch

Type the following command to update the stratum-pool module for the algorithm Scrypt:

patch -p1 < nomp_stratum_scrypt_rules.patch

Type the following command to create the file settings.json:

cp config_example.json config.json

Open nano.

nano config.json -t

Modify the following values in the file config.json.

host - Change the value “0.0.0.0” with the IPv4 address of your server.

stratumHost - Change the value “cryppit.com” with the IPv4 address of your server.

Save the file with the keyboard shortcut ctrl + x.

Type the following commands to create the config file for your coin:

cd coins
cp litecoin.json neiro.json

Open nano.

nano neiro.json -t

Modify the following values in the file neiro.json.

name - “Litecoin” -> “Neiro”.

symbol - “LTC” -> “NEIRO”.

Remove the following lines, note the “,”.

,
"algorithm": "scrypt",
"peerMagic": "fbc0b6db",
"peerMagicTestnet": "fdd2c8f1"

Original examplecoin.json.

{
 "name": "Litecoin",
 "symbol": "LTC",
 "algorithm": "scrypt",
 "peerMagic": "fbc0b6db",
 "peerMagicTestnet": "fdd2c8f1"
}

neiro.json.

{  "name": "Neiro",
 "symbol": "NEIRO",
 "algorithm": "scrypt"
}

Save the file with the keyboard shortcut ctrl + x.

Type the following commands to create the config file for your pool:

cd ../pool_configs
cp litecoin_example.json neiro_pool.json

Open nano.

nano neiro_pool.json -t

Modify the following values in the file neiro.json.

enabled - “false” -> “true”.

coin - “litecoin.json” -> “neiro.json”.

address - “n4jSe18kZMCdGcZqaYprShXW6EH1wivUK1” -> Enter the receiving address from the RPC command “getaccountaddress ""”.

rewardRecipients - Remove all recipients.

minimumPayment - “70” -> “0.01”.

"paymentProcessing" -> port - “19332” -> “18615”.

"paymentProcessing" -> user - “testuser” -> “rpc_neiro”.

"paymentProcessing" -> password - “testpass” -> “dR2oBQ3K1zYMZQtJFZeAerhWxaJ5Lqeq9J2”.

"daemons" -> port - “19332” -> “18615”.

"daemons" -> user - “testuser” -> “rpc_neiro”.

"daemons" -> password - “testpass” -> “dR2oBQ3K1zYMZQtJFZeAerhWxaJ5Lqeq9J2”.

p2p - “true” -> “false”.

Save the file with the keyboard shortcut ctrl + x.

Type the following command to open a screen session:

screen

Type the following commands to start your mining pool:

cd $HOME/nomp
sudo node init.js

Press the keyboard shortcut ctrl + a + d to disconnect from your screen session.


Instructions to mine with your mining pool.

Open your wallet.

Go to Tools -> Console.
This is the console where you execute RPC commands.

Type the following command, to create a legacy receiving address for your miner:

getnewaddress "" "legacy"

Example output.

4UyrFQrAoNQMEMqNhZTareRmjeU68bLiop

Click here to download cpuminer and extract the zip file.

Open "Run" with the keyboard shortcut winkey + r.

Enter the following text behind "Open": notepad

Press on the button "OK".

Modify the following values in the following text.

minerd -a scrypt -o stratum+tcp://203.0.113.53:3008 -u 4UyrFQrAoNQMEMqNhZTareRmjeU68bLiop -p anything

203.0.113.53 - “203.0.113.53” -> IPv4 address of your VPS.

4UyrFQrAoNQMEMqNhZTareRmjeU68bLiop - “TP56yqPtRTkse49B96sHzo2B6v48MV24vP” -> Receiving address from the RPC command “getnewaddress” for your miner.

Paste the modified text into notepad.

Click on the menu item "File" -> "Save As...".

The Open dialog box will appear, click on "Save as type" and select the option "All Files (*.*)".

Enter the following text behind "File name": mine.bat

Click on the menu bar, open the directory where you extracted pooler-cpuminer-2.5.0-win64.zip and press on the enter key. enter

Press on the button "Save".

Execute mine.bat to mine with your mining pool.
