## 250MB Snapshot 2022-10-31

## Stop node and tune pruning
```bash
sudo systemctl stop okp4d
okp4d tendermint unsafe-reset-all --home $HOME/.okp4d --keep-addr-book
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.okp4d/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.okp4d/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.okp4d/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.okp4d/config/app.toml
```
## Delete old data

```bash
cd $HOME/.okp4d
sudo rm -rf data wasm
```

## Download archive

```bash
cd $HOME
wget http://62.171.144.51:8001/okp4data.tar.gz
```

## Unpack archive

```bash
tar -C $HOME/ -zxvf okp4data.tar.gz --strip-components 1
mv $HOME/kolot/snap/data  $HOME/.okp4d
mv $HOME/kolot/snap/wasm  $HOME/.okp4d
```

## Delete archive

```bash
cd $HOME
sudo rm -rf okp4data.tar.gz kolot
```
## Add addresbook

```bash
wget -O $HOME/.okp4d/config/addrbook.json "https://raw.githubusercontent.com/Kolot86/Snapshots-SateSync/main/OKP4/addrbook.json"
```

## Restart 

```bash
sudo systemctl restart okp4d && sudo journalctl -u okp4d -f -o cat
```
## Add peers if addresbook not working 
But I recoment to delete thouse after your node will be synkd 

```bash
SEEDS=""
PEERS="ab44b52595a1803c9048dd62d56ca9003728acb3@154.12.236.153:10656,bfbd634aed4299b726424c744dee68c0675c88f5@65.108.124.172:29656,d3bd87b4241fc331a29db61753dffcd47ce04aae@204.48.27.64:26656,c0e16c8237dea4d1acd25b61a5408d45ef50966a@185.250.36.5:26656,981ccd4391aa025eedb88e2aa4e6a6de0e8d21c4@185.192.97.87:26656,51388de3100bfe09bef723de48d9f5ed9407eb7e@94.103.84.250:26656,8f1d1153ec2616dc72655aac128e2a3a95447240@62.113.113.65:26656,54cb435fbd717c49fa4c94849781608a8feb143e@65.108.69.68:27363,f3faa89c2066ca3856ab9f7ebf5a994343ee3ebf@185.192.97.198:26656,33ae31deae1a6a041eb817497d7ae08b74bc192a@95.142.47.20:26656,4045016e8d4d8580e5de471868ca4685d22f2a30@178.18.245.208:26656,ef595ee71a161131efadc9edfc0d1ec4bcd82b59@65.108.195.235:25656,7d801627805fb656e073b11277080ab014510643@167.235.149.1:26656,04e3c9b97e3e0cc4507f673ee5d015154211eaae@65.108.135.124:36656,fc781bf9b1663d0156902f0418fcb17242903dfd@94.180.57.95:26656,2bed011894036cef9f552fc077dceda42c4bfef2@178.20.46.100:26656,1a3469488a36e7a260b69e543f6306f6dce9723b@65.109.38.208:46656"; \
sed -i.bak -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.okp4d/config/config.toml
```

## Restart 

```bash
sudo systemctl restart okp4d && sudo journalctl -u okp4d -f -o cat
```
