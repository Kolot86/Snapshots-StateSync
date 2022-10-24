## 933MB Snapshot 2022-10-24

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
PEERS="bb4c7919c1c8cb5d3fed102f6019142f3a71c9f5@77.91.123.193:26656,c3e1646029109c374bedb4c1737c86a8d389a419@146.190.209.11:26656,dc48a2e124a0667504c6f6b74db0511e8ffba516@65.108.68.233:26601,624b5d754f79a2466bff14c1dd462c5508d35f78@167.235.197.90:26656,ab44363ce335469a2e867aa64b28d0cc45d3428d@135.181.149.211:26656,6894c679d851420522baf151e1d1bbf63d9defc9@144.76.97.251:12656,8710df7243b1657d6a37a7628f8d786c6124a842@149.102.140.230:26656,5fe2aaf297cc854431f2f8b481c659166b0eb868@38.242.143.53:26656,f4453c47eeceab3eae76a95b3e41189f8c1bb55e@128.199.40.148:26656,91e6871bda98f7c24047c14d39197f3c4c965e8d@135.181.156.52:26656,ad5d29c1fc2e5224a51547a677968d84bde76eb8@95.217.118.96:26858,6c1dfe43d9c0c06f639f7deb6c3b8bb3cabc2647@68.183.12.38:26656,89a8c6e1099fd6c4abd6522c574b939b2045f7fb@66.175.234.3:26656,41a7e27b8e9b0fdda60c786258bfd7b2a3ad1548@65.108.76.44:11684"; \
sed -i.bak -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.okp4d/config/config.toml
```

## Restart 

```bash
sudo systemctl restart okp4d && sudo journalctl -u okp4d -f -o cat
```
