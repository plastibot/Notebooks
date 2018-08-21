# These instructions so far are tested on Debian Stretch and Ubuntu 16.04 (Jessie)

#### To find the id of your pointing devices:

````bash
$ xinput list
````

#### To list the properties of your device, use the ID of your device from above command. In my case the trackpoint ID is 12

```` bash
$ xinput list-props 12
````

#### To disable the scrolling on Debian Stretch:

````bash
$ xinput set-prop "TPPS/2 IBM TrackPoint" "libinput Button Scrolling Button" 0
````

#### To disable the scrolling on Ubuntu 16.04 (Jessie):

````bash
$ xinput set-prop "TPPS/2 IBM TrackPoint" "Evdev Wheel Emulation" 0
````
