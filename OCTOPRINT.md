# Hardware
* Raspberry Pi 3
* External USB power should be 2.5A
* typical is 400ma so probably good at 1A
* SD Card (Class 10) at lest 4GB More like 16GB


# Install Octopi
1. Download the latest image
   https://octopi.octoprint.org
1. Download the imaging software for windows
   https://sourceforge.net/projects/win32diskimager/
1. Create the Octopi image
   Insert a suitable SD card and image using the software
# Configure Octopi Network
1. On card edit octopi-network.txt
   ```
   ## WPA/WPA2 secured
   iface wlan-octopi inet manual
      wpa-ssid "put SSID here"
      wpa-psk "put password here"
   ```
# Boot raspberrypi
1. install sd card
1. Power on printer (So we don't power printer from raspberry pi)
   Note: have to figure out how to configure this
1. connect usb to printer
1. Connect usb power to raspberry pi
1. Should see lights bliking
1. Check access point for ip address assigned to octopi
   Note: for your AP it will get a DNS of octopi.saint-rossy.net automatically
1. ssh into octopi (do this before accessing web interface)
   Note: if ssh is inaccessible do a hard reboot
   default credentials are: pi/raspberry
1. Change the default password
   passwd
1. Initial Config
   ```
   sudo raspi-config
   ```
   expand the filesystem
   select "Finish"
1. Access via a web browser
   http://octopi
1. Set a username and password
1. Login (ignore any updates for now)
# Configure the raspberrypi
1. ssh into the raspberry pi

and do any recommended updates

