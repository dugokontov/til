## How to override jack info
### Why?
For some reason, my audio card can't detect if jack is plugged in or not. When I open [pavucontrol](https://freedesktop.org/software/pulseaudio/pavucontrol/), on input devices I can see "Built-in Audio Analog Stereo", but all ports are marked as unplugged. When I run
```sh
pacmd list-sources
```
All ports are marked with `available: no`. Result of this is that I don't have an option to select this input device anywhere, (I can select only only "Monitor of Built-in Audio Analog Stereo").

While investigating this I figured out that you can override default Jack behavior. [[1]] [[2]]
### How?
The files in the `/usr/share/pulseaudio/alsa-mixer/paths/` directory control how [PulseAudio](https://en.wikipedia.org/wiki/PulseAudio) interacts with [ALSA](https://en.wikipedia.org/wiki/Advanced_Linux_Sound_Architecture) devices. I edited file with the same name that I got under `ports` section after running `pacmd list-sources` (analog-input-rear-mic). There, I edited following section:

```
[Jack Rear Mic]
required-any = any
state.unplugged = yes
```

### What does this mean?
`required-any = any` was present in this configuration before. If I understand this property correctly, it needs to be there to make this path valid.
`state.unplugged = yes`: this tells that when jack is unplugged, this port will become available. Since my card can't register state where my jack is plugged, this is the behavior I wanted.

### Debugging PulseAudio
```sh
echo autospawn = no >> ~/.config/pulse/client.conf  #use ~/.pulse/client.conf on Ubuntu <= 12.10
killall pulseaudio
LANG=C pulseaudio -vvvv --log-time=1 > ~/pulseverbose.log 2>&1
```

 > **Note to myself:** Don't forget to remove echoed lines in `~/.config/pulse/client.conf` after debugging is done.
 
 [1]: https://aweirdimagination.net/2018/12/16/pulseaudio-headphone-jack-troubles/
 [2]: https://github.com/pulseaudio/pulseaudio/blob/master/src/modules/alsa/mixer/paths/analog-output.conf.common
