Using Lua on Linux
================================================================================

This guide will explain the basics of using Lua on common GNU/Linux
distributions like Ubuntu or Linux Mint.

Compiling from Source
----------------------------------------

If you want to compile and install Lua from source code, you will need the
following:

- libreadline with all its development files. On ubuntu, these can be installed
  with `sudo apt install libreadline-dev`.
- make and a C compiler. These can be easily installed on ubuntu with `sudo apt
  install build-essential`

First, download the source files from [lua.org](http://lua.org/download.html).
Once you have downloaded the source code, packaged as a `.tar.gz` file,
unpack it all into a new directory, for example `$HOME/workspace/` (where $HOME
stands for your users home-directory).

```bash
# Create a "workspace" directory in your home directory.
# You can call this anything else if you want.
mkdir $HOME/workspace
cd $HOME/workspace

# Use wget to download the latest (at the time of writing this) version of Lua.
wget https://www.lua.org/ftp/lua-5.3.5.tar.gz

# Unpack the downloaded file into the current directory.
tar zxf lua-5.3.5.tar.gz
cd lua-5.3.5

# Compile Lua
make linux

# Install Lua system-wide
sudo make install

# See if everything worked
lua -v
luac -v
```

After this, Lua should be installed on your system.
