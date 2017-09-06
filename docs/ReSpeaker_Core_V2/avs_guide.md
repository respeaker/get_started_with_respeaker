## AVS(Amazon Voice Service) Guide

This guide will shows you how to build AVS on ReSpeaker Core V2.
<!-- todo:缺张图片 -->


### 获取Alexa授权

完成[ Voice Engine Setting ](/docs/getting_started.md#voice-engine-setting)后，在terminal或串口中运行:
```
source ~/env/bin/activate
alexa-auth
```
然后在[ VNC ](/docs/getting_started.md#ssh--vnc)中用浏览器访问`127.0.0.1:3000`，在网页中登陆您的Amazon账号

![](/img/aus-1.png)

授权成功

![](/img/aus-2.png)

### Kill pulseaudio
```
echo "autospawn = no" > ~/.config/pulse/client.conf
reboot -f
```

### Alexa Tap to Play
```
source ~/env/bin/activate
alexa-tap
```

### Alexa && snowboy
```
cd ~/respeaker_v2_eval/alexa
pip install numpy
python ns_kws_doa_alexa.py
```
加灯效
```
pip install spidev
sudo su
cp /home/respeaker/.avs.json /root/.avs.json    # 拷贝respeaker用户的alexa配置文件给root用户
source /home/respeaker/env/bin/activate         # 激活之前配置好的python虚拟环境
python ns_kws_doa_alexa_with_light.py
```
