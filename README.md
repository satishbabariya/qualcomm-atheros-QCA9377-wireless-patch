# Qualcomm-Atheros-QCA9377-wireless-not-working-on-lenovo-with-Ubuntu
Qualcomm Atheros QCA9377 wireless not working on lenovo with Ubuntu

This is how I fixed the WiFi issue in my Laptop

Identify your WiFi device. Open a terminal and issue

lspci  | grep Network
# It should display the name of your WiFi card
# If the output is similar to the one below, you are in luck, we can fix this easily
mansoor ~ $ lspci  | grep Network
03:00.0 Network controller: Qualcomm Atheros Device 0042 (rev 30)

Once you have made sure that the Network device is the one above, follow the below steps to install the driver for WiFi ##### Install git and tools to compile the driver

sudo apt-get install build-essential linux-headers-$(uname -r) git
Issue the following commands one by one. Anything written after “#” is a comment and you don’t have to execute it.

# Modify the config files
echo "options ath10k_core skip_otp=y" | sudo tee /etc/modprobe.d/ath10k_core.conf

# Download the backport
wget https://www.kernel.org/pub/linux/kernel/projects/backports/2015/11/20/backports-20151120.tar.gz

# Extract it
tar zxvf backports-20151120.tar.gz

# cd to the directory, compile and install it. The commands 'make' and 'make install' will take some time to finish
cd backports-20151120
make defconfig-wifi
make
sudo make install

# Download the firmware for the WiFi card
git clone https://github.com/kvalo/ath10k-firmware.git

# Copy the firmware to appropriate locations. 
sudo cp -r ath10k-firmware/QCA9377 /lib/firmware/ath10k/
sudo cp /lib/firmware/ath10k/QCA9377/hw1.0/firmware-5.bin_WLAN.TF.1.0-00267-1 /lib/firmware/ath10k/QCA9377/hw1.0/firmware-5.bin

Reboot your machine. That’s it. Your wifi should work now until you do a kernel update.
