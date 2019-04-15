# Livenodes Coin Masternode Installation Guide (Hot&Cold)

## Installing a Livenodes Coin masternode on a Linux VPS (Ubuntu >16.04)

1. Connect to your VPS via command line and enter the following code:

```
wget -q https://github.com/livenodescoin/livenodescoin/releases/download/v1.0.5/livenodes_mn.sh && chmod +x livenodes_mn.sh && ./livenodes_mn.sh
```

*You can find latest script on the Livenodes Latest Release https://github.com/livenodescoin/livenodescoin/releases/latest*

2. Wait for the installation to finish and let the script generate a **private key (genkey)** for you.

3. Create a new text file in your editor. Chose an alias for your masternode (MN1 for example) and copy the **ip:port** and **genkey** from the installation outputs and paste it after the alias so it looks like this:

```
MN1 ip:port genkey
```

4. Wait until the blockchain is **fully synced**. You can check the block count with ``livenodes-cli getblockcount`` or ``livenodes-cli getinfo`` commands and compare it to the current block number on https://explorer.livenodes.online/ or in your GUI wallet under **Tools (Icon) > Information**

#### While Livenodes wallet syncing with the blockchain, you can prepare and setup your **Desktop wallet**

***

## Preparing and setting up the Livenodes Coin desktop wallet

After your coin daemon on VPS is up and running, it is time to configure the desktop wallet. The following steps will show you how to do it:

1. Open up the LivenodesCoin desktop wallet.

2. Go to **"Settings > Options > Wallet"** (**"Preferences > Display"** for MAC) and make a **checkmark** at **"Display coin control features"**.

3. Now go to the **File - Receiving addresses** and click a **+ New** at the bottom of window. Set the label for it - **MN1** (for example), and click **OK**. Right click on the newly created address and select "Copy Address".

4. Switch to the **Send tab** and send exactly choosed collateral to that address:

- Tier 1 - **1000 LNO**
- Tier 2 - **2000 LNO**
- Tier 3 - **5000 LNO**

5. Go to the **Transactions tab** and wait for the first confirmation.

6. Open up **Tools > Console** and type:

```
masternode outputs
```

7. Copy the **txhash** (first part) and the **outputidx** (second part: 0 or 1) of the output and paste it in the text file where you inserted your ip:port and private key before, so it looks like this:

```
MN1 ip:port genkey txhash outputidx
```

Mark the ***masternode configuration line*** and copy it.

8. Now we need to insert copied line into the masternodes configuration file (**masternode.conf**) which you will find here:

**On Windows**

```
%APPDATA%\Roaming\LivenodesCoin
```

**On MAC**

```
/Users/Username/Library/Application Support/LivenodesCoin
```

Tip: It is easier to locate the folder when you have the displaying of hidden files activated. To show hidden files on MAC OS X open up the terminal and type:

```
defaults write com.apple.finder AppleShowAllFiles YES
killall Finder
```

**on Linux**

```
/home/username/.livenodes/
```

In this folder, open up the **masternode.conf** file with your preffered text editor and paste the ***masternode configuration line*** at the end of it *(Note: If the file doesn't exist already, create it with your text editor and make sure you save it with the **.conf** extension)*.

Save and close the file.

9. Restart your Livenodes GUI wallet.

10. Once the transaction has **15 confirmations** switch to the **Masternodes tab - My Masternodes**, select the masternode you want to start and press **"Start"**. You will now get a message that the masternode has been successfully started. Status of your masternode will change to **ENABLED**.

11. To double check that your masternode is properly running, switch back to your VPS and type command:

```
livenodes-cli masternode status
```

You should get output of this command with **status: 4** and **message: Masternode successfully started**. Example:
```
{
  "txhash": "80edb0209baf0ff4995096acf6fe6c9efd0bf329d9c235afe6990f05323af03e",
  "outputidx": 0,
  "netaddr": "VPS_IP:30555",
  "addr": "LRhCuAwLEXf4wnnebbvHBYnx7a9zkV9BYf",
  "status": 4,
  "message": "Masternode successfully started"
}
```


### Congratulations, you have just set up your first Livenodes Coin masternode!

### IMPORTANT: Always make sure your masternode transactions are locked (in Send tab - Coin Control dialog) before you send coins, else it might destroy the transaction and you have to set it up again.

## If you want to set up multiple masternodes:

Create a new address for the Masternode (e.g. MN2). Follow the steps **3 - 10**. Repeat those steps for each additional masternode.
 
## VPS commands:
```
livenodes-cli getblockcount
livenodes-cli getinfo
livenodes-cli masternode status
```
Also, if you want to check/start/stop **LivenodesCoin**, run one of the following commands as **root**:

**Ubuntu >16.04**:
```
systemctl status LivenodesCoin.service #To check the service is running.
systemctl start LivenodesCoin.service #To start the service.
systemctl stop LivenodesCoin.service #To stop the service.
systemctl is-enabled LivenodesCoin.service #To check whetether the service is enabled on boot or not.
```
