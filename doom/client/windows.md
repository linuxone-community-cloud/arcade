Windows comes with a Remote Desktop Connection application, but unfortunately we can't use it here because the latest version prevents users from connecting to localhost, which is what we'll use with our SSH tunnel.

Instead, install a VNC Viewer of your choice. Options include RealVNC Viewer and Remote Ripple.

Once you have that installed open PowerShell and set up your SSH tunnel:

```
ssh -L 5901:localhost:5901 linux1@LINUXONE_IP_ADDRESS
```

You will have to keep this connection active by keeping your PowerShell window open during the duration of your game.

Open up your VNC client and connect to localhost:5901 (hostname "localhost" and port "5901" may be different fields, depending on your client).

If you have issues with unusual coloring, go into your VNC client options and adjust the quality settings, you may need to reduce or increase them.
