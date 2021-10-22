# Exercise 4

Instructions from here: https://github.com/input-output-hk/Alonzo-testnet/blob/main/Alonzo-solutions/exercise4/Exercise4-solution.md

## Part 1
Compiling the contract
1. cabal run plutus-helloworld -- 1 helloworld2.plutus

Creating the script address
2. cardano-cli address build --payment-script-file helloworld2.plutus --testnet-magic 8 --out-file helloworld2.addr
3. cat helloworld2.addr

Querying the script address
4. cardano-cli query utxo --testnet-magic 8 --address $(cat helloworld2.addr)

## Part 2
Calculating the datum hash
5. cardano-cli transaction hash-script-data --script-data-value 79600447942433 > helloworld_hash.txt

Querying personal UTXOs
6. cardano-cli query utxo --testnet-magic 8 --address $(cat payment.addr)

Build the transaction
7. cardano-cli transaction build \
--alonzo-era \
--tx-in 228abaa4a4ac1f407352aa9af55ec3dc7f0b66e57c57911cf0250c303d77056b#0 \
--tx-out $(cat helloworld2.addr)+10000000 \
--tx-out-datum-hash $(cat helloworld_hash.txt) \
--change-address $(cat payment.addr) \
--testnet-magic 8 \
--out-file tx.raw

Sign and submit
8. cardano-cli transaction sign --tx-body-file tx.raw --signing-key-file payment.skey --out-file tx.sign
9. cardano-cli transaction submit --testnet-magic 8 --tx-file tx.sign

Check balances
11. cardano-cli query utxo --testnet-magic 8 --address $(cat payment.addr)
12. cardano-cli query utxo --testnet-magic 8 --address $(cat helloworld2.addr)

Spend from the script
13. cardano-cli transaction build \
--alonzo-era \
--protocol-params-file pparams.json \
--tx-in 6aa3567809f5ebd5717c715c168ed6cc169f9ef703ae11a2d192283b031411e6#1 \
--tx-in-script-file helloworld2.plutus \
--tx-in-datum-value 79600447942433 \
--tx-in-redeemer-value 79600447942433 \
--tx-in-collateral 87bd3d0d3d65d27bfc2a324860b4f956071db4421dce22cb5e0e8d3fd5e7ad4f#1 \
--change-address $(cat payment.addr) \
--testnet-magic 8 \
--out-file tx.raw

Sign and submit
14. cardano-cli transaction sign --tx-body-file tx.raw --signing-key-file payment.skey --out-file tx.sign
15. cardano-cli transaction submit --testnet-magic 8 --tx-file tx.sign