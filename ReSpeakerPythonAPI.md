#ReSpeaker Python API


##BingSpeechAPI

**class BingSpeechAPI:**

Provides methods to:

1. initialize BingSpeechAPI
2. apply Bing Speech API access token
3. realize speech to text and text to speech
4. Generate the WAV header and the WAV file contents

- **def \_\_init__(self, key):**
	
	Initialize BingSpeechAPI.
	
- **def authenticate(self):**

	Apply access token from Bing Speech API. Every call to the Speech API requires a JSON Web Token (JWT) access token and the token has a expire time of 10 minutes.
	
	It is called by **self.recognize()** and **self.synthesize()**.
	- **Parameters:**

		*self.access\_token* - access token from Bing Speech API 
		
	- **Raise RequestError:** if recognition connection failed

- **def recognize(self, audio\_data, language="en-US", show_all=False):**
	
	Translate the audio data to text by sending it to Bing Speech API.
	
	- **Parameters:**
	
		*audio\_data* - the audio frames
		
		*language* - the language of the audio data translating into, defaults to "en-US"
		
		*show\_all* - whether the entire JSON should be returned, defaults to False
		
	- **Return:** a JSON containing the hander and lexical
		
	- **Raise RequestError:** if recognition connection failed

	
- **def synthesize(self, text, language="en-US", gender="Female"):**

	Synthesize text to audio data by sending it to Bing Speech API.
	
	- **Parameters:**

		*text* - an instance of the String object.
		
		*language* - the language of the text, supported languages are listed among [*self.locales*](https://github.com/respeaker/respeaker_python_library/blob/master/respeaker/bing_speech_api.py)
		
		*gender* - the text to speech voice 
	
	- **Return:** raw audio data
	
	- **Raise LocaleError:** if *language* not in [*self.locales*](https://github.com/respeaker/respeaker_python_library/blob/master/respeaker/bing_speech_api.py)
		
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

