#Programming Guide



##How to write a simple voice interaction program





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




##Music player





##Fruit piano





##Weather Cloud