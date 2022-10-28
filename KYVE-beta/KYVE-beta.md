# StateSync 

## If you have used my guide to install chain node then you can use state sync without change 
# Instlall latest binaries
## Stop node 
```bash
sudo systemctl stop kyved
```
## Download latest binaries (beta 7)
```bash
cd $HOME
wget https://kyve-beta.s3.eu-central-1.amazonaws.com/v0.7.0_beta7_v2/chain_linux_amd64.tar.gz
```
## Unpack archive

```bash
tar -xvzf chain_linux_amd64.tar.gz
mv chaind $HOME/.kyve/cosmovisor/genesis/bin/chaind
```

## Delete archive

```bash
cd $HOME
rm -rf chain_linux_amd64.tar.gz
```


#
# StateSync
```bash
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"2000\"/" $HOME/.kyve/config/app.toml
```
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
## Add peers if needed 
```bash
SEEDS=""
PEERS="6273f6a5fcb5d9e7996b0d76cbf46188b539c009@65.21.123.69:38656,41ffaf0628e8e80fe9b3644edb7fc089999fbf56@5.161.86.216:26656,6d0b39a609ffa4684af9c4943d0dadd0f032a9ff@65.109.11.155:26656,d89b3aaf689adb0843ee62ed06a9a894da0591cb@141.95.106.246:26656,bf59342c59e1e8976420a0ee4b28b71394eeec16@178.250.242.94:26656,367ecb7d6288c7649c9053a4cf01ee110471764b@88.198.242.163:36656,1f3b64e06bf8ad2e1bcd89445797d26ff80fef7c@65.109.34.133:46656,1070a790adb28f4e39a1ca7961d0fd4ee28f676b@185.202.236.103:49656,897621af43a1ab19a9e8439f0d0d725cf6f558ab@80.82.215.243:26656,fe686e1f25651cb1b0e0fa8a61b2e015ffd63bca@18.197.245.69:26656"; \
sed -i.bak -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.kyve/config/config.toml
```
## Addrbook
```bash
wget -O $HOME/.kyve/config/addrbook.json "https://raw.githubusercontent.com/Kolot86/Snapshots-SateSync/main/KYVE-beta/addrbook.json"
```
## Restart
```bash
sudo systemctl restart kyved && sudo journalctl -u kyved -f -o cat
```
