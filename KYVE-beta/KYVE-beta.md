# Snapshots-StateSync 

## If you have used my guide to install chain node then you can use state sync without change 
# Instlall latest binaries
## Stop node 
```bash
sudo systemctl stop kyved
```
## Download latest binaries 
```bash
cd $HOME
wget http://62.171.144.51:8000/kyvechaind.tar.gz
```
## Unpack archive

```bash
tar -C $HOME/ -zxvf kyvechaind.tar.gz --strip-components 1
mv $HOME/kolot/chaind/chaind $HOME/.kyve/cosmovisor/genesis/bin/chaind
```

## Delete archive

```bash
cd $HOME
rm -rf kyvechaind.tar.gz kolot
```


#
# StateSync

```bash
peers="67a2cec5e11f59dca3df5d33e0dcd1983dbb9244@62.171.144.51:21656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.kyve/config/config.toml
```
```bash
SNAP_RPC=62.171.144.51:21657
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 500)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)
```
```bash
sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.kyve/config/config.toml
chaind tendermint unsafe-reset-all --home $HOME/.kyve --keep-addr-book
sudo systemctl restart kyved && sudo journalctl -u kyved -f -o cat
```
### Erase persistent peers 
After you fully synced, you can take away RPC from persistent peers. In case if it falls off or will be switch off.    
```bash
peers=""
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.kyve/config/config.toml
```
### Switch off state sync and restart
```bash
sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1false| ; \
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.kyve/config/config.toml
sudo systemctl restart kyved && sudo journalctl -u kyved -f -o cat
```
