# Sent ADA into another address

cardano-cli transaction build \
    --alonzo-era \
    --tx-in 6aa3567809f5ebd5717c715c168ed6cc169f9ef703ae11a2d192283b031411e6#0 \
    --tx-out $(cat payment2.addr)+5000000 \
    --change-address $(cat payment.addr) \
    --testnet-magic 8 \
    --out-file tx.raw

cardano-cli transaction sign \
    --testnet-magic 8 \
    --signing-key-file payment.skey \
    --tx-body-file tx.raw \
    --out-file tx.sign

cardano-cli transaction submit --testnet-magic 8 --tx-file tx.sign

-- ---------------

cardano-cli transaction build \
    --alonzo-era \
    --tx-in 460b7f923e9891088ecf79bf48c2890c5ac201d6d2345990d8739a92ae346faa#1 \
    --tx-out $(cat payment.addr)+3000000 \
    --change-address $(cat payment2.addr) \
    --testnet-magic 8 \
    --out-file tx.raw

cardano-cli transaction sign \
    --testnet-magic 8 \
    --signing-key-file payment2.skey \
    --tx-body-file tx.raw \
    --out-file tx.sign

cardano-cli transaction submit --testnet-magic 8 --tx-file tx.sign