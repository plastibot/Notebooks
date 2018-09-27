# Enable Support for 4K monitor

Enter the following commands on a shell terminal

````bash
xrandr --newmode "3840x2160_30"  297 3840 4016 4104 4400 2160 2168 2178 2250 +hsync +vsync
xrandr --addmode HDMI-1 "3840x2160_30"
xrandr --output HDMI-1 --mode "3840x2160_30"
````

You should then see the option to select 3840 x 2160 from the resolution options.
