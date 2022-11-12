<h1 align="center">Gitopia Testneti Kurulum Rehberi

## Merhabalar, bugün Gitopia testnetine katılıyor olacağız. Sağ üstten yıldızlayıp forklamayı unutmayalım. Sorularınız olursa: [LossNode Chat](https://t.me/LossNode)


## Gitopia Türkiye Telegram: https://t.me/GitopiaTurkish

![image](https://user-images.githubusercontent.com/101462877/201489822-1d35a702-8d38-4a63-933b-6112b226817f.png)


## Sistem gereksinimleri:
NODE TİPİ | CPU     | RAM      | SSD     |
| ------------- | ------------- | ------------- | -------- |
| Testnet | 4          | 8         | 200*  |

## Gitopia için önemli linkler:
- [Website](https://gitopia.com/)
- [Explorer](https://gitopia.explorers.guru/)
- [Twitter](https://twitter.com/gitopiaDAO)
- [Discord](https://discord.gg/EcwjHedFnp)

# 1a-1) Snapshot'lı script ile kurulum.

```
wget -O gitopia.sh https://raw.githubusercontent.com/thisislexar/Gitopia-Testnet/main/gitopia.sh && chmod +x gitopia.sh && ./gitopia.sh
```

# 1a-2) Snapshot'sız script ile kurulum.

```
wget -O gitopia-noSS.sh https://raw.githubusercontent.com/thisislexar/Gitopia-Testnet/main/gitopia-noSS.sh && chmod +x gitopia-noSS.sh && ./gitopia-noSS.sh
```


# 1b) Manuel kurulum.

Node bilginizi geliştirmek adına dilerseniz [Manuel Kurulum](https://github.com/thisislexar/Gitopia-Testnet/blob/main/gitopia_manual.md) da yapabilirsiniz.


# 2) Devam edelim. 

## Sync durumunu kontrol etmek için:

```
gitopiad status 2>&1 | jq .SyncInfo
``` 

## Cüzdan oluşturalım.
```
gitopiad keys add <CÜZDANADI>
``` 
Var olan bir cüzdanı kullanmak isterseniz:

```
gitopiad keys add <CÜZDANADI> --recover
``` 


## Faucet almak için hemen yukarıda kurduğumuz cüzdanın kelimelerini Keplr'a import ediyoruz.

![image](https://user-images.githubusercontent.com/101462877/201490749-0d060bfa-bca3-4b09-8569-c00f499c8a48.png)

![image](https://user-images.githubusercontent.com/101462877/201490756-32b6b1f1-fdd8-4af1-b3f0-9f18f5a0ad3b.png)

![image](https://user-images.githubusercontent.com/101462877/201490769-99978610-5dba-4758-b4d1-abac5f18a215.png)

![image](https://user-images.githubusercontent.com/101462877/201490784-e00537d8-8c50-4951-9d76-fe78adca8032.png)

## Cüzdanı Keplr'a import sonra [Gitopia platformuna](https://gitopia.com/home)'a giderek faucet alalım.

![image](https://user-images.githubusercontent.com/101462877/201490673-40e73d7d-0705-47c3-8ce6-391ad1326fdd.png)


![image](https://user-images.githubusercontent.com/101462877/201490821-bec0a6d7-9e97-4d22-b51d-092874799e52.png)


## [Explorer](https://gitopia.explorers.guru/)'dan cüzdanımıza token geldiğini kontrol edelim.

![image](https://user-images.githubusercontent.com/101462877/201490841-58a9340a-8bb9-43c0-b086-dec720951575.png)


## Validator oluşturalım.


```
gitopiad tx staking create-validator \
  --amount 1000000utlore \
  --from <CÜZDANADI> \
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


# Bazı komutlar:

Log kontrolü

```
journalctl -fu gitopiad -o cat
```


Servisi durdurma

```
sudo systemctl stop gitopiad
```

Servisi tekrar başlatma

```
sudo systemctl restart gitopiad
```

Token delege etme

```
gitopiad tx staking delegate gitopiavaloper1tdp45gtcfujsskg75k7tlxxxxxxx 10000000utlore --chain-id=gitopia-janus-testnet-2  --from <CÜZDANADI>
```

Validator düzenleme

```
gitopiad tx staking edit-validator \
  --moniker=$NODENAME \
  --identity="<KEYBASE ID'NİZ>" \
  --website="<WEBSİTE LİNKİ>" \
  --details="AÇIKLAMA" \
  --chain-id=gitopia-janus-testnet-2  \
  --from=<CÜZDANADI>
``` 


# Node silmek için:

```
sudo systemctl stop gitopiad
sudo systemctl disable gitopiad
sudo rm /etc/systemd/system/gitopia* -rf
sudo rm $(which gitopiad) -rf
sudo rm $HOME/.gitopia* -rf
sudo rm $HOME/gitopia -rf
sed -i '/GITOPIA_/d' ~/.bash_profile
``` 
