The Full Instructions for Doom on a Red Hat Enterprise Linux as the server have been published [on LinkedIn](https://www.linkedin.com/pulse/how-run-doom-ibm-z-linuxone-nielson-nino-de-carvalho-0ukxf/) by Nielson 'Nino' de Carvalho.

Because we going to build and compile the doom source code and SDL(Simple DirectMedia Layer) libraries. we need install the required "development tools" and dependencies.

```
$ sudo dnf groupinstall "Development Tools"
$ sudo dnf install gcc make cmake libX11-devel libXext-devel pulseaudio-libs-devel freetype freetype-devel 
```

Next clone and build the required SDL2 libraries starting with the Core library.

```
$ mkdir -p ~/sdl2-libs && cd ~/sdl2-libs
$ git clone https://github.com/libsdl-org/SDL.git SDL2
$ cd SDL2
$ git checkout release-2.28.5
$ mkdir build && cd build
$ cmake .. -DCMAKE_INSTALL_PREFIX=/usr/local
$ make -j$(nproc)

# Here make sure the target is SDL2 not SDL3 

$ sudo make install
```
Clone and Build the SDL2 Audio mixer. This is required for Chocolate-Doom but sound will only work if you follow the optional sound guide for MacOS.

```
$ cd ~/sdl2-libs
$ git clone -b SDL2 https://github.com/libsdl-org/SDL_mixer.git
$ cd SDL_mixer
$ mkdir build && cd build

# here we disable opusfile, libxmp, fluidsynth, wavpackr, becaues they are not available in the RHEL 9.5 repositories, but Chocolate Doom only plays .WAV and .OGG.

$ cmake .. -DCMAKE_INSTALL_PREFIX=/usr/local \
  -DSDL2MIXER_OPUS=OFF \
  -DSDL2MIXER_MOD=OFF \
  -DSDL2MIXER_MIDI=OFF \
  -DSDL2MIXER_WAVPACK=OFF

$ make -j$(nproc)
$ sudo make install
```

SDL2 Network Library.

```
$ cd ~/sdl2-libs
$ git clone -b SDL2 https://github.com/libsdl-org/SDL_net.git
$ cd SDL_net
$ mkdir build && cd build
$ cmake .. -DCMAKE_INSTALL_PREFIX=/usr/local
$ make -j$(nproc)
$ sudo make install
```

SDL2 Image library.

```
$ cd ~/sdl2-libs
$ git clone -b SDL2 https://github.com/libsdl-org/SDL_image.git
$ cd SDL_image
$ mkdir build && cd build
$ cmake .. -DCMAKE_INSTALL_PREFIX=/usr/local
$ make -j$(nproc)
$ sudo make install
```

Final SDL2 Step , clone and build the SDL2 FreeType and Harfbuzz Library.

```
$ cd ~/sdl2-libs
$ git clone -b SDL2 https://github.com/libsdl-org/SDL_ttf.git
$ cd SDL_ttf
$ mkdir build && cd build
$ cmake .. -DCMAKE_INSTALL_PREFIX=/usr/local
$ make -j$(nproc)
$ sudo make install
```

Chocolate Doom:
With SDL libraries out the way, we move to clone and build chocolate doom. 

```
$ cd ~
$ git clone https://github.com/chocolate-doom/chocolate-doom.git
$ cd chocolate-doom
$ mkdir build && cd build
$ cmake .. -DCMAKE_INSTALL_PREFIX=/usr/local

# you will get some errors for libsamplerate fludsynth and SDL2_library , but none are specifically needed.

$ make -j$(nproc)
```

Now copy the DOOM.WAD file to a directory on this RHEL guest. I placed it in /chocolate-doom/build/src , where ever is it you can specify its location it the -iwad argument. MobaXterm has a build in SSH browser (SCP) side bar thats allows you to drag and drop files to the linux guest. 

On the RHEL guest , install VNC Server (TigerVNC) and its dependencies.

```
$ sudo dnf install tigervnc-server
```

Set the VNC password to be used later when connecting.

```
$ vncpasswd
```

Start the VNC server with the DISPLAY number mapped to :1
Here 320x200 is the original resolution but you can also try 800×600

```
$ vncserver :1 -geometry 320x200 -depth 24
```

From your local machine download and install RealVNC viewer. 

Setting up a SSH tunnel
On MobaXterm, open the MobaSSHTunnel tool and create a new SSH Tunnel using Local port forwarding. 
```
Your local Port: 5901 
The Remote Server: localhost ,Port: 5901
SSH Server: <IP address of the RHEL Guest> ,Port: 22
```

Running Doom
On the RHEL guest, export the DISPLAY to the VNC server session ID we started earlier and run chocolate-doom. 

```
$ export DISPLAY=:1
$ cd chocolate-doom/build/src/
$ ./chocolate-doom -iwad DOOM.WAD
```

Using VNC Viewer on your local machine connect to localhost:5901
And you should see doom running in VNC Viewer :)

Tips : You can change the keyboard mapping and Mouse sensitivity by running:

```
./chocolate-setup
```

In mouse configuration untick "allow vertical mouse movement" this will disable player moving forward and backwards using the mouse.

Set keyboard movement to WASD.

If your VNC colours are weird , try changing the VNC viewer colour option "ColorSpaceRGB" to Device. 

Good luck and Have fun!
