# ReSpeaker Python API

[ReSpeaker Python Library](https://github.com/respeaker/respeaker_python_library) is an open source python library to provide functions of voice interaction for ReSpeaker.

## class BingSpeechAPI

Provides methods to:

1. Initialize BingSpeechAPI
2. Apply Bing Speech API access token
3. Realize speech to text and text to speech
4. Generate the WAV header and the WAV file contents

- **def \_\_init\_\_(self, key):**
	
	Initialize BingSpeechAPI.
	
	```python
	#Example usage
	BING_KEY = ''
	bing = BingSpeechAPI(key=BING_KEY)
	```
	
- **def recognize(self, audio\_data, language="en-US", show_all=False):**
	
	Translate the audio data to text by sending it to Bing Speech API.
	
	- **Parameters:**
	
		*audio\_data* - the audio frames
		
		*language* - the language of the audio data translating into, defaults to "en-US"
		
		*show\_all* - whether the entire JSON should be returned, defaults to False
		
	- **Return:** a JSON containing the hander and lexical
		
	- **Raise RequestError:** if recognition connection failed

	```python
	#Example usage
	
	try:
		text = bing.recognize(data)
		if text:
			print('Recognized %s' % text)
	except Exception as e:
		print(e.message)
	```

- **def synthesize(self, text, language="en-US", gender="Female"):**

	Synthesize text to audio data by sending it to Bing Speech API.
	
	- **Parameters:**

		*text* - an instance of the String object.
		
		*language* - the language of the text, supported languages are listed among [*self.locales*](https://github.com/respeaker/respeaker_python_library/blob/master/respeaker/bing_speech_api.py)
		
		*gender* - the text to speech voice 
	
	- **Return:** raw audio data
	
	- **Raise LocaleError:** if *language* not in [*self.locales*](https://github.com/respeaker/respeaker_python_library/blob/master/respeaker/bing_speech_api.py)

	```python
	#Example usage
	if spoken_text:
		audio = bing.synthesize(spoken_text)
		player.play_raw(audio)
	```	

- **def authenticate(self):**

	Apply access token from Bing Speech API. Every call to the Speech API requires a JSON Web Token (JWT) access token and the token has a expire time of 10 minutes.
	
	It is called by **self.recognize()** and **self.synthesize()**.
	- **Parameters:**

		*self.access\_token* - access token from Bing Speech API 
		
	- **Raise RequestError:** if recognition connection failed

		
- **@staticmethod**

	**def to\_wav(raw\_data):**
	
	Generate the WAV file contents with raw audio data.
	
	It is called by **self.recognize()**.
	
	- **Parameters:**
		
		*raw\_data* - raw audio data
		
	- **Return:** WAV data
	
	
- **@staticmethod**

    **def get\_wav\_header():**
    
    Generate the WAV header. 
    
    It is called by **self.recognize()**.
    
    - **Return:** WAV header


## class Player

Based on [PyAudio](https://people.csail.mit.edu/hubert/pyaudio/docs/) and [wave](https://docs.python.org/2/library/wave.html), provides methods to:

1. Initialize Player
2. Play wav file and raw audio data file.

- **def \_\_init\_\_(self, pa):**

	Initialize Player.
	
	- **Parameters:**
		
		*pa* - PyAudio instance
	
	```python
	#Example usage
	mic = Microphone()
   	player = Player(mic.pyaudio_instance)
	```

- **def play(self, wav\_file, block=True):**

	Play wav audio file.
	
	- **Parameters:**
		
		*wav\_file* - wav file
		
		*block* - whether wait for playing audio data in buffer, defaults to True
	
	```python
	#Example usage
	script_dir = os.path.dirname(os.path.realpath(__file__))
	hi = os.path.join(script_dir, 'audio/hi.wav')
	player.play(hi)
	```
	

- **def play\_raw(self, raw\_data, rate=16000, channels=1, width=2, block=True):**

	Play raw audio file.
	
	- **Parameters:**
		
		*raw\_data* - raw audio data file
		
		*block* - whether wait for playing audio data in buffer, defaults to True
	
	```python
	#Example usage	
	if spoken_text:
		audio = bing.synthesize(spoken_text)
		player.play_raw(audio)      
	```


## class Microphone

Provides methods to:

1. Initialize Microphone
2. Translate raw audio data into text
3. Wake up ReSpeaker with keyword
4. Listen and record the speech

- **def \_\_init\_\_(self, pyaudio\_instance=None, quit_event=None):**

	Initialize Microphone.

	- **Parameters:**
		
		*pyaudio_instance* - PyAudio instance, defaults to None 
		
		*quit_event* - if quit_event is set, defaults to None

	```python
	#Example usage	
	mic = Microphone()  
	```

- **def recognize(self, data):**

	Translate raw audio data into text with [PocketSphinx](https://github.com/cmusphinx/pocketsphinx).

	- **Parameters:**
		
		*data* - raw audio data
	
	- **Return:** string

	```python
	#Example usage	
	data = mic.listen()
	text = mic.recognize(data)
	if text:
		time.sleep(1)
		print('Recognized %s' % text) 
	```


- **def detect(self, keyword=None):**

	**def wakeup(self, keyword=None):**
	
	`detect` and `wakeup` are used to wake up ReSpeaker when the keyword is detected.
	
	- **Parameters:**
		
		*keyword* - the keyword to wake up ReSpeaker 
	
	- **Return:** if the keyword is detected, return the keyword; if not, return None.
	
	```python
	#Example usage	
	if mic.wakeup('respeaker'):
		print('wake up')
		data = mic.listen()
		text = mic.recognize(data)
		if text:
			time.sleep(1)
			print('Recognized %s' % text) 
	```


- **def listen(self, duration=9, timeout=3):**

	Listen and record the speech.

	- **Parameters:**
		
		*duration* - listen the speech for the given number of seconds, defaults to 9 seconds
		
		*timeout* - stop listening when don't detect any speeches for the given number of seconds, defaults to 3 seconds
	
	- **Return:** raw audio data
	
	```python
	#Example usage	
	if mic.wakeup('respeaker'):
		print('wake up')
		data = mic.listen()
		text = mic.recognize(data)
		if text:
			time.sleep(1)
			print('Recognized %s' % text) 
	```

- **def record(self, file_name, seconds=1800):**

	Record the speech and save the audio file.

	- **Parameters:**
		
		*file_name* - file name of the saved audio file
			
		*seconds* - recording seconds, defaults to 1800 seconds 
		

- **def start(self):**

	Start processing the audio stream.
	
- **def stop(self):**

	Pause playing/recording.

- **def close(self):**

	Terminate the stream.

- **def task(quit_event):**

	An example of a wakeup and recognize task.

	```python
	#Example usage	
	q = Event()
   	t = Thread(target=task, args=(q,))
   	t.start()
   	while True:
   		try:
    		time.sleep(1)
		except KeyboardInterrupt:
       	print('Quit')
       	q.set()
      		break
   	t.join()
	```


## class SPI

Provides methods to:

1. Initialize SPI
2. Send data to Arduino via SPI

- **def \_\_init\_\_(self, sck=15, mosi=17, miso=16, cs=14):**

	Initialize SPI. Note that class SPI has been instantiated in spi.py, so don't have to initialize it again.

	```python
	#Example usage
	from respeaker import spi
	spi.write(data = bytearray([1, 0, 0, 50]), address = 0x00)
	```

- **def write(self, data=None, address=None):**

	Send data to Arduino. Click this [Data exchange between Arduino and OpenWrt](ProgrammingGuide.md#data-exchange-between-arduino-and-openwrt) for more introduction.
	
	- **Parameters:**
		
		*data* - the data sent to Arduino, it should be a bytearray
		
		*address* - the address of SPI

	```python
	from respeaker import spi
	#send data [1, 0, 0, 50] to Arduino, which will make the leds turn blue.
	spi.write(data = bytearray([1, 0, 0, 50]), address = 0x00)
	```


## class PixelRing

Depends on `class SPI`, provides methods to:

1. Initialize PixelRing
2. Set pixel leds to `off``listen``wait` modes
3. Set specific color to pixel leds 

- **def \_\_init__(self):**

	Initialize PixelRing. Note that class PixelRing has been instantiated in pixel_ring.py, so you don't need to initialize it again.

	```python
	#Example usage
	from respeaker import pixel_ring
	
	pixel_ring.listen()
	time.sleep(3)
	pixel_ring.wait()
	time.sleep(3)
	for level in range(2, 8):
		pixel_ring.speak(level, 0)
		time.sleep(1)
	pixel_ring.set_volume(4)
	time.sleep(3)
	```

- **def off(self):**

	Set pixel leds all off.
	
	```python
	#Example usage
	pixel_ring.off()
	```

- **def listen(self, direction=None):**

	Set pixel leds to `listen` mode, which makes leds all green.
	
	- **Parameters:**
		
		*direction* - when direction is None, send `self.write(0, [7, 0, 0, 0])` and while direction is not None, send `self.write(0, [2, 0, direction & 0xFF, (direction >> 8) & 0xFF])`
		
	```python
	#Example usage
	pixel_ring.listen()
	```

- **def wait(self):**

	Set pixel leds to `wait` mode, which makes three leds green and running in circle.
	
	```python
	#Example usage
	pixel_ring.wait()
	```


- **def set\_color(self, rgb=None, r=0, g=0, b=0):**

	Set specific color to all pixel leds.
		
	- **Parameters:**
		
		*rgb* - hex color codes, for example, 0xff0000 means red and 0xffff00 means yellow
				
		*r* - color codes of red, from 0 to 255 or 0x00 to 0xff
		
		*g* - color codes of green, from 0 to 255 or 0x00 to 0xff
		
		*b* - color codes of blue, from 0 to 255 or 0x00 to 0xff
		
	```python
	#Example usage
	pixel_ring.set_color(rgb=0x505000)
	time.sleep(3)
	pixel_ring.set_color(r=150, g=100, b=20)
	```



