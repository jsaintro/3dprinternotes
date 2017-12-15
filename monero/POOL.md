1. Initially trying supportxmr.com

# Setup
1. Create a wallet
2. Select mining software
  Selecting https://github.com/fireice-uk/xmr-stak
3. Get your address

  ```
  monero-wallet-cli.exe
  address
  ```

# Compine the miner software
1. Get the source code
```
apt install libmicrohttpd-dev libssl-dev cmake build-essential libhwloc-dev
git clone https://github.com/fireice-uk/xmr-stak.git
cd xmr-stak
```

1. Remove the default donate of 2%

  ```
  vi xmrstak/donate-level.hpp
  set to 0.0
  ```
  Note: if you make some monero you might donate a bit to this guy

1. Compile
```
mkdir build
cd xmr-stak/build
cmake .. -DCUDA_ENABLE=OFF -DOpenCL_ENABLE=OFF -DMICROHTTPD_ENABLE=ON
make install
```

1. Add huge memory page support
sudo sysctl -w vm.nr_hugepages=128
vi /etc/sysctl.conf
vm.nr_hugepages = 128

vi /etc/security/limits.conf
jsaintrocc      soft    memlock         262144
jsaintrocc      hard    memlock         262144

Note: Put this before the end of file comment
Note: jsaintrocc is your user

1. Reboot
1. Verify huge pages
ulimit -l
262144

1. Run the miner
```
cd bin
./xmr-stak
Currency: monero
Pool address: pool.supportxmr.com:3333
  lowest difficulty is 3333 refer to the port list to change this, difficulty should match your hash rate
wallet_address: 48Bde2vJnr2PnSgPDEGNP14jTi6K8ByESYo9zdhda8qoM2fdMbg3izJbC4V1Sf8kq3TTEng1v2FhXgp9o3w7RcFs829yrs4
pool_password: jsaintrocc:james@saint-rossy.net
use_nicehash: false
use_tls: false
multi_pool: false
```
ex. ./xmr-stak -o pool.supportxmr.com:3333 -u 48Bde2vJnr2PnSgPDEGNP14jTi6K8ByESYo9zdhda8qoM2fdMbg3izJbC4V1Sf8kq3TTEng1v2FhXgp9o3w7RcFs829yrs4 -p jsaintrocc:james@saint-rossy.net --currency monero
   
  
