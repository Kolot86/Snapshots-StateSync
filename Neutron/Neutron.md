## 238mb Snapshot 2022-11-14

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
mv $HOME/kolot/neutrond-snap/data  $HOME/.neutrond
mv $HOME/kolot/neutrond-snap/wasm  $HOME/.neutrond
```

## Delete archive

```bash
cd $HOME
sudo rm -rf neutrondata.tar.gz kolot
```
## Add addresbook

```bash
wget -O $HOME/.neutrond/config/addrbook.json "https://raw.githubusercontent.com/Kolot86/Snapshots-StateSync/main/Neutron/addrbook.json"
```

## Restart 

```bash
sudo systemctl restart neutrond && sudo journalctl -u neutrond -f -o cat
```

