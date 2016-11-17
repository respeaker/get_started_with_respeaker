#Hardware

##ReSpeaker Core
<img src="https://statics3.seeedstudio.com/seeed/img/2016-09/EYmleODafL5rcUKhEV5FRzgO.jpg" 
width="50%" height="50%">

###Key Features

![](https://www.seeedstudio.com/upload/image/20161011/1476169138612844.jpg)

###Technology Specs

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



##ReSpeaker Mic array

###Description
The ReSpeaker Mic Array can be stacked (connected) right onto the top of ReSpeaker Core to significantly improve the voice interaction experience. It is developed based on the XVSM-2000 Smart Microphone from XMOS. The board integrates 7 PDM microphones to help enhance ReSpeaker's acoustic DSP performance to a much higher level.

<img src="https://statics3.seeedstudio.com/seeed/img/2016-09/Eb4RjfA2jaWSQn6h1nEN4JNe.jpg" 
width="50%" height="50%">

###Key Features

- Far-field Voice Capture
- Acoustic Source Localization
- Beamforming
- Noise Suppression
- De-reverberation
- Acoustic Echo Cancellation

###Technology Specs

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
  - Cirrus DAC CS43L32.
  - 24 or 16bit 16kHz stereo output.
  - 88 mW Power Into Stereo 16 Ω @ 2.5 V.
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

##ReSpeaker Grove Extension Board

###Description
The Grove extension board can be stacked (connected) right onto the top of ReSpeaker Mic Array. It brings even more possibilities as you can connect various Grove sensors and actuators to extend ReSpeaker’s capabilities. The board provides up to 10 Grove ports for interfacing with a range of plug-n-play Grove sensors and actuators.

<img src="https://statics3.seeedstudio.com/seeed/img/2016-09/JUrDDjfUKYhMjGZRszWnZ9U8.jpg" 
width="50%" height="50%">

###Key Features

- Up to 10 Grove Ports

- Stackable with ReSpeaker Core and ReSpeaker Mic Array

###Technology Specs

![](https://www.seeedstudio.com/upload/image/20161010/1476086262368352.jpg)

- Grove Ports: 10
- Digital I/Os: 3
- Analog I/Os: 4
- I2C Interface: 2
- UART: 1
- Dimensions: 70mm (Diameter)
- Weight: 15.25g


#Software

##OpenWrt

<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/openwrtlogo.png?raw=true" 
width="50%" height="50%">

ReSpeaker runs a distribution of Linux called [OpenWrt](https://openwrt.org/) on AI7688 Wi-Fi Module.
type something here

##Arduino

<img src="https://github.com/respeaker/get_started_with_respeaker/blob/master/img/arduinoide.png?raw=true" 
width="50%" height="50%">

The ATMega32U4 Coprocessor on ReSpeaker is programmed using the [Arduino Software(IDE) 1.6.8+](https://www.arduino.cc/en/Main/Software). 
