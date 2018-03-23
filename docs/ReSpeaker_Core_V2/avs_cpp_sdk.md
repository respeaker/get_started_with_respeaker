## AVS C++ SDK

This guide will show you how to run the Amazon official AVS C++ SDK with `respeakerd`.

### 1. Prerequisites

If you already passed the  [OOB demo](/docs/ReSpeaker_Core_V2/oob.md), move forward to next chapter.

If you just received the board and had done nothing on it, please learn the [basic operations](/docs/ReSpeaker_Core_V2/getting_started.md) of this board:
- System image burning - this demo needs the lxqt version system image
- Get serial console via OTG USB port
- Setup Wi-Fi / ethernet
- SSH
- VNC

Then install the basic software packages:
```shell
## install deps
sudo apt update
sudo apt install -y librespeaker git cmake
sudo apt install -y python-mraa python-upm libmraa1 libupm1 mraa-tools
sudo pip install pixel_ring pydbus

cd /home/respeaker
git clone https://github.com/respeaker/respeakerd.git

cd /home/respeaker/respeakerd

sudo cp -f build/respeakerd /usr/local/bin
sudo cp -f scripts/respeakerd_safe /usr/local/bin
sudo chmod a+x /usr/local/bin/respeakerd
sudo chmod a+x /usr/local/bin/respeakerd_safe
sudo mkdir -p /usr/local/etc/respeakerd
sudo cp -Rf build/resources /usr/local/etc/respeakerd/
sudo cp -f scripts/respeakerd.service /etc/systemd/system/


#enable system service
sudo systemctl enable respeakerd
sudo systemctl start respeakerd
```

### 2. Configure respeakerd

#### 2.1 PulseAudio configuratin

```shell
$ sudo vim /etc/pulse/default.pa
```

Add the following line to the end of the file:

```text
load-module module-pipe-source source_name="respeakerd_output" rate=16000 channels=1
set-default-source respeakerd_output
```

Reboot the board.

#### 2.2 Start `respeakerd` in PulseAudio mode

When `respeakerd` works in PulseAudio mode, it outputs the processed audio stream into a named pipe which is created by the module-pipe-source of PulseAudio.

```shell
$ sudo systemctl stop respeakerd
$ sudo vim /usr/local/bin/respeakerd_safe
```

Modify the content of this file to the following:

```text
#!/bin/bash

pulseaudio --check

while [ $? == 1 ]; do
    sleep 1
    pulseaudio --check
done

while [ ! -p /tmp/music.input ]; do
   sleep 1
done

sleep 5

/usr/local/bin/respeakerd --snowboy_res_path="/usr/local/etc/respeakerd/resources/common.res" --snowboy_model_path="/usr/local/etc/respeakerd/resources/snowboy.umdl" --snowboy_sensitivity="0.4" --source="alsa_input.platform-sound_0.seeed-8ch" --mode=pulse
```

Restart the service:

```shell
$ sudo systemctl start respeakerd
```

Or you want to manually start the `respeakerd` for debugging purpose:

```shell
$ sudo systemctl stop respeakerd
$ /usr/local/bin/respeakerd --snowboy_res_path="/usr/local/etc/respeakerd/resources/common.res" --snowboy_model_path="/usr/local/etc/respeakerd/resources/snowboy.umdl" --snowboy_sensitivity="0.4" --source="alsa_input.platform-sound_0.seeed-8ch" --mode=pulse --debug
```

### 3. Compile and Run AVS C++ SDK

Please refer to [Raspberry Pi Quick Start Guide](https://github.com/alexa/avs-device-sdk/wiki/Raspberry-Pi-Quick-Start-Guide) from Amazon for detail.

Here we only list the commands, you will notice that, the differences aginst the official guide are:
- replace home directory with `/home/respeaker`
- skip chapter 1.3 since we don't need a wake word engine, wake word engine is included in respeakerd
- replace `git://github.com/alexa/avs-device-sdk.git` with `git://github.com/respeaker/avs-device-sdk.git`
- new build option `-DRESPEAKERD_KEY_WORD_DETECTOR=ON`
- skip chapter 3.4 since  the default configuration file of ALSA works well for us

Now let's do some copy-paste:

```shell
$ cd /home/respeaker/ && mkdir sdk-folder && cd sdk-folder && mkdir sdk-build sdk-source third-party application-necessities && cd application-necessities && mkdir sound-files
$ sudo apt-get -y install git gcc cmake build-essential libsqlite3-dev libcurl4-openssl-dev libfaad-dev libsoup2.4-dev libgcrypt20-dev libgstreamer-plugins-bad1.0-dev gstreamer1.0-plugins-good libasound2-dev doxygen
$ cd /home/respeaker/sdk-folder/third-party && wget -c http://www.portaudio.com/archives/pa_stable_v190600_20161030.tgz && tar zxf pa_stable_v190600_20161030.tgz && cd portaudio && ./configure --without-jack && make
$ sudo pip install commentjson
$ sudo pip install flask
$ cd /home/respeaker/sdk-folder/sdk-source && git clone git://github.com/respeaker/avs-device-sdk.git
$ cd /home/respeaker/sdk-folder/sdk-build && cmake /home/respeaker/sdk-folder/sdk-source/avs-device-sdk -DCMAKE_BUILD_TYPE=DEBUG -DRESPEAKERD_KEY_WORD_DETECTOR=ON -DGSTREAMER_MEDIA_PLAYER=ON -DPORTAUDIO=ON -DPORTAUDIO_LIB_PATH=/home/respeaker/sdk-folder/third-party/portaudio/lib/.libs/libportaudio.a -DPORTAUDIO_INCLUDE_DIR=/home/respeaker/sdk-folder/third-party/portaudio/include
$ make SampleApp -j2

# obtain credentials of AVS
# https://github.com/alexa/avs-device-sdk/wiki/Raspberry-Pi-Quick-Start-Guide#3-obtain-credentials-and-set-up-your-local-auth-server
# back here when you finish chapter 3

$ /home/respeaker/sdk-folder/sdk-build/SampleApp/src/SampleApp /home/respeaker/sdk-folder/sdk-build/Integration/AlexaClientSDKConfig.json

```

Now you are able to make conversations with Alexa, but all user experiences are done through the command line messages.

### 4. LED Ring Light Effect

```shell
$ sudo cp -f /home/respeaker/respeakerd/scripts/pixel_ring_server /usr/local/bin/
$ sudo chmod a+x /usr/local/bin/pixel_ring_server
$ pixel_ring_server
```

### 5. Startup

```shell
$ sudo cp -f /home/respeaker/respeakerd/scripts/avs_cpp_sdk_safe /usr/local/bin
$ sudo chmod a+x /usr/local/bin/avs_cpp_sdk_safe
$ sudo cp -f /home/respeaker/respeakerd/scripts/pixel_ring_server.service /etc/systemd/system/
$ sudo cp -f /home/respeaker/respeakerd/scripts/avs_cpp_sdk.service /etc/systemd/system/
$ sudo systemctl enable pixel_ring_server
$ sudo systemctl enable avs_cpp_sdk
$ sudo systemctl start pixel_ring_server 
$ sudo systemctl start avs_cpp_sdk
```
