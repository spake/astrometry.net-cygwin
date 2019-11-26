Astrometry.net on Cygwin
========================

cygport file for astrometry.net, patched to point at Python 3.7 (as well as its dependency, astropy).

Building
--------
 - [Install Cygwin](https://cygwin.com/install.html).
 - In the Cygwin shell, install dependencies (substituting in the path to Cygwin's `setup-x86.exe` or `setup-x86_64.exe`):
```
/path/to/setup-x86.exe -P cygport -P gcc-core -P libcairo-devel -P libcfitsio-devel -P libjpeg-devel -P libnetpbm-devel -P libpng-devel -P make -P python37-devel -P python37-numpy -P python37-pip -P python37-wheel -P swig
```
 - Close and re-open the Cygwin shell (to ensure changes to your `$PATH` are reflected).
 - Download sources and build packages:
```
cygport python37-astropy.cygport download all
cygport astrometry.net.cygport download all
```

Installing (for testing)
------------------------
(See the [Cygwin Package Server docs](https://cygwin.com/package-server.html) for more info.)

 - Follow the build instructions above.
 - Install extra deps:
```
/path/to/setup-x86.exe -P calm
```
 - Create a package directory tree in `cygwin-packages` (setting `ARCH` to `x86_64` if applicable):
```
ARCH=x86
ARCH_DIR=cygwin-packages/$ARCH
mkdir -p $ARCH_DIR/release
cp -r */dist/{python37-astropy,astrometry.net} $ARCH_DIR/release/
mksetupini --arch $ARCH --inifile $ARCH_DIR/setup.ini --releasearea cygwin-packages --disable-check missing-depended-package,missing-required-package
xz -6e < $ARCH_DIR/setup.ini > $ARCH_DIR/setup.xz
bzip2 < $ARCH_DIR/setup.ini > $ARCH_DIR/setup.bz2
```
 - Install astrometry.net from the directory tree (making sure to also select a mirror when prompted, so dependencies can be downloaded too):
```
/path/to/setup-x86.exe -P astrometry.net -X -s file:///C:/path/to/cygwin-packages
```
