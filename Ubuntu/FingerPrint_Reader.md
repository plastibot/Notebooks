# Setup FingerPrint reader on Thinkpad T550 and Ubuntu 16.04

This instructions were copied from:

https://askubuntu.com/questions/511876/how-do-i-install-a-fingerprint-reader-on-lenovo-thinkpad

There are 2 applications you can use. Fingerprint GUI and FPrint. The best results according to 
this post are achieved with FPrint. To install FPrint do the following:

````bash
$ sudo apt install libpam-fprintd fprint-demo
````

After that, you can test it by running fprint_demo and save the fingerprint with fprintd-enroll. 
This will automatically make your login screen require a finger swipe instead of a password.
