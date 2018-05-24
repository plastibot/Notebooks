# Switch version of GCC/G++


#### First erased the current update-alternatives setup for gcc and g++:
```
sudo update-alternatives --remove-all gcc 
sudo update-alternatives --remove-all g++
```
#### Install Packages

It seems that both gcc-4.3 and gcc-4.4 are installed after install build-essential. However, we can explicitly install the following packages:
```
sudo apt-get install gcc-4.3 gcc-4.4 g++-4.3 g++-4.4
```
#### Install Alternatives

Symbolic links cc and c++ are installed by default. We will install symbol links for gcc and g++, then link cc and c++ to gcc and g++ respectively.
```
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.3 10
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.4 20

sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.3 10
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.4 20

sudo update-alternatives --install /usr/bin/cc cc /usr/bin/gcc 30
sudo update-alternatives --set cc /usr/bin/gcc

sudo update-alternatives --install /usr/bin/c++ c++ /usr/bin/g++ 30
sudo update-alternatives --set c++ /usr/bin/g++
```
#### Configure Alternatives

The last step is configuring the default commands for gcc, g++. It's easy to switch between 4.3 and 4.4 interactively:
```
sudo update-alternatives --config gcc
sudo update-alternatives --config g++
```
