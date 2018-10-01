# Enable Support for 4K monitor

More details can be found on the following link:

http://ubuntuhandbook.org/index.php/2017/04/custom-screen-resolution-ubuntu-desktop/


Enter the following commands on a shell terminal

````bash
xrandr --newmode "3840x2160_30"  297 3840 4016 4104 4400 2160 2168 2178 2250 +hsync +vsync
xrandr --addmode HDMI-1 "3840x2160_30"
xrandr --output HDMI-1 --mode "3840x2160_30"
````

You should then see the option to select 3840 x 2160 from the resolution options on the display menu.

## To make the changes persistent

edit the file ~/.profile and add the first 2 lines above to the end of the file.

````bash
$ emacs ~/.profile
````

then add the following:

````bash
xrandr --newmode "3840x2160_30"  297 3840 4016 4104 4400 2160 2168 2178 2250 +hsync +vsync
xrandr --addmode HDMI-1 "3840x2160_30"
````
