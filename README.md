# Nolus Snapshots

This  is a guide on how to sync your Nolus Testnet (nolus-rila) node using snapshot. Provided by medium#7343.

Snapshots are taken automatically every 24 hours.

1. Stop your Nolus service, then make a backup of ``priv_validator_state`` file.
```
sudo systemctl stop nolusd
cp $HOME/.nolus/data/priv_validator_state.json $HOME/.nolus/priv_validator_state.json.backup
```
2. Clear database.
```
nolusd tendermint unsafe-reset-all --home $HOME/.nolus --keep-addr-book
```
3. Install lz4 tool, download and install snapshot of data and wasm folders.
```
apt install lz4

wget http://38.242.238.124/nolus/nolus-rila-data.tar.lz4
lz4 -d nolus-rila-data.tar.lz4 -c | tar xf - -C $HOME/.nolus

wget http://38.242.238.124/nolus/nolus-rila-wasm.tar.lz4
lz4 -d nolus-rila-wasm.tar.lz4 -c | tar xf - -C $HOME/.nolus
```
4. Recover a backup of ``priv_validator_state`` file.
```
mv $HOME/.nolus/priv_validator_state.json.backup $HOME/.nolus/data/priv_validator_state.json
```
5. Lanch Nolus service.
```
sudo systemctl restart nolusd && journalctl -u nolusd -f -o cat
```
6. Remove downloaded snapshot archives if everything works fine.
```
rm nolus-rila-data.tar.lz4
rm nolus-rila-wasm.tar.lz4
```
