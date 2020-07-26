## Remove background noise in real time
Whatever microphone I connect to my computer, I get background noise when recording of having online meeting. While tacking with this issue, I've learned a few things.

### [PulseAudio](https://www.freedesktop.org/wiki/Software/PulseAudio/)
PulseAudio is sound server running in background. It accepts sound input from one or more **sources**. PulseAudio then redirects that sound to one or more **sinks**. [[1]]

#### [module-echo-cancel](https://www.freedesktop.org/wiki/Software/PulseAudio/Documentation/User/Modules/#index45h3)
In the file `/etc/pulse/default.pa` you can set what addition module PulseAudio will load in run time. This module (module-echo-cancel) is written to resolve issue that I had (remove background noise in real time). To activate this module: [[2]]

 - In this file:
```sh
sudo vim /etc/pulse/default.pa
```
 - Add the following line anywhere in the file:
 ```
 load-module module-echo-cancel source_name=mysource source_properties=device.description=EchoCancel
 ```
 `source_properties` is the name that will be displayed in the UI, eg:
 
 ![image](https://user-images.githubusercontent.com/1465340/88484736-1c396900-cf71-11ea-8607-17118283644f.png)
 
 `source_name` is used to reference this module in the file. For example, to set this module to be default source in `/etc/pulse/default.pa` add:
 ```
 set-default-source mysource
 ```
 To avoid the echo module from adjusting the volume slider automatically, set `load-module module-echo-cancel aec_method=webrtc aec_args="analog_gain_control=0 digital_gain_control=1"`
 - Reset PulseAudio
 ```sh
 pulseaudio -k
 ```
 
 
[1]: https://en.wikipedia.org/wiki/PulseAudio#Software_architecture
[2]: https://askubuntu.com/questions/18958/realtime-noise-removal-with-pulseaudio
