# FAQ

+ Factory reset

  Open the serial console or a ssh session and run `firstboot`. [More detail](https://github.com/respeaker/get_started_with_respeaker/wiki/factory-reset)

无法factory reset？ 拔出sd卡？


+ Failed to run Alexa with error `IOError: [Errno -9998] Invalid number of channels`

  There is another application or alexa instance using the audio input device. Run `/etc/init.d/alexa stop` and `/etc/init.d/mopidy stop` to stop them. To disable mopidy to startup, run `/etc/init.d/mopidy disable`.

+ Got SD card warning message `Volume was not properly unmounted. Some data may be corrupt. Please run fsck`

  If the files on the SD card are fine, ignore it. Otherwise, try to format it with [sd card formatter](https://www.sdcard.org/downloads/formatter_4/).

+ Failed to find Wi-Fi of ReSpeaker

    Can you find out which Wi-Fi channel do the respeaker or the router use? You can use android apps to find these channels, such as https://play.google.com/store/apps/details?id=com.vrem.wifianalyzer&rdid=com.vrem.wifianalyzer

    Channel 12 is not available at some countries and may not be supported by some devices.
    It seems the ReSpeaker only support one channel. The channel of ReSpeaker LAN will stay the same with the channel of its WAN.
    But I have no idea why the ReSpeaker changes the visible of the netgear router. It's weird!

+ Bad flash from Arduino

    Re-flash the bootloader on openwrt.
    /etc/init.d/mopidy stop  # stop mopidy if it's running, mopidy-hallo plugin will use SPI
    /etc/init.d/alexa stop      # stop alexa if it's running
    mt7688_pinmux set ephy gpio
    cd /etc/arduino
    avrdude -c linuxgpio -p m32u4 -e -U lfuse:w:0xFF:m -U hfuse:w:0xD8:m -U efuse:w:0xCB:m  -U flash:w:Caterina-ReSpeaker.hex -u -U lock:w:0xEF:m

+ Forgot the password of WebUI

    You can set juci password,"orangectl passwd root \*\*\*\*\*\*\*\*”



+ How to get audio source direction from respeaker microphone array?

    When using Windows, follow the guide - https://github.com/respeaker/get_started_with_respeaker/wiki/Mic-Array



+ How to support google speech or other Speach TO Text(STT) Engine?

    Install speech_recognition library following the guide - https://github.com/respeaker/get_started_with_respeaker/wiki/Use-speech_recognition-python-library



+ How to change wake up word?

+ Failed to run python playmusic.py

    It should be that mopidy is running in background and is using the USB device. try to run /etc/init.d/mopidy stop to stop mopidy and run your command again.

    **Status code 401 means unauthorized, check your api key**

    **That's what I figured and I've used both and regenerated both keys multiple times. Could there be any reason the keys wouldn't work?
Try this curl, if it works the problem is somewhere in the respeaker lib.
curl -v -X POST "https://api.cognitive.microsoft.com/sts/v1.0/issueToken" -H "Content-type: application/x-www-form-urlencoded" -H "Content-Length: 0" -H "Ocp-Apim-Subscription-Key: your_subscription_key"
(found here)
https://www.microsoft.com/cognitive-services/en-us/Speech-api/documentation/GetStarted/GetStarted-cURL#Step1**

+ Python Library update
    git clone https://github.com/respeaker/respeaker_python_library.git

    python setup.py install



+ JS or Node.js API

    We can use the javascript programming of LinkIt 7688, such as

    LinkIt Smart 7688 Developer’s Guide - https://labs.mediatek.com/en/download/ih80Qtjo
    https://iamblue.gitbooks.io/linkit-smart-nodejs
    For the limited on-board storage, we don't install node and npm by default. You install them using opkg.


+ dont have a RPC connection

	重装

+ keywords

	https://github.com/respeaker/get_started_with_respeaker/issues/68#issuecomment-285596068

+ 串口问题

	https://github.com/respeaker/get_started_with_respeaker/issues/81
	
+ spi速度

	respeaker的spi是用io口模拟的，您可以自己加delay来把速度降慢，但不能速度无法调快。
	
+ sftp & ftp

	没有ftp
	
+ ERRR!!! MlmeEnqueueForRecv(): un-recongnized mgmt->subtype=4, TA-1c:b9:c4:07:49:38

	上级路由网络不好
	
+ 麦克风矩阵采样率

	出厂固件16khz
	新8麦固件是8 11025 12 16 32khz 
	