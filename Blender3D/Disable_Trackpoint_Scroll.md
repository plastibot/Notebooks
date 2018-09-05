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

#### To make it persistent after reboot:

Open 'Startup Applications'.

Click 'Add' and in the command field paste the desired command. Click 'Add'. You have added the first command.

Add the other commands too in similar fashion (if you have more than one command). You are done. Next time you restart your computer, Ubuntu will automatically run these commands which will fix your mouse issues.
