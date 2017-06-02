# file: mouse-imaging-centre.md

## Description

Beginnings of an ansible role to install 

## Resources

* [  Generating deformation fields  ]( https://wiki.mouseimaging.ca/display/MICePub/Generating+deformation+fields#Generatingdeformationfields-Software )

## Notes:

### Vagrant testing

you will need a very large vm for installation testing. Currently using the following from geerlingguy with some extra ram.

```shell
# cd to role/repository directory then:
mkdir -p .vagrant/synced
vagrant up
vagrant ssh
```

## Requirements

### Testing requirements

* Virtualbox
* Vagrant

### Build requirements

```shell
# ??? sudo apt-get install ubuntu-desktop
```

### Build tools

```shell
sudo apt-get update
sudo apt-get install -y build-essential gfortran automake libtool cmake
```

### Opengl

```shell
sudo apt-get install -y freeglut3 freeglut3-dev
sudo apt-get install -y libxmu-dev libxi-dev
sudo apt-get install -y mesa-utils
```

### Freeglut

#### References

* []( http://freeglut.sourceforge.net/progress.php )

* []( http://prdownloads.sourceforge.net/freeglut/freeglut-3.0.0.tar.gz?download )

* []( https://en.wikipedia.org/wiki/FreeGLUT )

#### Testing

Test using glxinfo and glxgears

Connect using X forwarding

```shell
exit
vagrant ssh -- -X
```

then test

```shell
glxgears
glxinfo
```

## Display



## Register



### MINC libs

The MINC libraries need to be installed as well. These can be found on GitHub:

* [ rdvincent - BIC-MNI/libminc ]( https://github.com/BIC-MNI/libminc )

#### minc-toolkit-v2

Recommended MINC tools (this will install the library mentioned above as well):

* [](https://github.com/BIC-MNI/minc-toolkit-v2

  ​

OR

#### minc-toolkit (version 1)

* []( https://github.com/BIC-MNI/minc-toolkit )

Version 2 of the minc-toolkit, uses tools based on ITK version 4.x

##### Requirements

Installing from github, need CMake >= 3.1

###### Default cmake Ubuntu 16.04 version

```shell
cmake --version
```

Output example:

```shell
cmake version 3.5.1

CMake suite maintained and supported by Kitware (kitware.com/cmake).
```

###### git hub installation

Installing from github, need CMake >= 3.1

#### requirements

```shell
sudo apt-get install -y git
```

#### clone and build

```shell
mkdir ~/src
cd ~/src
git clone --recursive https://github.com/BIC-MNI/minc-toolkit-v2.git minc-toolkit-v2
```

get release

```shell
git tag -l
# get latest release in most cases?
git checkout tags/release-1.9.15
```

```shell
cd ~/src/minc-toolkit-v2
mkdir build && cd build
ccmake ..
```







```shell
                                                     Page 1 of 1
 GLUT_X11_LIBRARY                */usr/lib/x86_64-linux-gnu/libX11.so
 GLUT_Xi_LIBRARY                 *GLUT_Xi_LIBRARY-NOTFOUND
 GLUT_Xmu_LIBRARY                *GLUT_Xmu_LIBRARY-NOTFOUND
```



## Installation

```shell
ccmake .. \
-DCMAKE_BUILD_TYPE:STRING=Release   \
-DCMAKE_INSTALL_PREFIX:PATH=/opt/minc/1.9.13   \
-DMT_BUILD_ABC:BOOL=ON   \
-DMT_BUILD_ANTS:BOOL=ON   \
-DMT_BUILD_C3D:BOOL=ON   \
-DMT_BUILD_ELASTIX:BOOL=ON   \
-DMT_BUILD_IM:BOOL=OFF   \
-DMT_BUILD_ITK_TOOLS:BOOL=ON   \
-DMT_BUILD_LITE:BOOL=OFF   \
-DMT_BUILD_SHARED_LIBS:BOOL=ON   \
-DMT_BUILD_VISUAL_TOOLS:BOOL=ON   \
-DMT_USE_OPENMP:BOOL=ON   \
-DUSE_SYSTEM_FFTW3D:BOOL=OFF   \
-DUSE_SYSTEM_FFTW3F:BOOL=OFF   \
-DUSE_SYSTEM_GLUT:BOOL=OFF   \
-DUSE_SYSTEM_GSL:BOOL=OFF   \
-DUSE_SYSTEM_HDF5:BOOL=OFF   \
-DUSE_SYSTEM_ITK:BOOL=OFF   \
-DUSE_SYSTEM_NETCDF:BOOL=OFF   \
-DUSE_SYSTEM_NIFTI:BOOL=OFF   \
-DUSE_SYSTEM_PCRE:BOOL=OFF   \
-DUSE_SYSTEM_ZLIB:BOOL=OFF 
```

output error example

```shell
  Could NOT find OpenGL (missing: OPENGL_gl_LIBRARY OPENGL_INCLUDE_DIR)
Call Stack (most recent call first):
  /usr/share/cmake-3.5/Modules/FindPackageHandleStandardArgs.cmake:388 (_FPHSA_FAILURE_MESSAGE)
  /usr/share/cmake-3.5/Modules/FindOpenGL.cmake:172 (FIND_PACKAGE_HANDLE_STANDARD_ARGS)

  CMakeLists.txt:485 (FIND_PACKAGE)
```

If running ccmake here is output

```shell
 HDF5_DIR=/home/vagrant/minc-toolkit-v2/build/external//opt/minc/1.9.13/share/cmake/hdf5
 CMAKE_CXX_COMPILER=/usr/bin/c++
 CMAKE_C_COMPILER=/usr/bin/cc
 HDF5_LIBRARY=/home/vagrant/minc-toolkit-v2/build/external//opt/minc/1.9.13/lib/libhdf5.so
 HDF5_CPP_LIBRARY=/home/vagrant/minc-toolkit-v2/build/external//opt/minc/1.9.13/lib/libhdf5_cpp.so
 HDF5_HL_LIBRARY=/home/vagrant/minc-toolkit-v2/build/external//opt/minc/1.9.13/lib/libhdf5_hl.so
 HDF5_HL_CPP_LIBRARY=/home/vagrant/minc-toolkit-v2/build/external//opt/minc/1.9.13/lib/libhdf5_hl_cpp.so
```

then

```shell
sudo make DESTDIR=/opt/minc/1.9.13 | tee make.log
```


```shell
sudo make | tee make.log && make install | make_install.log
```




```shell
ccmake .. # Enter configuration details, recommend not to use any system-provided libraries that are included in minc-toolkit-v2
```

## Installation

### Dependencies

#### non GUI

* Cmake - https://cmake.org/
* Perl - http://www.perl.org/
* BISON - http://www.gnu.org/software/bison/
* FLEX - http://flex.sf.net/
* bc - http://ftp.gnu.org/gnu/bc/

```shell
sudo apt-get install -y cmake
sudo apt-get install -y perl
sudo apt-get install -y bison
sudo apt-get install -y flex
sudo apt-get install -y bc
```

#### to add GUI support...

* X11 development libraries
* libxi
* libxmu
* libjpeg-dev

```shell
sudo apt-get install -y libx11-dev
sudo apt-get install -y libxi
sudo apt-get install -y libxmu
sudo apt-get install -y libjpeg-dev
```

### go!

General way of installation (automake and libtool should be installed):

```shell
./autogen.sh
./configure --prefix=/path/to/where/tools/will/be/installed --with-minc2 --with-build-path=/path/to/where/minc/installation/lives
make
make install
```

### cython

The cython part of the code is not installed automatically at the moment. To compile the cython code make sure the cython library is installed and that the path to the library is in the $PYTHONPATH environment variable. Then run:

```shell
cd scripts
python setup.py build_ext --inplace
python setup.py install --prefix=/path/to/where/tools/will/be/installed --install-lib=/path/to/where/python/libs/should/go
in the scripts directory.
```

## Installation

```shell
cmake .. \
-DCMAKE_BUILD_TYPE:STRING=Release   \
-DCMAKE_INSTALL_PREFIX:PATH=/opt/minc/1.9.13   \
-DMT_BUILD_ABC:BOOL=ON   \
-DMT_BUILD_ANTS:BOOL=ON   \
-DMT_BUILD_C3D:BOOL=ON   \
-DMT_BUILD_ELASTIX:BOOL=ON   \
-DMT_BUILD_IM:BOOL=OFF   \
-DMT_BUILD_ITK_TOOLS:BOOL=ON   \
-DMT_BUILD_LITE:BOOL=OFF   \
-DMT_BUILD_SHARED_LIBS:BOOL=ON   \
-DMT_BUILD_VISUAL_TOOLS:BOOL=ON   \
-DMT_USE_OPENMP:BOOL=ON   \
-DUSE_SYSTEM_FFTW3D:BOOL=OFF   \
-DUSE_SYSTEM_FFTW3F:BOOL=OFF   \
-DUSE_SYSTEM_GLUT:BOOL=OFF   \
-DUSE_SYSTEM_GSL:BOOL=OFF   \
-DUSE_SYSTEM_HDF5:BOOL=OFF   \
-DUSE_SYSTEM_ITK:BOOL=OFF   \
-DUSE_SYSTEM_NETCDF:BOOL=OFF   \
-DUSE_SYSTEM_NIFTI:BOOL=OFF   \
-DUSE_SYSTEM_PCRE:BOOL=OFF   \
-DUSE_SYSTEM_ZLIB:BOOL=OFF 
```
then

```shell
sudo make && make install
```

