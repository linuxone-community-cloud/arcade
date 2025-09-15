These instructions have been tested with Ubuntu 22.04 on an IBM LinuxONE virtual server. Newer versions of Ubuntu likely work with similar instructions, but haven't been tested.

Install the TigerVNC server:

```
sudo apt install tigervnc-standalone-server
```

Install openbox as a simple window manager:

```
sudo apt install openbox
```

Install the chocolate-doom package from the Ubuntu repository:

```
sudo apt install chocolate-doom
```

Run the vncserver with openbox:

```
vncserver :1 -geometry 800x600 -depth 24 -xstartup openbox
```

Run chocolate-doom on the same display, it will default to loading up Freedoom:

```
export DISPLAY=:1
chocolate-doom
```

If you own Doom the game, you can optionally upload your WAD file of the game and run this command instead:

```
chocolate-doom -iwad DOOM.WAD
```

Now connect to it using a VNC client over the SSH tunnel you set up on your [desktop](../client/) of choice.
