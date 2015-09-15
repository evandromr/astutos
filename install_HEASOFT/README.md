How to install HEASOFT
===

This a simple guide with basic steps, the links and the details of the
installation can change with time, make sure to always check the [HEASOFT
website](http://heasarc.nasa.gov/lheasoft/) in case you run into errors.

This tutorial was last updated on __Sep. 15, 2015__

- __Step 1:__ Download HEASOFT  
  [CLICK HERE](http://heasarc.gsfc.nasa.gov/FTP/software/lheasoft/release/heasoft-6.17src.tar.gz)
  to download the full source code of HEASOFT.  
  You can also download binary distribution for your operating system,
  but the source code installation is recommended to avoid
  incompatibilities.  
  You can copy/paste the following commands, it will create a folder x-ray_softwares in your home directory and download the file there.  

      mkdir ~/x-ray_softwares
      cd ~/x-ray_softwares
      wget -c http://heasarc.gsfc.nasa.gov/FTP/software/lheasoft/release/heasoft-6.17src.tar.gz

- __Step 2:__ Unpack the downloaded file  
  Unpack the \*.tar.gz file in your installation directory. If you used the commands on Step 1 you can unpack with the following command:

      tar -zxvf heasoft-6.17src.tar.gz

  This will create a folder named `heasoft-6.17` in your directory.

- __Step 3:__ Run the configuration file  
  Run the `configure` file in the `BUILD_DIR` directory

      cd ~/x-ray_softwares/heasoft-6.17/BUILD_DIR
      ./configure

  Check the output of the command for warnings and errors. Usually you can safe ignore warnings, but is good to be aware of them. __This can take a while__.

- __Step 4:__ Compile and install the software
  Run the make file inside the `BUILD_DIR` directory

      make
      make install

  The first `make` command __might take a while__.

- __Final Step:__ Load the software and create an alias
  After _Step 4_ you should have a folder with a complicated name inside the folder `heasoft-6.17`, that folder reflects your operating system distribution. In my installation it is called `x86_64-unknown-linux-gnu-libc2.21-0` but here I'll call it `(PLATFORM)`.

  To load and HEASOFT and use all the tools you have to run a file called `headas-init` inside the `(PLATFORM)` folder.

  The instructions are different if your terminal uses C-shell (csh, or tcsh) or Bourne-Shell (bash)

  - __For users of C Shell variants (csh, tcsh):__

          setenv HEADAS /path/to/your/installed/heasoft-6.17/(PLATFORM)
          source $HEADAS/headas-init.csh

  - __For users of Bourne Shell (sh, ash, ksh, and bash):__

          export HEADAS=/path/to/your/installed/heasoft-6.17/(PLATFORM)
          . $HEADAS/headas-init.sh

  Instead of doing this all the time you can create an _alias_ (a shortcut) to load  the software with one single command. Again, the instructions are different according to the terminal.

  - __For users of C Shell variants (csh, tcsh):__

          setenv HEADAS /path/to/your/installed/heasoft-6.17/(PLATFORM)
          alias heainit "source $HEADAS/headas-init.csh"

  - __For users of Bourne Shell (sh, ash, ksh, and bash):__

          HEADAS=/path/to/your/installed/heasoft-6.17/(PLATFORM)
          export HEADAS
          alias heainit=". $HEADAS/headas-init.sh"

  Then you can only run the command `heainit` before using `Xspec`, `powspec`, `fv`, or any other tool from `HEASOFT`

  :)
