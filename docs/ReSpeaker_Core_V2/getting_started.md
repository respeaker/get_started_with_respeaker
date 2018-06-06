## Basics

This guide will show you the basic operations of ReSpeaker Core V2. Please note that this wiki is a developer preview wiki during the development phase of ReSpeaker Core V2. The documentations will be finalized into [the official wiki pages of ReSpeaker product line](http://wiki.seeed.cc/ReSpeaker) after release.

(@xyu6  From now on, this wiki will drop all the verbosities of operations on old system versions, and will always assume that the latest system image is used.)

### Prerequisites

- ReSpeaker Core V2
- Wi-Fi Network
- 4GB (or bigger) SD card and SD card reader
- PC or Mac
- [USB To Uart Adapter](https://www.seeedstudio.com/USB-To-Uart-5V%26amp%3B3V3-p-1832.html) (optional)
- 5V 1A Micro USB adapter for power (optional)

### Hardware Ports

<div align="center"><img src="/img/hardware.jpg" width="80%" ></div>

### Image Installation

Similar to a Raspberry Pi, you must burn the system image of ReSpeaker Core V2  to an SD card to get it up and running. You may noticed that there's also an eMMC (Embedded Multi Media Card) onboard. We'll burn the latest system into the onboard eMMC when every unit of ReSpeaker Core V2 is shipped. But feel free to upgrade/re-flash the system image inside eMMC according to the guide [here](#flash-emmc). Now please follow these steps to burn the system image:

1. Download our latest system image from <a href="https://ibschoolnz-my.sharepoint.com/:f:/g/personal/baozhu_zuo_vace_co/Eny3PWvb3xdArLutUBAITwIB2t4kMZaAnkp94jAY0_3Pog"><img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/onedrive.png?raw=true" height="25"></img></a>.

    The name of the files follows such convention:
    `respeaker-debian-9-[iot|lxqt]-[flasher|sd]-[date]-4gb.img.xz`

    | section      | description                              |
    | ------------ | ---------------------------------------- |
    | `iot` / `lxqt`   | The `lxqt` version comes with a desktop GUI while the `iot` version does not. If you are new to ReSpeaker Core V2, `lxqt` version is recommended. |
    | `flasher` / `sd` | The `flasher` version is used to flash the onboard eMMC, after flashing you can remove the SD card. The `sd` version will require the SD card to stay inserted all the time. |

    For development, we recommend the `lxqt` + `sd` version. So please download the `respeaker-debian-9-lxqt-sd-[date]-4gb.img.xz` file.


2. Burn the `.img.xz` file directly into SD card with [Etcher](https://etcher.io/) (you don't need to unzip when using Etcher).

    ![](/img/v2-flash-sd.png)

3. After burning the image, insert the SD card into your ReSpeaker Core V2. Power the board using any of the micro USB ports (you may need to read [A.1](#a-the-otg-usb-port) and chose the `OTG` port to get console) and DO NOT remove the SD card after powering on. ReSpeaker Core V2 will boot from the SD card, and you can see USER1 and USER2 LEDs light up. USER1 is typically configured at boot to blink in a heartbeat pattern and USER2 is configured to blink during SD card read/write operation.

If instead you want to flash the image to the onboard eMMC, follow [these instructions](#flash-emmc-onboard-flash-memory).

### Serial Console

Now your ReSpeaker Core V2 can boot, you might want to get access to the Linux system via a console, to setup the WiFi, etc. You have two choices to get the console:

- A. The OTG USB port - This requires a running Linux system on the board
- B. The UART port - This is the hard way to access the console, it can be used for debugging low level issues

#### A. The OTG USB port

1. Find a micro USB cable, and please make sure it's a data cable (not just a power cable), plug the micro USB end to the ReSpeaker's `OTG` micro USB port (There're two micro USB ports on the ReSpeaker board, they are labeled with different silk-screen, one is `PWR_IN` and another is `OTG`, both can be used to power the board, but the `OTG` port has extra functionality). **Please note that only the `OTG` USB port should be connected, leave the `PWR_IN` port empty.**

2. Check at your computer if the serial port has been detected

    - Windows: check the device manager, there should be new serial deviced named `COMx` which `x` is an increasing number. If you use windows XP/7/8, maybe you  need install [windows CDC drivers](https://github.com/respeaker/get_started_with_respeaker/blob/master/files/ReSpeaker_Gadget_CDC_driver.7z).
    - Linux: `ls /dev/ttyACM*`, you should get `/dev/ttyACMx` where `x` will vary depending on which USB port you used
    - Mac: `ls /dev/cu.usb*`, you should get `/dev/cu.usbmodem14xx` where `xx` will vary depending on which USB port you used

3. Use your favorite serial debugging tool to connect the serial port, the serial has: 115200 baud rate, 8Bits, Parity None, Stop Bits 1, Flow Control None. For examples:

    - Windows: use [PUTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), select `Serial` protocol, fill in the correct COM port of ReSpeaker Core V2, 115200 baud, 8Bits, Parity None, Stop Bits 1, Flow Control None
    - Linux: Depend on the USB port you used, it could be `screen /dev/ttyACM0(,1, and so on) 115200` or `screen /dev/ttyUSB0(,1, and so on) 115200`
    - Mac: Depend on the USB port you used, it could be `screen /dev/cu.usbserial1412(,1422, and so on) 115200` or `screen /dev/cu.usbmodem1412(,1422, and so on) 115200`
  
4. The login user name is `respeaker`, and password is `respeaker` too.

<div align="center"><img src="/img/v2_login.png"></div>

#### B. The UART port

In this section we will guide you how to establish a connection from your computer to your ReSpeaker using your USB to TTL adapter which will be connected to the ReSpeaker's Uart port (Uart port located just to the left of the ReSpeaker speaker plug).

1. Connect Uart port and your PC/Mac with an [USB To TTL Adapter](https://www.seeedstudio.com/USB-To-Uart-5V%26amp%3B3V3-p-1832.html). Note that the voltage of RX/TX are 3.3V. If you don't have an USB To TTL Adapter, please see **step 4**.

2. Use the following Serial debugging tools with 115200 baud:
    - Windows: use [PUTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), select `Serial` protocol, fill in the correct COM port of ReSpeaker Core V2, 115200 baud, 8Bits, Parity None, Stop Bits 1, Flow Control None
    - Linux: Depend on your USB To TTL Adapter, it could be `screen /dev/ttyACM0(,1, and so on) 115200` or `screen /dev/ttyUSB0(,1, and so on) 115200`
    - Mac: Depend on your USB To TTL Adapter, it could be `screen /dev/cu.usbserial1412(,1422, and so on) 115200` or `screen /dev/cu.usbmodem1412(,1422, and so on) 115200`

3. The login user name is `respeaker`, and password is `respeaker` too, or login user name `root`,and password `root`.

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
respeaker@v2:~$ sudo nmtui
```
Enter the password of `respeaker` user at the first time. Then you will see an UI like this, select `Activate a connection` and press `Enter`.

![](/img/nmtui1-1.png)

Select your Wi-Fi for ReSpeaker V2, press `Enter` and type your Wi-Fi password and `Enter` again. When you see a `*` mark, it means that your ReSpeaker has successfully connected to your Wi-Fi network. Tap `Esc` twice to leave the network manager configure tool.

![](/img/nmtui1-2.png)

Now find the IP address of your ReSpeaker by using the `ip address` command. In this example, the IP address is `192.168.7.108`
```
respeaker@v2:~$ ip address

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

Please find the common instructions for network configurations of Debian Linux. Those instructions should be applicable for ReSpeaker Core V2.

### SSH & VNC
#### 1. SSH

The SSH server should be running by default.

- Windows: Use [PUTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), select `SSH` protocol, fill in correct IP address and click `open`. Login as `respeaker` user and password is `respeaker` too.


- Linux/Mac:
    ```
    ssh respeaker@192.168.***.***
    // password: respeaker
    ```

#### 2. VNC

The system has VNC server built-in. The VNC server will launch the [`lxqt`](https://lxqt.org/) desktop GUI which is a lightweight Qt desktop environment.

![](/img/vnc-2.png)

The VNC service also starts automatically. Use [VNC Viewer](https://www.realvnc.com/en/connect/download/viewer/) or [VNC Viewer for Google Chrome](https://chrome.google.com/webstore/detail/vnc%C2%AE-viewer-for-google-ch/iabmpiboiopbgfabjmgeedhcmjenhbla?hl=en)] to connect to the desktop of ReSpeaker Core V2.

![](https://user-images.githubusercontent.com/5130185/34665797-93b222d6-f49c-11e7-8112-704f91163038.png)

If you meet `Unencrypted connection`, click `Continue` to go on. The password for VNC is `respeaker`.

*Please note that the VNC connection relies on good quality of the network, please have a mental preparation that you will probably get very low refresh rate of the VNC display.*

### Connect Speaker or Headset

The board uses the built-in codec of the SOC to render playback. Both the JST speaker port and the headset port are driven by their own amplifier, and both amplifiers are connected to the same codec of the SOC. The sound card driver that SEEED implemented drives both the capture device and the playback device. So there's no discrete capture or playback sound card in ALSA device list. They're all named `seeed-8mic-voicecard`.

The simplest way to heard sound from the board is to plugin a headset. If you prefer loud speaker, the board can output up to 8W of drive capability.

### Voice Capture and Playback Testing

#### a) Test via ALSA

As this is a technical documentation of development phase, the index of the sound device may change along versions. So check out the correct device index first with the following commands:

```
respeaker@v2:~$ arecord -l
**** List of CAPTURE Hardware Devices ****
card 0: seeed8micvoicec [seeed-8mic-voicecard], device 0: 100b0000.i2s1-ac108-pcm0 ac108-pcm0-0 []
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 1: bluetoothvoice [bluetooth-voice], device 0: 100e0000.i2s2-bt-sco-pcm bt-sco-pcm-0 []
  Subdevices: 1/1
  Subdevice #0: subdevice #0
respeaker@v2:~$ aplay -l
**** List of PLAYBACK Hardware Devices ****
card 0: seeed8micvoicec [seeed-8mic-voicecard], device 1: 100b0000.i2s1-rk3228-hifi rk3228-hifi-1 []
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 1: bluetoothvoice [bluetooth-voice], device 0: 100e0000.i2s2-bt-sco-pcm bt-sco-pcm-0 []
  Subdevices: 1/1
  Subdevice #0: subdevice #0
```

Find the sound card whose name has `seeed` prefix. For the example above, the capture device is `hw:0,0`, the playback device is `hw:0,1`. Then test recording and playing sound with the following commands:

```
# record & playback 2 channels audio
arecord -Dhw:0,0 -f S16_LE -r 16000 -c 2 hello.wav
aplay -Dhw:0,1 -r 16000 -c 2 hello.wav

# record 8 channels audio
# there are 6 microphones on board, and ac108 compose the 2 remaining channels.
arecord -Dhw:0,0 -f S16_LE -r 16000 -c 8 hello_8ch.wav
```

#### b) Test via PulseAudio

First check if the PulseAudio is running:

```
respeaker@v2:~$ ps aux|grep pulse|grep -v grep
respeak+  1109  0.0  0.7 363272  7932 ?        S<l  01:01   0:00 /usr/bin/pulseaudio --start --log-target=syslog
```

If it's not, please refer to PulseAudio's documentation to enable the auto-spawn of PulseAudio. Then test via:

```
parecord --channels=8 --rate=16000 --format=s16le hello2.wav
paplay hello2.wav
```

Further more, the `default` ALSA device now hooks to PulseAudio, so using the following commands also plays/records sound via PulseAudio:

```
arecord -v -f cd hello3.wav
aplay hello3.wav
```

So far we learned the basic operations of the ReSpeaker Core V2 board, let's move forward, to [build our own AVS(Alexa Voice Service) device](/docs/ReSpeaker_Core_V2/avs_guide.md) using ReSpeaker Core V2.


### Flash eMMC (Onboard Flash Memory)

You can directly flash the system to the onboard eMMC, and then get rid of the SD card.

1. Download our latest system image from <a href="https://bfaceafsieduau-my.sharepoint.com/personal/miaojg22_off365_cn/_layouts/15/guestaccess.aspx?folderid=0bb3c4f3f122d4c2bb0f65eee2b5938f8&authkey=AfLSkcE8QeeUHTQ8GGfrrsU"><img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/onedrive.png?raw=true" height="25"></img></a>.

    The name of the files follows such convention:
    `respeaker-debian-9-[iot|lxqt]-[flasher|sd]-[date]-4gb.img.xz`

    | section      | description                              |
    | ------------ | ---------------------------------------- |
    | `iot` / `lxqt`   | The `lxqt` version comes with a desktop GUI while the `iot` version does not. If you are new to ReSpeaker Core V2, `lxqt` version is recommended. |
    | `flasher` / `sd` | The `flasher` version is used to flash the onboard eMMC, after flashing you can remove the SD card. The `sd` version will require the SD card to stay inserted all the time. |

    So please download the `flasher` version, and we recommend the `lxqt` version for development.

2. Burn the `.img.xz` file directly into SD card with [Etcher](https://etcher.io/) (you don't need to unzip when using Etcher).

3. After burning, insert the SD card into the ReSpeaker Core V2. Power the board using the `PWR_IN` micro USB port and **do not remove the SD card while it's flashing**. The flashing will start automatically if a valid `flasher` system image is detected in the SD card.

4. During the flashing process, you'll see the USER1 and USER2 LEDs **blink alternately**. It will take about 10 minutes to complete. When the LEDs stop blinking (both off or both on), you can power off the board, pull out the SD card and power the board again. If the LEDs light up, it means the image was flashed to the eMMC correctly.
