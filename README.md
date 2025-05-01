# Build an IBM LinuxONE Arcade

Want to try your hand at some classic style arcade games while also validating
that the tools you're used to are available on IBM LinuxONE? Read on!

## Red Hat Enterprise Linux

### Moon Buggy

Install the Extra Packages for Enterprise Linux (EPEL) repository and then install moon-buggy.

```
sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
sudo dnf install moon-buggy
```

And you're ready to roll! To give it a spin, just run:

```moon-buggy```

### Pong-CLI

Install a Rust game via Cargo.

```
sudo dnf install cargo
cargo install pong-cli
export PATH=$PATH:~/.cargo/bin/
```

Ready to play Pong? Run:

```pong-cli```

### Tetris

Build a game written in C from source with your standard build tools.

```
sudo dnf install make gcc
curl -LO https://github.com/vicgeralds/vitetris/archive/refs/tags/v0.59.1.tar.gz
tar xvf v0.59.1.tar.gz
cd vitetris-0.59.1/
./configure 
make
```

Want to start fitting some blocks together? Start your game with:

```./tetris```
