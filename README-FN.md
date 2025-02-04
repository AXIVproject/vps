# AXIV Fundamentalnode VPS Installation

This masternode and fundamentalnode installation script vastly simplifies the setup of a AXIV node running on a virtual private server (VPS), and it also adds a number of other powerful features, including:

* IPv6 Support
* Installs 1-100 (or more!) AXIV nodes in parallel on one VPS, with individual axiv.conf and data directories
* It can install nodes for other coins on the same VPS as AXIV
* 100% auto-compilation and 99% of configuration on the masternode side of things
* Automatically compiling from the latest AXIV release tag, or another tag can be specified
* Some security hardening is done, including firewalling and a separate user, increasing security
* Automatic startup for all masternode daemons

Some notes and requirements:

* Script has only been tested on a Vultr VPS, but should work almost anywhere where IPv6 addresses are available
* Currently only Ubuntu 16.04 Linux is supported
* This script needs to run as root or with sudo, the masternodes will and should not!

This project was forked from https://github.com/masternodes/vps. @marsmensch (Florian) is the primary author behind this VPS installation script.
# Install guide on Vultr

## How to get VPS server

For new fundamentalnode owners, **Vultr** is recommended as a VPS hosting provider, but other providers that allow direct root SSH login access and offer Ubuntu 16.04/18.04/20.04 may work.

You can use the following referral link to sign up with Vultr for VPS hosting:

<a href="https://www.vultr.com/?ref=7316561"><img src="https://www.vultr.com/media/banner_2.png" width="468" height="60"></a>

## Deploy a new system

First, create a new VPS by clicking that small "+" button.

<img src="docs/images/masternode_vps/deploy-a-new-system.png" alt="VPS creation" class="inline"/>

## Location choice

You can choose any location. You may wish to have it hosted in a city/country near you, or choose a different area to help with the global decentralization of the AXIV fundamentalnode network.

<img src="docs/images/masternode_vps/location-choice.png" alt="VPS location choice" class="inline"/>

## Linux distribution (Ubuntu 18.04 LTS)

Select Ubuntu 18.04.

<img src="docs/images/masternode_vps/linux-distribution--ubuntu-1604-lts-.png" alt="VPS location choice" class="inline"/>

## VPS size

The 25 GB SSD / 1024MBB Memory instance is enough for 2-3 masternodes until the blockchain grows very large. You may need more memory and disk space as the AXIV blockchain grows over time, or if you want to run more nodes.

<img src="docs/images/masternode_vps/vps-size.png" alt="VPS sizing" class="inline"/>

## Activating additional features (IPv6)

Toggle "Enable IPv6" to activate that feature--at Vultr there is no additional cost for this.

<img src="docs/images/masternode_vps/activating-additional-features--ipv6-.png" alt="VPS sizing" class="inline"/>

Optionally, you can enable DDOS Protection to protect your nodes against a potential denial of service attack--these are not common, but may offer additional protection to keep your nodes running. Vultr charges an additional fee for this.

## Hostnames & number of VPS

Choose how many instances you want and click "Deploy Now".

<img src="docs/images/masternode_vps/hostnames--amp--number-of-vps.png" alt="VPS sizing" class="inline"/>

## Installation of PuTTY as SSH client (Windows)
If you are running your wallet from Windows, install PuTTY while the server is being set up. You can download PuTTY from here: http://www.putty.org/. Skip this step if you are using a Mac--you will use the built in Terminal application instead.

Once PuTTY is installed, return to the Vultr dashboard to get the login details by clicking on the ... to the right of your server, and select Server Details.

## Accessing your VPS via SSH

Copy your password for SSH access from the server details page.
<img src="docs/images/masternode_vps/accessing-your-vps-via-ssh.png" alt="check hostname and password" class="inline"/>

Now open PuTTY to add the server.

<img src="docs/images/masternode_vps/login-to-vps-via-PuTTY.png" alt="login to VPS" class="inline"/>

Enter the IP address in the Host Name field, and enter the server name you wish to use for this VPS (e.g., axiv-fn-1) to Saved Sessions. Click save.

Click the open button. When the console has opened, click Yes in the PuTTY Security Alert box.
<img src="docs/images/masternode_vps/PuTTY-Security-Alert.png" alt="Alert from PuTTY" class="inline"/>

Now enter your server login details provided in your Vultr account.
You cannot Ctrl+V to paste in the console. Either right click the mouse or type shift+insert (sometimes
on keyboard it will just be INS key)

User: root
Password: (paste or type password)

When you paste it will not display, so don't try to paste again.
Just paste once and press Enter.

For Mac users, open Terminal (e.g., Press Command-Space and type Terminal and press Enter). Then type:

```
ssh -l root <IP address>
```
## Install Fundamentalnode

Login to your newly installed node as "root".

<img src="docs/images/masternode_vps/first-ssh-session.png" alt="VPS sizing" class="inline"/>

To install a single AXIV fundamentalnode, use this command:

```bash
git clone https://github.com/AXIVproject/vps.git && cd vps && ./install.sh -p axiv-fn --download https://github.com/AXIVproject/AXIV/releases/download/v1.0.2/axiv-1.0.2-x86_64-linux-gnu.tar.gz
```

If you have your fundamentalnode private key, please use this (you can generate your fundamentalnode private key with Step 2 below).

```bash
git clone https://github.com/AXIVproject/vps.git && cd vps && ./install.sh -p axiv --download https://github.com/AXIVproject/AXIV/releases/download/v1.0.2/axiv-1.0.2-x86_64-linux-gnu.tar.gz -k **PRIVATE KEY**
```

Using this alternate command that specifies the private key, you can skip "Configure node configuration files" below, because the command above has already added the node private key to the node configuration file.

Note: As new versions of AXIV are released, the link after the --download argument will need to be updated to point to the latest official release. Please check the offical AXIV github to verify that you are downloading the latest offical release at https://github.com/AXIVproject/AXIV/.

This prepares the system and installs the AXIV fundamentalnode daemon. This includes downloading the latest AXIV release, creating a swap file, configuring the firewall. This process should take less than a minute when using the ``--download`` option. If you omit the ``--download`` option, the script will compile the binaries using the AXIV github source code, which typically takes about 10-15 minutes.

<img src="docs/images/masternode_vps/install-the-desired-masternode-and-amount.png" alt="VPS configuration" class="inline"/>

For the next step, go back to your local desktop and open axiv-qt.

### More complex situations (ignore if you are installing a single fundamentalnode on a new VPS)

If you wish to install more than one fundamentalnode on the same VPS, you can add a -c parameter to tell the script how many to configure. For example, this would install three AXIV fundamentalnodes (all entered on one line):

```bash
git clone https://github.com/AXIVproject/vps.git && cd vps && ./install.sh -p axiv-fn --download https://github.com/AXIVproject/AXIV/releases/download/v1.0.2/axiv-1.0.2-x86_64-linux-gnu.tar.gz -c 3 
```

If you are installing a different number of fundamentalnodes, modify the number after -c accordingly.

If you already have your fundamentalnode private keys, you can add them as shown below (all entered on one line):

```bash
git clone https://github.com/AXIVproject/vps.git && cd vps && ./install.sh -p axiv-fn --download https://github.com/AXIVproject/AXIV/releases/download/v1.0.2/axiv-1.0.2-x86_64-linux-gnu.tar.gz -c 3 --key **PRIVATE KEY 01** --key2 **PRIVATE KEY 02** --key3 **PRIVATE KEY 03**
```

Using this command, you can skip the step for "Configure fundamentalnode configuration files", because the command above has already added the fundamentalnode private keys to the fundamentalnode configuration files.

If you are upgrading your node(s) to a new release, you should first remove the old version of the VPS script so that the new one you download is tagged with the latest version, and then add a -u parameter to tell the script to upgrade existing nodes:

```bash
rm -rf /root/vps
```
```bash
git clone https://github.com/AXIVproject/vps.git && cd vps && ./install.sh -p axiv-fn --download https://github.com/AXIVproject/AXIV/releases/download/v1.0.2/axiv-1.0.2-x86_64-linux-gnu.tar.gz -p axiv-fn -u
```

The project is configured to use the latest official release of the AXIV node code, and we will update this project each time a new release is issued, but without downloading the latest version of this project and using the -u parameter, the script will not update an existing AXIV node that is already installed.

## Configure AXIV Wallet
### Step1 - Create Collateral Transaction
Once the wallet is open on your local computer, select the 'Receive' tab and generate a new receive address for the amount of 10,000 AXIV. You may label it however you want to identify your node rewards (e.g., AXIV-FN-1). This label will show up in your transactions each time you receive a fundamentalnode block reward.


Click the Request payment button, and copy the address.

<img src="docs/images/masternode_vps/receive-address.png" alt="Generate Receive Address" class="inline"/>

Now go to the ‘Send' tab, paste the copied address, enter *exactly* 10,000 AXIV in the Amount field, check the Fundamentalnode Payment checkbox, click Send, enter your wallet passphrase if it is requested, and then click Yes when the dialog asks if you are sure. For a fundamentalnode, the AXIV will be burned and will leave behind a 0.1234 AXIV amount that will be locked and is needed to keep receiving AXIV fundamentalnode rewards. If you are setting up more than one node at a time, repeat this process for each one.

*You must wait for at least one confirmation (approx. 1 minute) on the blockchain before moving on to the next step!*

<img src="docs/images/masternode_vps/send-fn-collateral.png" alt="Send Fundamentalnode Collateral" class="inline"/>

### Step 2 - Local PC Node Setup

After waiting for at least one confirmation on the collateral transaction we just created, select Tools->Debug Console from the menu to open the debug console.

Type the following commands in the debug console:
```
fundamentalnode genkey
fundamentalnode outputs
```

Copy and record this information in a notepad or other text editor file. The output from the ``fundamentalnode genkey`` command will need to be added to both the local PC's fundamentalnode.conf file and the VPS node's configuration file. The output from the ``fundamentalnode outputs`` command will need to be added to the local PC's fundamentalnode.conf file.

## End of installations

When the script finishes, it will look similar to this:

<img src="docs/images/masternode_vps/vps-script-complete.png" alt="Send Fundamentalnode Collateral" class="inline"/>

You only have a few steps remaining to complete your fundamentalnode configuration.

## Configure fundamentalnode Configuration Files

Since this installation method supports multiple fundamentalnodes, the AXIV configuration files have a node number added to them (e.g., axiv_n1.conf, axiv_n2.conf), stored in the /etc/masternodes directory. If you have a single fundamentalnode on the VPS, you will only need to edit /etc/masternodes/axiv_n1.conf.

To open axiv_n1.conf for editing on the VPS, enter these commands (in Putty or terminal):

```sudo apt-get install nano```

```nano /etc/masternodes/axiv_n1.conf```

You will see something similar to this:

<img src="docs/images/masternode_vps/nano-fn-conf.png" alt="Edit VPS Fundamentalnode Configuration File" class="inline"/>

Following the steps above, you only need to change the fundamentalnode private key. We recommend using IPv6 which is the default, but if you choose IPv4 when you ran the installation script, please replace each place you see ``[#NEW_IPv4_ADDRESS_FOR_MASTERNODE_NUMBER:::1]`` with your VPS IP address, which can be found by running ``ifconfig`` at the VPS command prompt and looking in the ``emp1so`` section for the value after ``inet`` that looks like four numbers separated by periods. 

Replace HERE_GOES_YOUR_FUNDAMENTALNODE_KEY_FOR_FUNDAMENTALNODE_axiv_1 with the copy of your ‘Priv Key’ from the local wallet.

![alt text](https://i.imgur.com/D6Z8sIe.png "Logo Title Text 1")

Once you have your fundamentalnode private key entered, press ‘Ctrl+X’ to exit, press ‘Y’ to save when prompted, and press ‘Enter’ to exit.


Copy the IP from ```fundamentalnodeaddr=``` (highlighted in red in the image above) and paste it into the same place you recorded the fundamentalnode private key and . Every field should now be full. Copy your ‘Priv Key’ (we will need this soon) and click ‘OK’.

Once you exit the configuration screen, you can start your fundamentalnode at the command prompt with this command:

```
activate_masternodes_axiv
```
The masternode daemons will start and begin loading the AXIV Blockchain.

## Edit fundamentalnode.conf file

The next step will be to add your fundamentalnode information to your local PC's fundamentalnode.conf file.

From your AXIV wallet on your PC, go to the menu and select Tools->Open Fundamentalnode Configuration File. This will open the file in the default text editor if one is set up for .conf files (typically on Windows this is set up to use Notepad).

The format of each entry is as follows, one line per fundamentalnode, with each item separated by one space:

<Alias> <IP Address:Port> <Fundamentalnode Private Key> <Transaction Output TXID> <Transaction Output Index>

- Alias: This can be whatever you want to identify the fundamentalnode, but typically would be the same as what was used in the Receive step above (e.g., AXIV-FN-1)
- IP Address:Port: This would use the IP address copied from the VPS, and the port would always be 10135, with a colon in between the IP address and the port
- Fundamentalnode Private Key: This is the output copied from the ``fundamentalnode genkey`` command
- Transaction Output TXID: This is the long string from the "txhash" part of output from the ``fundamentalnode outputs`` command, between double quotes
- Transaction Output Index: This is typically a single number, often 0 or 1 (but it can be higher), coming from the "outputidx" part of the output from the ``fundamentalnode outputs`` command, again between double quotes

For example, if you recorded the following:
- IP Address: 43.31.243.219
- fundamentalnode genkey: 8C8cXXwn6bGxtaMJKkQweXeqfNDN1WgHCZU3yeeuGzUgtqqJmh1
- fundamentalnode outputs: 
```
  {
    "txhash": "e6337aa1c800852eb7c8c26e08b330a76dcbb888e424b3c6f62750a9290248fb",
    "outputidx": 1
  }
```

Then the line that should be added to fundamentalnode.conf would read:

``AXIV-FN-1 43.31.243.219:10135 8C8cXXwn6bGxtaMJKkQweXeqfNDN1WgHCZU3yeeuGzUgtqqJmh1 e6337aa1c800852eb7c8c26e08b330a76dcbb888e424b3c6f62750a9290248fb 1``

This would all be entered on a single line with exactly 1 space separating each item. If you have more than one fundamentalnode being controlled by that wallet, then there would be sequential lines in the file, one for each fundamentalnode.

Add the appropriate line(s), save the file, exit from the text editor, close the AXIV wallet and then restart it.
## Check Syncing Status of Masternode

The fundamentalnode cannot complete activation until it is fully synced with the AXIV blockchain network.

To check the status of your fundamentalnode, you can enter the command below in the VPS terminal. If you have multiple masternodes on the same VPS, you can change n1 to n2 etc. to check the status of each one.

```/usr/local/bin/axiv-cli -conf=/etc/masternodes/axiv_n1.conf getinfo```

The output will look like this:

```{
  "version": 1000200,
  "protocolversion": 71025,
  "walletversion": 61000,
  "balance": 2402646.21090299,
  "blocks": 103752,
  "timeoffset": 0,
  "connections": 7,
  "proxy": "",
  "difficulty": 39438.2266625039,
  "testnet": false,
  "moneysupply": 7598551.70426021,
  "keypoololdest": 1621182523,
  "keypoolsize": 1001,
  "unlocked_until": 0,
  "paytxfee": 0.00000000,
  "relayfee": 0.00010000,
  "staking status": "Staking Active",
  "errors": ""
}
```

The blocks line above needs to be the latest block in the blockchain. You can check your local wallet to see the latest block by hovering over the yellow check mark in the lower right corner of the wallet.

Once your fundamentalnode has synced up to the latest block, go to next step. The syncing process may take 15-30 minutes, or more, as the AXIV Blockchain grows. You can keep checking progress with the command above, by pressing the ‘Up’ arrow and the ‘Enter’ key to repeat it (or by pasting the command in again).

## Start Fundamentalnode

When your fundamentalnode is fully synced on the VPS, go back to the debug console of the wallet (Tools->Debug Console), and type the following command, replacing AXIV-FN-1 with the alias you chose to put in the fundamentalnode.conf file:

```
startfundamentalnode alias false "AXIV-FN-1"
```

Once you have done this, you should get a confirmation that the Masternode has started successfully!

If you go back to the Nodes tab of the wallet and click on FUNDAMENTAL NODES, the ‘Status’ for the fundamentalnode should now say ‘Enabled’, and within an hour or so the timer in the ‘Active’ column should start increasing.

Congratulations, your AXIV fundamentalnode is now set up and running! Depending on how many fundamentalnodes there are, it may take 12-24 hours before you see your first fundamentalnode reward. This is normal and rewards should come at more regular intervals after the first one.

## Issues and Questions

Please open a GitHub Issue  here: https://github.com/AXIVproject/vps/issues if there are problems with this installation method. AXIV team members actively support people installing fundamentalnodes and can provide assistance.
