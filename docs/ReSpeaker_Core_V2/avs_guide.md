## AVS(Alexa Voice Service) Guide

This guide will shows you how to build an AVS device based on the ReSpeaker Core V2.

![AVS](https://user-images.githubusercontent.com/5130185/36649780-a0b1acc2-1ada-11e8-8145-9ad0de4e7f7f.png)

### Prerequisites

- Latest system, otherwise you'll be given some errors of missing software packages
- Network has been configured
- Serial console / SSH

### Install AVS library (Python)

```
respeaker@v2:~$ pip install avs
```

This will also install the following executables into `~/.local/bin`: alexa-audio-check, alexa-auth, dueros-auth, alexa-tap and alexa.

Check the audio configuration via:

```
respeaker@v2:~$ ~/.local/bin/alexa-audio-check
```

This script calculates the RMS of the sound recorded by the microphones.

### Get Alexa Authorization

Connect to the board via [VNC](/docs/ReSpeaker_Core_V2/getting_started.md#ssh--vnc). In the VNC desktop, open terminal and execute:

```
respeaker@v2:~$ ~/.local/bin/alexa-auth
```

This script will open the web browser automatically, the web browser will display a login page. Sign in with your Amazon account:

![](/img/aus-1.png)

Succeed:

![](/img/aus-2.png)

### Alexa Tap to Play

```
respeaker@v2:~$ ~/.local/bin/alexa-tap
```
Wait util you see `on_ready` in the log printing. Press `enter` and talk to Alexa.

### Alexa Hands-Free via snowboy

```
sudo apt install libatlas-dev                # required by snowboy
git clone https://github.com/respeaker/respeaker_v2_eval.git
cd respeaker_v2_eval
pip install --no-deps snowboy*.whl           # install pre-build snowboy
pip install webrtc_audio_processing*.whl
pip install voice-engine
python ns_kws_alexa.py
```

Say `snowboy` to trigger the conversation with Alexa.

Demo with light effect:

```
pip install pixel-ring
python ns_kws_alexa_with_light.py
```

### Related Libraries

- [avs](https://github.com/respeaker/avs)
- [pixel-ring](https://github.com/respeaker/pixel_ring)
- [voice-engine](https://github.com/voice-engine/voice-engine)