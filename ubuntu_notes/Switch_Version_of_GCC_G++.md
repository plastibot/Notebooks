# Switch version of GCC/G++


#### First erased the current update-alternatives setup for gcc and g++:
```
sudo update-alternatives --remove-all gcc 
sudo update-alternatives --remove-all g++
```
#### Install Packages

Say you already have gcc-7 installed but you want to install gcc-6 as well. install the package as follows:
```
sudo apt-get install gcc-6 gcc-ar-6 gcc-nm-6 gcc-ranlib-6 g++-6
```
#### Install Alternatives

Symbolic links cc and c++ are installed by default. We will install symbol links for gcc and g++, then link cc and c++ to gcc and g++ respectively if needed.
```
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-6 10 \
    --slave /usr/bin/gcc-ar gcc-ar /usr/bin/gcc-ar-6 \
    --slave /usr/bin/gcc-nm gcc-nm /usr/bin/gcc-nm-6 \
    --slave /usr/bin/gcc-ranlib gcc-ranlib /usr/bin/gcc-ranlib-6

sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 20 \
    --slave /usr/bin/gcc-ar gcc-ar /usr/bin/gcc-ar-7 \
    --slave /usr/bin/gcc-nm gcc-nm /usr/bin/gcc-nm-7 \
    --slave /usr/bin/gcc-ranlib gcc-ranlib /usr/bin/gcc-ranlib-7
    
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-6 10
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-7 20

#These two below may be already setup. You can check first with sudo update-alternatives --display cc

sudo update-alternatives --install /usr/bin/cc cc /usr/bin/gcc 30
sudo update-alternatives --set cc /usr/bin/gcc

sudo update-alternatives --install /usr/bin/c++ c++ /usr/bin/g++ 30
sudo update-alternatives --set c++ /usr/bin/g++

```
#### Configure Alternatives

The last step is configuring the default commands for gcc, g++. It's easy to switch between 6 and 7 interactively:
```
sudo update-alternatives --config gcc
sudo update-alternatives --config g++
```
