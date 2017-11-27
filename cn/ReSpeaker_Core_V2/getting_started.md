## 起手式

[English Version](/docs/ReSpeaker_Core_V2/getting_started.md)

这里介绍如何使用ReSpeaker Core V2.
<!-- todo: 这里会有一段话介绍getting started这个章节的内容／结构／目的 -->

### Prerequisites

- ReSpeaker Core V2
- Wi-Fi
- 大于等于4GBSD卡和读卡器
- PC or Mac
- Micro USB线

<!-- ### Open Box -->

### 制作系统SD卡

跟树莓派类似，ReSpeaker Core V2从SD卡启动系统，我们要先将系统镜像烧写SD卡：

1. 从以下OneDrive链接下载: `respeaker-debian-9-lxqt-sd-********-4gb.img.xz` 或 `respeaker-debian-9-iot-sd-********-4gb.img.xz` : 

    <a href="https://bfaceafsieduau-my.sharepoint.com/personal/miaojg22_off365_cn/_layouts/15/guestaccess.aspx?folderid=0bb3c4f3f122d4c2bb0f65eee2b5938f8&authkey=AfLSkcE8QeeUHTQ8GGfrrsU"><img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/onedrive.png?raw=true" height="25"></img></a>

    其中，文件名中含`lxqt`的镜像是带有LXQT图形化桌面的Debian系统，含`iot`是只有命令行的轻量版. 建议使用带有`lxqt`的.

2. 可以用 [Etcher](https://etcher.io/) 直接把 `*.img.xz` 文件写SD卡。当然也可以用其它工具，比如Win32ImageWriter，这时需要先从 `*.img.xz` 文件中解压出 `*.img` 文件。
![](/img/v2-flash-sd.png)

3. 把写好的SD卡插入ReSpeaker Core V2的SD卡座，用Micro USB线连接板上带 `OTG` 标签的Micro USB口（另一个带 `PWR_IN` 标签。ReSpeaker Core V2 会从 SD 卡启动，启动过程中，`USER1`和`USER2`LED灯会被点亮。`USER1`LED是一闪一闪的心跳灯。`USER2`LED是SD读写指示灯。然后就可以通过串口终端访问系统了。

### 串口终端

ReSpeaker Core V2运行的是Debian Linux，我们可以串口访问系统，以设置SSH服务、WiFi等。我们可以通过USB OTG口虚拟的串口，或者标有RX、TX和GND的UART口（使用UART口，需要一个额外的USB转UART硬件）。

1. 查看系统中ReSpeaker Core V2对应的串口

    - Windows: 打开设备管理器查看
    - Linux: 用命令`ls /dev/ttyACM*`或`dmesg | tail`查看
    - Mac: 用命令`ls /dev/cu.usb*`查看
   
2. 选一个自己喜欢的串口终端工具打开串口。如果使用UART口，波特率为115200, 8比特位宽, 无奇偶校验，1位停止位，无流控制；如果是USB虚拟串口，任何设置都可以，推荐的串口终端工具有:

    - Windows: 可以用 [PUTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), 选 `Serial` 模式，配置好相应的设置。
    - Linux: 可以用 `screen /dev/ttyACM0 115200` 或 `minicom -D /dev/ttyACM0 -b 115200`（根据具体情况修改命令中串口名，minicom在串口断开重连时自动重连）
    - Mac: 可以用 `screen /dev/cu.usbserial1412(,1422, ...) 115200` or `screen /dev/cu.usbmodem1412(,1422, ...) 115200`
  
4. 打开串口后，可以看到串口输出提示登录的信息（如果没有看到，敲回车试试）。登录名可以是`respeaker`，其密码为`respeaker`。也可以用`root`用户登录，密码为`root`。可以修改默认的密码，这样安全一点。以下内容以`respeaker`用户登录为例。

<div align="center"><img src="/img/v2_login.png"></div>

### 网络设置

#### 1. 有线以太网设置
连接上网线即可，可以用`ip addr`查看IP地址。注：网卡的灯可能不亮，这是正常的，可根据是否获得到了IP地址，判断网卡是否连接正常。

#### 2. Wi-Fi设置

系统用 Network Manager 管理网络，在命令行中，可以用`nmtui`命令设置WiFi
```
respeaker@v2:~$ sudo nmtui              # respeaker user needs sudo
```
然后你可以看一个终端图形界面，选择 `Activate a connection`，然后按回车。

![](/img/nmtui1-1.png)

选择要连接WiFi热点，按回车键，然后输入WiFi密码，再按回车键确认。在连接WiFi成功后，WiFi名旁边可以看到一个 `*`。按 `Esc` 可以退出`nmtui`。

![](/img/nmtui1-2.png)

连接WiFi成功后，可以通过`ip addr`命令查看IP。下图中，ReSpeaker的IP是`192.168.7.108`
```
root@v2:/home/respeaker# ip addr

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
另外，Network Manager还有一个纯命令行工具`nmcli`，如果想连一个隐藏的WiFi，可以用`nmcli`：
```
nmcli c add type wifi con-name mywifi ifname wlan0 ssid your_wifi_ssid
nmcli con modify mywifi wifi-sec.key-mgmt wpa-psk
nmcli con modify mywifi wifi-sec.psk your_wifi_password
nmcli con up mywifi
```

### SSH & VNC
#### 1. SSH

SSH 服务在ReSpeaker V2上已经默认启动了。在windows上，需要第三方的SSH客户端，而Linux、Mac系统，已经内置了SSH客户端。

- Windows: 可以之前推荐的 [PUTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)，选择 `SSH` 协议，填好IP地址，然后打开。用户名和密码跟前面串口登录一样，用户`respeaker`的密码也为`respeaker`。


- Linux/Mac:
```
ssh respeaker@192.168.***.***
// password: respeaker
```

*注：如果SSH登录很卡，建议切换网络*

#### 2. VNC远程桌面
![](/img/vnc-2.png)

VNC服务也是默认自启动的，推荐可以用 [VNC Viewer](https://www.realvnc.com/en/connect/download/viewer/) 或者 [VNC Viewer for Google Chrome ](https://chrome.google.com/webstore/detail/vnc%C2%AE-viewer-for-google-ch/iabmpiboiopbgfabjmgeedhcmjenhbla?hl=en)]。

跟SSH一样，要先获取ReSpeaker的IP，比如为`192.168.7.108`。VNC服务的端口号为`5901`，在 VNC Viewer 地址栏中，输入 `192.168.7.108:5901`。如果看到一个 `Unencrypted connection` 提示，点击 `Continue`，登录密码为 `respeaker`。

![](/img/vnc-1.png)

连上VNC之后，如果看到一个空白的桌面，可以单击右键，选择`terminal`，然后运行`lxpanel`，桌面控件会显示出来。

### ALSA设置

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
