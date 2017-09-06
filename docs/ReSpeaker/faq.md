# FAQ

This is the FAQ of ReSpeaker and Mic Array. If you have any other questions, feel free to post it here :) We are willing to help you!

### Factory reset

  Open the serial console or a ssh session and run `firstboot`. [More detail](https://github.com/respeaker/get_started_with_respeaker/wiki/factory-reset)

### Wi-Fi config

We advise you to configure Wi-Fi via [WEB-UI](https://github.com/respeaker/get_started_with_respeaker/blob/master/QuickStart.md#setup-wi-fi) and if it can't be used, try command line tool [wictl](https://github.com/respeaker/get_started_with_respeaker/wiki/WiFi) at the serial console.

	
### How to change BING speech api recognize language

If you don't need to change the wake up words, just change `text = bing.recognize(data)` into `text = bing.recognize(data,language="zh-CN")` is fine. [More details](https://github.com/respeaker/respeaker_python_library/blob/master/respeaker/bing_speech_api.py).


### Got SD card warning message "Volume was not properly unmounted. Some data may be corrupt. Please run fsck"

If the files on the SD card are fine, ignore it. Otherwise, try to format it with [sd card formatter](https://www.sdcard.org/downloads/formatter_4/).

### ReSpeaker fail to find my Wi-Fi

Try [factory reset](https://github.com/respeaker/get_started_with_respeaker/blob/master/faq.md#factory-reset) first.

  And the Wi-Fi Channel 12 is not supported by ReSpeaker. Make sure your router is not using that channel.


### Bad flash from Arduino

Re-flash the bootloader on openwrt.

	/etc/init.d/mopidy stop  # stop mopidy if it's running, mopidy-hallo plugin will use SPI
	/etc/init.d/alexa stop      # stop alexa if it's running
    mt7688_pinmux set ephy gpio
    cd /etc/arduino
    avrdude -c linuxgpio -p m32u4 -e -U lfuse:w:0xFF:m -U hfuse:w:0xD8:m -U efuse:w:0xCB:m  -U flash:w:Caterina-ReSpeaker.hex -u -U lock:w:0xEF:m

### Forgot the password of WebUI

Reset the juci password

	orangectl passwd root 12345678  //replace 12345678 with the password you want to set


### How to get audio source direction from respeaker microphone array?

ReSpeaker microphone array uses USB HID protocol.

When using Windows, follow the [guide](https://github.com/respeaker/get_started_with_respeaker/wiki/Mic-Array) or use our [HID tool](https://github.com/Fuhua-Chen/ReSpeaker-Microphone-Array-HID-tool).

### How to support google speech or other Speach TO Text(STT) Engine?

Install speech\_recognition library following the [guide](https://github.com/respeaker/get_started_with_respeaker/wiki/Use-speech_recognition-python-library)


### How to change wake up word?

- `keywords.txt` contains keywords and their threshold. For example, `keywords.txt` from [here](https://github.com/respeaker/respeaker_python_library/blob/master/respeaker/pocketsphinx-data/keywords.txt) is

	```
	respeaker /1e-30/
	alexa /1e-30/
	play music /1e-40/
	```

	`respeaker` is a keyword, `1e-30` is its threshold. To improve sensitive, we can decrease the threshold, for example, `1e-50`. We should know  decreasing the threshold will increase False Acceptance Rate.

	If you want to add new keyword, you should firstly add the keyword to  [`dictionary.txt`](https://github.com/respeaker/respeaker_python_library/blob/master/respeaker/pocketsphinx-data/dictionary.txt).  The `dictionary.txt` is like:
	
	```
	respeaker	R IY S P IY K ER
	alexa	AH L EH K S AH
	play	P L EY
	music	M Y UW Z IH K
	```

	The first part is a name (respeaker, alexa or music), the second part is its phonemes. You can find words in a large dictionary at [here](https://github.com/respeaker/pocketsphinx-data/blob/master/dictionary.txt).

- then change the code:
	
	```
	if mic.wakeup('respeaker'):
	```
		
- more details [issues#50](https://github.com/respeaker/get_started_with_respeaker/issues/50), [issues#68](https://github.com/respeaker/get_started_with_respeaker/issues/68)


### Failed to run Alexa with error "IOError: [Errno -9998] Invalid number of channels"

There is another application or alexa instance using the audio input device. Run `/etc/init.d/alexa stop` and `/etc/init.d/mopidy stop` to stop them. To disable mopidy to startup, run `/etc/init.d/mopidy disable`.


### Failed to run python playmusic.py

It should be that mopidy is running in background and is using the USB device. try to run ```/etc/init.d/mopidy stop``` to stop mopidy and run your command again.


### Python Library update
    git clone https://github.com/respeaker/respeaker_python_library.git

    python setup.py install


### JS or Node.js API

We can use the javascript programming of LinkIt 7688, such as

    LinkIt Smart 7688 Developer’s Guide - https://labs.mediatek.com/en/download/ih80Qtjo
    https://iamblue.gitbooks.io/linkit-smart-nodejs
For the limited on-board storage, we don't install node and npm by default. You install them using opkg.

### Don't have a RPC connection

You need to reflash the firmware, following the [guide](https://github.com/respeaker/get_started_with_respeaker/blob/master/QuickStart.md#update-for-old-version)
	
### SFTP & FTP
We don't have a FTP on respeaker, just SFTP.
	

### Serial Console locked up

Try to update the [arduino code](https://github.com/respeaker/respeaker_arduino_library/blob/master/examples/pixels_pattern/pixels_pattern.ino).

### How to disable 'ap' mode
You could set the 'ssid' flag of the 'ap' interface to '' at `vi /etc/config/wireless`

Then the ap will be hidden.

### I2C Sound card issue
You need check your codec driver compatible name and codec i2c address. Then rebuild the image firmware.


### Arduino32u4 driver
http://www.driverscape.com/download/arduino-leonardo

### Updated mic array firmware on Mac
[Click here](https://github.com/respeaker/get_started_with_respeaker/issues/97)