## Getting Started

This guide will show you how to get started with ReSpeaker Core V2.
<!-- todo: 这里会有一段话介绍getting started这个章节的内容／结构／目的 -->

### Prerequisites

- ReSpeaker Core V2
- Wi-Fi network
- 4GB(or more) SD card and SD card reader
- PC or Mac
- [USB To Uart Adapter](https://www.seeedstudio.com/USB-To-Uart-5V%26amp%3B3V3-p-1832.html)
- 5V 1A Micro USB for powering

<!-- ### Open Box -->

### Image Installation

Similar to Raspberry, ReSpeaker Core V2 needs you to install our image to an SD card to get it up and running. Steps for installing our image:


1. Download our latest image zip files: `respeaker-debian-9-lxqt-sd-********-4gb.img.xz` or `respeaker-debian-9-iot-sd-********-4gb.img.xz` at [百度云盘](https://pan.baidu.com/s/1c2piKW4) or [Google Drive](https://drive.google.com/open?id=0B7R2TH-ioqAKQjBfZGp0M3VaVjQ). The `lxqt` version comes with Debian desktop and the `iot` version does not. If you are new to ReSpeaker Core V2, `lxqt` version is recommended.

2. Burn the `*.img.xz` file directly to SD card with [Etcher](https://etcher.io/), or unzip the `*.img.xz` file to a `*.img` file, then burn it to SD card with other image writing tools.

3. After burning SD card, put the SD card in ReSpeaker Core V2. Power the board at `PWR_IN` micro usb port，note that **Try not to hot-plugging SD card**.

4. When powering, ReSpeaker Core V2 boot from the SD card(not eMMC), and you can see USER1 and USER2 LEDs light up. USER1 is typically configured at boot to blink in a heartbeat pattern and USER2 is typically configured at boot to light during SD card accesses. Now, you should go to the next part: [Serial Console](#serial-console).

5. If you want to flash the image to eMMC, please see [here](#flash-emmc).

![](/img/hardware.jpg)


### Serial Console

You can connect to the Serial Console with Uart port on board (if you can't find the Uart port, please see the picture above):

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

Configure network with **Network Manager** tool:
```
respeaker@v2:~$ sudo nmtui              # respeaker user needs sudo
```
Then you could see a config page like this, select `Activate a connection` and press `Enter`.

![](/img/nmtui1-1.png)

Select your Wi-Fi for ReSpeaker V2, press `Enter` and type your Wi-Fi password and `Enter` again. When you see a `*` mark, it means that the board has connected to Wi-Fi already. Tap `Esc` twice to leave the config page.

![](/img/nmtui1-2.png)

Check network connection with `ip address` command, you can get IP address of your device, to establish SSH or VNC. From the following log, I can know that my ReSpeaker V2 IP address is `192.168.7.108`.
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
    inet 192.168.7.108/24 brd 192.168.7.255 scope global dynamic wlan0
       valid_lft 604332sec preferred_lft 604332sec
    inet6 2601:647:4680:ebf0:ec0a:5965:e710:f329/64 scope global noprefixroute dynamic
       valid_lft 345598sec preferred_lft 345598sec
    inet6 fe80::64de:cac8:65ef:aac8/64 scope link
       valid_lft forever preferred_lft forever

```

Except UI configuration page, Network Manager also has a command line tool. You can use this for connecting a hidden Wi-Fi:
```
nmcli c add type wifi con-name mywifi ifname wlan0 ssid your_wifi_ssid
nmcli con modify mywifi wifi-sec.key-mgmt wpa-psk
nmcli con modify mywifi wifi-sec.psk your_wifi_password
nmcli con up mywifi
```

#### 2. Ethernet Setting Up
Temporarily unavailable.

### SSH & VNC
#### 1. SSH

SSH server starts automatically in ReSpeaker V2. For Windows Users, third-party SSH clients are available. For Linux/Mac Users, SSH client is built in.

- Windows: Use [PUTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), select `SSH` protocol, fill in correct IP address and click `open`. Login as `respeaker` user and password is `respeaker` too.


- Linux/Mac:
```
ssh respeaker@192.168.***.***
// password: respeaker
```

*Note that if you find there is not very smooth when using SSH, please switch to a uncrowed LAN network.*

#### 2. VNC
![](/img/vnc-2.png)

VNC service also starts automatically. Use [VNC Viewer](https://www.realvnc.com/en/connect/download/viewer/) or [VNC Viewer for Google Chrome](https://chrome.google.com/webstore/detail/vnc%C2%AE-viewer-for-google-ch/iabmpiboiopbgfabjmgeedhcmjenhbla?hl=en)] to connect to the desktop of ReSpeaker Core V2.

To use VNC, you should connect your PC/Mac and ReSpeaker V2 to the same Local-Area-Network. Then open VNC Viewer, type `192.168.xxx.xxx:5901` at the address bar. `192.168.xxx.xxx` is IP address of the board and `5901` is the default port of VNC service. If you meet `Unencrypted connection`, click `Continue` to go on. The password is `respeaker`.

![](/img/vnc-1.png)

If there is nothing showed in the VNC desktop, please right-click on the gray area, then select `terminal`, type `lxpanel` in the terminal.

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


### Flash eMMC

There is another way to boot ReSpeaker Core V2, without SD card. You could directly flash image files to eMMC with your PC or Mac:

1. Download our latest image zip file `respeaker-debian-9-iot-flasher-********-4gb.img.xz` or `respeaker-debian-9-lxqt-flasher-********-4gb.img.xz` at [百度云盘](https://pan.baidu.com/s/1c2piKW4) or [Google Drive](https://drive.google.com/open?id=0B7R2TH-ioqAKQjBfZGp0M3VaVjQ). The `lxqt` version comes with Debian desktop and the `iot` version does not. And the `flasher` version is for flashing eMMC, and the `sd` version is for booting from SD card.

2. Burn the `*.img.xz` file directly to SD card with [Etcher](https://etcher.io/), or unzip the `*.img.xz` file to a `*.img` file, then burn it to SD card with other image writing tools.

3. After burning SD card, put the SD card in ReSpeaker Core V2. Power the board at `PWR_IN` micro usb port，note that **Try not to hot-plugging SD card**.

4. When powering, ReSpeaker Core V2 **starts to flash eMMC**. And you can see USER1 and USER2 LEDs **blink alternately**. It will take about 10mins. When the LEDs turn off, you can power off the board, pull out the SD card and power again. If the LEDs light up, that means the image is flashed to eMMC already.

5. Also you can check image version with this command: `cat /etc/issue.net`.
