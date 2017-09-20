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


1. Download our latest image zip file `respeaker-v2-stretch-xxxxxxxx.7z` at [百度云盘](https://pan.baidu.com/s/1c2piKW4) or [Google Drive](https://drive.google.com/open?id=0B7R2TH-ioqAKQjBfZGp0M3VaVjQ)

2. Unzip the `.7z` file to get a `.img` file. You can unzip `.7z` file with the following tools for different platforms:
    - Windows: [7-Zip](http://www.7-zip.org/)
    - Linux: [7za](http://www.thegeekstuff.com/2010/04/7z-7zip-7za-file-compression)
    - Mac: [The
     Unarchiver](https://theunarchiver.com/)

3. Burn the `.img` file to SD card with [Etcher](https://etcher.io/) or other image writing tools.

4. After burning SD card, put the SD card in ReSpeaker Core V2. Power the board at `PWR_IN` micro usb port，note that **Try not to hot swap SD card**.

5. When powering, USER1 and USER2 LEDs light up. USER1 is typically configured at boot to blink in a heartbeat pattern and USER2 is typically configured at boot to light during SD (microSD) card accesses

![](/img/hardware.jpg)


### Serial Console

You can connect to the Serial Console with Uart port on board:

1. Connect Uart port and your PC/Max with an USB To TTL Adapter. Note that the voltage of RX/TX are 3.3V.

2. Use the following Serial debugging tools with 115200 baud:
    - Windows: usb [PUTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), select `Serial` protocol, fill in the correct COM port of ReSpeaker Core V2, 115200 baud, 8Bits, Parity None, Stop Bits 1, Flow Control None
    - Linux: Depend on your USB To TTL Adapter, it could be `screen /dev/ttyACM0(,1, and so on) 115200` or `screen /dev/ttyUSB0(,1, and so on) 115200`
    - Mac: Depend on your USB To TTL Adapter, it could be `screen /dev/cu.usbserial1412(,1422, and so on) 115200` or `screen /dev/cu.usbmodem1412(,1422, and so on) 115200`
    
If you do not have a USB to TTL Adapter, you may also use an Arduino. If using an Arduino, connect one end of a jumper wire to the 'RESET' pin on the Arduino and the other end to the 'GND' pin on the Arduino. This will bypass your Arduino's ATMEGA MCU and turn your Arduino into a USB to TTL adapter, see video tutorial here https://www.youtube.com/watch?v=qqSLwK1DP8Q. Now connect the GND pin on the Arduino to the GND pin on the Uart port of the Respeaker. Connect the Rx pin on the Arduino to the Rx pin on the Uart port of the Respeaker. Connect the Tx pin on the Arduino to the Tx pin on the Uart port of the Respeaker. And lastly, connect the Arduino to your PC/Mac via the Arduino's USB cable. Now check that your Mac or Linux PC finds your Arduino by typing this command:

ls /dev/cu.usb* (Mac)
ls /dev/ttyACM* (Linux)

You should get back something like:

/dev/cu.usbmodem14XX where XX will vary depending on which USB port you used (on Mac)
/dev/ttyACMX where X will vary depending on which USB port you used  (on Linux)

Now use this port with the commands above to connect to your Respeaker over this serial connection. And note this is a one time procedure as you'll next setup your Respeaker for Wi-Fi connectivity and then connect via ssh or VNC going forward.

3. The default user is `root`, you can switch to `respeaker` user with `su respeaker` command.

### Network Setting Up

#### 1. Wi-Fi Setting Up

Configure network with **Network Manager** tool:
```
root@v2:/home/respeaker# nmtui              # respeaker user needs sudo
```
Then you could see a config page like this, select `Activate a connection` and press `Enter`.

![](/img/nmtui1-1.png)

Select your Wi-Fi for ReSpeaker V2, press `Enter` and type your Wi-Fi password and `Enter` again. When you see `succeeded`, it means that the board has connected to Wi-Fi already. Tap `Esc` twice to leave the config page.

![](/img/nmtui1-2.png)

Check network connection with `ifconfig` command, you can get IP address of your device, to establish SSH or VNC. From the following log, I can know that my ReSpeaker V2 IP address is `192.168.7.108`.
```
root@v2:/home/respeaker# ifconfig
lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1  (Local Loopback)
        RX packets 86  bytes 6428 (6.2 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 86  bytes 6428 (6.2 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.7.108  netmask 255.255.254.0  broadcast 192.168.7.255
        inet6 fe80::1918:34ca:77fa:3909  prefixlen 64  scopeid 0x20<link>
        ether e0:76:d0:37:5b:cd  txqueuelen 1000  (Ethernet)
        RX packets 4091  bytes 640163 (625.1 KiB)
        RX errors 0  dropped 6  overruns 0  frame 0
        TX packets 243  bytes 22926 (22.3 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
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

<!-- #### 方式2
修复一个PulseAudio配置问题，修改`/etc/pulse/default.pa`，禁用`module-udev-detect`和`module-detect`，静态加载`alsa sink`和`source`，修改之后用respeaker用户重启pulseaudio，运行`pulseaudio -k || pulseaudio -D`
```
### Load audio drivers statically                                                                                                       
### (it's probably better to not load these drivers manually, but instead
### use module-udev-detect -- see below -- for doing this automatically)
load-module module-alsa-sink device=hw:0,2   
load-module module-alsa-source device=hw:0,0            
#load-module module-oss device="/dev/dsp" sink_name=output source_name=input  
#load-module module-oss-mmap device="/dev/dsp" sink_name=output source_name=input
#load-module module-null-sink           
#load-module module-pipe-sink           

### Automatically load driver modules depending on the hardware available      
#.ifexists module-udev-detect.so        
#load-module module-udev-detect   
#.else                  
### Use the static hardware detection module (for systems that lack udev support)    
#load-module module-detect    
#.endif
``` -->

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

There is another way to boot ReSpeaker Core V2, without SD card. You could  directly flash image files to eMMC with your PC or Mac:

#### For Windows Users

1. Download from Github:
```
git clone https://github.com/respeaker/rkbin.git
```

2. Download from [百度云盘](https://pan.baidu.com/s/1c2piKW4) or [Google Drive](https://drive.google.com/open?id=0B7R2TH-ioqAKQjBfZGp0M3VaVjQ): `boot.img` `linaro-rootfs.img`  `u-boot/uboot.img` `AndroidTool_Release_v2.31.zip` `DriverAssitant_v4.4.zip`

3. Unzip `DriverAssitant_v4.4.zip` and install `DriverInstall`

4. Unzip `AndroidTool_Release_v2.31.zip` and run `AndroidTool`. Configure address, name and file path as the following picture:
![](/img/emmc-1.png)

5. Press the `UPDATE` button on ReSpeaker Core V2 and connect its `OTG` port to your PC.

6. Click `执行` to flash emmc, it takes about 10min. When it finish, reboot ReSpeaker Core V2 and that is ok.
![](/img/emmc-2.png)


#### For Linux User

1. Download from Github:
```
git clone https://github.com/respeaker/rkbin.git
cd rkbin
```

2. Download from [百度云盘](https://pan.baidu.com/s/1c2piKW4) or [Google Drive](https://drive.google.com/open?id=0B7R2TH-ioqAKQjBfZGp0M3VaVjQ): `boot.img` `linaro-rootfs.img`  `u-boot/uboot.img`

3. Press the `UPDATE` button on ReSpeaker Core V2 and connect its `OTG` port to your PC.

4. run flash-emmc commands:
```
sudo ./tools/upgrade_tool ul rk32/rk322x_loader_v1.04.232.bin
sudo ./tools/upgrade_tool di -p rk32/rk3229_parameter
sudo ./tools/upgrade_tool wl 0x2000 uboot.img
sudo ./tools/upgrade_tool wl 0x4000 rk32/trust.img
sudo ./tools/upgrade_tool wl 0x6000 boot.img
sudo ./tools/upgrade_tool wl 0x3e000 linaro-rootfs.img
sudo ./tools/upgrade_tool rd
```


#### For Mac User
You need to use a Windows or Linux virtual machine to achieve this.
