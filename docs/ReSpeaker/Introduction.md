# Hardware

## ReSpeaker Core
<img src="https://statics3.seeedstudio.com/seeed/img/2016-09/EYmleODafL5rcUKhEV5FRzgO.jpg" 
width="50%" height="50%">

### Key Features

![](https://www.seeedstudio.com/upload/image/20161011/1476169138612844.jpg)

### Technology Specs

![](https://www.seeedstudio.com/upload/image/20161010/1476090344476240.jpg)

- AI7688 Wi-Fi Module:
  - Operation system: GNU/Linux based OpenWrt
  - Wi-Fi Network: Support Legacy 802.11b/g and HT 802.11n modes
  - Expansion: Two expansion headers for I2C, GPIO and USB 2.0 host
  - Interfaces: Built-in 3.5mm AUX port, Micro USB and SD card slot
- ATMega32U4 Coprocessor:
  - USB CDC virtual serial port for linux console
  - 12 programmable RGB LED indicators
  - 8 on board touch sensors
- Codec WM8960:
  - DAC SNR 98dB (‘A’ weighted), THD -84dB at 48kHz, 3.3V  
  - ADC SNR 94dB (‘A’ weighted), THD -82dB at 48kHz, 3.3V  
  - Stereo Class D Speaker Driver with 87% efficiency (1W output)  
  - On-chip Headphone Driver  
  - 40mW output power into 16Ω at 3.3V  
  - THD -75dB at 20mW, SNR 90dB with 16Ω load  
  - On-chip PLL provides flexible clocking scheme  
  - Sample rates: 8, 11.025, 12, 16, 22.05, 24, 32, 44.1, 48 kHz
- Power Supply: 5V DC  
- Dimensions: 70mm diameter  
- Weight: 17g 

### Pin-out Diagram

<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/corepinmap.png?raw=true" >

- GPIO0/I2S_ADC: Drive external encoder/decoder, ADC signal

- GPIO1/I2S_DAC: Drive external encoder/decoder, DAC signal

- GPIO2/I2S_LRCLK: Drive external encoder/decoder, Left/right channel sample clock 

- GPIO3/I2S_BCLK: Drive external encoder/decoder, Bit clock

- MCLK_OUT: Master clock for external device

- HP\_SEL: Headphone channel select. If use ReSpeaker Mic Array to output audio, set HP_SEL high

- HP\_L: Analog audio left channel from ReSpeaker Mic Array

- HP_R: Analog audio right channel from ReSpeaker Mic Array

- AGND: Analog ground for audio


### Resources

- [ReSpeaker Core v1.0 SCH](https://github.com/respeaker/get_started_with_respeaker/blob/master/files/RespeakerCorev1.0_SCH.sch)
- [ReSpeaker Core v1.0 BRD](https://github.com/respeaker/get_started_with_respeaker/blob/master/files/RespeakerCorev1.0_BRD.brd)
- [ReSpeaker Core v1.0 Schematic(pdf)](https://github.com/respeaker/get_started_with_respeaker/blob/master/files/RespeakerCorev1.0_Schematic.pdf)
- [ReSpeaker Core v1.0 PCB bottom(pdf)](https://github.com/respeaker/get_started_with_respeaker/blob/master/files/RespeakerCorev1.0_PCB_bottom.pdf)
- [ReSpeaker Core v1.0 PCB top(pdf)](https://github.com/respeaker/get_started_with_respeaker/blob/master/files/RespeakerCorev1.0_PCB_top.pdf)
- [ReSpeaker Pro Case 3D](https://github.com/respeaker/get_started_with_respeaker/blob/master/files/respeaker_pro_case_3d.zip)



## ReSpeaker Mic Array

### Description
The ReSpeaker Mic Array can be stacked (connected) right onto the top of ReSpeaker Core to significantly improve the voice interaction experience. It is developed based on the XVSM-2000 Smart Microphone from XMOS. The board integrates 7 PDM microphones to help enhance ReSpeaker's acoustic DSP performance to a much higher level.

<img src="https://statics3.seeedstudio.com/seeed/img/2016-09/Eb4RjfA2jaWSQn6h1nEN4JNe.jpg" 
width="50%" height="50%">

### Key Features

- Far-field Voice Capture
- Acoustic Source Localization
- Beamforming
- Noise Suppression
- De-reverberation


### Technology Specs

![](https://www.seeedstudio.com/upload/image/20161010/1476085761315357.jpg)

- XVSM-2000 with 16 cores inside:
  - 16 real-time logical cores on 2 xCore tiles.
  - Cores share up to 2000 MIPS in dual issue mode.
  - 512KB internal single-cycle SRAM and 2MB built-in flash. 
  - 16KB internal OTP (max 8KB per tile),
  - USB PHY, fully compliant with USB 2.0 specification.
  - Programmable I/O.
  - Supply DFU Mode.
- 7 Digital Microphones: 
  - far field voice recognition or sound localization usefulness.
  - ST MP34DT01-M.
  - -26 dBFS sensitivity.
  - 120 dBSPL acoustic overload point.
  - 61 dB signal-to-noise ratio.
  - Omnidirectional sensitivity.
  - PDM output.
- 12 RGB LEDs: 
  - 256 levels brightness.
  - 800kHz line data transmission.
- Audio output: 
  - On board 3.5mm Aux output.
  - WOLFSON WM8960.
  - 24 or 16bit 16kHz stereo output.
  - 40 mW output Power into 16 Ω @ 3.3 V.
- Clock Sync：
  - On board PLL.
  - Programmable sample clock for DAC,MIC.
	(Disable if DSP is used in XVSM-2000).
- Power supply: 
  - 5V supply from Micro USB or expansion header. 
- Size: 
  - Diameter 70mm.
- Weight: 
  - 15.25g

### Resources

- [ReSpeaker Microphone Array SCH&PCB](https://github.com/Fuhua-Chen/ReSpeaker_Microphone_Array_SCH_PCB)


## ReSpeaker Grove Extension Board

### Description
The Grove extension board can be stacked (connected) right onto the top of ReSpeaker. It brings even more possibilities as you can connect various Grove sensors and actuators to extend ReSpeaker’s capabilities. The board provides up to 10 Grove ports for interfacing with a range of plug-n-play Grove sensors and actuators.

<img src="https://statics3.seeedstudio.com/seeed/img/2016-09/JUrDDjfUKYhMjGZRszWnZ9U8.jpg" 
width="50%" height="50%">

### Key Features

- Up to 10 Grove Ports

- Stackable with ReSpeaker Core and ReSpeaker Mic Array

### Technology Specs

![](https://www.seeedstudio.com/upload/image/20161010/1476086262368352.jpg)

- Grove Ports: 10
- Digital I/Os: 3
- Analog I/Os: 4
- I2C Interface: 2
- UART: 1
- Dimensions: 70mm (Diameter)
- Weight: 15.25g


# Software

## OpenWrt

<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/openwrtlogo.png?raw=true" 
width="50%" height="50%">

ReSpeaker runs a open-source distribution of embedded Linux called [OpenWrt](https://github.com/respeaker/openwrt) on AI7688 Wi-Fi Module. 


When Voice Interaction, onbroad pixel leds and touch sensors, kinds of [Grove sensors and actuators](http://wiki.seeed.cc/Grove/) and powerful network capabilities all focus on a Linux system which has small volume, low power consumption and enough computing capacity, ReSpeaker may become a perfect carrier of your IoT or artificial intelligence applications. That is why we create ReSpeaker.


## Arduino

<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/arduinoide.png?raw=true" 
width="50%" height="50%">

The ATMega32U4 Coprocessor on ReSpeaker is programmed using the [Arduino Software(IDE) 1.6.8+](https://www.arduino.cc/en/Main/Software) and our [ReSpeaker Arduino Library](https://github.com/respeaker/respeaker_arduino_library). 


Arduino is an open-source electronics platform based on easy-to-use hardware and software. With Arduino on ReSpeaker, developers are able to make creative applications in their familiar area.


---

Follow us on [Facebook](https://facebook.com/seeedstudiosz) [Youtube](https://www.youtube.com/channel/UC5mX-JaRWXc8cBc1gm5kKhg) [Twitter](https://twitter.com/seeedstudio).
