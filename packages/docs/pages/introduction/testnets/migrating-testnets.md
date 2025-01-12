# Steps for migrating testnets

This guide will help you migrate your validator node from one testnet to another. These steps are optional, and for most testnets, these steps will not be needed.

## Resetting your validator node (optional)


> With the release of `v0.15.3` we have introduced a new base directory. This means that you will need to reset your validator node to use the new base directory. This is a one time operation.
>The base directory has been moved from `.namada` to `.local/share/namada` on Linux and `Library/Application Support/Namada` on MacOS.


### Locate the namada base directory

Depending on the testnet you are migrating from, the base directory will be located in different places.
For this reason, we will save the base directory path to a variable.

#### Before `v0.15.3`
If you are migrating from a testnet BEFORE `v0.15.3`, then your home directory and relevant files will be located in a `.namada`. The location of this directory depends where you initially ran the command `namadac utils join-network --chain-id <CHAIN_ID> --genesis-validator <ALIAS>`. It will be located in the directory in which that command was executed.

Once located, you can save the base directory path to a variable. For example, if the join-network command was ran from your home directory, you can run:
```bash
export BASE_DIR=$HOME/.namada
```

#### After `v0.15.3`
If you are migrating from a testnet AFTER `v0.15.3`, then your base directory and relevant files will be located in `.local/share/namada` on Linux and `Library/Application Support/Namada` on MacOS. You can verify the default directory on your machine by running:
```bash
export BASE_DIR=$(namadac utils default-base-dir)
```


>Technically, the correct directory will be the one assigned to `$XDG_DATA_HOME`, but if you haven't set that variable, it will default to the above.


### 1. IMPORTANT! Save your `pre-genesis` folder in the ledger base directory

Before we delete any folders, we want to make sure we save our `pre-genesis` folder. This folder contains your validator keys, and we want to make sure we don't lose them.

```bash
mkdir $HOME/backup-pregenesis && cp -r $BASE_DIR/pre-genesis $HOME/backup-pregenesis/
```

### 2. **Ensure keys are saved**

`ls backup-pregenesis` should output a saved `wallet.toml`.

### 3. Delete the base directory

```bash
rm -rf $BASE_DIR/*
```

### 4. Check that namada and cometbft binaries are correct. 

`namada --version` should yield `v0.15.3` and `cometbft version` should output `0.37.2`



### 5. Create a pre-genesis directory

```bash
mkdir $BASE_DIR/pre-genesis
```

### 6. Copy the backuped file back to `$BASE_DIR/pre-genesis` folder
```bash
cp -r backup-pregenesis/* $BASE_DIR/pre-genesis/
```

You should now be ready to go!