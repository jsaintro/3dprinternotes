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
