# Using PPA repositories

Using Blender's PPA as example.

## Install Blender 2.79 via PPA in Ubuntu:

Thomas Schiex’s PPA contains the Blender packages for Ubuntu 14.04, Ubuntu 16.04, Ubuntu 17.04.

1. Open terminal via Ctrl+Alt+T or from app launcher. When it opens, run command to add the PPA:

````bash
$ sudo add-apt-repository ppa:thomas-schiex/blender
````

2. Then upgrade Blender if you have a previous installed via Software Updater or run commands to check updates and install blender package:

````bash
$ sudo apt-get update

$ sudo apt-get install blender
````

## How to Remove Blender and the PPA repository:

To remove Blender packages either use your system package manager or run commands:

```bash
sudo apt-get remove --autoremove blender
````

And to remove the PPA repository, launch “Software & Updates” utility and navigate to Other Software tab.
