## Run as a Service
We're going to define a service to automatically run the Call Attendant on the Raspberry Pi at start up. Our simple service will run the `callattendant.py` script and if by any means is aborted it is going to be restarted automatically. 

### Create the Service

####  Step 1. Create the Unit File
The service definition unit file must be on the `/lib/systemd/system` folder. Our service is going to be called "callattendant.service".

But first, we must identify the path to the `callattendant` command. Use the `which` command to locate the `callattendant` script. Note the path, we'll use it later in the unit file.
```bash
which callattendant
/home/pi/.local/bin/callattendant
```

Now create the unit file using the `nano` editor:
```bash
sudo nano /lib/systemd/system/callattendant.service
```

Copy the following text into the `callattendant.service` unit file; edit the paths as necessary for your system using path identified earlier. _You can use `Ctl-Shift-V` or `Right-Click > Paste` to paste this text into the nano editor._
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
## LED Indicators
I built a custom Raspberry Pi HAT to show the __callattendant__'s status with several LEDs. 

- A blinking orange LED indicates an incoming call
- A blinking green LED indicates a permitted/approved caller: _Get up and answer the phone._
- A blinking red LED indicates a blocked/denied caller: _Stay seated._
- A Blue LED indicates message activity: pulsing means unplayed messages are waiting; blinking means the caller is in the voice messaging menu; steady means a message is being recorded.
- A 7-segment LEG shows the number of unplayed messages that are waiting

I used the following prototype board to solder up the LEDs and connect them to the GPIO pins: _DIGOBAY 3pcs Prototype Breakout Shield PCB Expansion Board Breadboard DIY Kit for Raspberry Pi 4B 3B+ 3B 2B B+ A+_. Learn more: https://www.amazon.com/dp/B089M4GFWL/ref=cm_sw_em_r_mt_dp_P1TsFb6PN3ZZA

### Diagrams
Following are the Fritzing diagrams for my board:

##### Empty PCM
![PCB](https://github.com/emxsys/callattendant/raw/master/docs/callattendant-hat_pcb.png)

##### Wiring
![wiring](https://github.com/emxsys/callattendant/raw/master/docs/callattendant-hat_bb.png)

##### Assembly List
Label |	Part Type | Properties
-----| ----- | -----
7 SEG LED | Kingbright SA03-12HDB | pins 14; 
BLU |	Blue (525nm) LED |	package 3 mm [THT]; color Blue (525nm); 
GRN |	Green (555nm) LED |	package 3 mm [THT]; color Green (555nm);
ORG |	Orange (620nm) LED |	package 3 mm [THT]; color Orange (620nm); 
RED |	Red (633nm) LED	| package 3 mm [THT]; color Red (633nm); 
R1 |	15Ω Resistor	| bands 4; package THT; tolerance ±5%; resistance 15Ω; pin spacing 400 mil
R2 |	15Ω Resistor	| bands 4; package THT; tolerance ±5%; resistance 15Ω; pin spacing 400 mil
R3 |	56Ω Resistor	| bands 4; package THT; tolerance ±5%; resistance 56Ω; pin spacing 400 mil
R4 |	56Ω Resistor	| bands 4; package THT; tolerance ±5%; resistance 56Ω; pin spacing 400 mil
R5 |	100Ω Resistor	| bands 4; package THT; tolerance ±5%; resistance 100Ω; pin spacing 400 mil
R6 |	100Ω Resistor	| bands 4; package THT; tolerance ±5%; resistance 100Ω; pin spacing 400 mil
R7 |	100Ω Resistor	| bands 4; package THT; tolerance ±5%; resistance 100Ω; pin spacing 400 mil
R8 |	100Ω Resistor	| bands 4; package THT; tolerance ±5%; resistance 100Ω; pin spacing 400 mil
R9 |	100Ω Resistor	| bands 4; package THT; tolerance ±5%; resistance 100Ω; pin spacing 400 mil
R10 |	100Ω Resistor	| bands 4; package THT; tolerance ±5%; resistance 100Ω; pin spacing 400 mil
R11 |	100Ω Resistor	| bands 4; package THT; tolerance ±5%; resistance 100Ω; pin spacing 400 mil
R12 |	100Ω Resistor	| bands 4; package THT; tolerance ±5%; resistance 100Ω; pin spacing 400 mil

##### Shopping List
Amount | Part Type | Properties
------ | --------- | -----------
1 | Kingbright SA03-12HDB	| pins 14; 
1 |	Blue (525nm) LED	| package 3 mm [THT]; color Blue (525nm)
1 |	Green (555nm) LED	| package 3 mm [THT]; color Green (555nm); 
1 |	Orange (620nm) LED	| package 3 mm [THT]; color Orange (620nm); 
1 |	Red (633nm) LED |	| package 3 mm [THT]; color Red (633nm); 
2 |	15Ω Resistor	| bands 4; package THT; tolerance ±5%; resistance 15Ω; pin spacing 400 mil
2 |	56Ω Resistor	| bands 4; package THT; tolerance ±5%; resistance 56Ω; pin spacing 400 mil
8 |	100Ω Resistor	| bands 4; package THT; tolerance ±5%; resistance 100Ω; pin spacing 400 mil

### Configuration values
#### LED Indicators
LED | Color | GPIO
------| -------- | ------
Ring | ORG | GP14
Approved | GRN | GP15
Blocked | RED | GP17
Message | BLU | GP4

#### Kingbright LED Segments

Segment | Pin | GPIO
------ | -------- | --------
a | 1 | GP11
b | 13 | GP8
c | 10 | GP25
d | 8 | GP 5
e | 7 | GP18
f | 2 | GP9
g | 11 | GP7
dp | 6 | GP27 


***

## Record Audio Files
You can record your own audio wav files for playback to your callers.

See: (https://iotbytes.wordpress.com/play-audio-file-on-phone-line-with-raspberry-pi/)
