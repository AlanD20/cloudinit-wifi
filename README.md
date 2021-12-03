## Ubuntu Server Installation on RPi 4

- This repository is not the original one and if you want to you can [checkout Björn Samuelsson's original repository](https://gitlab.com/Bjorn_Samuelsson/raspberry-pi-cloud-init-wifi) for this script.

- if you have one of the following problems when installing Ubuntu server on Raspberry Pi 4, you may want to check this configuration out.

  1. Cloud-init fails to connect to Wifi.
  2. Can not find Raspberry pi ip address.
  3. Connecting your Raspberry pi 4 to a huge network.

- You can also run custom scripts or install packages at first boot.

#### 1. Flashing Image

Flash ubuntu server image with
[Etcher](https://www.balena.io/etcher/) or [Raspberry Pi Imager](https://www.raspberrypi.com/software/).

- If the `system-boot` volume doesn't show up on your mounted devices. Make sure to use [Raspberry Pi Imager](https://www.raspberrypi.com/software/) then before writing to the SD card, press `Ctrl+Shift+X` to open the hidden menu and tick `set hostname` and `Enable SSH` then write the ubuntu server image to the SD card again. It will create a configured file into the SD card that allows you to see the `system-boot` volume on your computer.

#### 2. Script Preparations

Download the repository and edit the following files in `config` folder.

- Customize time zone, hostname, and WiFi country code in [settings-public.sh](./config/settings-public.sh).
- Create the file settings-private.sh to set up shell-variables as shown in
  [settings-private-dummy.sh](./config/settings-private-dummy.sh).
  - You can create encrypted password with the following commands.
    `openssl passwd -6 <secret>`
  - Don't forget to skip $ characters with backslash and add forward slash at the end of the encrypted password.
- Customize the WiFi configuration in [user-data-template](./config/user-data-template) if needed.
- Remove the argument `ipv6.disable=1` from
  `system-boot/cmdline.txt` if you have a network connection that can use ipv6.
- Feel free to customize all the packages in `system-boot/cloud-init/setup.sh`.

#### 3. Setup procedure

- Run the script [config.sh](./config/config.sh) and check the output in `system-boot/cloud-init/user-data`.
- Copy the contents of `system-boot/` to the newly created boot partition on the SD card.
- Boot the Raspberry Pi with the SD card and wait for about 5-8 minutes.

#### 4. Finding Raspberry Pi IP Address

The easiest way to find your Raspberry Pi's IP address is using [Angry IP Scanenr](https://angryip.org/) that allows you to search for specific ports and shows only opened ports. The program is also works perfect with a huge network or a public network that has thousands of connected devices.

- First find your network's ip address.
  - **Windows:** `ipconfig /all`
  - **Linux:** `ifconfig`
- Find your available ip range with an [online ip calculator](https://www.calculator.net/ip-subnet-calculator.html) by putting the subnet mask and your local ip address and you will get the range of available ip address.
- Open `Angry IP Scanner` then enter your ip range. Then, go to `Tools > Preferences > Ports` clear everthing inside `Port Selection` and type `22` because we are only looking for SSH port not interested in other ports.
- Click scan and wait for the result.

With the provided steps you should now successfully accessed into your Raspberry Pi 4 with Ubuntu Server installed on.
