# Setting up Python environments for My needs

3 different ways to do it without root privileges (read: do not mess with the
system) in Ubuntu 13.10

## 1- With Ureka

Download the [Ureka installer](http://ssb.stsci.edu/ureka/)

---

As their description says:

_"Ureka is a collection of useful astronomy software that is generally centered
around Python and IRAF. The software provides everything you need to run the
data reduction packages provided by STScI and Gemini. System privileges are not
normally required to install Ureka, and there are minimal operating system
requirements. Ureka is a binary distibution for Macs and Linux."_

and that means that beside a python installation with some handy packages (like
`numpy`, `pyds9`, `astropy`, `pyraf`, etc.) it comes with some astronomy
softwares (like `iraf`, `ds9`, `xgterm`, `ximtool`, etc.).

This is the best option if you are going to work with python and those other
softwares, and don't need python3 support (see comments below).

Ureka 1.0 comes with python 2.7.5, and is not compatible with python-3 yet.
Also, `IRAF` and some other softwares requires 32-bit libraries that doesn't
come installed as default in 64-bit distributions of Ubuntu.
Have a look at their notes about [32-bit support on 64-bit
Linux](http://ssb.stsci.edu/ureka/1.0/docs/32bit_iraf.html)

---

Put the installer in your place of choice to install it, run:

    bash install_ureka_1.0

__Note:__ You will need an internet connection and some coffee.

When finished, close and open your terminal again.

You can start the Ureka environment with:

    ur_setup

And deactivate it with:

    ur_forget

And you can change this aliases in your `~/.bashrc` file (and/or put then in
`~/.bash_aliases`).

Ureka comes with `setuptools` and `pip` already included.  

To install/upgrade python packages, just use `pip` (with `ur_setup` __active__):

    pip install package
    pip install --upgrade package

---

## 2 - With Anaconda

Download and install [Anaconda](https://store.continuum.io/cshop/anaconda/#)

---

According to their description, Anaconda is a

   "Completely free enterprise-ready Python distribution for large-scale data
processing, predictive analytics, and scientific computing".

If you need an environment with enphasys on velocity and easy of use thi is your
choice, but it is a little tricky to make it work with some external softwares.

---

Update `conda` and python (python from 2.7.5 to 2.7.6):

    conda update conda

Install Accelerate and IOpro (if you got an academic license, or bought a
license)

    conda install accelerate iopro

To install/update packages using conda:

    conda install package
    conda update package

__Tip:__ Do not update the _`anaconda`_ package, or it will downgrade python to
2.7.5. I don't know why.

### To create a Python-3.3 environment:

    conda create -n py3conda python=3.3 anaconda

And to run it (I gave it an alias on my _`bash_aliases`_ file, see [my
configuration files](https://github.com/evandromr/configuration_files)):

    source activate py3conda

To deactivate:

    deactivate

Update the python version and install accelerate (IOpro doesn't support
python3):

    conda update -n py3conda python
    conda install -n py3conda accelerate

#### Important:
To install/update the packages on the environment, use conda __outside__ the
environment (with it deactivated):

    conda install -n py3conda package
    conda update -n py3conda package

Or use `pip` instead of conda, then you can use it __inside__ the environment:

    pip install package
    pip install --upgrade package

---

## 3 - With Python only

---

Well, Python is Python.

This is the best choice if you want full control of your environment and
libraries versions, it is not so optimized as Anaconda nor easily integrated
with other astronomy softwares as Ureka, but you will always know which version
of python and its libraries you are using.

---

Download the latest version of [Python](www.python.org), and install it locally
like this:

Create a local installation in a folder with wright permission

    mkdir ~/MyPython
    mkdir ~/MyPython/Python-X.x.x

Extract the installer

    tar -xvf Python-X.x.x
    cd Python-X.x.x

Install at the local path

    ./configure --prefix=/home/<user>/MyPython/Python-X.x.x
    make
    make install

---

### To create a Python-3.3 environment

If you just installed python-3.3 or later, it comes with a virtual environment
manager called pyvenv

Setting up python environment with `pyvenv`:

    /home/MyPython/Python-3.3.x/bin/pyvenv ~/MyPython/python3env

To run it you must use  (or create an alias to do it, see the file
_`bash_aliases`_ in [my configuration
files](https://github.com/evandromr/configuration_files))

    source ~/MyPython/python3env/bin/activate

To install pip on it (or in the main python installation)

    wget https://bitbucket.org/pypa/setuptools/raw/bootstrap/ez_setup.py -O - | python
    easy_install pip

From then on, install or upgrade python packages with

    pip install package
    pip install --upgrade package

---
