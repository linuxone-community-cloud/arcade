Install TigerVNC on the Ubuntu desktop side as well, this time all you need is the viewer:

```
sudo apt install tigervnc-viewer
```

Set up your ssh terminal:

```
 ssh -L 5901:localhost:5901 linux1@LINUXONE_IP_ADDRESS
```

Now load up TigerVNC Viewer and connect to localhost:5901

If you have issues with unusual coloring, go into your VNC client options and adjust the quality settings, you may need to reduce or increase them.
