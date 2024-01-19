# Jackal_mainnet
![ALT-logo](https://raw.githubusercontent.com/MaxFoton/Jackal_mainnet/main/jackal.png)

`Validator` MaxFoton nodes

`Valoper` jklvaloper18tnqgtzttxzf9nr8hswhwp2kgusvndn4xs3asn

`Delegate` https://explorer.stavr.tech/Jackal/staking/jklvaloper18tnqgtzttxzf9nr8hswhwp2kgusvndn4xs3asn

`RPC` http://95.165.89.222:24474/

`grpc` http://95.165.89.222:9288/

`API` http://95.165.89.222:1321/

`Peer` 1fc03ab6bfad4741e4d9717688a35185591eb08a@95.165.89.222:24475

**Start from state-sync**

```
systemctl stop canined && canined tendermint unsafe-reset-all --home $HOME/.canine --keep-addr-book

curl https://raw.githubusercontent.com/MaxFoton/Jackal_mainnet/main/addrbook.json > ~/.canine/config/addrbook.json
```

```
SNAP_RPC=95.165.89.222:24474 && \
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 2000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash) && \
echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH
```

```
sed -i.bak -e  "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.canine/config/config.toml
sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.canine/config/config.toml
```

**Enable service and start**

```
sudo systemctl daemon-reload && sudo systemctl enable canined

sudo systemctl restart canined && sudo journalctl -fu canined -o cat
```
```
curl -o - -L https://raw.githubusercontent.com/MaxFoton/Jackal_mainnet/main/wasm.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.canine/
```
