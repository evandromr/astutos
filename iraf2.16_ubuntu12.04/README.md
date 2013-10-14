# How to install IRAF 2.16 on Ubuntu 12.04 (LTS) [64bits]

A (perhaps) comprehensive guide to install IRAF on Ubuntu, without all the
Whibley-Wobbly of creating users, copy/paste unknow commands, and running third-
parties scripts.


## Step 1: Download IRAF and dependences

- Go to the [IRAF website](http://iraf.noao.edu/) and download the linux 64bit
binary distribution.

- The installation process have some dependences, I believe that the following packages is enough to satisfy all of them
  - A terminal handling library: 
      - `sudo apt-get install libncures5-dev`
  - Development libraries of the X server
      - `sudo apt-get install xserver-xorg-dev-lts-raring`
      - `sudo apt-get install xorg-dev`
      - Be carefull with the last one, do not let it remove any package.
  - A csh shell (see explanation on Step 2)
      - `sudo apt-get install tcsh`
  - 32bits libraries (see explanation on Step 4)
      - `sudo apt-get install ia32-libs`


##Step 2: Installing IRAF

### Skippable Explanation (slightly technical details)

Due to how IRAF was built, it is better to use the `tcsh` shell to deal with
installations and upgrades of IRAF.
Ubuntu (and all major linux distributions that I know of) uses `bash` insead.
That seems to be the main reason that the majority of tutorials about IRAF and
Ubuntu makes you createa a new user with the `tcsh` as the default shell.

Here, we are just call the `tcsh` shell inside a normal gnome-terminal session,
but for that you will have to make sure that you have `tcsh` installed.

The steps below will follow the instructions found on the IRAF [README/Install
Guide](ftp://iraf.noao.edu/iraf/v216/README). If you feel unconfortable of just
following instructions (wich you should), I recommend have a look at it.

### What you should do:

 - Create your iraf directory somewhere
 (in my case it is /home/username/Work/Softwares/iraf)

`cd ~/Work/Softwares`  
`mkdir iraf`

  - Create another iraf directory inside the last one
 (that means, /home/username/Work/Softwares/iraf/iraf)

`mkdir iraf/iraf`  

 - Copy the downladed file to the second iraf directory  

`cd iraf/iraf`  

`cp ~/Downloads/iraf.lnux.x86_64.tar.gz .`  
 __Note__: The '.' at the end of the copy command means "HERE"


 - Decompress the archive


`tar zxvf iraf.lnux.x86_64.tar.gz`  

 - Start the `tcsh` shell as 'sudo'.

`sudo tcsh`

- Inside the shell do:
     - creates an $iraf environment variable poin to the iraf/iraf/ folder
(yes, with a trailing "/")
     - run the $iraf/unix/hlib/install script


__Note:__ Since you are at the iraf/iraf directory, you can use the $PWD
variable, it means "The path to where I am". Also, notice the '/' after $PWD,
it is necessary for the iraf installation script.

`setenv iraf $PWD/`  
`$iraf/unix/hlib/install`  

__Note:__ The installations script are gonna ask you where to install stuff and
what else you wanto to configure at the post-installation. I will assume thant
you can decide that for yourself.

 - logout of the `tcsh` shell

`exit`  

### That should be it, but...

The graphical routines of IRAF requires a kind of terminal that supports them,
and the common gnome-terminal on Ubuntu doesn't. Because of that we have to install X11IRAF.


## Step 3: Downloading X11IRAF

- Go to the [X11IRAF ftp area](http://iraf.noao.edu/projects/x11iraf/) and
download the linux binary distribution (I recommend the v2.0BETA version
[x11iraf-v2.0BETA-
bin.linux.tar.gz](http://iraf.noao.edu/projects/x11iraf/x11iraf-v2.0BETA-
bin.linux.tar.gz)).

## Step 4: Installing X11IRAF binary distribution

### Skippable explanation

To use the graphical routines of IRAF we will use the `xgterm` terminal, wich is
part of the X11IRAF. For it work properly we need to install some 32-bits
library wrappers in Ubuntu, those libraries can be found as part of the package
"`ia32-libs`".


The following instructions were taken from the X11IRAF Readme.install file, wich
comes with the installation packges, you should read them too.

### What you should do

- Create a folder called x11iraf and copy the Download to it

`#go to the iraf directory`  
`cd ..`  

`#create x11iraf dir inside iraf/`  
`mkdir x11iraf`  
`cd x11iraf`  

`#copy the downloaded package to the x11iraf dir`  
`cp ~/Downloads/x11iraf-v2.0BETA-bin.linux.tar.gz .`  

- Extract the archive  

`tar zxvf x11iraf-v2.0BETA-bin.linux.tar.gz`  

 - Follow the installation instructions:  

`sudo cp bin.linux/* /usr/local/bin/               # for the binaries`  
`sudo cp lib.linux/* /usr/local/lib/               # for the CDL library`  
`sudo cp include/* /usr/local/include/             # for CDL include files`  
`sudo cp app-defaults/* /usr/lib/X11/app-defaults/ # app resource defaults`  
`sudo cp man/* /usr/man/man1/                      # man pages`  

__Note:__ I noticed that there wasn't a `/usr/lib/X11/app-defaults` in my system
and the same thing for the `usr/man/man1/`, if that is your case you can create
those folders with the commands below, and retry to copy the files.

`sudo mkdir /usr/lib/X11/app-defaults`  
`sudo mkdir /usr/man/man1`  

__Note 2:__ I also noticed that the there is a `/etc/X11/app-defaults/` folder.
So, just to be sure, I copied the app-defaults files to this folder too, with:

`sudo cp app-defaults/* /etc/X11/app-defaults/`  

### What now?

If everything worked as expected, you are ready to start using IRAF, but...


# Advices

### Calling xgterm on the background

To sucessfully start and use a IRAF session you are gonna need to use the
`xgterm` terminal, wich we just installed with the x11iraf package.

For that you just need to call `xgterm` from the bash. If you want to still be
able to use your bash session while using xgterm, you can run it as a background
process with:

`xgterm &`

__Note:__ If your terminal do not find the `xgterm` command, try to close the
terminal window and open it again, to properly recognize the files on the system
`PATH`.

### Initializing IRAF and start a session

IRAF needs a place to work, for general purpose tasks I recommend to use the
`iraf/` folder that we created at the start of this tutorial (in my example
`/home/username/Work/Softwares/iraf/`). And you still have to initialize this
work place. To do that, using `xgterm`:

 - go your working folder
   - `cd ~/Work/Softwares/iraf/`
 - run mkiraf in this folder
   - `mkiraf`
   - This will create a `login.cl` file and a `uparm` directory
   - If asked for a type of terminal, choose `xgterm`
 - start an iraf session in this directory
   - `cl`
   - Or, for __e__nhanced capabilities, `ecl`

### Open image tools

When dealing with images, remember to have a `ds9` or `ximtool` windows opened.
to open a window inside the iraf environment do:

!ds9 &

or

!ximtool &

__Note:__ The "!" symbols runs any following command in the shell, instead of
inside IRAF. This is useful.

## A small but useful script

Copy the following code to a file. You can do it in any editor of your choice, 
but be sure to open it with administrative permissions (use `sudo`).

```
#!/bin/bash
PID=`pidof ds9`
if [ ! $PID ]; then
   ds9 &
fi
pushd ~/iraf > /dev/null
xgterm -geometry 80x43 -sb -title "IRAF" -bg "lemon chiffon" -fg "black" -e "ecl" &
popd > /dev/null
```

Save in `/usr/local/bin/irafshell`.

Now, type in your terminal. 

```
sudo chmod +x /usr/local/bin/pyrafshell
```

This will turn the script executable and you can type `irafshell` in your terminal to open 
`iraf` and `DS9`.

---
