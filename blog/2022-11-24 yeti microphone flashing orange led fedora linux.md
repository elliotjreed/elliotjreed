# How to fix the flashing orange LED light on a Yeti microphone on Fedora Linux

After a recent upgrade on Fedora, my Yeti microphone started flashing orange, but did still work.

The LED on the button flashes orange when the sampling rate is incorrect, it should be 48000 Hz.

Previously this would be fixed by editing the Pulseaudio configuration, but now we need to look at the Pipewire configuration.

First create a `pipwire` directory in `/etc` if it does not already exist:

```bash
sudo mkdir -p /etc/pipewire
```

Then copy the contents of `/usr/share/pipewire/pipewire.conf` to `/etc/pipewire/pipewire.conf` so that the edits will persist across updates:

```bash
sudo cp /usr/share/pipewire/pipewire.conf /etc/pipewire/pipewire.conf
```

Next open `/etc/pipewire/pipewire.conf` and uncomment and edit the `default.clock.allowed-rates` setting:

```bash
default.clock.allowed-rates = [ 44100, 48000 ]
```

After saving that file, reboot your computer and the orange LED should have stopped blinking (and the correct sampling rate be being used).
