
Using Lua on Windows
================================================================================

This guide will explain the basics of using Lua on Windows.

Compiling from Source using MinGW
----------------------------------------

### Install MinGW

**The latest version of MinGW needs to be installed on the system**

MinGW is a project that brings many of the developer tools from linux to
windows, including tools like `make` and `gcc`.

To get started, download and install the latest version of
[MinGW](https://osdn.net/projects/mingw/releases/).
More precisely, you will need `mingw-get`,
which is a tool to install various mingw packages.

Take note of the installation directory;
after the installation is done, this should contain a few folders
like `lib` and `include`, but most importantly, `bin`.

In the MinGW Installation Manager, install at least the packages `mingw32-base`
and `mingw-developer-toolkit`.
<!-- TODO: confirm whether the dev toolkit this is actually necessary) -->

### Download the Lua sources

**The source code of Lua needs to be downloaded from lua.org**

Next, download the source files from [lua.org](http://lua.org/download.html).
Once you have downloaded the source code, packaged as a `.tar.gz` file,
unpack it all into a new folder, for example `%HOME%/workspace/` (where
%HOME% stands for your users home-folder).

By default, windows has no program to extract `.tar.gz` files,
you could use a graphical tool like 7Zip,
or the linux tool `tar`, installed via MinGW.

The `.tar.gz` archive *should* contain a single folder,
named `lua-<version>` (Where \<version\> is Luas version number).
After extracting it, open this folder on the command line,
either by opening a new command-line window and navigating to the folder,
or from the windows explorer.

<details>
<summary><i>How do I do that?</i></summary>

To open a directory in a commandline window:

- Holding shift, right-click the folder you want to open
- Click on *Open command window here* or *Open PowerShell window here*
- A commandline window should now appear running either `cmd` or powershell (`ps`)
- If you clicked on the *PowerShell* option, type `cmd` and press enter

</details>

### Build Lua

**`mingw32-make` needs to be called in the project directory**

Make sure you're in the right directory by typing `dir` and looking for the
`src` and `doc` folders, as well as the `Makefile` and `README` files.

Temporarily add MinGWs bin folder to your path variable,
if you haven't already configured this for your user account:

In the commandline window, type `set PATH=C:\MinGW\bin:%PATH%`
assuming you installed MinGW under `C:\MinGW` (see installation step).
Otherwise, replace `C:\MinGW` your chosen MinGW installation directory.

Now you should be able to compile Lua by typing `mingw32-make mingw`.

If this step completes without errors, the following new files should have been
created in the `src` folder:

<details>
<summary><code>lua53.dll</code></summary>
This is really the main "Lua" library, which contains all that's needed for
another program to run Lua code.
</details>

<details>
<summary><code>lua.exe</code></summary>
This program uses the <code>lua53.dll</code> to load and run Lua code.
It is usually the main way for users to run Lua scripts.
</details>

<details>
<summary><code>luac.exe</code></summary>
This program compiles Lua source code to Lua byte code.
This is different from compiling, for example, a C program to machine code,
in that you still need the Lua interpreter to run the program,
but it should load considerably faster, as it is already converted to a binary
format that the Lua VM can directly interpret.
</details>

### Installing

**the make task "install" needs to be called**

To install Lua on your system,
after running `make` to compile the files,
run `mingw32-make install INSTALL_TOP='C:\Lua'`.

By replacing "C:\Lua" with any other directory,
you can change where the files will be installed to,
for example, you can install Lua to `'C:\Program Files\Lua'`.

### Adding Lua to the PATH variable

**The `bin` subdirectory needs to be added to the PATH environment variable**

If you want to call Lua on the command line without having to type the full
path, you will have to add its `bin` directory to your PATH variable.

To do this open the start menu by pressing the windows key and type "environment
variables".
To add the path for all users,
choose the option to modify system environment variables.
If you want this just for yourself,
choose the option to modify environment variables for your account.

In the list, find and double-click the variable called `PATH`.

In windows 10, you should get a simple UI with a table layout.
Click new and type (or copy) the path to your Lua installation directory,
followed by `\bin`. For example, this could be `C:\Lua\bin`.

When using windows 7, the UI looks a mit different,
and you will only get a big text-box.
Scroll to the beginning of the text box
(you can just hold the left arrow for this)
and type in the path to the `bin` directory as described above.
Then add a `;` at the end to separate it from the next path.

Installing Luarocks as a standalone executable
----------------------------------------

If you're installing Lua, chances are, you will want to install additional
functionality from the internet. Currently, the main way of distributing Lua
modules easily is using Luarocks.

The easiest way to install luarocks on Windows is to get the standalone
executables. These are just two `.exe` files that contain everything you need to
run luarocks.

- Open `https://luarocks.org` in your browser
- Click "install" on the navigation bar at the top
- Choose one of the two "all-in-one executable" options
- Click the link and download the file
- Unzip the two executables into a directory of your choosing

The two executables can be saved in any directory you want,
but it would make sense to place them either in Luas `bin` directory
or give them their own subdirectory in `C:\Program Files`

Installing Luarocks from Source
----------------------------------------

For a variety of reasons,
you may want to compile LuaRocks from source.
To do this, follow the following steps:

- Open github.com/luarocks/luarocks
- Click the *clone or download* button
- Click Download ZIP and download the file
- Unpack the ZIP file wherever you like
- Within the folder you just unpacked, open a new commandline
- Run the command `install.bat /LUA C:\Lua`

Where `C:\Lua` should be replaced with Luas install directory.

Installing LuaSocket and LuaSec via Luarocks
----------------------------------------

LuaSocket is a very common Lua module that allows communication over the
network (LAN networks or the Internet) using a set of common protocols like TCP,
UDP or HTTP.

If you already have MinGW installed,
you should be able to just install LuaSocket by running

`luarocks install luasocket`

on the commandline.

To use secure protocols like HTTPS, you additionally need the LuaSec module.

For this, you first need to install the OpenSSL library,
which is originally a linux library that implements several cryptographic
algorithms and protocols.

Most sources recommend the *Win32/Win64 OpenSSL installer for windows* from the
*Shining Lights Productions* website.
During the installation, take note of the installation directory.

After openssl has been successfulyl installed,
run `luarocks install luasec OPENSSL_INCDIR='C:\path\to\openssl\include'`,
where "C:\path\to\openssl" should be replaced with OpenSSL install directory.
