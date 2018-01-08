## Getting Started

[中文版](/cn/ReSpeaker_Core_V2/getting_started.md)

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

    <a href="https://bfaceafsieduau-my.sharepoint.com/personal/miaojg22_off365_cn/_layouts/15/guestaccess.aspx?folderid=0bb3c4f3f122d4c2bb0f65eee2b5938f8&authkey=AfLSkcE8QeeUHTQ8GGfrrsU"><img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/onedrive.png?raw=true" height="25"></img></a>

    (old image at [百度云盘](https://pan.baidu.com/s/1c2piKW4) or [Google Drive](https://drive.google.com/open?id=0B7R2TH-ioqAKQjBfZGp0M3VaVjQ))

    The `lxqt` version comes with Debian desktop and the `iot` version does not. If you are new to ReSpeaker Core V2, `lxqt` version is recommended.

2. Burn the `*.img.xz` file directly to your SD card with [Etcher](https://etcher.io/), or unzip the `*.img.xz` file to a `*.img` file, then burn it to SD card with other image writing tools.
![](/img/v2-flash-sd.png)

3. After writing the image to the SD card, insert the SD card in your ReSpeaker Core V2. Power the board using the `PWR_IN` micro usb port and DO NOT remove the SD card after powering on. ReSpeaker Core V2 will boot from the SD card, and you can see USER1 and USER2 LEDs light up. USER1 is typically configured at boot to blink in a heartbeat pattern and USER2 is typically configured at boot to light during SD card accesses. Now, you should go to the next part: [Serial Console](#serial-console).

4. If instead you want to flash the image to the ReSpeaker's eMMC (onboard flash memory), follow these instructions [here](#flash-emmc).

<div align="center"><img src="/img/hardware.jpg" width="80%" ></div>


### Serial Console

Now that your ReSpeaker can boot (it runs Debian Linux), you might want to get access to the Linux system by a console, to setup the ssh server, or setup WiFi, etc. You will have two choices to get the console, but please note that the first choice depends on your hardware version and your system version.

- A. The OTG USB port, for hardware version not earlier than "8/5/2017" (see the silk-screen on the board) and system image version not earlier than "20171023".
- B. The UART port

#### A. The OTG USB port

1. Find a micro USB cable, and please make sure it's a data cable (not just a power cable), plug the micro USB end to the ReSpeaker's `OTG` micro USB port (There're two micro USB ports on the ReSpeaker board, which are labeled with different silk-screen, one is `PWR_IN` and another is `OTG`), then another end of this cable into your computer.

2. Check at your computer if the serial port has risen

    - Windows: check the device manager, there should be new serial deviced named `COMx` which `x` is an increasing number. If you use windows XP/7/8, maybe you  need install [windows CDC drivers](https://github.com/respeaker/get_started_with_respeaker/blob/master/files/ReSpeaker_Gadget_CDC_driver.7z).
    - Linux: `ls /dev/ttyACM*`, you should get `/dev/ttyACMx` where `x` will vary depending on which USB port you used
    - Mac: `ls /dev/cu.usb*`, you should get `/dev/cu.usbmodem14xx` where `xx` will vary depending on which USB port you used
   
3. Use your favorite serial debugging tool to connect the serial port, the serial has: 115200 baud rate, 8Bits, Parity None, Stop Bits 1, Flow Control None. For examples:

    - Windows: use [PUTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), select `Serial` protocol, fill in the correct COM port of ReSpeaker Core V2, 115200 baud, 8Bits, Parity None, Stop Bits 1, Flow Control None
    - Linux: Depend on your USB To TTL Adapter, it could be `screen /dev/ttyACM0(,1, and so on) 115200` or `screen /dev/ttyUSB0(,1, and so on) 115200`
    - Mac: Depend on your USB To TTL Adapter, it could be `screen /dev/cu.usbserial1412(,1422, and so on) 115200` or `screen /dev/cu.usbmodem1412(,1422, and so on) 115200`
  
4. The login user name is `respeaker`, and password is `respeaker` too.

<div align="center"><img src="/img/v2_login.png"></div>

#### B. The UART port

In this section we will guide you how to establish a connection from your computer to your ReSpeaker using your USB to TTL adapter which will be connected to the ReSpeaker's Uart port (Uart port located just to the left of the ReSpeaker speaker plug).

1. Connect Uart port and your PC/Mac with an [USB To TTL Adapter](https://www.seeedstudio.com/USB-To-Uart-5V%26amp%3B3V3-p-1832.html). Note that the voltage of RX/TX are 3.3V. If you don't have an USB To TTL Adapter, please see **step 4**.

2. Use the following Serial debugging tools with 115200 baud:
    - Windows: use [PUTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), select `Serial` protocol, fill in the correct COM port of ReSpeaker Core V2, 115200 baud, 8Bits, Parity None, Stop Bits 1, Flow Control None
    - Linux: Depend on your USB To TTL Adapter, it could be `screen /dev/ttyACM0(,1, and so on) 115200` or `screen /dev/ttyUSB0(,1, and so on) 115200`
    - Mac: Depend on your USB To TTL Adapter, it could be `screen /dev/cu.usbserial1412(,1422, and so on) 115200` or `screen /dev/cu.usbmodem1412(,1422, and so on) 115200`

3. The login user name is `respeaker`, and password is `respeaker` too.

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
Now follow step 2 above to connect to your Respeaker over this serial connection. And note this is a one time procedure as you'll next setup your Respeaker for Wi-Fi connectivity and then connect via ssh or VNC going forward.



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
You can connect to a network using an Ethernet cable.

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

To use VNC, connect your PC/Mac and ReSpeaker V2 to the same Wi-Fi network. Then open VNC Viewer, input the IP address of your board, e.g. `192.168.1.100`, this will implicitly use the default port `5900` of the VNC protocol. If you're still using the system version prior to `20180107`, use port `5901`, i.e. input `192.168.x.x:5901` at the address bar of your VNC viewer. If you meet `Unencrypted connection`, click `Continue` to go on. The password is `respeaker`.

![](https://user-images.githubusercontent.com/5130185/34665797-93b222d6-f49c-11e7-8112-704f91163038.png)

For system version prior to `20180107`:
![](/img/vnc-1.png)

If nothing appears in the VNC desktop, please right-click on the gray area, then select `terminal`, type `lxpanel` in the terminal.

*Note that if you find there is not very smooth when using VNC, please switch to a uncrowed LAN network.*

### ALSA Setting

The following steps are no longer needed after system image version `xxxx`(TODO).

```
su respeaker && cd     # skip this steps if you are already using respeaker user
git clone https://github.com/respeaker/respeaker_v2_eval.git       # skip this step if you have already downloaded
cd ~/respeaker_v2_eval
sudo cp asound.conf /etc/
```

### PulseAudio Setting

The following steps are no longer needed after system image version `xxxx`(TODO).

```
su respeaker && cd     # skip this steps if you are already using respeaker user
git clone https://github.com/respeaker/respeaker_v2_eval.git       # skip this step if you have already downloaded
cd ~/respeaker_v2_eval
sudo cp pulse/default.pa /etc/pulse/            # config pulseaudio
cp pulse/client.conf ~/.config/pulse/
pulseaudio -k && pulseaudio -D                  # restart pulseaudio, don't run it as root user
```

### Voice Capture and Playback Testing

```
# record & playback 2 channels audio
arecord -Dhw:0,0 -f S16_LE -r 16000 -c 2 hello.wav
aplay -Dhw:0,2 -r 16000 -c 2 hello.wav
arecord -v -f cd hello.wav
aplay hello.wav
# record 8 channels audio
# there are 6 microphones on board, and ac108 compose the 2 remaining channels.
arecord -Dhw:0,0 -f S16_LE -r 16000 -c 8 hello.wav
```

### Voice Engine Setting

Setup Python virtual environment:

```
sudo pip install virtualenv                                # install virtualenv
python -m virtualenv --system-site-packages ~/env          # create python virtual environment
source ~/env/bin/activate                                  # activate python venv
deactivate                                                 # deactivate python venv
```

Install and configure ReSpeaker Voice Engine in virtual environment:

```
source ~/env/bin/activate                                  # activate python venv
cd ~/respeaker_v2_eval
sudo apt update
# These packages might be pre-installed in the system, but double check here
sudo apt install libatlas-base-dev gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gir1.2-gstreamer-1.0 python-gi python-gst-1.0
pip install ./webrtc*.whl
pip install ./snowboy*.whl
pip install avs
pip install voice-engine
```

After installation, free feel to build your own [AVS(Amazon Voice Service)](/docs/ReSpeaker_Core_V2/avs_guide.md) on ReSpeaker Core V2.


### Flash eMMC (Onboard Flash Memory)

You may also directly flash the ReSpeaker image files to the ReSpeaker's eMMC (onboard flash memory) using your PC or Mac. Then the ReSpeaker will boot from it's eMMC (onboard flash memory) and not from the SD card.

1. Download our latest image zip file `respeaker-debian-9-iot-flasher-********-4gb.img.xz` or `respeaker-debian-9-lxqt-flasher-********-4gb.img.xz` at [OneDrive](https://bfaceafsieduau-my.sharepoint.com/personal/miaojg22_off365_cn/_layouts/15/guestaccess.aspx?folderid=0bb3c4f3f122d4c2bb0f65eee2b5938f8&authkey=AfLSkcE8QeeUHTQ8GGfrrsU). The `lxqt` version comes with Debian desktop and the `iot` version does not. And the `flasher` version is for flashing eMMC, and the `sd` version is for booting from SD card.

2. Burn the `*.img.xz` file directly to SD card with [Etcher](https://etcher.io/), or unzip the `*.img.xz` file to a `*.img` file, then burn it to SD card with other image writing tools.

3. After burning SD card, insert the SD card in the ReSpeaker Core V2. Power the board using the `PWR_IN` micro usb port and **do not remove the SD card while it's flashing**.

4. During the flashing process, you'll see the USER1 and USER2 LEDs **blink alternately**. It will take about 10 minutes to complete. When the LEDs turn off, you can power off the board, pull out the SD card and power again. If the LEDs light up, that means the image was flashed to the eMMC correctly.

5. You can also check the image version with this command: `cat /etc/issue.net`.
