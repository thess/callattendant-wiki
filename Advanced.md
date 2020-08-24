## Run as a Service
We're going to define a service to automatically run the Call Attendant on the Raspberry Pi at start up. Our simple service will run the `callattendant.py` script and if by any means is aborted it is going to be restarted automatically. 

### Create the Service

####  Step 1. Create the Unit File
The service definition must be on the `/lib/systemd/system` folder. Our service is going to be called "callattendant.service":

```Shell
cd /lib/systemd/system/
sudo nano callattendant.service
```

Copy the following text into the `callattendant.service` unit file; edit the paths as necessary for your system

```text
[Unit]
Description=Call Attendant
After=multi-user.target

[Service]
Type=simple
ExecStart=/home/pi/.local/bin/callattendant --config app.cfg
Environment="PYTHONUNBUFFERED='True'"
WorkingDirectory=/home/pi/.callattendant
User=pi
Restart=on-abort

[Install]
WantedBy=multi-user.target
```

You can check more on service's options in the next wiki: https://wiki.archlinux.org/index.php/systemd.

#### Step 2. Activate the Service
Now that we have our service we need to activate it:

```Shell
# Change the permissions on the service file to read/write
sudo chmod 644 /lib/systemd/system/callattendant.service

# Reload the unit files
sudo systemctl daemon-reload

# Enable the service...
sudo systemctl enable callattendant.service

# And now run the service
sudo systemctl start callattendant.service
```

### Service Tasks
For every change that we do on the `/lib/systemd/system` folder we need to execute a `daemon-reload` (third line of previous code). Execute the following commands as needed to check the status, start and stop the service, or check the logs.
```bash
# Reload the unit file(s)
sudo systemctl daemon-reload
```

#### Start service
```bash
# Use the supplied command script
start-callattendant

# or use the system command
sudo systemctl start callattendant.service
```


#### Stop service
```bash
# Use the supplied command script
stop-callattendant

# or use the system command
sudo systemctl stop callattendant.service
```

#### Check/Monitor service's log
```bash
# Use the supplied command script
monitor-callattendant

# or use the system command
sudo journalctl -f -u callattendant.service
```

#### Check status
```bash
sudo systemctl status callattendant.service
```

### REFERENCES
- https://wiki.archlinux.org/index.php/systemd
- https://stackoverflow.com/questions/37211115/how-to-enable-a-virtualenv-in-a-systemd-service-unit
- http://www.diegoacuna.me/how-to-run-a-script-as-a-service-in-raspberry-pi-raspbian-jessie/
- https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files
- https://coreos.com/os/docs/latest/getting-started-with-systemd.html

***
## Record Audio Files
You can record your own audio wav files for playback to your callers.

See: (https://iotbytes.wordpress.com/play-audio-file-on-phone-line-with-raspberry-pi/)
