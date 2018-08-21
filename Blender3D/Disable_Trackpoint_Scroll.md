# These instructions were tested on Debian Stretch

To find the id of your pointing devices:

````bash
$ xinput list
````

To list the properties of your device, use the ID of your device from above command. In my case the trackpoint is 12

```` bash
$ xinput list-props 12
````

To disable the scrolling

````bash
$ xinput set-prop "TPPS/2 IBM TrackPoint" "libinput Button Scrolling Button" 0
````
