Instructions for using MacOS as the client have been published [on LinkedIn](https://www.linkedin.com/pulse/how-run-doom-ibm-z-linuxone-nielson-nino-de-carvalho-0ukxf/) by Nielson 'Nino' de Carvalho.

This guide focuses on connecting with a MacOS client to a RHEL guest already running Doom and getting sound working. In this tutorial i used the LinuxONE Community Cloud (L1CC) https://community.ibm.com/zsystems/l1cc/ 
These steps will likely vary depending on your setup. For example: As I'm using a mac with Apple silicon and installed pulseaudio through homebrew. The path to the pactl command is different to non-apple silicon or to a non-homebrew install, also I've used terminal to establish the ssh tunnel to L1CC, which differs from MobaXterm setup covered in the RHEL Server guide.

On MacOS:

```
brew install pulseaudio
```
Start pulseaudio with : 
```
pulseaudio --start --exit-idle-time=-1
```
If you get errors and want to see the log try: 
```
pulseaudio -vv --log-time=1 --exit-idle-time=-1
```
In another terminal window, start the CoreAudio TCP on local host port :4713
```
PCTL=/opt/homebrew/bin/pactl
$PCTL load-module module-coreaudio-detect
$PCTL load-module module-native-protocol-tcp auth-anonymous=1 listen=127.0.0.1 port=4713
```
Check if its listening with:
```
lsof -nP -iTCP:4713 -sTCP:LISTEN
```
Next list the sink devices:
```
pactl -s tcp:127.0.0.1:4713 list short sinks
```
I got sound woking on the sink device ID 1, name "1__2" this might be different for you, if you don't hear sound you can try the other sink devices. ill note below where you need to specify chosen sink device id.
example for ID 1:
```
pactl -s tcp:127.0.0.1:4713 set-default-sink 1
pactl -s tcp:127.0.0.1:4713 set-sink-mute 1 0
pactl -s tcp:127.0.0.1:4713 set-sink-volume 1 0dB
```

In another terminal window, Start the SSH tunnel for VNC and a Reverse tunnel for pulse audio and let it run. In this example im using a SSH key to logon to the L1CC environment.
```
ssh -i ./sshkey.pem -o IdentitiesOnly=yes \
  -N \
  -L 5901:localhost:5901 \
  -R 4713:localhost:4713 \
  username@linuxguestIPaddress
```
On the RHEL Guest install pulse audio: 
```
sudo dnf install -y pulseaudio-utils
```
Export audio to port 4713 and the sink device to the one selected previously:

#or another sink device id
```
export PULSE_SERVER=tcp:localhost:4713
export SDL_AUDIODRIVER=pulse
export PULSE_SINK=1
```
run doom as per usual on the RHEL guest.
```
chocolate-doom -iwad DOOM.WAD
```
you should hear sound now, if not try another sink device from the output of: 
```
pactl -s tcp:127.0.0.1:4713 list short sinks
```
Note: If id 1 doesn't work then try the other devices for example 0 or 2:

#ID 0:
```
pactl -s tcp:127.0.0.1:4713 set-default-sink 0
pactl -s tcp:127.0.0.1:4713 set-sink-mute 0 0
pactl -s tcp:127.0.0.1:4713 set-sink-volume 0 0dB
```
#ID 2
```
pactl -s tcp:127.0.0.1:4713 set-default-sink 2
pactl -s tcp:127.0.0.1:4713 set-sink-mute 2 0
pactl -s tcp:127.0.0.1:4713 set-sink-volume 2 0dB
```



