### THIS DIDN"T WORK AS THE CARD WAS TOO OLD


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

1. Download the CUDA toolkit (Assuming NVIDIA GPU)
   https://developer.nvidia.com/cuda-downloads

   Select "deb (network)"

   ```
   wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_9.1.85-1_amd64.deb
   ```
1. Install the nvidia repo
   ```
   dpkg -i cuda-repo-ubuntu1604_9.1.85-1_amd64.deb
   apt install -f   
   apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub
   apt update
   ```

1. See the latest driver that will work with your card
   http://www.nvidia.com/Download/index.aspx

   Get the driver number (Don't download it here)

1. Install the correct driver for your card
   ```
   apt install nvidia-340 nvidia-340-dev
   ```
1. Verify the driver
   ```
   lspci -vnn | grep -i VGA -A 12
   Should see Kernel driver in use: nvidia
   ```
   ```
   nvidia-smi
   ```
1. Install some dependencies
   ```
   sudo apt-get -y install gcc g++ build-essential automake linux-headers-$(uname -r) git gawk libcurl4-openssl-dev libjansson-dev xorg libc++-dev libgmp-dev python-dev
   ```
1. Install the legacy repo (Using 14.04 repo for 16.04)
   ```
   wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1404/x86_64/cuda-repo-ubuntu1404_6.5-19_amd64.deb
   dpkg -i cuda-repo-ubuntu1404_6.5-19_amd64.deb
   ```

1. install the toolkit
   ```
   sudo apt-get -y install cuda-toolkit-6-5
   ```

1. Add your user to the video group

   ```
   sudo usermod -a -G video $USER
   ```

1. Set some environement variables

   ```
   echo "" >> ~/.bashrc
   echo "export PATH=/usr/local/cuda-6.5/bin:$PATH" >> ~/.bashrc
   echo "export LD_LIBRARY_PATH=/usr/local/cuda-6.5/lib64:$LD_LIBRARY_PATH" >> ~/.bashrc
   ```   
 
1. Reboot

1. Validate CUDA
   ```
   cd /usr/local/cuda/samples/1_Utilities/deviceQuery && sudo make
   /usr/local/cuda/samples/1_Utilities/deviceQuery/deviceQuery
   ```
   You should see your card and "Result = PASS"

# Install ccminer
1. Install some dependencies

   ```
   sudo apt-get install libcurl4-openssl-dev libssl-dev libjansson-dev automake autotools-dev build-essential
   ```

1. Install legacy gcc4.8 and make it the default
```
sudo apt-get install gcc-4.8 g++-4.8
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 50
```

1. Download the ccminer source
```
git clone https://github.com/tpruvot/ccminer.git
cd ccminer
git checkout linux
```

1. Edit make file to use old compute engine
```
vi Makefile.am
nvcc_ARCH = -gencode=arch=compute_20,code=\"sm_21,compute_20\"
````

1. Configure ccminer build
```
./build.sh
./ccminer --verion
```

./ccminer -t 1 -a cryptonight -o stratum+tcp://pool.supportxmr.com:3333 -u 48Bde2vJnr2PnSgPDEGNP14jTi6K8ByESYo9zdhda8qoM2fdMbg3izJbC4V1Sf8kq3TTEng1v2FhXgp9o3w7RcFs829yrs4 -p jsaintrocc:james@saint-rossy.net
