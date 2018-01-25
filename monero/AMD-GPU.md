# OS prep
1. Install latest packages

```
apt update
apt dist-upgrade
```
1. Enable huge memory page support

```
sudo sysctl -w vm.nr_hugepages=128
vi /etc/sysctl.conf
# Enable hugepages 
vm.nr_hugepages=128
```

1. Unlimit huge memory for all users
```
vi /etc/security/limits.conf
* soft memlock 262144
* hard memlock 262144
```
Note: Do this before # End of file

1. Reboot

```
reboot
```

1. Verify huge pages

```
cat /proc/meminfo | grep Huge
HugePage_Total: 128
HugePage_Free: 128
Hugepagesize: 2048 kB
```

1. Verify user limits
```
ulimit -l -H
262144
```

# Install the AMD driver
1. Verify you are running the legacy/compatible driver 1st

   ```
    lspci -vv | grep AMD
08:00.0 VGA compatible controller: Advanced Micro Devices, Inc. [AMD/ATI] Device 67df (rev e7) (prog-if 00 [VGA controller])
08:00.1 Audio device: Advanced Micro Devices, Inc. [AMD/ATI] Device aaf0
   ```

1. Download the AMD Pro Driver from this link
   http://support.amd.com/en-us/kb-articles/Pages/AMDGPU-PRO-Driver-for-Linux-Release-Notes.aspx

   Note: Need to download from browser and scp over wget doesn't work

1. Uncompress the xz file

   ```
   tar xf amdgpu-pro-17.40-492261.tar.xz
   ```

1. Install

   ```
   cd amdgpu-pro-17.40-492261
   ./amdgpu-pro-install -y
   ```

1. Reboot

   ```
   reboot
   ```

1. Add user to video group


   ```
   sudo usermod -a -G video $LOGNAME 
   ```

1. Logout and back in

1. Install the rocm componets for OpenCL applications

   ```
   sudo apt install -y rocm-amdgpu-pro
   ```

1. Verify it works

   ```
   env LLVM_BIN=/opt/amdgpu-pro/bin /opt/amdgpu-pro/bin/clinfo
   Should see a bunch of output including `Board name`
   ```

1. Set the environment variable permanently

   ```
   echo 'export LLVM_BIN=/opt/amdgpu-pro/bin' | sudo tee /etc/profile.d/amdgpu-pro.sh
   ```

# Install the sdk 
1. Download the latest AMD SDK from 
  
   http://developer.amd.com/amd-accelerated-parallel-processing-app-sdk/

1. Extract and install

   ```
   tar xf AMD-APP-SDKInstaller-v3.0.130.136-GA-linux64.tar.bz2
   sudo sh ./AMD-APP-SDKInstaller-v3.0.130.136-GA-linux64.sh

   ```

1. Relogin

1. Verify

   ```
   /opt/AMDAPPSDK-3.0/bin/x86_64/clinfo
   Should see a bunch of output including `Board name`
   ```   
   
1. Fix missing library path

   ```
   cd /opt/AMDAPPSDK-3.0/lib/x86_64
   sudo ln -sf sdk/libOpenCL.so.1 libOpenCL.so
   ```

# Compine the miner software
1. Do the compile (Modern Ubuntu Gt 14.04)

   ```
   cd ~
   sudo apt install libmicrohttpd-dev libssl-dev cmake build-essential libhwloc-dev
   git clone https://github.com/fireice-uk/xmr-stak.git
   cd xmr-stak
   ```
1. Zero out the donation level

  ```
  vi xmrstak/donate-level.hpp
  set to 0.0
  ```
  Note: if you make some monero you might donate a bit to this guy

1. Time to compile

```
mkdir build
cd build
cmake -DMICROHTTPD_ENABLE=ON -DOpenCL_ENABLE=ON -DCUDA_ENABLE=OFF -DXMR-STAK_CURRENCY=monero ..
make install
```

1. Run the miner
```
cd bin
./xmr-stak -o pool.supportxmr.com:3333 -u {your wallet id} -p uniqueid:email
Currency: monero
Pool address: pool.supportxmr.com:3333
  lowest difficulty is 3333 refer to the port list to change this, difficulty should match your hash rate
wallet_address: you got this up above
pool_password: uniqueid:email
use_nicehash: false
use_tls: false
multi_pool: false

My RX 580
W Switch towards heatsink
GPU H/s 574.3 TOT H/s ??? Watt 290
W Switch towards case and 4 cores
H/s 577.2 TOT H/s 660 Watt 297 $520.40
W Switch towards case and 4 cores I 1024 W 8
H/s 585.2 TOT H/s 670 Watt 300 $537.88 TESTING
W Switch towards case and 8 Cores
H/s 574.3 TOT H/s 677 Watt 318 $518.60

## Overclocking in linux
sudo sh -c "echo 5 > /sys/class/drm/card0/device/pp_sclk_od"
Increase GPU freq by 5%
Note: 5% seems to be the max for the card before you start seeing errors
Note: This seems sketchy as you need to adjust voltages which isn't easy to do.

H/s 577.2 TOT H/s 660 Watt 297 $520.40 TESTING
W Switch towards case and 5 Cores GPU worksize 1024
H/s 574.3 TOT H/s 684 Watt 312 $372.04
W Switch towards case and 8 Cores
H/s 574.3 TOT H/s 677 Watt 318 $518.60
W Switch towards case and 4 Cores GPU worksize 1024
H/s 589.3 TOT H/s 675 Watt 300 $378.26
