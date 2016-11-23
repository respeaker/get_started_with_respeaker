#Quick Start

##Setup Wi-Fi

ReSpeaker is set to Repeater Mode as default, and you have to connect it to an existing wireless network before enjoying the speech recognition with the Internet.

When you first power on ReSpeaker, it will create a Wi-Fi network called "ReSpeakerXXXXXX". Here "XXXXXX" is the last 6 of your ReSpeaker MAC address, which is marked on the board. Connect your computer to this network. 

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/wifi1.png?raw=true">
</div>

Once you've obtained an IP address, open a web browser, and enter `192.168.100.1` in the address bar. After a few seconds, a web page will appear asking for ssid and password of an existing Wi-Fi network.

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/wifi2.png?raw=true" width="50%" height="50%">
</div>

Select the Wi-Fi you wish to connect to and enter the password. When you press the `OK` button, ReSpeaker will join the specified network.

Now your ReSpeaker is able to visit the Internet.

Also, here are some ways to setup Wi-Fi with command line. [Click here!](https://github.com/respeaker/get_started_with_respeaker/wiki/WiFi)


##Dashboard

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/dashboard.png?raw=true" width="50%" height="50%">
</div>

Visit `192.168.100.1` again, the Dashboard of ReSpeaker will appear. You are able to have a overview of ReSpeaker, set up network, wifi and system on Dashboard.


##System Update

To update the firmware of you ReSpeaker, enter `http://192.168.100.1/home.html` in a web browser and click `System Update`. 


<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/home.png?raw=true" width="50%" height="50%">
</div>


Then ReSpeaker will check its version and the following web page will appear when there is an new firmware for ReSpeaker. Click "UPDATE" to continue and click "UPDATE NOW" after finish download. It will cost about 5 minutes for ReSpeaker to install the firmware and reboot, please **don't turn off** ReSpeaker when updating.

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/systemupdate.png?raw=true" width="50%" height="50%">
</div>


Note: If you can not update your ReSpeaker via Web or can not visit `http://192.168.100.1/home.html`, please click [here](https://s3-us-west-2.amazonaws.com/respeaker.io/firmware/ramips-openwrt-latest-LinkIt7688-squashfs-sysupgrade.bin) to download the lastest firmware on your computer, copy it to a SD card and plug the SD card into ReSpeaker. 


Connect to the [serial console](QuickStart.md#serial-console) of ReSpeaker, type the following command lines to update the firmware:


```
mount /dev/mmcblk0p1 /mnt
cd /mnt
sysupgrade -n ramips-openwrt-latest-LinkIt7688-squashfs-sysupgrade.bin
```

It will cost about 3 minutes for ReSpeaker to install the firmware and reboot, please **don't turn off** ReSpeaker when updating.

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/systemupdate2.png?raw=true" width="50%" height="50%">
</div>


##Mopidy music player

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

##File manager

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

##Web terminal

Web terminal [Pyxterm](https://github.com/respeaker/pyxterm), a pure python websocket terminal server,  is also an extension of Mopidy music server to get the web terminal. Enter `http://192.168.100.1/home.html` in a web browser and click `Web Terminal` to login in ReSpeaker terminal. 
The default username and password are all "root".

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/terminal.png?raw=true" width="50%" height="50%">
</div>



##Serial console

- Baudrate: 57600
- [Driver for windows xp/7/8](https://github.com/respeaker/get_started_with_respeaker/raw/master/serial.inf)
- Terminal app - on windows, [putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) is recommended. On linux/mac, use `screen /dev/xxx 57600`


##First impression with Voice Interaction - ReSpeaker, play music!

With Bing Speech API, ReSpeaker can turn on and recognize audio coming from the microphone in real-time, or recognize audio from a file. 

To use Bing Speech API, first you have to get a key of Microsoft Cognitive Services from [here](https://www.microsoft.com/cognitive-services/en-us/speech-api), and copy it to `BING_KEY = '' `, then save the following code in `playmusic.py` and run it `python playmusic.py `

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/getbingapi.png?raw=true" width="50%" height="50%">
</div>

```
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
                        os.system('madplay Beethoven_Symphony_No.5p.mp3')
            except Exception as e:               
                print(e.message)                 
                                                                         
def main():                                                              
    logging.basicConfig(level=logging.DEBUG)                                                           
    quit_event = Event()        
    thread = Thread(target=task, args=(quit_event,))
    thread.start()                          
    while True:                             
        try:                                
            time.sleep(1)                           
        except KeyboardInterrupt:                   
            print('Quit')                           
            quit_event.set()
            break        
    thread.join()                
                                 
if __name__ == '__main__':       
    main()                  
```
Try to say "ReSpeaker, play music!". Then ReSpeaker will play "Beethoven\_Symphony\_No.5p.mp3" in the current path with **madplay**.

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/bingplaymusic.png?raw=true" width="50%" height="50%">
</div>


##Play with DLNA



##Use SD Card to Extend Storage
More often than not, a limited amount of storage is available on embedded devices(ReSpeaker has only 5M on-board flash storage left for users). More storage for applications and data can expand ReSpeaker's potential, so use SD card to extend storage as an **extroot** is a good choice.

By employing **extroot**, expansion of the storage capacity of your root file system is accomplished by using an added storage device. 
During the boot process, external storage space is mounted as the root file system, or in an overlay configuration over the original file system. 

1. Make sure your SD card is plugged into ReSpeaker and `/dev/mmcblk0p1` can be detected.

	```
	root@mylinkit:/# df -h
	Filesystem                Size      Used Available Use% Mounted on
	rootfs                    4.1M    432.0K      3.6M  10% /
	/dev/root                26.8M     26.8M         0 100% /rom
	tmpfs                    61.7M    272.0K     61.5M   0% /tmp
	/dev/mtdblock6            4.1M    432.0K      3.6M  10% /overlay
	overlayfs:/overlay        4.1M    432.0K      3.6M  10% /
	tmpfs                   512.0K         0    512.0K   0% /dev
	/dev/mmcblk0p1            3.2G      2.8G    246.0M  92% /tmp/run/mountd/mmcblk0p1
	```

2. Write an new emtry ext4 file system to your SD card.（Warning: When doing this, all the data on your SD card will be cleared） 

	```
	umount /dev/mmcblk0p1
	mkfs.ext4 /dev/mmcblk0p1
	```

3. Prepare your external storage root overlay.
	
	```
	mount /dev/mmcblk0p1 /mnt ; tar -C /overlay -cvf - . | tar -C /mnt -xf - ; umount /mnt
	```

4. Create fstab with the following command. This command will create a fstab template enabling all partitions and setting '/mnt/mmcblk0p1' partition as '/overlay' partition.
	
	```
	block detect > /etc/config/fstab;
	sed -i s/option$'\t'enabled$'\t'\'0\'/option$'\t'enabled$'\t'\'1\'/ /etc/config/fstab;
	sed -i s#/mnt/mmcblk0p1#/overlay# /etc/config/fstab;
	cat /etc/config/fstab;
   ```
   
5. Check if it is mountable to overlay.

	```
	root@mylinkit:/# mount /dev/mmcblk0p1 /overlay/
	[ 1771.940000] [EXFAT] trying to mount...
	[ 1771.950000] EXT4-fs (mmcblk0p1): couldn't mount as ext3 due to feature incompatibilities
	[ 1771.970000] EXT4-fs (mmcblk0p1): couldn't mount as ext2 due to feature incompatibilities
	[ 1771.990000] EXT4-fs (mmcblk0p1): mounted filesystem with ordered data mode. Opts: (null)
	root@mylinkit:/# df
	Filesystem           1K-blocks      Used Available Use% Mounted on
	rootfs                    4160       432      3728  10% /
	/dev/root                27392     27392         0 100% /rom
	tmpfs                    63224       276     62948   0% /tmp
	/dev/mtdblock6         3360336      6564   3163360   0% /overlay
	overlayfs:/overlay        4160       432      3728  10% /
	tmpfs                      512         0       512   0% /dev
	/dev/mmcblk0p1         3360336      6564   3163360   0% /overlay 
	```

6. Reboot ReSpeaker and check again. If SD card is mounted automatically, you are done. More informations about **extroot**, please click [here](https://wiki.openwrt.org/doc/howto/extroot).

##Install software on ReSpeaker

After extending storage with a SD card, there are enough storage to install software on ReSpeaker.

1. Install git

	```
	opkg update
	opkg install git git-http
	```
	