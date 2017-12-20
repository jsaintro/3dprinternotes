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
1. Download the source code
  ```
  wget https://github.com/fireice-uk/xmr-stak/archive/v2.1.0.tar.gz
  tar -xzvf v2.1.0.tar.gz
  cd v2.1.0.tar.gz
  ```

1. Zero out the donation level

  ```
  vi xmrstak/donate-level.hpp
  set to 0.0
  ```
  Note: if you make some monero you might donate a bit to this guy

1. Download the CUDA toolkit (Assuming NVIDIA GPU)
   Note: Becuase we're using Ubuntu 14.04 we're using the older libraries
   https://developer.nvidia.com/cuda-80-ga2-download-archive

   Select the deb (network)
   
   ```
   apt install systemd
   mkdir cuda
   cd cuda
   wget https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda-repo-ubuntu1404-8-0-local-ga2_8.0.61-1_amd64-deb
   wget https://developer.nvidia.com/compute/cuda/8.0/Prod2/patches/2/cuda-repo-ubuntu1404-8-0-local-cublas-performance-update_8.0.61-1_amd64-deb
   ```
1. Install
   ```
   dpkg -i cuda-repo-ubuntu1404-8-0-local-ga2_8.0.61-1_amd64-deb
   apt-get update
   apt-get install cuda
   apt-get autoremove
   dpkg -i cuda-repo-ubuntu1404-8-0-local-cublas-performance-update_8.0.61-1_amd64-deb
   apt install -f
   apt upgrade

   ```

1. Do the compile (Modern Ubuntu Gt 14.04)
```
sudo apt install libmicrohttpd-dev libssl-dev cmake build-essential libhwloc-dev
git clone https://github.com/fireice-uk/xmr-stak.git
### Remove the automatic donation to the dev ... see above
mkdir xmr-stak/build
cd xmr-stak/build
cmake ..
make install
```
1. For Ubuntu 14.04
```
    # Ubuntu 14.04
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt update
    sudo apt install gcc-5 g++-5 make
    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 1 --slave /usr/bin/g++ g++ /usr/bin/g++-5
    curl -L http://www.cmake.org/files/v3.4/cmake-3.4.1.tar.gz | tar -xvzf - -C /tmp/
    cd /tmp/cmake-3.4.1/ && ./configure && make && sudo make install && cd -
    sudo update-alternatives --install /usr/bin/cmake cmake /usr/local/bin/cmake 1 --force
    sudo apt install libmicrohttpd-dev libssl-dev libhwloc-dev
    git clone https://github.com/fireice-uk/xmr-stak.git
    ### Remove the automatic donation to the dev ... see above
    mkdir xmr-stak/build
    cd xmr-stak/build
    cmake -D CUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda-8.0 -DOpenCL_ENABLE=OFF ..
    make install
```
1. Add huge memory page support
sudo sysctl -w vm.nr_hugepages=128
vi /etc/security/limits.conf
soft memlock 262144
hard memlock 262144

1. Run the miner
```
cd bin
./xmr-stak
Currency: monero
Pool address: pool.supportxmr.com:3333
  lowest difficulty is 3333 refer to the port list to change this, difficulty should match your hash rate
wallet_address: you got this up above
pool_password: uniqueid:email
use_nicehash: false
use_tls: false
multi_pool: false

   
  
