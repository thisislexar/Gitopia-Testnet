<h1 align="center">Gitopia Testnet Installation Guide

## Hi everyone, today we will join Gitopia Janus Testnet 2. Don't forget to star and fork this repository from the top right. For your questions: [LossNode Chat](https://t.me/LossNodeChat)


## Gitopia Turkish Telegram: https://t.me/GitopiaTurkish

![image](https://user-images.githubusercontent.com/101462877/201489822-1d35a702-8d38-4a63-933b-6112b226817f.png)


## System requirements:
NODE TYPE | CPU     | RAM      | SSD     |
| ------------- | ------------- | ------------- | -------- |
| Testnet | 4          | 8         | 200*  |

## Important links for Gitopia:
- [Website](https://gitopia.com/)
- [Explorer](https://gitopia.explorers.guru/)
- [Twitter](https://twitter.com/gitopiaDAO)
- [Discord](https://discord.gg/EcwjHedFnp)

# 1a-1) Scripted installation with StateSync.

```
wget -O gitopiaEN.sh https://raw.githubusercontent.com/thisislexar/Gitopia-Testnet/main/gitopiaEN.sh && chmod +x gitopiaEN.sh && ./gitopiaEN.sh
```

# 1a-2) Scripted installation without StateSync.

```
wget -O gitopiaEN-noSS.sh https://raw.githubusercontent.com/thisislexar/Gitopia-Testnet/main/gitopiaEN-noSSgitopiaEN-noSS.sh && chmod +x gitopiaEN-noSS.sh && ./gitopiaEN-noSS.sh
```


# 1b) Manual installation.

You can also install the node [manually](https://github.com/thisislexar/Gitopia-Testnet/blob/main/gitopiaEN_manual.md) to improve your Node knowledge.


# 2) Continue. 

## To check the sync status:

```
gitopiad status 2>&1 | jq .SyncInfo
``` 

![image](https://user-images.githubusercontent.com/101462877/201514358-41303e3f-6c51-4f12-b3e3-49bcecb3770c.png)

If the sync status is false as in the photo above, you can continue, if it is still true, wait and continue after false

## Create a wallet.
```
gitopiad keys add <WALLETNAME>
``` 
If you want to use an existing wallet:

```
gitopiad keys add <WALLETNAME> --recover
``` 


## To get testnet tokens, we import the words of the wallet we set up just above, to Keplr.

![image](https://user-images.githubusercontent.com/101462877/201490749-0d060bfa-bca3-4b09-8569-c00f499c8a48.png)

![image](https://user-images.githubusercontent.com/101462877/201490756-32b6b1f1-fdd8-4af1-b3f0-9f18f5a0ad3b.png)

![image](https://user-images.githubusercontent.com/101462877/201490769-99978610-5dba-4758-b4d1-abac5f18a215.png)

![image](https://user-images.githubusercontent.com/101462877/201490784-e00537d8-8c50-4951-9d76-fe78adca8032.png)

## After importing the wallet to Keplr, go to [Gitopia platform](https://gitopia.com/home) and get testnet tokens.

![image](https://user-images.githubusercontent.com/101462877/201490673-40e73d7d-0705-47c3-8ce6-391ad1326fdd.png)


![image](https://user-images.githubusercontent.com/101462877/201490821-bec0a6d7-9e97-4d22-b51d-092874799e52.png)


## Check that tokens have arrived in our wallet from [Explorer](https://gitopia.explorers.guru/)

![image](https://user-images.githubusercontent.com/101462877/201490841-58a9340a-8bb9-43c0-b086-dec720951575.png)


## Create validator.


```
gitopiad tx staking create-validator \
  --amount 1000000utlore \
  --from <WALLETNAME> \
  --commission-max-change-rate "0.01" \
  --commission-max-rate "0.2" \
  --commission-rate "0.07" \
  --min-self-delegation "1" \
  --pubkey  $(gitopiad tendermint show-validator) \
  --moniker $NODENAME \
  --chain-id gitopia-janus-testnet-2 \
  --website="http://linktr.ee/LossNode" \
  --details="Testing the Gitopia"
```


# Useful commands:

Checking logs

```
journalctl -fu gitopiad -o cat
```


Stopping the service

```
sudo systemctl stop gitopiad
```

Restarting the service

```
sudo systemctl restart gitopiad
```

Delegating tokens

```
gitopiad tx staking delegate gitopiavaloper1tdp45gtcfujsskg75k7tlxxxxxxx 10000000utlore --chain-id=gitopia-janus-testnet-2  --from <WALLETNAME>
```

Editing validator

```
gitopiad tx staking edit-validator \
  --moniker=$NODENAME \
  --identity="<YOUR KEYBASE ID>" \
  --website="<WEBSITE LINK>" \
  --details="DETAILS" \
  --chain-id=gitopia-janus-testnet-2  \
  --from=<WALLETNAME>
``` 


# Deleting node:

```
sudo systemctl stop gitopiad
sudo systemctl disable gitopiad
sudo rm /etc/systemd/system/gitopia* -rf
sudo rm $(which gitopiad) -rf
sudo rm $HOME/.gitopia* -rf
sudo rm $HOME/gitopia -rf
sed -i '/GITOPIA_/d' ~/.bash_profile
``` 
