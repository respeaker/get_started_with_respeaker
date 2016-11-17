#Programming Guide

This programming guide is a series of tutorials designed to get you started with ReSpeaker. Starting from the basics, it describes how to make voice interaction, create audio output and program Arduino on ReSpeaker.

ReSpeaker runs the [OpenWrt](https://openwrt.org/) system on MT7688, and provided with on broad [ReSpeaker Python API](https://github.com/respeaker/respeaker_python_library), which makes it friendly and quickly for developers to build IoT applications. On Arduino side, ReSpeaker also provide easy-to-use [ReSpeaker Arduino Library](https://github.com/respeaker/respeaker_arduino_library).


##How to write a simple voice interaction program

With Bing Speech API, ReSpeaker can turn on and recognize audio coming from the microphone in real-time, or recognize audio from a file. 

To use Bing Speech API, first you have to get a key of Microsoft Cognitive Services from [here](https://www.microsoft.com/cognitive-services/en-us/speech-api), and copy it to `BING_KEY = '' `, then save the following code in `bing.py` and run it `python bing.py`

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/getbingapi.png?raw=true" width="50%" height="50%">
</div>

The following code is an example of how to use Bing Speech API and Microphone on ReSpeaker. After waking up ReSpeaker by saying "ReSpeaker" to the board, the code will start to record your voice, translate it to text and display it.

```
import logging
import time
from threading import Thread, Event
from respeaker import Microphone
from respeaker.bing_speech_api import BingSpeechAPI
                                               
                                                   
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

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/voiceinteraction.png?raw=true" width="50%" height="50%">
</div>


##Play Arduino with light, touch, sound and Internet

[ReSpeaker Arduino Library](https://github.com/respeaker/respeaker_arduino_library) is a library for controlling WS2812 RGB LEDs, touch sensors, SeeedStudio Grove modules on Arduino (ATmega32U4) and building communication bridge between Arduino (ATmega32U4) and linux based OpenWrt (MT7688).

### Features
+ supported capacitive touch sensing
+ implemented WS2812 RGB LED driver
+ built USB to Serial bridge and SPI bridge between Arduino (ATmega32U4) and linux based OpenWrt (MT7688)

### Requirements
+ Arduino IDE 1.6.8+. Please use Arduino IDE 1.6.8+ which has some useful new features

### Installation
1. Download [zip file](https://github.com/respeaker/respeaker_arduino_library/archive/master.zip) and extract it into Arduino's libraries directory.
2. Rename `respeaker_arduino_library-master` to `respeaker`

### Getting Started

1. Light - chasing colors

  ```
  #include "respeaker.h"

  uint8_t offset = 0;
  
  void setup() {
    respeaker.begin();
    respeaker.pixels().set_brightness(128);      // set brightness level (from 0 to 255)
  }
  
  void loop() {
    respeaker.pixels().rainbow(offset++);
    delay(10);
  }
  ```
  
2. Touch & Sound - touch to play

  ```
  #include "respeaker.h"

  // wav or mp3 files on SD card
  const char *sound_map[] = {"a1.wav", "b1.wav", "c1.wav", "d1.wav", "e1.wav", "f1.wav", "g1.wav", "c2.wav"};
  
  void setup() {
    respeaker.begin();
    respeaker.attach_touch_handler(touch_event);  // add touch event handler
  }
  void loop() {}
  
  // id: 0 ~ 7 - touch sensor id; event: 1 - touch, 0 - release
  void touch_event(uint8_t id, uint8_t event) {
    if (event) {
      respeaker.play(sound_map[id]);
    }
  }
  ```

3. [Connect to IFTTT maker channel](https://ifttt.com/maker)

  ```
  #include "respeaker.h"

  #define IFTTT_MAKER_CHANNEL_KEY ""              // add the key of your ifttt maker channel
  #define EVENT                   "ping"
  
  const char *ifttt_ping = "curl -X POST https://maker.ifttt.com/trigger/" EVENT "/with/key/" IFTTT_MAKER_CHANNEL_KEY;
  
  void setup() {
    respeaker.begin();
    respeaker.attach_touch_handler(touch_event);  // add touch event handler
  }
  void loop() {}
  
  // id: 0 ~ 7 - touch sensor id; event: 1 - touch, 0 - release
  void touch_event(uint8_t id, uint8_t event) {
    if (event == 1 && id == 0) {
      respeaker.exec(ifttt_ping);
    }
  }
  ```

  




##Data exchange between Arduino and OpenWrt

There are 2 communication ways between Arduino and OpenWrt: UART and SPI bridge. **It means you can control MT7688 with touch buttons or awaken the LEDs when MT7688 detects something.**

###SPI bridge

With SPI bridge, MT7688 works as master and ATmega32U4 works as salve. Import spi.py in your python script on the MPU side and send data with SPI.write(). On Arduino side, you will get the data sent from MT7688 in the spi interrupt handler.

void spi_event(uint8_t addr, uint8_t *data, uint8_t len){
  //handle the data sent from MT7688
}




###UART

There are 2 serial ports available in ATmage32U4: "serial" and "serial1".The "serial" is sumilated by the USB port shared with MT7688 and the "serial1" (on TXD1 and RXD1) is connected to MT7688 (on UART_RXD2 and UART_TXD2). They have been set their baudrate to 57600 bps in respeaker.begin().

void ReSpeaker::begin(int touch, int pixels, int spi)
{
    Serial.begin(57600);
    Serial1.begin(57600);
    ......
}
And read or write something in UART like this:

while (Serial.available() && Serial1.availableForWrite()) {
Serial1.write((char)Serial.read());
    }
while (Serial1.available() && Serial.availableForWrite()) {
Serial.write((char)Serial1.read());
    }
On the MPU side, libmraa provide API for UART in C/C++, Java, Python and JS.



##Music player





##Fruit piano


Rather than the on board MT7688 Wi-FI module which runs the Linux based OpenWrt, ReSpeaker is also powered by the ATmega32u4 chip and it’s absolutely Arduino compatible, which means, we can use ReSpeaker as a powerful Arduino board and do many ‘Arduino’ things. It’s for learning, it’s for practicing, and it’s for fun.

For example, you can program it with Arduino IDE to have a special DIY piano that is built on 8 cherry tomatoes connecting to the 8 touch sensors of ReSpeaker.
	
<embed src="https://www.youtube.com/watch?v=I-US_4S7yMo&feature=youtu.be" allowFullScreen="true" quality="high" width="480" height="400" align="middle" allowScriptAccess="always" type="application/x-shockwave-flash"></embed>


##Weather Cloud

<div class="text-center">
<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/weathercloud.jpg?raw=true" width="50%" height="50%">
</div>

Weather Cloud is an awesome project for ReSpeaker. This cool build turns a ReSpeaker into a Weather Cloud, which is able to show you the whether with vivid light and sounds.

In this project, Openwrt is in charge of getting realtime weather information from the Internet, making voice interaction and audio output, while Arduino is responsible for controlling the colorful RGB LEDs.

The following code realize

http://www.instructables.com/id/How-to-DIY-an-in-House-Weather-telling-Cloud/

