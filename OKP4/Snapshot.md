## 3GB Snapshot 2022-10-20

## Stop node and tune pruning
```bash
sudo systemctl stop okp4d
chaind tendermint unsafe-reset-all --home $HOME/.okp4d --keep-addr-book
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.okp4d/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.okp4d/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.okp4d/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.okp4d/config/app.toml
```

## Download archive

```bash
sudo rm -rf $HOME/.okp4d/data wasm
mkdir $HOME/.okp4d/data/
cd $HOME
wget http://62.171.144.51:8001/okp4data.tar.gz
```
## Unpack archive

```bash
tar -C $HOME/ -zxvf okp4data.tar.gz --strip-components 1
mv $HOME/kolot/.okp4d/data $HOME/.okp4d
```

## Delete archive

```bash
cd $HOME
rm okp4data.tar.gz
```

## Start node

```bash
sudo systemctl restart okp4d && sudo journalctl -u okp4d -f -o cat
```

## Addresbook 

```bash

```
## Restart 

```bash
sudo systemctl restart okp4d && sudo journalctl -u okp4d -f -o cat
```

## Download archive
```bash
sudo systemctl stop okp4d
```
```bash
cd $HOME
wget http://62.171.144.51:8000/okp4dchaind.tar.gz
```
## Unpack archive

```bash
tar -C $HOME/ -zxvf okp4dchaind.tar.gz --strip-components 1
mv $HOME/kolot/chaind/chaind $HOME/.okp4d/cosmovisor/genesis/bin/chaind
```

## Delete archive

```bash
cd $HOME
rm -rf okp4dchaind.tar.gz kolot
