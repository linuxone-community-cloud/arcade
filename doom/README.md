Run [chocolate-doom](https://www.chocolate-doom.org/), described on their website as *"Doom source port that accurately reproduces the experience of Doom as it was played in the 1990s"*, with [Freedoom](https://freedoom.github.io/) on an IBM LinuxONE virtual machine.

In most configurations, this provides graphics only, no sound.

To set this up, you need an IBM LinuxONE virtual machine (most of these instructions assume you're using the [IBM LinuxONE Community Cloud](https://community.ibm.com/zsystems/l1cc/)) where you will set up and run:

 - TigerVNC server with a basic X window environment
 - chocolate-doom

We have instructions for:

 - [Red Hat Enterprise Linux 9](server/rhel9.md)
 - [Ubuntu 22.04](server/ubuntu2204.md)

This will get Freedoom running on the server.

To connect to it and play, you will need to configure an SSH tunnel and a VNC client on your desktop/laptop.

We have instructions for the following desktop environments:

 - [MacOS](client/macos.md)
 - [Ubuntu](client/ubuntu.md)
 - [Windows 11](client/windows.md)

*Original instructions that inspired this tutorial were published by Nielson 'Nino' de Carvalho [on LinkedIn](https://www.linkedin.com/pulse/how-run-doom-ibm-z-linuxone-nielson-nino-de-carvalho-0ukxf/) in July 2025.*
