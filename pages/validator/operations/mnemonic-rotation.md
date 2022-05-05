# Rotation of mnemonics in tofnd

Starting from `axelard` `v0.17.3` and `tofnd` `v0.10.1`, validators can use `tofnd -m rotate` to generate a new mnemonic and slowly rotate out their old mnemonics for better security practices. New Axelar key rotations will automatically use the most recent mnemonic generated by `tofnd -m rotate`.

Validators need to keep their old `tofnd` mnemonics until all keys generated from those mnemonics are considered "old" by the Axelar network. A key becomes _old_ after `x` subsequent key rotations for that EVM chain. (Currently `x=7`.)

```bash
# Kill vald/tofnd processes
pkill -9 -f "vald"
pkill -f "tofnd"

# Rotate tofnd mnemonic
tofnd -m rotate -d $AXELARD_HOME/.tofnd

# Note: Keep the old mnemonic backups around

# BACKUP the new exported mnemonic and then DELETE the local copy
cp $TOFND_HOME/export ...
rm $TOFND_HOME/export

# Restart vald/tofnd processes as usual
```

## Recovery of mnmenonics

As always, you can import a `tofnd` mnemonic with `tofnd -m import -d $AXELARD_HOME/.tofnd`. If there are no other mnemonics yet in `tofnd` storage then the imported mnemonic will be treated as the latest mnemonic, 
and automatically used for future key ids that are rotated to and any previous key ids it was already a part of.
Each subsequent imported mnemonic is considered as "old"
and so only used for any past key ids that corresponded to it.