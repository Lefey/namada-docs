# Quickstart 

## About this guide

This guide is for those interested in getting Namada up and running as quickly as possible. It will guide you through installing Namada, joining a testnet network, running a node, and grabbing some tokens.

## Installing Namada

See [the installation guide](../install/intro.md) for details on installing the Namada binaries. Commands in this guide will assume you have the Namada binaries (`namada`, `namadan`, `namadaw`, `namadac`) on your `$PATH`.

If the binaries are stored somewhere, but are not on your path (perhaps you downloaded the binaries), you can add the binaries to to your $PATH with:

```shell
export PATH=$PATH:<path-to-namada-binaries>
```

If you build from source, and run `make install`, the binaries will be installed to your `$PATH` by default.

## Joining a network

See [the testnets page](../testnets/intro.md) for details of how to join a testnet. The rest of this guide will assume you have joined a testnet chain using the `namadac utils join-network --chain-id <some-chain-id>` command.

## Run a ledger node

We recommend this step with [tmux](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/), which allows the node to keep running without needing the terminal to be open indefinitely. If not, skip to the subsequent step.

```shell
tmux

# inside the tmux/or not

namadan ledger run

# can detach the tmux (Ctrl-B then D)
```

For a more verbose output, one can run 
```shell
NAMADA_LOG=info TM_LOG_LEVEL=p2p:none,pex:error NAMADA_CMT_STDOUT=true namada ledger
```

This should sync your node to the ledger and will take a while (depending on your machine's hardware as well as the time between genesis and the start of sync). Subsequent commands (generating an account, etc.)  are unlikely to work until it is fully synced. Enquire the current block height with other participants to make sure you are synced in order to proceed.

## Generate an account and grab some tokens
First you will need an implicit account in order to receive tokens. You can generate one with:

```shell
namadaw account gen --alias <your-go-to-alias>
```
For the remainder of this introduction, let's assume your go-to alias is `stanley`.

This will generate a new account and store it in the default keychain. You can see the account with:

```shell
namadaw account list
```

<!-- #TODO: ADD some output-->
.
## Grabbing the tokens (testnet only)
The "faucet" is a native established account on Namada that is willing to give a maximum of 1000 tokens to any user at request. Let's transfer ourselves `1000 NAM` from the faucet with the same alias using:

```shell
namadac transfer \
  --source faucet \
  --target stanley \
  --token NAM \
  --amount 1000 \
  --signer stanley
```

To query the balance of `stanley`, run:

```shell
namadac balance \
  --owner stanley
```

## From here
From here, you can do a variety of cool things. Perhaps try [shielding your NAM](../../users/shielded-transfers.md), bonding your tokens to a validator for [delegating purposes](../../delegators/intro.md), or [becoming a validator](../../validators/intro.md).
