## Run as a Service
We're going to define a service to automatically run the Call Attendant on the Raspberry Pi at start up. Our simple service will run the `callattendant.py` script and if by any means is aborted it is going to be restarted automatically. 

### Create the Service

####  Step 1. Create the Unit File
The service definition must be on the `/lib/systemd/system` folder. Our service is going to be called "callattendant.service":

```Shell
cd /lib/systemd/system/
sudo nano callattendant.service
```

Copy the following text into the `callattendant.service` unit file; edit the paths as necessary for your system:

```text
[Unit]
Description=Call Attendant
After=multi-user.target

[Service]
Type=simple
ExecStart=/usr/bin/python3 -u /home/pi/callattendant/callattendant --config app.cfg
WorkingDirectory=/home/pi/callattendant/callattendant
Restart=on-abort

[Install]
WantedBy=multi-user.target

```
You can check more on service's options in the next wiki: https://wiki.archlinux.org/index.php/systemd.

#### Step 2. Activate the Service
Now that we have our service we need to activate it:

```Shell
sudo chmod 644 /lib/systemd/system/callattendant.service
chmod +x /home/pi/callattendant/src/callattendant.py
sudo systemctl daemon-reload
sudo systemctl enable callattendant.service
sudo systemctl start callattendant.service
```

### Service Tasks
For every change that we do on the `/lib/systemd/system` folder we need to execute a `daemon-reload` (third line of previous code). Execute the following commands as needed to check the status, start and stop the service, or check the logs.
```bash
sudo systemctl daemon-reload
```

#### Check status
`sudo systemctl status callattendant.service`

#### Start service
`sudo systemctl start callattendant.service`

#### Stop service
`sudo systemctl stop callattendant.service`

#### Check/Monitor service's log
`sudo journalctl -f -u callattendant.service`

### REFERENCES
- https://wiki.archlinux.org/index.php/systemd
- http://www.diegoacuna.me/how-to-run-a-script-as-a-service-in-raspberry-pi-raspbian-jessie/
- https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files
- https://coreos.com/os/docs/latest/getting-started-with-systemd.html

***
## Record Audio Files
You can record your own audio wav files for playback to your callers.

See: (https://iotbytes.wordpress.com/play-audio-file-on-phone-line-with-raspberry-pi/)
