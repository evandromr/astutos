How to install HEASOFT
===

This a simple guide with basic steps, the links and the details of the
installation can change with time, make sure to always check the [HEASOFT
website](http://heasarc.nasa.gov/lheasoft/) in case you run into errors.

This tutorial was last updated on __Sep. 15, 2015__

- __Step 1:__ Download HEASOFT  
  [CLICK HERE](http://heasarc.gsfc.nasa.gov/FTP/software/lheasoft/release/heasoft-6.21src.tar.gz)
  to download the full source code of HEASOFT.  
  You can also download binary distribution for your operating system,
  but the source code installation is recommended to avoid
  incompatibilities.  
  You can copy/paste the following commands, it will create a folder xray in your home directory and download the file there.  

      mkdir ~/xray
      cd ~/xray
      wget -c http://heasarc.gsfc.nasa.gov/FTP/software/lheasoft/release/heasoft-6.21src.tar.gz

- __Step 2:__ Unpack the downloaded file  
  Unpack the \*.tar.gz file in your installation directory. If you used the commands on Step 1 you can unpack with the following command:

      tar -zxvf heasoft-6.21src.tar.gz

  This will create a folder named `heasoft-6.21` in your directory.

- __Step 3:__ Run the configuration file  
  Run the `configure` file in the `BUILD_DIR` directory

      cd ~/xray/heasoft-6.21/BUILD_DIR
      ./configure

  Check the output of the command for warnings and errors. Usually you can safe ignore warnings, but is good to be aware of them. __This can take a while__.

- __Step 4:__ Compile and install the software
  Run the make file inside the `BUILD_DIR` directory

      make
      make install

  The first `make` command __might take a while__.

- __Final Step:__ Load the software and create an alias
  After _Step 4_ you should have a folder with a complicated name inside the folder `heasoft-6.21`, that folder reflects your operating system distribution. In my installation it is called `x86_64-unknown-linux-gnu-libc2.12` but here I'll call it `(PLATFORM)`.

  To load and HEASOFT and use all the tools you have to run a file called `headas-init` inside the `(PLATFORM)` folder.

  The instructions are different if your terminal uses C-shell (csh, or tcsh) or Bourne-Shell (bash)

  - __For users of C Shell variants (csh, tcsh):__

          setenv HEADAS $HOME/xray/heasoft-6.21/(PLATFORM)
          source $HEADAS/headas-init.csh

  - __For users of Bourne Shell (sh, ash, ksh, and bash):__

          export HEADAS=$HOME/xray/heasoft-6.17/(PLATFORM)
          . $HEADAS/headas-init.sh

  Instead of doing this all the time you can create an _alias_ (a shortcut) to load  the software with one single command. Again, the instructions are different according to the terminal and you have to add this instructions to your terminal configuration file (`.profile`, `.cshrc`, or `.bashrc`)

  - __For users of C Shell variants (csh, tcsh):__

          # Write this into .cshrc
          setenv HEADAS $HOME/xray/heasoft-6.21/(PLATFORM)
          alias heainit "source $HEADAS/headas-init.csh"

  - __For users of Bourne Shell (sh, ash, ksh, and bash):__

          # Write this into .bashrc
          HEADAS=$HOME/xray/heasoft-6.21/(PLATFORM)
          export HEADAS
          alias heainit=". $HEADAS/headas-init.sh"

  Then you can only run the command `heainit` before using `Xspec`, `powspec`, `fv`, or any other tool from `HEASOFT`



  ## Issues and Patches

  Current Issues of HEASOFT are listed in the *Known Issues* page:
  https://heasarc.gsfc.nasa.gov/lheasoft/issues.html

  However, most of the independent packages of HEASOFT track their
  issues separately. This list of packages is shown in the *Known Issues* page:

    - Swift
    - CALDB
    - XSPEC
    - XSTAR
    - XIMAGE
    - XRONOS


   ### Applying XSPEC patches

  Xspec is one of the main tools of HEASOFT, its known issues are listed in
  https://heasarc.gsfc.nasa.gov/xanadu/xspec/bugs.html and fixes for the issues
  available in the form of code patches.

  To apply those patches Download both the most recent patch and the patch
  installation tool (a script that will install the patch). Recent Xspec patches
  incorporate previous changes so you only have to download the latest patch
   available.

  ```sh
  # the patch itself
  wget -c https://heasarc.gsfc.nasa.gov/docs/xanadu/xspec/issues/Xspatch_120901i.tar.gz  
  # the installation script
  wget -c https://heasarc.gsfc.nasa.gov/docs/xanadu/xspec/issues/patch_install_4.8.tcl
  ```

  To apply the patches, copy both these files to the Xspec source code directory
  usually found at `<heasoft installation fonder>/Xspec/src`.
  In this example it would be: `~/xray/heasoft6.21/Xspec/src`

  ```sh
      cp Xspatch_120901i.tar.gz patch_install_4.8.tcl ~/xray/heasoft6.21/Xspec/src
  ```

  To run the installation script make sure to have your HEASOFT environment
  initialized. Following this example this would be done by runing the command
  `heainit`.

  And then finally run the script with tclsh:

  ```sh
      cd ~/xray/heasoft6.21/Xspec/src
      tclsh patch_install_4.8.tcl
  ```
