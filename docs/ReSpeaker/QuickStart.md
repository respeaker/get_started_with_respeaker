# Quick Start

## Setup Wi-Fi

ReSpeaker is set to Repeater Mode as default, and you have to connect it to an existing wireless network before enjoying the speech recognition with the Internet.

When you first power on ReSpeaker, it will create a Wi-Fi network called "ReSpeakerXXXXXX". Here "XXXXXX" is the last 6 of your ReSpeaker MAC address, which is marked on the board. Connect your computer to this network.

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/wifi1.png?raw=true" width="50%" height="50%">
</div>



**If "ReSpeakerXXXXXX" does not appear, but "LinkIt_Smart_7688_XXXXXX" is found. Please click [here](QuickStart.md#update-for-old-version).**

Once you've obtained an IP address, open a web browser, and enter `192.168.100.1` in the address bar. After a few seconds, a web page will appear asking for ssid and password of an existing Wi-Fi network.

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/wifi2.png?raw=true" width="50%" height="50%">
</div>

Select the Wi-Fi you wish to connect to and enter the password. When you press the `OK` button, ReSpeaker will join the specified network.

Now your ReSpeaker is able to visit the Internet.

Also, here are some ways to setup Wi-Fi with command line. [Click here!](https://github.com/respeaker/get_started_with_respeaker/wiki/WiFi)


## Dashboard

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/dashboard.png?raw=true" width="50%" height="50%">
</div>

Visit `192.168.100.1` again, the Dashboard of ReSpeaker will appear. You are able to have a overview of ReSpeaker, set up network, wifi and system on Dashboard.


## System Update

To update the firmware of you ReSpeaker, enter `http://192.168.100.1/home.html` in a web browser and click `System Update`.


<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/home.png?raw=true" width="50%" height="50%">
</div>


Then ReSpeaker will check its version and the following web page will appear when there is an new firmware for ReSpeaker. Click "UPDATE" to continue and click "UPDATE NOW" after finish download. It will cost about 5 minutes for ReSpeaker to install the firmware and reboot, please **don't turn off** ReSpeaker when updating.

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/systemupdate.png?raw=true" width="50%" height="50%">
</div>


### Update for old version

**Note:** If you can not update your ReSpeaker via Web or can not visit `http://192.168.100.1/home.html`, please click [here](https://s3-us-west-2.amazonaws.com/respeaker.io/firmware/ramips-openwrt-latest-LinkIt7688-squashfs-sysupgrade.bin) to download the lastest firmware on your computer, copy it to a SD card and plug the SD card into ReSpeaker.


Connect to the [serial console](QuickStart.md#serial-console) of ReSpeaker, type the following command lines to update the firmware:


```
mount /dev/mmcblk0p1 /mnt
cd /mnt
sysupgrade -n -F ramips-openwrt-latest-LinkIt7688-squashfs-sysupgrade.bin
```

It will cost about 3 minutes for ReSpeaker to install the firmware and reboot, please **don't turn off** ReSpeaker when updating.

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/systemupdate2.png?raw=true" width="50%" height="50%">
</div>


## Mopidy music player

Mopidy is an extensible music server written in Python. `ReSpeaker` runs Mopidy server for playing music from local disk, Spotify, SoundCloud, Google Play Music and more.

After connecting your computer to ReSpeaker's Wi-Fi, enter `http://192.168.100.1/home.html` in a web browser.
Then Mopidy web page will appear.

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/home.png?raw=true" width="50%" height="50%">
</div>

Please click `Music Player` to enter the HTML frontend for the Mopidy music server. Now you are able to play music on ReSpeaker from local disk and radio streams!

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/mopidymusic.png?raw=true" width="50%" height="50%">
</div>

## File manager

File manager is an extension of Mopidy music server. It allows you to browse/search/edit/upload your local file system. Enter `http://192.168.100.1/home.html` in a web browser and click `File Manager` to get started.

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/filemanager.png?raw=true" width="50%" height="50%">
</div>

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/fileupload.png?raw=true" width="50%" height="50%">
</div>

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/editfile.png?raw=true" width="50%" height="50%">
</div>

## Web terminal

Web terminal [Pyxterm](https://github.com/respeaker/pyxterm), a pure python websocket terminal server,  is also an extension of Mopidy music server to get the web terminal. Enter `http://192.168.100.1/home.html` in a web browser and click `Web Terminal` to login in ReSpeaker terminal.
The default username and password are all "root".

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/terminal.png?raw=true" width="50%" height="50%">
</div>



## Serial console

- Baudrate: 57600
- Terminal app - on Windows, [putty](https://github.com/respeaker/get_started_with_respeaker/blob/master/docs/ReSpeaker/SetupPutty.md) is recommended. On Linux/Mac, use `screen /dev/xxx 57600`


## First impression with Voice Interaction - ReSpeaker, play music!

With Bing Speech API, ReSpeaker can turn on and recognize audio coming from the microphone in real-time, or recognize audio from a file.

To use Bing Speech API, first you have to get a key of Microsoft Cognitive Services from [here](https://www.microsoft.com/cognitive-services/en-us/speech-api), and copy it to `BING_KEY = '' `, then save the following code in `playmusic.py` and run it

```
//stop mopidy and alexa to avoid USB device occupation
/etc/init.d/mopidy stop
/etc/init.d/alexa stop
python playmusic.py
```

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/getbingapi.png?raw=true" width="50%" height="50%">
</div>

```python
import logging
import time
import os
from threading import Thread, Event
from respeaker import Microphone
from respeaker.bing_speech_api import BingSpeechAPI

# use madplay to play mp3 file     
os.system('madplay')               

# get a key from https://www.microsoft.com/cognitive-services/en-us/speech-api
BING_KEY = ''      


def task(quit_event):                                                         
    mic = Microphone(quit_event=quit_event)                                   
    bing = BingSpeechAPI(key=BING_KEY)                                        

    while not quit_event.is_set():
        if mic.wakeup('respeaker'):        
            print('Wake up')               
            data = mic.listen()            
            try:                      
                text = bing.recognize(data)
                if text:           
                    print('Recognized %s' % text)
                    if 'play music' in text:
                        print('I will play music!')
                        os.system('madplay Tchaikovsky_Concerto_No.1p.mp3')
            except Exception as e:               
                print(e.message)                 

def main():                                                              
    logging.basicConfig(level=logging.DEBUG)                                                           
    quit_event = Event()        
    thread = Thread(target=task, args=(quit_event,))
    thread.daemon = True
    thread.start()                          
    while True:                             
        try:                                
            time.sleep(1)                           
        except KeyboardInterrupt:                   
            print('Quit')                           
            quit_event.set()
            break 
    time.sleep(1)
    # thread.join()                

if __name__ == '__main__':       
    main()                  
```
After "INFO:mic:Start Detecting" coming out, try to say "ReSpeaker" to wake up the program, and say "play music" to let it play music. Then ReSpeaker will play "Tchaikovsky\_Concerto\_No.1p.mp3" in the current path with **madplay** tool.

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/bingplaymusic.png?raw=true" width="50%" height="50%">
</div>


## Play with AirPlay

- With Airplay, you can stream music to ReSpeaker.


### For iOS

1. Connect to the same Wi-Fi network on your iOS device and ReSpeaker.
2. On your iOS device, swipe up from the bottom of your screen to open Control Center.
3. In Control Center, swipe horizontally to find the Now Playing screen.
4. Select ReSpeaker as the following picture:

	<div class="text-center">
	<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/airplay.png?raw=true" width="50%" height="50%">
	</div>

5. Connect your headphone/speaker to respeaker, then you can enjoy the music now.


### For Android

1. Connect your smart phone to **ReSpeaker's Wi-Fi**.
2. On your smart phone, open an AirPlay client software, such as: *AllConnect*.
3. Select ReSpeaker as the following picture:

	<div class="text-center">
	<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/dlna.png?raw=true" width="50%" height="50%">
	</div>

4. Connect your headphone/speaker to respeaker, then you can enjoy the music now.



## Use SD Card to Extend Storage
More often than not, a limited amount of storage is available on embedded devices(ReSpeaker has only 5M on-board flash storage left for users). More storage for applications and data can expand ReSpeaker's potential, so use SD card to extend storage as an **extroot** is a good choice.

By employing **extroot**, expansion of the storage capacity of your root file system is accomplished by using an added storage device.
During the boot process, external storage space is mounted as the root file system, or in an overlay configuration over the original file system.

1. Make sure your SD card is plugged into ReSpeaker and `/dev/mmcblk0p1` can be detected by `df -h` or `ls /dev`.

	```
	root@ReSpeaker:/# df -h
	Filesystem                Size      Used Available Use% Mounted on
	rootfs                    1.8M    832.0K    960.0K  46% /
	/dev/root                29.0M     29.0M         0 100% /rom
	tmpfs                    61.7M    276.0K     61.5M   0% /tmp
	/dev/mtdblock6            1.8M    832.0K    960.0K  46% /overlay
	overlayfs:/overlay        1.8M    832.0K    960.0K  46% /
	tmpfs                   512.0K         0    512.0K   0% /dev
	/dev/mmcblk0p1            7.4G      2.5M      7.4G   0% /tmp/run/mountd/mmcblk0p1
	```

2. Format your SD card into two partitions, one is FAT32, the other is EXT4. EXT4 file system will be as an extroot while FAT32 will be as a normal storage device, which is able to transfer files between ReSpeaker and your PC.

	```
	umount /dev/mmcblk0p1
	fdisk /dev/mmcblk0
	# ------------------ fdisk ------------------------
	>Command (m for help):o
	>Created a new DOS disklabel
	>Command (m for help):n
	>Partition type
	p   primary (0 primary, 0 extended, 4 free)
	e   extended (container for logical partitions)
	>Select (default p):p
	>Partition number (1-4, default 1):1
	>First sector (2048-15523839, default 2048):
	>Last sector, +sectors or +size{K,M,G,T,P} (2048-15523839, default 15523839): +2G
	>Command (m for help):n
	>Partition type
	p   primary (1 primary, 0 extended, 3 free)
	e   extended (container for logical partitions)
	>Select (default p):p
	>Partition number (1-4, default 2):2
	>First sector (4196352-15523839, default 4196352):
	>Last sector, +sectors or +size{K,M,G,T,P} (4196352-15523839, default 15523839):
	>Command (m for help):w
	>The partition table has been altered.
	>Calling i[  292.010000]  mmcblk0: p1 p2
	>octl() to re-read partition table.
	>Syncing disks.
	# ------------------ end ------------------------

	mkfs.fat /dev/mmcblk0p1
	mkfs.ext4 /dev/mmcblk0p2

	# reload mtk_sd kernel module
	rmmod mtk_sd
	insmod mtk_sd
	```

3. Prepare your external storage root overlay.

	```
	mount /dev/mmcblk0p2 /mnt ; tar -C /overlay -cvf - . | tar -C /mnt -xf - ; umount /mnt
	```

4. Create fstab with the following command. This command will create a fstab template enabling all partitions and setting '/mnt/mmcblk0p2' partition as '/overlay' partition.

	```
	block detect > /etc/config/fstab;
	sed -i s/option$'\t'enabled$'\t'\'0\'/option$'\t'enabled$'\t'\'1\'/ /etc/config/fstab;
	sed -i s#/mnt/mmcblk0p2#/overlay# /etc/config/fstab;
	cat /etc/config/fstab;
   	```

5. Check if it is mountable to overlay.

	```
	root@mylinkit:/# mount /dev/mmcblk0p2 /overlay/
	root@ReSpeaker:/# df -h
	Filesystem                Size      Used Available Use% Mounted on
	rootfs                    1.8M    832.0K    960.0K  46% /
	/dev/root                29.0M     29.0M         0 100% /rom
	tmpfs                    61.7M    276.0K     61.5M   0% /tmp
	/dev/mtdblock6            5.2G     11.8M      4.9G   0% /overlay
	overlayfs:/overlay        1.8M    832.0K    960.0K  46% /
	tmpfs                   512.0K         0    512.0K   0% /dev
	/dev/mmcblk0p2            5.2G     11.8M      4.9G   0% /tmp/run/mountd/mmcblk0p2
	/dev/mmcblk0p1            2.0G      4.0K      2.0G   0% /tmp/run/mountd/mmcblk0p1
	/dev/mmcblk0p2            5.2G     11.8M      4.9G   0% /overlay
	```

6. Reboot ReSpeaker and check again. If SD card is mounted automatically, you are done. More informations about **extroot**, please click [here](https://wiki.openwrt.org/doc/howto/extroot).

## Install software on ReSpeaker

After extending storage with a SD card, there are enough storage to install software on ReSpeaker.

1. Install git

	```
	opkg update
	opkg install git git-http
	```


## ReSpeaker Drive Unit 

To drive the ReSpeaker Drive Unit, the system image has been slightly modified. Please update the ReSpeaker Core with the image in the following OneDrive link.

<a href="https://1drv.ms/f/s!AqG2uRmVUhlShUyg92Q-oNAxNjPR"><img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/onedrive.png?raw=true" height="25"></img></a>

**Some packages have been removed from this image to save space, most notably: the juci frontend, mopidy, pocketsphinx and the respeaker python libraries. These packages will not be present on a Respeaker Core that has been updated with this image.**

Please see the change log file in the OneDrive link. There's also a tarball file named `package.tar.bz2` which contains the removed packages. Packages can be installed manually from this archive using [opkg](https://wiki.openwrt.org/doc/techref/opkg).

## Factory Reset
Open the serial console or a ssh session and run `firstboot`. [More detail](https://github.com/respeaker/get_started_with_respeaker/wiki/factory-reset).


---

Follow us on [Facebook](https://facebook.com/seeedstudiosz) [Youtube](https://www.youtube.com/channel/UC5mX-JaRWXc8cBc1gm5kKhg) [Twitter](https://twitter.com/seeedstudio).
