# How to install IRAF 2.16 on Ubuntu 14.04 (LTS) [64bits]

A (perhaps) comprehensive guide to install IRAF on Ubuntu, without all the
Whibley-Wobbly of creating users, copy/paste unknow commands, and running third-
parties scripts.

## Step 1: Download IRAF and dependences

- Go to the [IRAF website](http://iraf.noao.edu/) and download the linux 64bit
binary distribution.

- The installation process have some dependences, I believe that the
following packages is enough to satisfy all of them
  - A terminal handling library: 
      - `sudo apt-get install libncures5-dev`

If after installation anything related to graphical interface breaks try
installing the following 'X' development libraries and restart the process

  - Development libraries of the X server
      - `sudo apt-get install xserver-xorg-dev-lts-raring`
      - `sudo apt-get install xorg-dev`
      - Be carefull with the last one, do not let it remove any package.

##Step 2: Installing IRAF

### Skippable Explanation (slightly technical details)

Install IRAF used to be a hard task, but since the 2.16.1 version (that is,
version 2.16 with patch 1), the process is much easier.

This version comes with (the once problematic) xgterm and its depences and
creates a global login.cl file and uparm directory, so you don't need to always
work on the same directory and/or creates a login.cl every time.

This tutorial is to highlight one small point missing from the IRAF installation
(i.e. moving the x11iraf stuff to the .iraf/bin path)
and one trick to make PyRAF use the same login.cl and uparm directory.

The steps below will follow the instructions found on the IRAF [README/Install
Guide](ftp://iraf.noao.edu/iraf/v216/PCIX/README.install).

### What you should do:

 - Create your iraf installation directory somewhere
 (in my case it is /home/username/softwares/iraf)
 
   `mkdir ~/softwares/iraf`

 - Copy the downladed file to that directory  

   `cd iraf`  

   `cp ~/Downloads/iraf.lnux.x86_64.tar.gz .`  

   __Note__: The '.' at the end of the copy command means "HERE"


 - Decompress the archive

   `tar -zxvf iraf.lnux.x86_64.tar.gz`  

 - Run the install script

   `./install`

__Note:__ The installations script are gonna ask you where do you want your
images and cache to be stored, just hit <Enter> to accept the default locations.

The script will add a few lines to your ~/.bashrc file, so you don't have to
manually define environment variables like $IRAFARCH and $iraf. Your original
~/.bashrc will be saved as ~/.bashrc.bak with you want to restore it.

### That should be it, but...

You can invoke iraf with the command `cl` in your terminal but the
gnome-terminal (default terminal in ubuntu) wont hadle the iraf graphics.

If you type `iraf` you will get an error saying that the system couldn't find
the command `xgterm`. Let's fix that...

copyt the contents of the folder <iraf>/vendor/x11iraf/bin.linux to the created
location for iraf binaries, the default location is `~/.iraf/bin/`
if you choose another location during installation be sure to use it in the
command bellow, and the same thing if your iraf installation is not in
`~/softwares/iraf/`:

   `cp ~/softwares/iraf/vendor/x11iraf/bin.linux/* ~/.iraf/bin/`

Now just open a new terminal window (to refresh the PATH of your system) and
type `iraf`. This will open a xgterm window runing IRAF.

## Step 3: Make Pyraf use the global login.cl

Pyraf is a Python interpreter that substitute the classical cl shell of iraf.
You can find more information in [their page](http://www.stsci.edu/institute/software_hardware/pyraf)

And, really, you should give it a try.

you can install pyraf with pip using `pip install pyraf` or even the whole
stsci_python bundle using `pip install stscipython`.

Pyraf will look for the login.cl file in the working directory or in `~/iraf`

I hope the future versions of Pyraf will fix that to also look at `~/.iraf`.
For now, all you have to do is make a symbolic link to the correct place wher
the global login.cl is.

   `ln -s ~/.iraf ~/iraf`

That's it, this will suffice to work with pyraf from any directory. It will use
the login.cl file in the working directory if there is one, and the global one
at ~/.iraf/ if not.

## Bonus Track: PyRAF plot with matplotlib backend

Pyraf has the abbility to use matplotlib as a plot device, wich allow to better
resolution graphics. To allow this put the following line in you ~/.bashrc
file:

   `export PYRAFGRAPHICS=matplotlib`

  
The End
---
