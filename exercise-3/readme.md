# Exercise 3

## Part 1

Reference: https://github.com/input-output-hk/Alonzo-testnet/blob/main/Alonzo-solutions/exercise3/e3SampleSolution.md

Generate a second payment address:

    cardano-cli address key-gen \
    --verification-key-file payment2.vkey \
    --signing-key-file payment2.skey

    cardano-cli stake-address key-gen \
    --verification-key-file stake2.vkey \
    --signing-key-file stake2.skey

    cardano-cli address build \
    --payment-verification-key-file payment2.vkey \
    --stake-verification-key-file stake2.vkey \
    --out-file payment2.addr \
    ${MAGIC}

Check UTxO for a payment address:

    cardano-cli query utxo ${MAGIC} --address $(cat payment.addr)

Build a transaction where ${ADDR} = TxHash#TxIx from previous command. In this example, 5 ADA are transferred:

    cardano-cli transaction build \
    --alonzo-era \
    --tx-in ${ADDR} \
    --tx-out $(cat payment2.addr)+5000000 \
    --change-address $(cat payment.addr) \
    ${MAGIC} \
    --out-file tx.raw

Sign the transaction:

    cardano-cli transaction sign \
    ${MAGIC} \
    --signing-key-file payment.skey \
    --tx-body-file tx.raw \
    --out-file tx.sign

Submit it to the testnet:

    cardano-cli transaction submit ${MAGIC}  --tx-file tx.sign

Check results:

    cardano-cli query utxo ${MAGIC}  --address $(cat payment2.addr)


## Part 2

Create a datum hash from a random_datum.txt file:
    
    cardano-cli transaction hash-script-data --script-data-value $(cat random_datum.txt) > random_datum_hash.txt

Generate script address:

    cardano-cli address build --payment-script-file AlwaysSucceeds.plutus ${MAGIC} --out-file script.addr

Generate protocol-parameters file:

    cardano-cli query protocol-parameters ${MAGIC} > pparams.json

Build the transaction, pays 3 ADA to script address:

    cardano-cli transaction build \
    --alonzo-era \
    --tx-in e31713f80dd99c0597a2c0faa6843a08ec8fade1bc8776964a43cc8fca4dd904#0 \
    --tx-out $(cat script.addr)+3000000 \
    --tx-out-datum-hash $(cat random_datum_hash.txt) \
    --change-address addr_test1qqwr6lfu5st9snwqyfexvrf7ns0alnnwr54euc96pkgq6ltw772p0q2n59xg54fkq540u6nxv3ql8pmt9sgerr5wvfqs4f23nt \
    ${MAGIC} \
    --protocol-params-file pparams.json \
    --out-file tx.raw

Sign the transaction:

    cardano-cli transaction sign --tx-body-file tx.raw --signing-key-file payment2.skey ${MAGIC} --out-file tx.sign

Submit the transaction:

    cardano-cli transaction submit ${MAGIC} --tx-file tx.sign

## Part 3

Build the transaction using:
1. UTxO from script address
2. Script file
3. Datum value
4. Redeemer
5. Collateral Input (an UTxO to pay if transaction fails: these inputs are only collected in the case of script validation failure)
6. Address to be paid
```
    cardano-cli transaction build \
    --alonzo-era \
    --tx-in 06f5a3c37fa7a2746362aec4949b4870633f3e1dab37c2bc1ad7ddc5b81a62d4#1 \
    --tx-in-script-file AlwaysSucceeds.plutus \
    --tx-in-datum-value $(cat random_datum.txt) \
    --tx-in-redeemer-value $(cat random_datum.txt) \
    --tx-in-collateral 06f5a3c37fa7a2746362aec4949b4870633f3e1dab37c2bc1ad7ddc5b81a62d4#0 \
    --change-address addr_test1qqwr6lfu5st9snwqyfexvrf7ns0alnnwr54euc96pkgq6ltw772p0q2n59xg54fkq540u6nxv3ql8pmt9sgerr5wvfqs4f23nt \
    --protocol-params-file pparams.json \
    ${MAGIC} \
    --out-file tx.raw
```

Sign the transaction:

    cardano-cli transaction sign --tx-body-file tx.raw --signing-key-file payment2.skey ${MAGIC} --out-file tx.sign

Submit the transaction:

    cardano-cli transaction submit ${MAGIC} --tx-file tx.sign
