# Minimal boot image for Lichee RV dock (currently 30MB to download)

Lichee RV system-on-module is using Allwinner D1 SOC. Inside the chip there is a 64-bit single core low cost RISC-V CPU. Paired with 512MB DDR3 RAM its nice, small and low-power system. It is hardly any replacement for high performance SBC's (Raspberry Pi, Banana Pi, Rock Pi etc.). You probably don't want to run desktop operating system with that board. Also, the Allwinner D1 SOC itself is not fully supported by mainline Linux kernel yet (as of january 2023).

This repo presents specially built linux based system for Lichee RV dock. Its targeted for IoT use cases. Or just for fun.

The image was generated based on customized `buildroot` master branch. One important change is added I2C support (look into /root/demos/i2c).
The size of the unpacked image is ca. 240MB. There is 125MB free space for you initially. The rest is used by the system.

This may look an oddly small operating system but its a real thing.
1. Download
2. Unpack
3. Write the image to SD card
- on windows use eg. Raspberry Pi imager (you have to select `Use custom` option at the end of the list)
- on linux use `dd` command

Insert SD card, connect HDMI display cable (the resolution is set to `1280x800` initially) and power up the board via USB-C port. Also, when working with the board directly you have to connect keyboard to the USB port. You need this to configure WiFi first time.
When the system boots it will play sound (to hear this you have to connect `4-8 Ohm` speaker to the speaker connector on the board) and the RGB led will blink several times.

The hostname is initially `rvdock`.
To log in - username: `root`, password: `rvdock`

You have to configure WiFi manually:
```
# wpa_passphrase "yourssid" yourpasswd >> /etc/wpa_supplicant.conf
# /etc/init.d/S40network restart
# ip a
```
Now you can login via `ssh` also. Warning - you may want to remove plain text password from `/etc/wpa_supplicant.conf` later.

What the system includes:
* linux kernel 6.1-rc3 riscv64
* ssh server (dropbear)
* python 3.10.8 with modules `libevdev` `spidev` `smbus_cffi` `serial` `requests` `alsaaudio`
* wget (with SSL)
* preinstalled CA certificates
* `nano` text editor
* vfat/exfat/ext2 tools
* alsa utils (`amixer`, `aplay`, `alsamixer`)
* pipewire for audio processing (configuration part is missing)
* utils to manipulate GPIOs: `gpiodetect`, `gpioinfo`, `gpioset`,  `gpioget` (check the demo `gpio/gpio_control.sh`)
* NTP (not yet fully configured)
* `htop` command for better monitoring
* simple demo scripts under `/root/demos`

Issues:
- onboard mic doesn't work yet
