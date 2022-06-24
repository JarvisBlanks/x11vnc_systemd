# About
**x11vnc** is a fantastic **VNC server** which runs regardless of the **Desktop Environment** which means it can run before a user session has initialised and therefore works with a reboot at the login screen.

This works in any **systemd** environment that I have tried.

# Setup

First install x11vnc:

~~~bash
sudo apt-get install x11vnc net-tools
~~~

Next set a VNC password:

~~~bash
x11vnc -storepasswd 

Enter VNC password: *********
Verify password: *********  
Write password to /home/USER/.vnc/passwd?  [y]/n y
Password written to: /home/USER/.vnc/passwd
~~~

Now create the service file:

~~~bash
sudo nano /lib/systemd/system/x11vnc.service
~~~

Add this content (Replacing USER with your username):

~~~bash
[Unit]
Description=Start x11vnc at startup.
After=multi-user.target

[Service]
Type=simple
ExecStart=/usr/bin/x11vnc -auth guess -forever -loop -noxdamage -repeat -rfbauth /home/**USER**/.vnc/passwd -rfbport 5900 -shared

[Install]
WantedBy=multi-user.target
~~~

Then do a daemon-reload, enable and start the service:

~~~bash
sudo systemctl daemon-reload
sudo systemctl enable --now x11vnc.service
~~~

Just like that you should now be able to VNC to your machine secured with a password!
