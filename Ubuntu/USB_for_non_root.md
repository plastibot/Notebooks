

**Asumptions:**
- USB device is called /dev/ttyUSB0.
- Userid is "user"
- hostname is "machine"


### Check the current permissions and owner/group of the device.

````bash
[user@machine ~]$ ls -la /dev/ttyUSB0

crw-rw----. 1 root dialout 188, 0 Apr 3 21:16 /dev/ttyUSB0
````

Looking at the response above we can see that the owner of this usb port is root, the group is dialout and both the user
and group have read/write permissions.

So in order to give access to other users than root you need to add your userid to the group associated with the usb device,
in this case dialout.

### Adding userid user to group dialout
To add the group dialout to our userid user, we use the usermod command. This command requires root privileges to run.

````bash
[user@machine ~]$ sudo usermod -a -G dialout user
````

You will need to log out then log back in and now you should have access to the device.
