# Snapshots-StateSync

# 13GB Snapshot 2022-09-20

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
rm -rf $HOME/.kyve/data/
mkdir $HOME/.kyve/data/
cd $HOME
wget http://62.171.144.51:8000/kywedata.tar.gz
```
## Unpack and move archive

```bash
tar -C $HOME/ -zxvf kywedata.tar.gz --strip-components 1
mv $HOME/kolot/.kyve/data $HOME/.kyve
```

## Delete archive

```bash
cd $HOME
rm kywedata.tar.gz
```

## Start node

```bash
sudo systemctl restart kyved && sudo journalctl -u kyved -f -o cat
```

## Add peers if needed 

```bash
SEEDS=""
PEERS="410bf0cb2cdb9a6e159c14b9d01531b9ecb1edd4@3.70.26.46:26656,c485e2b46e16a7e853469ef22dcc436946f92445@116.202.236.115:20656,62278ce4360d7c4b426cd504004496bf25cf2222@88.198.203.118:26656,897621af43a1ab19a9e8439f0d0d725cf6f558ab@80.82.215.243:26656,8f91fcf3970a8390f8afab353a52da220eb27cd2@65.109.13.193:28656,09aae3c348b1fbd6153dc141b733a1e6e3d21b06@5.161.134.175:26656,2b7b524ceb6867f0856df35a5bed2ca300b8fb69@5.189.167.148:26656,45fa92e77f051000342a64b3595f02544ef0d476@157.90.149.248:26656,1070a790adb28f4e39a1ca7961d0fd4ee28f676b@185.202.236.103:49656,3e97e4489e68335c5a69fbdac0d67502079e18ae@5.161.47.249:26656"; \
sed -i.bak -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.kyve/config/config.toml
```
## Restart 

```bash
sudo systemctl restart kyved && sudo journalctl -u kyved -f -o cat
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
# Erase persistent peers 
After you fully synced, you can take away RPC from persistent peers. In case if it falls off or will be switch off.    
```bash
peers=""
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.kyve/config/config.toml
```
# Switch off state sync  
and restart
```bash
sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1false| ; \
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.kyve/config/config.toml
sudo systemctl restart kyved && sudo journalctl -u kyved -f -o cat
```
