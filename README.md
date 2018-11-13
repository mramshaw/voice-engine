Voice Engine
============

[![Build Status](http://travis-ci.org/mramshaw/voice-engine.svg?branch=master)](http://travis-ci.org/mramshaw/voice-engine)
[![](http://img.shields.io/pypi/v/voice-engine.svg)](http://pypi.python.org/pypi/voice-engine)


The library is used to create voice interface applications.
It includes building blocks such as KWS ([keyword spotting](http://en.wikipedia.org/wiki/Keyword_spotting))
 and DOA ([Direction Of Arrival](http://en.wikipedia.org/wiki/Direction_of_arrival)). There are also elements
 to measure [RMS amplitude](http://en.wikipedia.org/wiki/RMS_amplitude) (dBFS or dB(A)).


### Requirements
+ pyaudio
+ numpy
+ snowboy


### Installation
Install pyaudio, numpy and snowboy, use virtualenv a virtual python environment.

```
sudo apt install python-pyaudio python-numpy python-virtualenv
sudo apt-get install swig python-dev libatlas-base-dev build-essential make
git clone --depth 1 https://github.com/Kitt-AI/snowboy.git
cd snowboy
virtualenv --system-site-packages env
source env/bin/activate
python setup.py build
python setup.py bdist_wheel
pip install dist/snowboy*.whl
cd ..
git clone https://github.com/voice-engine/voice-engine.git
cd voice-engine
python setup.py bdist_wheel
pip install dist/*.whl
```

### Get started
To record audio and search keyword "snowboy", see also [`kws_snowboy.py`](voice_engine/kws_snowboy.py)

```python
import time
from voice_engine.kws import KWS
from voice_engine.source import Source

src = Source()
kws = KWS()
src.link(kws)

def on_detected(keyword):
    print('found {}'.format(keyword))
kws.on_detected = on_detected

kws.start()
src.start()
while True:
    try:
        time.sleep(1)
    except KeyboardInterrupt:
        break
kws.stop()
src.stop()
```

### Teardown
Deactivate the virtual environment as follows:

    deactivate

### Building blocks
The library uses gstreamer-like [elements](voice_engine/element.py) which can be linked together as an audio pipeline.
One element can connect to more than one other elements.

The topology can be:
```
Source --> ChannelPicker --> KWS          Source --> ChannelPicker --> KWS --> Alexa
  |                          /\
  V                        /   \
 DOA                   Alexa   Google Assistant 
  
```
