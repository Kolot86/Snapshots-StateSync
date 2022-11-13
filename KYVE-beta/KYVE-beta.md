# Snapshot 
## 85MB Snapshot 2022-11-13

## Stop node and tune pruning
```bash
sudo systemctl stop kyved
chaind tendermint unsafe-reset-all --home $HOME/.kyve --keep-addr-book
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.kyve/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.kyve/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.kyve/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.kyve/config/app.toml
```

## Download archive

```bash
cd $HOME
sudo rm -rf $HOME/.kyve/data/
wget http://62.171.144.51:8000/kyvedata.tar.gz
```
## Unpack archive

```bash
tar -C $HOME/ -zxvf kyvedata.tar.gz --strip-components 1
sudo mv $HOME/kolot/kyvesnap/data  $HOME/.kyve
```

## Delete archive

```bash
cd $HOME
sudo rm -rf kyvedata.tar.gz kolot
```
## Update your binaries if needed 

```bash
cd $HOME
wget https://kyve-beta.s3.eu-central-1.amazonaws.com/v0.7.0_beta8/kyved_linux_amd64.tar.gz
tar -xvzf kyved_linux_amd64.tar.gz
sudo mv kyved /usr/local/bin/chaind
sudo sudo chmod +x $(which chaind)
```

## Copy binaries to cosmovisor 

```bash
cd $HOME
cp /usr/local/bin/chaind $HOME/.kyve/cosmovisor/genesis/bin/chaind
```

## Add addresbook

```bash
wget -O $HOME/.kyve/config/addrbook.json "https://raw.githubusercontent.com/Kolot86/Snapshots-SateSync/main/KYVE-beta/addrbook.json"
```

## Restart 

```bash
sudo systemctl restart kyved && sudo journalctl -u kyved -f -o cat
```
