---
name: ReSpeaker 4-Mic Array for Raspberry Pi
category: ReSpeaker
bzurl: https://www.seeedstudio.com/ReSpeaker-4-Mic-Array-for-Raspberry-Pi-p-2941.html
oldwikiname:
prodimagename:
surveyurl: https://www.research.net/r/ReSpeaker_4-Mic_Array_for_Raspberry_Pi
sku: 103030216
---

![](https://files.seeedstudio.com/wiki/ReSpeaker-4-Mic-Array-for-Raspberry-Pi/img/overview.jpg)

ReSpeaker 4-Mic Array for Raspberry Pi is a quad-microphone expansion board for Raspberry Pi designed for AI and voice applications. This means that we can build a more powerful and flexible voice product that integrates Amazon Alexa Voice Service, Google Assistant, and so on.


Different from [ReSpeaker 2-Mics Pi HAT](https://www.seeedstudio.com/ReSpeaker-2-Mics-Pi-HAT-p-2874.html), this board is developed based on AC108, a highly integrated quad-channel ADC with I2S/TDM output transition for high definition voice capture, which allows the device to pick up sounds in a 3 meters radius. Besides, this 4-Mics version provides a super cool LED ring, which contains 12 APA102 programable LEDs. With that 4 microphones and the LED ring, Raspberry Pi would have the ability to do VAD(Voice Activity Detection), estimate DOA(Direction of Arrival), do KWS(Keyword Search) and show the direction via LED ring, just like Amazon Echo or Google Home.

<iframe width="800" height="450" src="https://www.youtube.com/embed/uAQf0RKBNHo" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

[![](https://files.seeedstudio.com/wiki/Seeed-WiKi/docs/images/300px-Get_One_Now_Banner-ragular.png)](https://www.seeedstudio.com/ReSpeaker-4-Mic-Array-for-Raspberry-Pi-p-2941.html)

## Features

* Raspberry Pi compatible(Support Raspberry Pi Zero and Zero W, Raspberry Pi B+, Raspberry Pi 2 B, Raspberry Pi 3 B, Raspberry Pi 3 B+, Raspberry Pi 3 A+ and Raspberry Pi 4)
* 4 Microphones
* 3 meters radius voice capture
* 2 Grove Interfaces
* 12 APA102 User LEDs
* Software Algorithm: VAD(Voice Activity Detection), DOA(Direction of Arrival) and KWS(Keyword Search)

Note: There is no audio output interface on ReSpeaker 4-Mic Array for Raspberry Pi. It is only for voice capture. We can use the [headphone jack](https://www.raspberrypi.org/documentation/configuration/audio-config.md) on Raspberry Pi for audio output.

## Application Ideas

* Voice Interaction Application
* AI Assistant

## Hardware Overview

![](https://files.seeedstudio.com/wiki/ReSpeaker-4-Mic-Array-for-Raspberry-Pi/img/features.png)

- MIC: 4 analog microphones
- LED: 12 APA102 programable RGB LEDs, connected to SPI interface
- Raspberry Pi 40-Pin Headers: support Raspberry Pi Zero, Raspberry Pi 1 B+, Raspberry Pi 2 B, Raspberry Pi 3 B and Raspberry Pi 3 B+
- AC108: highly integrated quad-channel ADC with I2S/TDM output transition
- I2C: Grove I2C port, connected to I2C-1
- GPIO12: Grove digital port, connected to GPIO12 & GPIO13

Note: If we want to use the APA102 RGB LEDs, please write HIGH to `GPIO5` first to enable VCC of the LEDs.


## Getting Started

**Connect ReSpeaker 4-Mic Array to Raspberry Pi**

Mount ReSpeaker 4-Mic Array on Raspberry Pi, make sure that the pins are properly aligned when stacking the ReSpeaker 4-Mic Array for Raspberry Pi.

Note: Hot-plugging ReSpeaker 4-Mic Array is not allowed.It will damage the respeaker.

![connection pic1](https://files.seeedstudio.com/wiki/ReSpeaker-4-Mic-Array-for-Raspberry-Pi/img/connect1.jpg)
![connection pic2](https://files.seeedstudio.com/wiki/ReSpeaker-4-Mic-Array-for-Raspberry-Pi/img/connect2.jpg)

**Install driver**

The AC108 codec is not supported by Pi kernel builds currently, we have to build it manually.

- Step 1. Please Make sure running [the lastest Raspbian Operating System(debian 9)](https://www.raspberrypi.org/downloads/raspbian/) on Pi. *(updated at 2018.11.13)*

- Step 2. Get the seeed voice card source code.

```sh
$ sudo apt-get update
$ sudo apt-get upgrade
$ git clone https://github.com/respeaker/seeed-voicecard.git
$ cd seeed-voicecard
$ sudo ./install.sh  
$ reboot
```

- Step 3. Then select the headphone jack on Raspberry Pi for audio output:

```sh
sudo raspi-config
# Select 7 Advanced Options
# Select A4 Audio
# Select 1 Force 3.5mm ('headphone') jack
# Select Finish
```

- Step 4. Check that the sound card name looks like this:

```sh
pi@raspberrypi:~ $ arecord -L
null
    Discard all samples (playback) or generate zero samples (capture)
jack
    JACK Audio Connection Kit
pulse
    PulseAudio Sound Server
default
playback
ac108
sysdefault:CARD=seeed4micvoicec
    seeed-4mic-voicecard, 
    Default Audio Device
dmix:CARD=seeed4micvoicec,DEV=0
    seeed-4mic-voicecard, 
    Direct sample mixing device
dsnoop:CARD=seeed4micvoicec,DEV=0
    seeed-4mic-voicecard, 
    Direct sample snooping device
hw:CARD=seeed4micvoicec,DEV=0
    seeed-4mic-voicecard, 
    Direct hardware device without any conversions
plughw:CARD=seeed4micvoicec,DEV=0
    seeed-4mic-voicecard, 
    Hardware device with all software conversions
usbstream:CARD=seeed4micvoicec
    seeed-4mic-voicecard
    USB Stream Output
usbstream:CARD=ALSA
    bcm2835 ALSA
    USB Stream Output
```

If we want to change the `alsa` settings, we can use `sudo alsactl --file=ac108_asound.state store` to save it. And when we need to use the settings again, copy it to: `sudo cp ~/seeed-voicecard/ac108_asound.state /var/lib/alsa/asound.state`

- Step 5. Open Audacity and select **AC108 & 4 channels** as input and **bcm2835 alsa: - (hw:0:0)** as output to test:

```sh
$ sudo apt update
$ sudo apt install audacity
$ audacity                      // run audacity
```

![](https://files.seeedstudio.com/wiki/ReSpeaker-4-Mic-Array-for-Raspberry-Pi/img/audacity.png)

- Step 6. Or we could record with `arecord` and play with `aplay`:

```
arecord -Dac108 -f S32_LE -r 16000 -c 4 hello.wav    // only support 4 channels
aplay hello.wav                                      // make sure default device
                                                     // Audio will come out via audio jack of Raspberry Pi
```

**Play with APA102 LEDs**

Each on-board APA102 LED has an additional driver chip. The driver chip takes care of receiving the desired colour via its input lines and then holding this colour until a new command is received.

![](https://files.seeedstudio.com/wiki/ReSpeaker-4-Mic-Array-for-Raspberry-Pi/img/rainbow.jpg)

- Step 1. Activate SPI:
    - sudo raspi-config
    - Go to "Interfacing Options"
    - Go to "SPI"
    - Enable SPI
    - Exit the tool
- Step 2. Get APA102 LEDs Library and examples

```sh
pi@raspberrypi:~ $ cd /home/pi
pi@raspberrypi:~ $ git clone https://github.com/respeaker/4mics_hat.git
pi@raspberrypi:~ $ cd /home/pi/4mics_hat
pi@raspberrypi:~/4mics_hat $ sudo apt install python-virtualenv          # install python virtualenv tool
pi@raspberrypi:~/4mics_hat $ virtualenv --system-site-packages ~/env     # create a virtual python environment
pi@raspberrypi:~/4mics_hat $ source ~/env/bin/activate                   # activate the virtual environment
(env) pi@raspberrypi:~/4mics_hat $ pip install spidev gpiozero           # install spidev and gpiozero
```

- Step 3. Then run the example code under virtualenv, now we can see the LEDs blink like Google Assistant.

```
(env) pi@raspberrypi:~/4mics_hat $ python pixels_demo.py
```

## Extract Voice

We use [PyAudio python library](https://people.csail.mit.edu/hubert/pyaudio/) to extract voice.

- Step 1, We need to run the following script to get the device index number of 4 Mic pi hat:

```sh
$ sudo pip install pyaudio
$ cd ~
$ nano get_index.py
```

- Step 2, copy below code and paste on get_index.py.

```Python
import pyaudio

p = pyaudio.PyAudio()
info = p.get_host_api_info_by_index(0)
numdevices = info.get('deviceCount')

for i in range(0, numdevices):
        if (p.get_device_info_by_host_api_device_index(0, i).get('maxInputChannels')) > 0:
            print "Input Device id ", i, " - ", p.get_device_info_by_host_api_device_index(0, i).get('name')
```

- Step 3, press Ctrl + X to exit and press Y to save.

- Step 4, run 'sudo python get_index.py' and we will see the device ID as below.

```
Input Device id  2  -  seeed-4mic-voicecard: - (hw:1,0)
```

- Step 5, change `RESPEAKER_INDEX = 2` to index number. Run python script record.py to record a speech.

```Python
import pyaudio
import wave

RESPEAKER_RATE = 16000
RESPEAKER_CHANNELS = 4 
RESPEAKER_WIDTH = 2
# run getDeviceInfo.py to get index
RESPEAKER_INDEX = 2  # refer to input device id
CHUNK = 1024
RECORD_SECONDS = 5
WAVE_OUTPUT_FILENAME = "output.wav"

p = pyaudio.PyAudio()

stream = p.open(
            rate=RESPEAKER_RATE,
            format=p.get_format_from_width(RESPEAKER_WIDTH),
            channels=RESPEAKER_CHANNELS,
            input=True,
            input_device_index=RESPEAKER_INDEX,)

print("* recording")

frames = []

for i in range(0, int(RESPEAKER_RATE / CHUNK * RECORD_SECONDS)):
    data = stream.read(CHUNK)
    frames.append(data)

print("* done recording")

stream.stop_stream()
stream.close()
p.terminate()

wf = wave.open(WAVE_OUTPUT_FILENAME, 'wb')
wf.setnchannels(RESPEAKER_CHANNELS)
wf.setsampwidth(p.get_sample_size(p.get_format_from_width(RESPEAKER_WIDTH)))
wf.setframerate(RESPEAKER_RATE)
wf.writeframes(b''.join(frames))
wf.close()
```

- Step 6. If you want to extract channel 0 data from 4 channels, please follow below code. For other channel X, please change [0::4] to [X::4].

```python
import pyaudio
import wave
import numpy as np

RESPEAKER_RATE = 16000
RESPEAKER_CHANNELS = 4
RESPEAKER_WIDTH = 2
# run getDeviceInfo.py to get index
RESPEAKER_INDEX = 2  # refer to input device id
CHUNK = 1024
RECORD_SECONDS = 3
WAVE_OUTPUT_FILENAME = "output.wav"

p = pyaudio.PyAudio()

stream = p.open(
            rate=RESPEAKER_RATE,
            format=p.get_format_from_width(RESPEAKER_WIDTH),
            channels=RESPEAKER_CHANNELS,
            input=True,
            input_device_index=RESPEAKER_INDEX,)

print("* recording")

frames = [] 

for i in range(0, int(RESPEAKER_RATE / CHUNK * RECORD_SECONDS)):
    data = stream.read(CHUNK)
    # extract channel 0 data from 4 channels, if you want to extract channel 1, please change to [1::4]
    a = np.fromstring(data,dtype=np.int16)[0::4]
    frames.append(a.tostring())

print("* done recording")

stream.stop_stream()
stream.close()
p.terminate()

wf = wave.open(WAVE_OUTPUT_FILENAME, 'wb')
wf.setnchannels(1)
wf.setsampwidth(p.get_sample_size(p.get_format_from_width(RESPEAKER_WIDTH)))
wf.setframerate(RESPEAKER_RATE)
wf.writeframes(b''.join(frames))
wf.close()
```

## Real-time Sound Source Localization and Tracking

[ODAS](https://github.com/introlab/odas) stands for Open embeddeD Audition System. This is a library dedicated to performing sound source localization, tracking, separation and post-filtering. Let's have fun with it.

- Step 1. Get ODAS and build it.

```py
$ sudo apt-get install libfftw3-dev libconfig-dev libasound2-dev libgconf-2-4
$ sudo apt-get install cmake
$ git clone https://github.com/introlab/odas.git
$ mkdir odas/build
$ cd odas/build
$ cmake ..
$ make
```

- Step 2. Get [ODAS Studio](https://github.com/introlab/odas_web/releases)  and open it.

- Step 3. The odascore will be at **odas/bin/odaslive**, the config file is at **odas/config/odaslive/respeaker_4_mic_array.cfg**. 

![](https://files.seeedstudio.com/wiki/ReSpeaker-4-Mic-Array-for-Raspberry-Pi/img/odas.png)


## Enabling Voice Recognition at Edge with Picovoice

<div align=center><img width = 700 src="https://files.seeedstudio.com/wiki/ReSpeaker_4_Mic_Array_for_Raspberry_Pi/banner.gif"/></div>

[**Picovoice**](https://picovoice.ai/) **enables enterprises to innovate and differentiate rapidly with private voice AI**. Build a unified AI strategy around your brand and products with our speech recognition and [**Natural-language understanding (NLU) technologies**](https://searchenterpriseai.techtarget.com/definition/natural-language-understanding-NLU). 

**Seeed has partnered with Picovice to bring Speech Recognition solution on the edge using [ReSpeaker 4 Mic](https://www.seeedstudio.com/ReSpeaker-4-Mic-Array-for-Raspberry-Pi.html) for developers.**

Picovoice is an end-to-end platform for building voice products on your terms. It enables creating voice experiences similar to Alexa and Google. But it entirely runs 100% on-device. There are advantages of Picovoice:

- **Private**: Everything is processed offline. Intrinsically HIPAA and GDPR compliant.
- **Reliable**: Runs without needing constant connectivity.
- **Zero Latency**: Edge-first architecture eliminates unpredictable network delay.
- **Accurate**: Resilient to noise and reverberation. It outperforms cloud-based alternatives by wide margins.
- **Cross-Platform**: Design once, deploy anywhere. Build using familiar languages and frameworks.


### Picovocie with ReSpeaker 4-Mic Array Getting Started

**Step 1.** Please follow the **above step-to-step tutorial of ReSpeaker 4-Mic Array with Raspberry Pi** before the followings.

**Note:** Please make sure that `Audacity` and the `APA102` LEDs are working properly on the ReSpeaker 4-Mic Array with Raspberry Pi.

**Step 2.** Open Terminal and type following command to install `pyaudio` driver.

```sh
$ sudo pip3 install pyaudio
```

**Note**: Please make sure you have `pip3` installed in your Raspberry Pi

**Step 3.** Type the following command on the terminal to **install the Picovoice demo for ReSpeaker 4-Mic Array**.

```sh
$ sudo pip3 install pvrespeakerdemo
```

### Demo Usage

The demo utilises the ReSpeaker 4-Mic array on a Raspberry Pi with Picovoice technology to control the LEDs. **This demo is triggered by the wake word "`Picovoice`" and will be ready to take follow-on actions, such as turning LEDs on and off, and changing LED colors.**

After the installation is finished, type this command to run the demo in the terminal:

```sh
$ picovoice_respeaker_demo
```

### Voice Commands

Here are voice commands for this demo:

- **Picovoice** 

The demo outputs: 

```
wake word
```

- **Turn on the lights**

You should see the lights turned on and the following message in the terminal:

```
{
    is_understood : 'true',
    intent : 'turnLights',
    slots : {
        'state' : 'on',
    }
}
```

The list of commands are shown on the terminal:

```
context:
  expressions:
    turnLights:
      - "[switch, turn] $state:state (all) (the) [light, lights]"
      - "[switch, turn] (all) (the) [light, lights] $state:state"
    changeColor:
      - "[change, set, switch] (all) (the) (light, lights) (color) (to) $color:color"
  slots:
    state:
      - "off"
      - "on"
    color:
      - "blue"
      - "green"
      - "orange"
      - "pink"
      - "purple"
      - "red"
      - "white"
      - "yellow"
```

also, you can try this command to change the colour by:

- **Picovoice, set the lights to orange**

Turn off the lights by:

- **Picovoice, turn off all lights**

**Demo Video Demonstration**

<p style="text-align:center;"><iframe width="720" height="480" src="https://www.youtube.com/embed/icTZeMIJAxw" frameborder="0" allow="accelerometer; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></p>

### Demo Source Code

The demo is built with the **[Picovoice SDK](https://github.com/Picovoice/picovoice)**. The demo source code is available on GitHub at https://github.com/Picovoice/picovoice/tree/master/demo/respeaker.

### Different Wake Words

The [**Picovoice SDK**](https://github.com/Picovoice/picovoice) includes free sample wake words licensed under Apache 2.0, including major voice assistants (e.g. "**`Hey Google`**", "**`Alexa`**") and fun ones like "**`Computer`**" and "**`Jarvis`**".

### Custom Voice Commands

The lighting commands are defined by a Picovoice *Speech-to-Intent context*. You can design and train contexts by typing in the allowed grammar using Picovoice Console. You can test your changes in-browser as you edit with the microphone button. Go to Picovoice Console (https://picovoice.ai/console/) and sign up for an account. Use the **Rhino Speech-to-Intent editor** to make contexts, then train them for Raspberry Pi.

<div align=center><img width = 700 src="https://files.seeedstudio.com/wiki/ReSpeaker_4_Mic_Array_for_Raspberry_Pi/respeaker_demo_console_edit.gif"/></div>

### Multiple Wake Word Examples

<p style="text-align:center;"><iframe width="720" height="480" src="https://www.youtube.com/embed/Dfn3wBE2pwY" frameborder="0" allow="accelerometer; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></p>

To demonstrate the Picovoice's cabability we have also prepared a multi wake word examples using ReSpeaker 4-Mic Array with Raspberry Pi! Different wake word can set to execute certain tasks.

*This package contains a commandline demo for controlling ReSpeaker 4-mic microphone array LEDs using Porcupine.*

### Porcupine

**Porcupine** is a highly-accurate and lightweight wake word engine. It enables building always-listening voice-enabled
applications. It is

- using deep neural networks trained in real-world environments.
- compact and computationally-efficient. It is perfect for IoT.
- cross-platform. Raspberry Pi, BeagleBone, Android, iOS, Linux (x86_64), macOS (x86_64), Windows (x86_64), and web
  browsers are supported. Additionally, enterprise customers have access to the ARM Cortex-M SDK.
- scalable. It can detect multiple always-listening voice commands with no added runtime footprint.
- self-service. Developers can train custom wake word models using [Picovoice Console](https://picovoice.ai/console/).


#### Multi Wake Word Getting Started

Running the following command in terminal to install demo driver:

```sh
$ sudo pip3 install ppnrespeakerdemo
```

#### Multi Wake Word Usage

Run the following in terminal after the driver installation:

```sh
$ porcupine_respeaker_demo
```

Wait for the demo to initialize and print `[Listening]` in the terminal. Say:

> Picovoice

The demo outputs:

```text
detected 'Picovoice'
```

The lights are now set to `green`. Say:

> Alexa

The lights are set to `yellow` now. Say:

> Terminator

to turn off the lights.

#### Wake Word to Colors

Below are the colors associated with supported wake words for this demo:

- ![#ffff33](https://via.placeholder.com/15/ffff33/000000?text=+) `Alexa`
- ![#ff8000](https://via.placeholder.com/15/ff8000/000000?text=+) `Bumblebee`
- ![#ffffff](https://via.placeholder.com/15/ffffff/000000?text=+) `Computer`
- ![#ff0000](https://via.placeholder.com/15/ff0000/000000?text=+) `Hey Google`
- ![#800080](https://via.placeholder.com/15/800080/000000?text=+) `Hey Siri`
- ![#ff3399](https://via.placeholder.com/15/ff3399/000000?text=+) `Jarvis`
- ![#00ff00](https://via.placeholder.com/15/00ff00/000000?text=+) `Picovoice`
- ![#0000ff](https://via.placeholder.com/15/0000ff/000000?text=+) `Porcupine`
- ![#000000](https://via.placeholder.com/15/000000/000000?text=+) `Terminator`

#### Multiple Wake Word Example Source Code

Please see the complete source code of this example here: https://github.com/Picovoice/porcupine/tree/master/demo/respeaker.

### Picovoice Tech Support

If you encounter technical problems using Picovoice, please visit **[Picovoice](https://github.com/Picovoice)** for discussions.

## FAQ

**Q1: How to change the Raspbian Mirrors source?**

A1: Please refer to [Raspbian Mirrors](http://www.raspbian.org/RaspbianMirrors) and follow below instructions to modify the source at beginning.

```sh
pi@raspberrypi ~ $ sudo nano /etc/apt/sources.list
```

For example, we suggest using the tsinghua source for China users. So please modify the sources.list as below.

```sh
deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ stretch main non-free contrib
deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ stretch main non-free contrib
```

**Q2: We can hear the voice by `aplay` from the 3.5mm audio jack but we can't hear the voice when running ns_kws_doa_alexa_with_light.py**

A2: We have 3 players (mpv, mpg123 and gstreamer) to use. SpeechSynthesizer and Alerts prefer mpg123 which is more responsive. AudioPlayer likes gstreamer > mpv > mpg123. Gstreamer supports more audio format and works well on raspberry pi. We can also specify the player of AudioPlayer using the environment variable PLAYER. So please try below commands to enable the voice.

```sh
sudo apt install mpg123
PLAYER=mpg123 python ns_kws_doa_alexa_with_light.py
```

**Q3: There is no response When we run kws_doa.py and say snow boy**

A3: Please run audacity to make sure 4 channels are good. If there is one channel without data, there will be no response when we say snow boy.


**Q4: #include "portaudio.h" Error when run "sudo pip install pyaudio".**

A4: Please run below command to solve the issue. 

```sh
sudo apt-get install portaudio19-dev
```

## Schematic Online Viewer

<div class="altium-ecad-viewer" data-project-src="https://http://files.seeedstudio.com/wiki/ReSpeaker-4-Mic-Array-for-Raspberry-Pi/src/ReSpeaker%204-Mic%20Array%20for%20Raspberry%20Pi%20v1.0.dxf.zip" style="border-radius: 0px 0px 4px 4px; height: 500px; border-style: solid; border-width: 1px; border-color: rgb(241, 241, 241); overflow: hidden; max-width: 1280px; max-height: 700px; box-sizing: border-box;" /></div>

We have this part available in [geppetto](https://geppetto.seeedstudio.com/), easy modular electronic design with Seeed and Geppeto. Build it Now. [geppetto.seeedstudio.com](https://geppetto.seeedstudio.com/)


## Resources

- **[PDF]** [ ReSpeaker 4-Mic Array for Raspberry Pi(PDF)](https://files.seeedstudio.com/wiki/ReSpeaker-4-Mic-Array-for-Raspberry-Pi/src/ReSpeaker%204-Mic%20Array%20for%20Raspberry%20Pi%20%20v1.0.pdf)
- **[DXF]** [ReSpeaker 4-Mic Array for Raspberry Pi v1.0](https://files.seeedstudio.com/wiki/ReSpeaker-4-Mic-Array-for-Raspberry-Pi/src/ReSpeaker%204-Mic%20Array%20for%20Raspberry%20Pi%20v1.0.dxf.zip)
- **[3D]** [ReSpeaker 4-Mic Array for Raspberry Pi v1.0 3D Model](https://files.seeedstudio.com/wiki/ReSpeaker-4-Mic-Array-for-Raspberry-Pi/src/ReSpeaker%204-Mics%20Pi%20HAT%20v1.0.skp.zip)
- **[AC108]** [AC108 DataSheet](http://www.x-powers.com/en.php/Info/product_detail/article_id/41)
- **[Driver]** [Seeed-Voice Driver](https://github.com/respeaker/seeed-voicecard)
- **[Algorithms]** [Algorithms includes DOA, VAD, NS](https://github.com/respeaker/mic_array)
- **[Voice Engine** [Voice Engine project, provides building blocks to create voice enabled objects](https://github.com/voice-engine/voice-engine)
- **[Algorithms]** [AEC](https://github.com/voice-engine/ec)

## Tech Support

Please submit any technical issue into our [forum](https://forum.seeedstudio.com/).

<br /><p style="text-align:center"><a href="https://www.seeedstudio.com/act-4.html?utm_source=wiki&utm_medium=wikibanner&utm_campaign=newproducts" target="_blank"><img src="https://files.seeedstudio.com/wiki/Wiki_Banner/new_product.jpg" /></a></p>

