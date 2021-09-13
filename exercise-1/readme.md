# Exercise 1

## Configure node
Steps performed taken from here: https://github.com/input-output-hk/Alonzo-testnet/blob/main/Alonzo-solutions/exercise1/exercise1-linux-solution.md

Using this differences:

Cardano node:
    https://hydra.iohk.io/build/7408438

```
cardano-node 1.29.0 - linux-x86_64 - ghc-8.10
git rev 4c59442958072657812c6c0bb8e0b4ab85ce1ba2
```
Configuration files:

    wget https://hydra.iohk.io/build/7366583/download/1/alonzo-purple-config.json
    wget https://hydra.iohk.io/build/7366583/download/1/alonzo-purple-byron-genesis.json
    wget https://hydra.iohk.io/build/7366583/download/1/alonzo-purple-shelley-genesis.json
    wget https://hydra.iohk.io/build/7366583/download/1/alonzo-purple-alonzo-genesis.json
    wget https://hydra.iohk.io/build/7366583/download/1/alonzo-purple-topology.json

Command to run passive node:

    cardano-node run \
    > --topology alonzo-purple-topology.json \
    > --database-path db \
    > --socket-path db/node.socket \
    > --host-addr 0.0.0.0 \
    > --port 3001 \
    > --config alonzo-purple-config.json

Query blockchain:

    cardano-cli query tip --testnet-magic 8

## Get test ada (faucet)
Api key could be copied from Alonzo purple channel: https://discord.com/channels/826816523368005654/873221525514358884

    curl -v -XPOST "https://faucet.alonzo-purple.dev.cardano.org/send-money/$ADDRESS?apiKey=$KEY"