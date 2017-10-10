## Getting Started

This guide will show you how to get started with ReSpeaker Core V2.
<!-- todo: 这里会有一段话介绍getting started这个章节的内容／结构／目的 -->

### Prerequisites

- ReSpeaker Core V2
- Wi-Fi Network
- 4GB (or more) SD card and SD card reader
- PC or Mac
- [USB To Uart Adapter](https://www.seeedstudio.com/USB-To-Uart-5V%26amp%3B3V3-p-1832.html)
- 5V 1A Micro USB adapter for power

<!-- ### Open Box -->

### Image Installation

Similar to a Raspberry Pi, you must install the ReSpeaker Core V2 image from an SD card to get up and running. Follow these steps to install the image:



1. Download our latest image zip files: `respeaker-debian-9-lxqt-sd-********-4gb.img.xz` or `respeaker-debian-9-iot-sd-********-4gb.img.xz` : 

<p href="https://1drv.ms/f/s!AqG2uRmVUhlShTwmHJfrLUZ9rlMR"><img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/onedrive.png?raw=true" height="25"></img></p>

(old image at [百度云盘](https://pan.baidu.com/s/1c2piKW4) or [Google Drive](https://drive.google.com/open?id=0B7R2TH-ioqAKQjBfZGp0M3VaVjQ))

The `lxqt` version comes with Debian desktop and the `iot` version does not. If you are new to ReSpeaker Core V2, `lxqt` version is recommended.

2. Burn the `*.img.xz` file directly to your SD card with [Etcher](https://etcher.io/), or unzip the `*.img.xz` file to a `*.img` file, then burn it to SD card with other image writing tools.
![](/img/v2-flash-sd.png)

3. After writing the image to the SD card, insert the SD card in your ReSpeaker Core V2. Power the board using the `PWR_IN` micro usb port and DO NOT remove the SD card after powering on. ReSpeaker Core V2 will boot from the SD card, and you can see USER1 and USER2 LEDs light up. USER1 is typically configured at boot to blink in a heartbeat pattern and USER2 is typically configured at boot to light during SD card accesses. Now, you should go to the next part: [Serial Console](#serial-console).

4. If instead you want to flash the image to the ReSpeaker's eMMC (onboard flash memory), follow these instructions [here](#flash-emmc).

![](/img/hardware.jpg)


### Serial Console

Now that your ReSpeaker can boot (it runs Debian Linux), you will next establish a connection from your computer to your ReSpeaker using your USB to TTL adapter which will be connected to the ReSpeaker's Uart port (Uart port located just to the left of the ReSpeaker speaker plug).

1. Connect Uart port and your PC/Mac with an [USB To TTL Adapter](https://www.seeedstudio.com/USB-To-Uart-5V%26amp%3B3V3-p-1832.html). Note that the voltage of RX/TX are 3.3V. If you don't have an USB To TTL Adapter, please see **step 4**.

2. Use the following Serial debugging tools with 115200 baud:
    - Windows: usb [PUTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), select `Serial` protocol, fill in the correct COM port of ReSpeaker Core V2, 115200 baud, 8Bits, Parity None, Stop Bits 1, Flow Control None
    - Linux: Depend on your USB To TTL Adapter, it could be `screen /dev/ttyACM0(,1, and so on) 115200` or `screen /dev/ttyUSB0(,1, and so on) 115200`
    - Mac: Depend on your USB To TTL Adapter, it could be `screen /dev/cu.usbserial1412(,1422, and so on) 115200` or `screen /dev/cu.usbmodem1412(,1422, and so on) 115200`

3. The login user name is `respeaker`, and password is `respeaker` too.
![v2_login.png](/img/v2_login.png)

4. If you do not have a USB to TTL Adapter, you may also use an Arduino. If using an Arduino, connect one end of a jumper wire to the *RESET* pin on the Arduino and the other end to the *GND* pin on the Arduino. This will bypass your Arduino's ATMEGA MCU and turn your Arduino into a USB to TTL adapter, see video tutorial [here](https://www.youtube.com/watch?v=qqSLwK1DP8Q). Now connect the *GND* pin on the Arduino to the *GND* pin on the *Uart port* of the Respeaker. Connect the *Rx* pin on the Arduino to the *Rx* pin on the Uart port of the Respeaker. Connect the *Tx* pin on the Arduino to the *Tx* pin on the Uart port of the Respeaker. And lastly, connect the Arduino to your PC/Mac via the Arduino's USB cable. Now check that your Mac or Linux PC finds your Arduino by typing this command:
```
ls /dev/cu.usb* (Mac)
ls /dev/ttyACM* (Linux)
```
You should get back something like:
```
/dev/cu.usbmodem14XX where XX will vary depending on which USB port you used (on Mac)
/dev/ttyACMX where X will vary depending on which USB port you used  (on Linux)
```
Now use this port with the commands above to connect to your Respeaker over this serial connection. And note this is a one time procedure as you'll next setup your Respeaker for Wi-Fi connectivity and then connect via ssh or VNC going forward.



### Network Setting Up

#### 1. Wi-Fi Setting Up

Configure your ReSpeaker's network with the Network Manager tool, nmtui. nmtui will already be installed on the ReSpeaker image.  
```
respeaker@v2:~$ sudo nmtui              # respeaker user needs sudo
```
Then you will see a config page like this, select `Activate a connection` and press `Enter`.

![](/img/nmtui1-1.png)

Select your Wi-Fi for ReSpeaker V2, press `Enter` and type your Wi-Fi password and `Enter` again. When you see a `*` mark, it means that your ReSpeaker has successfully connected to your Wi-Fi network. Tap `Esc` twice to leave the network manager config tool.

![](/img/nmtui1-2.png)

Now find the IP address of your ReSpeaker by using the `ip address` command. In the example below, we can see that this ReSpeaker's IP address is `192.168.7.108`
```
root@v2:/home/respeaker# ip address

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: sit0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1
    link/sit 0.0.0.0 brd 0.0.0.0
3: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether e0:76:d0:37:38:6d brd ff:ff:ff:ff:ff:ff
    inet **192.168.7.108**/24 brd 192.168.7.255 scope global dynamic wlan0
       valid_lft 604332sec preferred_lft 604332sec
    inet6 2601:647:4680:ebf0:ec0a:5965:e710:f329/64 scope global noprefixroute dynamic
       valid_lft 345598sec preferred_lft 345598sec
    inet6 fe80::64de:cac8:65ef:aac8/64 scope link
       valid_lft forever preferred_lft forever

```

In addition to the Networ Manager GUI interface, Network Manager also has a command line tool. If you are connecting to a hidden Wi-Fi network, you'll need to use this command line tool:
```
nmcli c add type wifi con-name mywifi ifname wlan0 ssid your_wifi_ssid
nmcli con modify mywifi wifi-sec.key-mgmt wpa-psk
nmcli con modify mywifi wifi-sec.psk your_wifi_password
nmcli con up mywifi
```

#### 2. Ethernet Connectivity
Sorry, Ethernet connectivity is currently not supported and will be added in a future release.

### SSH & VNC
#### 1. SSH

SSH server starts automatically in ReSpeaker V2. For Windows Users, third-party SSH clients are available. For Linux/Mac Users, SSH client is built in.

- Windows: Use [PUTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), select `SSH` protocol, fill in correct IP address and click `open`. Login as `respeaker` user and password is `respeaker` too.


- Linux/Mac:
```
ssh respeaker@192.168.***.***
// password: respeaker
```

*Note that if experience slow performance using SSH, please switch to a less crowded WiFi network.*

#### 2. VNC
![](/img/vnc-2.png)

The VNC service also starts automatically. Use [VNC Viewer](https://www.realvnc.com/en/connect/download/viewer/) or [VNC Viewer for Google Chrome](https://chrome.google.com/webstore/detail/vnc%C2%AE-viewer-for-google-ch/iabmpiboiopbgfabjmgeedhcmjenhbla?hl=en)] to connect to the desktop of ReSpeaker Core V2.

To use VNC, connect your PC/Mac and ReSpeaker V2 to the same Wi-Fi network. Then open VNC Viewer, type `192.168.xxx.xxx:5901` at the address bar. `192.168.xxx.xxx` is IP address of the board and `5901` is the default port of VNC service. If you meet `Unencrypted connection`, click `Continue` to go on. The password is `respeaker`.

![](/img/vnc-1.png)

If nothing appears in the VNC desktop, please right-click on the gray area, then select `terminal`, type `lxpanel` in the terminal.

*Note that if you find there is not very smooth when using VNC, please switch to a uncrowed LAN network.*

### ALSA Setting

Download Github repository with `respeaker` user:
```
su respeaker && cd     # skip this steps if you are already using respeaker user
git clone https://github.com/respeaker/respeaker_v2_eval.git       # skip this step if you have already downloaded
cd ~/respeaker_v2_eval
sudo cp asound.conf /etc/
```

### PulseAudio Setting

<!-- #### 方式1 -->
```
su respeaker && cd     # skip this steps if you are already using respeaker user
git clone https://github.com/respeaker/respeaker_v2_eval.git       # skip this step if you have already downloaded
cd ~/respeaker_v2_eval
sudo cp pulse/default.pa /etc/pulse/            # config pulseaudio
cp pulse/client.conf ~/.config/pulse/
pulseaudio -k && pulseaudio -D                  # restart pulseaudio, don't run it as root user
```

### Virtual Environment

```
pip install virtualenv                                     # install virtualenv
python -m virtualenv --system-site-packages ~/env          # create python virtual environment
source ~/env/bin/activate                                  # activate python venv
deactivate                                                 # deactivate python venv
```

### Voice Engine Setting

Install and configure ReSpeaker Voice Engine in virtual environment:
```
source ~/env/bin/activate                                  # activate python venv
cd ~/respeaker_v2_eval
sudo apt update
sudo apt install libatlas-base-dev                         
pip install ./webrtc*.whl
pip install ./snowboy*.whl
pip install avs
pip install voice-engine
```

After installation, free feel to build your own [AVS(Amazon Voice Service)](/docs/ReSpeaker_Core_V2/avs_guide.md) on ReSpeaker Core V2.

### Voice Capture and Playback

```
// record & playback 2channels
arecord -Dhw:0,0 -f S16_LE -r 16000 -c 2 hello.wav
aplay -Dhw:0,2 -r 16000 -c 2 hello.wav
arecord -v -f cd hello.wav
aplay hello.wav
// record 8channels
// there are 6 microphones on board, and ac108 compose the 2 remaining channels.
arecord -Dhw:0,0 -f S16_LE -r 16000 -c 8 hello.wav
```


### Flash eMMC (Onboard Flash Memory)

You may also directly flash the ReSpeaker image files to the ReSpeaker's eMMC (onboard flash memory) using your PC or Mac. Then the ReSpeaker will boot from it's eMMC (onboard flash memory) and not from the SD card.

1. Download our latest image zip file `respeaker-debian-9-iot-flasher-********-4gb.img.xz` or `respeaker-debian-9-lxqt-flasher-********-4gb.img.xz` at [百度云盘](https://pan.baidu.com/s/1c2piKW4) or [Google Drive](https://drive.google.com/open?id=0B7R2TH-ioqAKQjBfZGp0M3VaVjQ). The `lxqt` version comes with Debian desktop and the `iot` version does not. And the `flasher` version is for flashing eMMC, and the `sd` version is for booting from SD card.

2. Burn the `*.img.xz` file directly to SD card with [Etcher](https://etcher.io/), or unzip the `*.img.xz` file to a `*.img` file, then burn it to SD card with other image writing tools.

3. After burning SD card, insert the SD card in the ReSpeaker Core V2. Power the board using the `PWR_IN` micro usb port and **do not remove the SD card while it's flashing**.

4. During the flashing process, you'll see the USER1 and USER2 LEDs **blink alternately**. It will take about 10 minutes to complete. When the LEDs turn off, you can power off the board, pull out the SD card and power again. If the LEDs light up, that means the image was flashed to the eMMC correctly.

5. You can also check the image version with this command: `cat /etc/issue.net`.
