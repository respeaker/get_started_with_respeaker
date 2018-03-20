## AVS(Alexa Voice Service) Guide

This guide will show you how to build an AVS device based on the ReSpeaker Core V2.

![AVS](https://user-images.githubusercontent.com/5130185/36649780-a0b1acc2-1ada-11e8-8145-9ad0de4e7f7f.png)

Now we have two options to build such AVS device:

- With open-sourced solution
- With close-sourced solution

The open-sourced solution utilizes the algorithms from webrtc, speex projects. It aims for the users/hackers who want to go deep into the algorithms, e.g. algorithms research. The close-sourced solution may give less control to users, but it already has powerful algorithms builtin, such as: noise suppression, beamforming, echo cancellation, auto gain control.

### 1. Prerequisites

- Latest system, otherwise you'll be given some errors of missing software packages
- Network has been configured
- Serial console / SSH
- Familiar with VNC operation

### 2. Open-sourced Solution
#### 2.1 Install AVS Library (Python)

```
respeaker@v2:~$ pip install avs
```

This will also install the following executables into `~/.local/bin`: alexa-audio-check, alexa-auth, dueros-auth, alexa-tap and alexa.

Check the audio configuration via:

```
respeaker@v2:~$ ~/.local/bin/alexa-audio-check
```

This script calculates the RMS of the sound recorded by the microphones.

#### 2.2 Authorize Alexa

Connect to the board via [VNC](/docs/ReSpeaker_Core_V2/getting_started.md#ssh--vnc). In the VNC desktop, open terminal and execute:

```
respeaker@v2:~$ ~/.local/bin/alexa-auth
```

This script will open the web browser automatically, the web browser will display a login page. Sign in with your Amazon account:

![](/img/aus-1.png)

Succeed:

![](/img/aus-2.png)

Now you can close the VNC client. The following commands can be executed in the SSH (If you prefer the VNC desktop, the terminal in VNC desktop also works).

#### 2.3 Alexa Tap to Play

```
respeaker@v2:~$ ~/.local/bin/alexa-tap
```

Wait until you see `on_ready` in the log printing. Press `enter` and talk to Alexa.

#### 2.4 Alexa Hands-Free via Snowboy

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

With light effect:

```
pip install pixel-ring
python ns_kws_alexa_with_light.py
```

#### 2.5 Related Libraries

- [avs](https://github.com/respeaker/avs)
- [pixel-ring](https://github.com/respeaker/pixel_ring)
- [voice-engine](https://github.com/voice-engine/voice-engine)

### 3. Close-sourced Solution

The quickest way to experience the close-sourced solution is to run the [out-of-box demo](/docs/ReSpeaker_Core_V2/oob.md).

This solution consists of the following components:
- librespeaker - [documentation](http://respeaker.io/librespeaker_doc), [license](http://respeaker.io/librespeaker_doc/md__home_gitlab-runner_builds_dc7d1974_0_seeedstudio_librespeaker_doc_LICENSE.html), close-source
- respeakerd - [github](https://github.com/respeaker/respeakerd), open-source
- AVS client - open-source
  - [Python version](https://github.com/respeaker/avs)
  - C++ version, [guide](/docs/ReSpeaker_Core_V2/avs_cpp_sdk.md), [github](https://github.com/respeaker/avs-device-sdk)

The [OOB demo](/docs/ReSpeaker_Core_V2/oob.md) combines `librespeaker`, `respeakerd` and the Python version AVS client. 

Another AVS client implementation is the C++ version from the Amazon official, which is more robust and more compatible with the AVS API. We have done some mod to let it work with respeakerd. The main idea is, pass the events through D-Bus, the events include: wakeword detected, all states that a LED ring UI requires. The `wake word detected` event is emitted by the respeakerd, and will be received by AVS C++ SDK and the LED ring server application. The `states` events are emitted by the AVS C++ SDK, and will be listened by the LED ring server application. Here's [the guide](/docs/ReSpeaker_Core_V2/avs_cpp_sdk.md) of how to compile and run the AVS C++ SDK, and how to configure respeakerd to work with each other.



