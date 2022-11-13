##  Snapshot 2022-11-13

## Stop node and tune pruning
```bash
sudo systemctl stop neutrond
neutrond tendermint unsafe-reset-all --home $HOME/.neutrond --keep-addr-book
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.neutrond/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.neutrond/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.neutrond/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.neutrond/config/app.toml
```
## Delete old data

```bash
cd $HOME/.neutrond
sudo rm -rf data wasm
```

## Download archive

```bash
cd $HOME
wget http://62.171.144.51:8002/neutrondata.tar.gz
```

## Unpack archive

```bash
tar -C $HOME/ -zxvf neutrondata.tar.gz --strip-components 1
mv $HOME/kolot/snap/data  $HOME/.neutrond
mv $HOME/kolot/snap/wasm  $HOME/.neutrond
```

## Delete archive

```bash
cd $HOME
sudo rm -rf neutrondata.tar.gz kolot
```
## Add addresbook

```bash
wget -O $HOME/.neutrond/config/addrbook.json "https://raw.githubusercontent.com/Kolot86/Snapshots-SateSync/main/OKP4/addrbook.json"
```

## Restart 

```bash
sudo systemctl restart neutrond && sudo journalctl -u neutrond -f -o cat
```
## Add peers if addresbook not working 
But I recoment to delete thouse after your node will be synkd 

```bash
SEEDS=""
PEERS="3376b31aa59a65c174260d9d083d3345b4f3db08@45.61.161.192:26656,ae373a3758864115c2e0693553cc0c3d0ed60c90@135.181.43.140:26656,eff365c8e0e2f99026e3dd91704d3764eb38e0a1@65.108.13.212:26756,400e31ef8ee56f33a57d5b2fa18c62085c9a1869@149.102.135.94:26656,83a5bec69a78e63f18e665b59a6f162363d8a518@65.108.100.53:34656,c9e0d661f49b9fe4d67693e2179501567a68b62c@65.109.5.123:26656,886cbce0d90cc025d7480935fdae16af3f3c8bfe@65.109.34.9:46656,5d0ada752728ad5dd8c62d9866fdef2b7322cc26@45.79.250.108:26656,ebcd770b7fcea7a9634b33aeb4b4643330c9d553@113.30.191.103:26656,88aeebd9790485a52994c78463aa9bcf47aaa682@89.163.231.236:26656"; \
sed -i.bak -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.neutrond/config/config.toml
```

## Restart 

```bash
sudo systemctl restart neutrond && sudo journalctl -u neutrond -f -o cat
```
