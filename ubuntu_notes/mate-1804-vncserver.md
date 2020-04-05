Begin with a Ubuntu Desktop 18.04 installation.

Update the repositories
```bash
sudo apt update
```

## Install desktop Environment and VNC Server

#### 1. install mate and make sure you select Lightdm as the windows manager during installation.

````bash
$ sudo apt-get install ubuntu-mate-desktop
````

#### 2. Install VNCserver

````bash
$ sudo apt-get install tightvncserver
````

#### 3. Run vncserver and create password.

```bash
$ vncserver :1
```

You’ll be prompted to enter and verify a password to access your machine remotely:

````bash
Output
You will require a password to access your desktops.

Password:
Verify:
````

The password must be between six and eight characters long. Passwords more than 8 characters will be truncated automatically.

Once you verify the password, you’ll have the option to create a a view-only password. Users who log in with the view-only password will not be able to control the VNC instance with their mouse or keyboard. This is a helpful option if you want to demonstrate something to other people using your VNC server, but this isn’t required.

The process then creates the necessary default configuration files and connection information for the server:

````bash
Output
Would you like to enter a view-only password (y/n)? n
xauth:  file /home/sammy/.Xauthority does not exist

New 'X' desktop is your_hostname:1

Creating default startup script /home/sammy/.vnc/xstartup
Starting applications specified in /home/sammy/.vnc/xstartup
Log file is /home/sammy/.vnc/your_hostname:1.log
````

Now let’s configure the VNC server.

## Configure the VNC Server

#### 4. stop vncserver

````bah
$ vncserver -kill :1
````

#### 5. Edit xstartup config file

````bash
$ nano ~/.vnc/xstartup
````
#### 6. Add the following to the beinning of the file right after the shebang
```bash
unset DBUS_SESSION_BUS_ADDRESS
```

#### 7. Add the following line to the end of the file

````bash
exec /usr/bin/mate-session &
````

7. run the VNC server

````bash
$ vncserver :1 -geometry 1680x1050 -depth 24
````

In case you have the UFW firewall enabled, open the port 5901 for incoming connections 
or see below how to tunnel the VNC connections via the SSH protocol:

````bash
$ sudo ufw allow from any to any port 5901 proto tcp
Rule added
Rule added (v6)
````

Note, it is also possible to run a secure VNC client/server connection via the SSH tunnel. Given that you have 
the SSH user access (in this case username linuxconfig is used) to your VNC server eg. ubuntu-vnc-server.

First, create an SSH tunnel on a local port 5901 leading to a remote port 5901 on your VNC server.

````bash
$ ssh -L 5901:127.0.0.1:5901 -N -f -l linuxconfig ubuntu-vnc-server
````

The above command will open a local port 5901 on a localhost loop-back network interface 127.0.0.1:

Next, use the local port 5901 to connect to a remote VNC server via the SSH tunnel:

````bash
vncviewer localhost:1
````

## VNC Server service at Start up

Create a new file /etc/systemd/system/vncserver@.service

````bahs
$ sudo nano /etc/systemd/system/vncserver@.service
````

The @ symbol at the end of the name will let us pass in an argument we can use in the service configuration. We’ll use this to specify the VNC display port we want to use when we manage the service.

Once you have the file opened insert the following lines while replacing the username with username 
of your VNC user on Lines 6, 7, 8 and Line 10. Optionally, change screen resolution settings and apply other vncserver 
options or arguments:

````bash
[Unit]
Description=Start TightVNC server at startup
After=syslog.target network.target

[Service]
Type=forking
User=freeman
Group=freeman
WorkingDirectory=/home/freeman

PIDFile=/home/freeman/.vnc/%H:%i.pid
ExecStartPre=-/usr/bin/vncserver -kill :%i > /dev/null 2>&1
ExecStart=/usr/bin/vncserver -depth 24 -geometry 1920x1080 :%i
ExecStop=/usr/bin/vncserver -kill :%i

[Install]
WantedBy=multi-user.target
````

Next, make the system aware of the new unit file.

````bash
sudo systemctl daemon-reload
````

Enable the unit file.

````bash
sudo systemctl enable vncserver@1.service
````

The 1 following the @ sign signifies which display number the service should appear over, in this case the default :1 
as was discussed in Step 2..

Stop the current instance of the VNC server if it’s still running.

````bash
vncserver -kill :1
````

Then start it as you would start any other systemd service.

````bash
sudo systemctl start vncserver@1
````

You can verify that it started with this command:

````bash
sudo systemctl status vncserver@1
````

If it started correctly, the output should look like this:

````bash
Output
● vncserver@1.service - Start TightVNC server at startup
   Loaded: loaded (/etc/systemd/system/vncserver@.service; indirect; vendor preset: enabled)
   Active: active (running) since Mon 2018-07-09 18:13:53 UTC; 2min 14s ago
  Process: 22322 ExecStart=/usr/bin/vncserver -depth 24 -geometry 1280x800 :1 (code=exited, status=0/SUCCESS)
  Process: 22316 ExecStartPre=/usr/bin/vncserver -kill :1 > /dev/null 2>&1 (code=exited, status=0/SUCCESS)
 Main PID: 22330 (Xtightvnc)
...

Your VNC server will now be available when you reboot the machine.

Start your SSH tunnel again:

````bash
ssh -L 5901:127.0.0.1:5901 -C -N -l sammy your_server_ip
````

Then make a new connection using your VNC client software to localhost:5901 to connect to your machine.
