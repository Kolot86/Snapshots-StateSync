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
## Add addresbook if needed 

```bash
wget -O $HOME/.okp4d/config/addrbook.json "https://raw.githubusercontent.com/Kolot86/Snapshots-SateSync/main/OKP4/addrbook.json"
```

## Restart 

```bash
sudo systemctl restart okp4d && sudo journalctl -u okp4d -f -o cat
```


