# ReSpeaker Arduino Library


[ReSpeaker Arduino Library](https://github.com/respeaker/respeaker_arduino_library) provides the following features: 

- Supported capacitive touch sensing
- Implemented WS2812 RGB LED driver
- Built USB to Serial bridge and SPI bridge between - Arduino (ATmega32U4) and linux based OpenWrt (MT7688)


## class ReSpeaker


- **void begin(int touch=1, int pixels=1, int spi=1);**
    
    Setup touch buttons, full color pixels and spi bridge
    
    - **Parameters:**
    
		*touch* - 1-enable touch buttons, 0-disable
	
		*pixeld* - 1-enable pixels, 0-disable
	
		*spi* - 1-enable spi bridge, 0-disable

	```C++
	//Example usage
	void setup() {
	respeaker.begin();
	}
	```

- **void play(const char \*name);**
    
    Play music file on sd card, support wav & mp3
    
    - **Parameters:**

		*name* - music file without path

	```C++
	//Example usage
	respeaker.play("hi.wav");
	```

- **void exec(const char \*cmd);**
    
    Execute a shell command

	 - **Parameters:**
 
		*cmd* - command to be executed
		
	```C++
	//Example usage
	respeaker.exec("python example.py");
	```

- **uint16\_t read\_touch(uint8\_t id);**

	Charge a touch button's capacity, read the charging time
	
	- **Parameters:**

		*id* - the id of the touch button
	
	- **Return:** measurement value

	```C++
	//Example usage
	uint8_t mytouchsensor1 = 1;
	if (respeaker.read_touch(mytouchsensor1) >= 100) {
		//do something here
	  }
	```
    
- **uint16\_t detect\_touch();**

	Detect all touch buttons's status
	
	- **Return:** all the touch buttons' status in bit. 
    
	```C++
	//Example usage
	uint16_t buttonstatus = 0;
	buttonstatus = respeaker.detect_touch();
	if (buttonstatus == 0xf0) {
	//do something here
	}
	```

- **void attach\_touch\_handler(void (\*handler)(uint8\_t id, uint8\_t event));**
    
	Attach an interrupt handler which will be called when a touch event happens
	
	```C++
	//Example usage
	respeaker.attach_touch_handler(touch_event);  // add touch event handler
	```

- **void attach\_spi\_handler(void (\*handler)(uint8\_t addr, uint8\_t \*data, uint8\_t len));**     
        
	Attach an interrupt handler which will be called when a spi packet is received
	
	```C++
	//Example usage
	respeaker.attach_spi_handler(spi_event);
	```

- **void attach\_spi\_raw\_handler(void (\*handler)(uint8_t data));** 

	Attach an interrupt handler which will be called when a single byte is received from spi
	
	```C++
	//Example usage
	respeaker.attach_spi_raw_handler(spi_event);
	```

- **Pixels &pixels();**

	Get the Pixels reference of the 12 pixels on respeaker
	
	- **Return:** Pixels reference 

	```C++
	//Example usage
	Pixels *pixels;
	pixels = &respeaker.pixels();
	```
	
- **void Pixels::set\_color(uint32\_t rgb);**

	Set color of all pixels(12)
	
	- **Parameters:**

		*rgb* - hex color codes, for example, 0xff0000 means red and 0xffff00 means yellow
		
	```C++
	//Example usage
	pixels->set_color(Pixels::RGB(0xff, 0, 0x7f));
	pixels->update();
	```
	

- **void Pixels::set\_color(uint16\_t index, uint32\_t rgb);**

	Set color of one pixel

	- **Parameters:**
		
		*index* - the index of pixel
		
		*rgb* - hex color codes, for example, 0xff0000 means red and 0xffff00 means yellow
		
	```C++
	//Example usage
	for (int i = 0; i < PIXELS_NUM; i++) {
		pixels->set_color(i, Pixels::RGB(0, 0, i * 255 / (PIXELS_NUM - 1)));
	  }
	pixels->update();
	```
	
- **void Pixels::set\_color(uint16\_t index, uint8\_t r, uint8\_t g, uint8\_t b);**

	Set color of one pixel

	- **Parameters:**
		
		*index* - the index of pixel
		
		*r* - color codes of red, from 0 to 255 or 0x00 to 0xff
		
		*g* - color codes of green, from 0 to 255 or 0x00 to 0xff
		
		*b* - color codes of blue, from 0 to 255 or 0x00 to 0xff
				
	```C++
	//Example usage
	for (int i = 0; i < PIXELS_NUM; i++) {
		pixels->set_color(i, 0, 0, i * 255 / (PIXELS_NUM - 1));
	  }
	pixels->update();
	```
	
- **void Pixels::update();**

	Update color code to all pixels and light them up

- **void Pixels::clear();**

	Leave all the pixels off
	
	```C++
	//Example usage
	pixels->clear();
	```
	
- **void Pixels::blink(uint32\_t px\_value, uint8\_t time);**

	Set all the pixels blink

	- **Parameters:**

		*px_value* - hex rgb color codes
		
		*time* - 	delay time 
	
	```C++
	//Example usage
	pixels->blink(0xff00ff, 500);
	```

- **void Pixels::blink(uint32\_t px\_value, uint8\_t time, uint16\_t index);**

	Set one pixel blink

	- **Parameters:**

		*px_value* - hex rgb color codes
		
		*time* - 	delay time 
		
		*index* - the index of pixel
	
	```C++
	//Example usage
	for (int i = 0; i < PIXELS_NUM; i++) {
		pixels->blink(0xff00ff, 500, i);
	  }
	```
