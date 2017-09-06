## AVS(Amazon Voice Service) Guide

This guide will shows you how to build AVS on ReSpeaker Core V2.
<!-- todo:缺张图片 -->


### Get Alexa Authorization

After[ Voice Engine Setting ](/docs/ReSpeaker_Core_V2/getting_started.md#voice-engine-setting), run authorization command in terminal with `respeaker` user:
```
source ~/env/bin/activate
alexa-auth
```
Then visit `127.0.0.1:3000` at[ VNC ](/docs/ReSpeaker_Core_V2/getting_started.md#ssh--vnc) web browser. Sign in to ReSpeaker AVS with your Amazon account:

![](/img/aus-1.png)

Succeed:

![](/img/aus-2.png)

### Kill pulseaudio
Have to kill pulseaudio to use AVS:
```
echo "autospawn = no" > ~/.config/pulse/client.conf
reboot -f
```

### Alexa Tap to Play
```
source ~/env/bin/activate
alexa-tap
```
Press `enter` and talk to ReSpeaker V2, it will answer you.

### Alexa && snowboy
```
cd ~/respeaker_v2_eval/alexa
pip install numpy
python ns_kws_doa_alexa.py
```
Alexa works with pixel light:
```
pip install spidev
sudo su
cp /home/respeaker/.avs.json /root/.avs.json    # copy respeaker user alexa authorization to root user
source /home/respeaker/env/bin/activate         # activate python venv
python ns_kws_doa_alexa_with_light.py
```
