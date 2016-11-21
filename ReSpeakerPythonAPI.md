#ReSpeaker Python API

[ReSpeaker Python Library](https://github.com/respeaker/respeaker_python_library) is an open source python library to provide functions of voice interaction for ReSpeaker.

##class BingSpeechAPI

Provides methods to:

1. Initialize BingSpeechAPI
2. Apply Bing Speech API access token
3. Realize speech to text and text to speech
4. Generate the WAV header and the WAV file contents

- **def \_\_init\_\_(self, key):**
	
	Initialize BingSpeechAPI.
	
	```
//Example usage
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

	```
	//Example usage
	
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

	```
	//Example usage
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


##class Player

Based on [PyAudio](https://people.csail.mit.edu/hubert/pyaudio/docs/) and [wave](https://docs.python.org/2/library/wave.html), provides python api to play wav file or raw audio file. 

- **def \_\_init\_\_(self, pa):**

	Initialize Player.
	
	- **Parameters:**
		
		*pa* - PyAudio instance
	
	```
	//Example usage
	mic = Microphone()
   player = Player(mic.pyaudio_instance)
```

- **def play(self, wav\_file, block=True):**

	Play wav audio file.
	
	- **Parameters:**
		
		*wav\_file* - wav file
		
		*block* - whether wait for playing audio data in buffer, defaults to True
	
	```
	//Example usage
	script_dir = os.path.dirname(os.path.realpath(__file__))
hi = os.path.join(script_dir, 'audio/hi.wav')
player.play(hi)
```
	

- **def play\_raw(self, raw\_data, rate=16000, channels=1, width=2, block=True):**

	Play raw audio file.
	
	- **Parameters:**
		
		*raw\_data* - raw audio data file
		
		*block* - whether wait for playing audio data in buffer, defaults to True
	
	```
	//Example usage	
	if spoken_text:
		audio = bing.synthesize(spoken_text)
		player.play_raw(audio)      
```


##class Microphone


- **def \_\_init\_\_(self, pyaudio\_instance=None, quit_event=None):**

	Initialize Microphone.

	- **Parameters:**
		
		*pyaudio_instance* - PyAudio instance, defaults to None 
		
		*quit_event* - , defaults to None


- **def recognize(self, data):**

- **def detect(self, keyword=None):**

	**def wakeup(self, keyword=None):**

- **def listen(self, duration=9, timeout=3):**

- **def record(self, file_name, seconds=1800):**

- **def quit(self):**

- **def start(self):**

- **def stop(self):**

- **def close(self):**

- **def task(quit_event):**