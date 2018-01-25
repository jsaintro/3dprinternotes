1. Download the bin
  Main site can be slow
  try the gui version and the github site
  verify the sha256sum

1. download the raw blockchain as a strting point
  
  ```
  wget -c --progress=bar https://downloads.getmonero.org/blockchain.raw

  ```

1. import the blockchain

   ```
   ./monero-blockchain-import --verify 0 --input-file ./blockchain.raw

   ``` 

# Accessing the wallet
1. Sync first
  ./monerod
  wallet: monero-wallet
  password: see lastpass

1. run wallet cli
  ./monero-wallet-cli.exe
  balance

  Note Will need to fully sync first
