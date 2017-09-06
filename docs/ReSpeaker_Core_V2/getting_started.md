## Getting Started

This guide will show you how to get started with ReSpeaker Core V2.
<!-- todo: 这里会有一段话介绍getting started这个章节的内容／结构／目的 -->

### Prerequisites

- 首先你得有一块ReSpeaker Core V2
- Wi-Fi网络
- 4GB以上SD card一张、读卡器一个
- PC or Mac
- [USB To Uart Adapter](https://www.seeedstudio.com/USB-To-Uart-5V%26amp%3B3V3-p-1832.html)
- 5V 1A Micro USB 供电

<!-- ### Open Box -->

### Image Installation

和树莓派类似，ReSpeaker Core V2出厂时并没有烧录操作系统，因此用户拿到板子后的第一件事就是要为ReSpeaker Core V2制作一张image SD card, 使得ReSpeaker Core V2能够通过SD卡顺利启动。下面是制作image SD card的步骤：

1. 在[百度云盘](192.168.4.48)或[respeaker.io]()下载最新的系统镜像压缩文件`respeaker-v2-stretch-xxxxxxxx.7z`

2. 下载完成后，解压 `.7z` 文件，以得到烧录至SD卡中的`.img`镜像文件。下面的压缩工具支持解压`.7z`文件：
    - Windows: [7-Zip](http://www.7-zip.org/)
    - Linux: [7za](http://www.thegeekstuff.com/2010/04/7z-7zip-7za-file-compression)
    - Mac: [The
     Unarchiver](https://itunes.apple.com/us/app/the-unarchiver/id425424353?mt=12)

3. 你需要用一个image writing tool将镜像文件烧录至SD card，推荐使用全平台通杀的[Etcher](https://etcher.io/)

4. 待镜像文件烧录完成，即可将SD card插入ReSpeaker Core V2，然后从`PWR_IN` micro usb接口给板子上电，注意**不要热插拔SD card**

5. 上电后，USER1和USER2 LED会亮起，USER1 is typically configured at boot to blink in a heartbeat pattern and USER2 is typically configured at boot to light during SD (microSD) card accesses

![](/img/hardware.jpg)


### Serial Console

ReSpeaker Core V2 启动起来后，可以通过板上串口(Uart)访问系统的控制台，下面是操作步骤:

1. 使用USB To TTL Adapter 连接板上串口和您的电脑，注意TX RX的电压为3.3V

2. 使用串口调试工具打开串口，波特率115200
    - Windows: 使用[PUTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)工具，选择Serial协议，填写正确的COM口，波特率115200，8Bits，Parity None，Stop Bits 1，Flow Control None
    - Linux: Depend on your USB To TTL Adapter, it could be `screen /dev/ttyACM0(,1, and so on) 115200` or `screen /dev/ttyUSB0(,1, and so on) 115200`
    - Mac: Depend on your USB To TTL Adapter, it could be `screen /dev/cu.usbserial1412(,1422, and so on) 115200` or `screen /dev/cu.usbmodem1412(,1422, and so on) 115200`

3. 登陆后默认用户是root，建议使用`su respeaker`命令切换至respeaker用户

### Network Setting Up

#### 1. Wi-Fi Setting Up

配置无线网络使用的是 **Network Manager** 工具，在命令行输入：
```
root@v2:/home/respeaker# nmtui              # 如果是respeaker用户，需要sudo权限
```
即可进入如下配置页面，选择`Activate a connection`，敲回车

![](/img/nmtui1-1.png)

选择一个Wi-Fi，敲回车——输入密码——敲回车，在串口中看到`succeeded`后说明已成功连接Wi-Fi，随后敲两下ESC，离开配置页面

![](/img/nmtui1-2.png)

建议使用`ifconfig`检查网络连接，并获取设备IP地址，用于建立SSH或VNC连接。由下可知本设备Wi-Fi网络的IP地址为: `192.168.7.108`
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

除了UI配置页面，Network Manager也有命令行工具，可用于连接隐藏Wi-Fi，使用方法如下：
```
nmcli c add type wifi con-name mywifi ifname wlan0 ssid your_wifi_ssid
nmcli con modify mywifi wifi-sec.key-mgmt wpa-psk
nmcli con modify mywifi wifi-sec.psk your_wifi_password
nmcli con up mywifi
```

#### 2. Ethernet Setting Up
暂时无法使用有线网络

### SSH & VNC
#### 1. SSH

在ReSpeaker Core V2中，SSH服务是自动开启的. for Windows Users, third-party SSH clients are available. For Linux/Mac Users, SSH is built in.

- Windows: 使用[PUTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)工具，选择SSH协议，填写正确的IP地址，点击`open`即可。SSH的登陆账号为respeaker，密码也是respeaker。


- Linux/Mac:
```
ssh respeaker@192.168.***.***
// password: respeaker
```

*Note that if you find there is not very smooth when using SSH, please switch to a uncrowed LAN network.*

#### 2. VNC
![](/img/vnc-2.png)

在ReSpeaker Core V2中，VNC服务是自动开启的，用户能够通过[VNC Viewer](https://www.realvnc.com/en/connect/download/viewer/)软件或[VNC Viewer for Google Chrome](https://chrome.google.com/webstore/detail/vnc%C2%AE-viewer-for-google-ch/iabmpiboiopbgfabjmgeedhcmjenhbla?hl=en)，远程登陆开发板的桌面。

这需要你的PC/Mac与ReSpeaker Core V2连接在同一个局域网，然后打开VNC Viewer，在上方地址栏输入`192.168.xxx.xxx:5901`，其中`xxx`指的是你的开发板的IP地址，`5901`是开发板VNC服务的默认端口号。If you meet `Unencrypted connection`, click `Continue` to go on. The password is `respeaker`.

![](/img/vnc-1.png)

如果打开后的VNC窗口只有一片灰白，没有显示任何图标，请在灰白处单击鼠标右键，选择开启terminal，然后在terminal中输入`lxpanel`
<!-- If there is nothing showed in the VNC desktop, please restart vncserver service with `systemctl status v2vncserver.service` command and reconnect to it. -->

*Note that if you find there is not very smooth when using VNC, please switch to a uncrowed LAN network.*

### ALSA Setting

使用respeaker用户下载Github仓库:
```
su respeaker && cd     # 如果已经是respeaker用户，跳过这一步
git clone https://github.com/respeaker/respeaker_v2_eval.git       # 如果已经下载过此仓库，跳过这一步
cd ~/respeaker_v2_eval
sudo cp asound.conf /etc/
```

### PulseAudio Setting

#### 方式1
```
su respeaker && cd     # 如果已经是respeaker用户，跳过这一步
git clone https://github.com/respeaker/respeaker_v2_eval.git       # 如果已经下载过此仓库，跳过这一步
cd ~/respeaker_v2_eval
sudo cp pulse/default.pa /etc/pulse/            # 配置pulseaudio
cp pulse/client.conf ~/.config/pulse/
pulseaudio -k && pulseaudio -D                  # 重启pulseaudio，不要使用root运行
```

#### 方式2
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
```

### Virtual Environment

```
pip install virtualenv                                     # 安装virtualenv
python -m virtualenv --system-site-packages ~/env          # 创建python虚拟环境
source ~/env/bin/activate                                  # 激活python虚拟环境
deactivate                                                 # 退出python虚拟环境
```

### Voice Engine Setting

在虚拟环境中安装、配置ReSpeaker音频引擎
```
source ~/env/bin/activate                                  # 激活python虚拟环境
cd ~/respeaker_v2_eval
sudo cp etc/apt/apt.conf.d/02proxy /etc/apt/apt.conf.d/    # 如果不在SEEED局域网，跳过这一步，这是设置apt缓存代理，加速apt下载
sudo apt update
sudo apt install libatlas-base-dev                         # 安装snowboy依赖包atlas
pip install ./webrtc*.whl
pip install ./snowboy*.whl
pip install avs
pip install voice-engine
```

安装完成后, 您可以尝试在ReSpeaker Core V2上搭建[AVS(Amazon Voice Service)](/docs/avs_guide.md).

### Voice Capture and Playback

```
// 两通道录音&播音
arecord -Dhw:0,0 -f S16_LE -r 16000 -c 2 hello.wav
aplay -Dhw:0,2 -r 16000 -c 2 hello.wav
arecord -v -f cd hello.wav
aplay hello.wav
// 八通道录音
// 为什么有八通道？板上有6个麦克风提供了6个通道，然后每个ac108各合成了一个通道
arecord -Dhw:0,0 -f S16_LE -r 16000 -c 8 hello.wav
```


### Flash eMMC

There is another way to boot ReSpeaker Core V2, without SD card. You could  directly flash image files to eMMC with your PC or Mac:

#### For Windows User

1. 从Github下载
```
git clone https://github.com/respeaker/rkbin.git
```

2. 从[百度云盘](192.168.4.48)或[respeaker.io]()下载5个文件：`boot.img` `linaro-rootfs.img`  `u-boot/uboot.img` `AndroidTool_Release_v2.31.zip` `DriverAssitant_v4.4.zip`

3. 解压 `DriverAssitant_v4.4.zip` 并双击安装`DriverInstall`

4. 解压 `AndroidTool_Release_v2.31.zip`，双击打开 `AndroidTool`，按照下图配置好指定的文件路径和烧录地址：
![](/img/emmc-1.png)

5. 点击执行开始烧录，等待10分钟左右，烧录完成，断电重启ReSpeaker Core V2
![](/img/emmc-2.png)


#### For Linux User

1. 从Github下载
```
git clone https://github.com/respeaker/rkbin.git
cd rkbin
```

2. 从[百度云盘](192.168.4.48)或[respeaker.io]()下载3个文件：`boot.img` `linaro-rootfs.img`  `u-boot/uboot.img`

3. 按住ReSpeaker Core V2上的UPDATE按钮，使用Micro USB线连接板子的OTG口和PC/Mac的USB口，直到电源灯亮起，松开UPDATE按钮

4. 执行烧录命令
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
装Windows或Linux虚拟机，然后按照上述步骤执行
