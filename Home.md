# Call Attendant

#### Contents
1. [[Overview|home#overview]]
2. [[Installation|home#installation]]
3. [[Operation|home#operation]]
4. [[Configuration|home#configuration]]

#### Other pages
* [[User Guide|User-Guide]]
* [[Developer Guide|Developer-Guide]]
* [[Advanced|Advanced]]

## Overview
The Call Attendant (__callattendant__) is a python-based, automated call attendant that runs on a lightweight Raspberry Pi or other Linux-based system. Coupled with a modem, it provides a call blocker and voice messaging system that can screen callers and block robocall and scams from your landline.

This wiki page shows you how to setup and run the __callattendant__. If you'd like a preview of what you can do with it, check out the [[User Guide|User-Guide]].

##### _Home Page_
![Dashboard - Small](https://github.com/emxsys/callattendant/blob/master/docs/dashboard-responsive.png)

### Hardware
- Raspberry Pi 3B+ or better
- US Robotics 5637 Modem

##### _Raspberry Pi 3B+ and USR5637 modem_
![Raspberry Pi and USR5637 Modem](https://github.com/emxsys/callattendant/raw/master/docs/raspberry_pi-modem.jpg)

***

## Installation
This section describes how to install the hardware and the software.

### Hardware
* The USR5637 Modem is connect to your Raspberry Pi via USB.
* Your home phone land-line system is connected to the USR5637 Modem via the RJ11 connection to a wall jack,
or to a splitter connected to a wall jack and shared with a phone, or a splitter connected to your Telco modem
and shared with the land-line phone wiring.
* Your Raspberry Pi is connected to your home network via wireless or an RJ45 network connection.

##### Schematic
![Hardware Connections](https://github.com/emxsys/callattendant/blob/master/docs/design/images/Deployment_View.png)


### Software

The installation calls for Python3.X.

#### Setup a Virtual Environment
###### _Optional_
A virtual environment is useful if your Raspberry Pi is being shared with other functions, like the [Pi-hole](https://pi-hole.net/) add blocker. A virtual environment allows you isolate the Python run-time environment from other programs. If your Raspberry Pi is dedicated to the __callattendant__ then this step is not necessary.

The following instructions create and activate a virtual environment named _venv_ within the current folder. 
```bash
# Intall - if necessary
sudo apt install virtualenv

# Create the virtual environment
virtualenv venv --python=python3

# Activate it
source venv/bin/activate
```
Now you're operating with a virtual Python. To check, issue the `which` command and ensure the output points to your virtual environment; and also check the Python version:
```bash
which python
# /home/pi/venv/bin/python

python --version
# Python 3.7.3
```
Later, when you install the __callattendant__ software, it will be placed within the virtual environment
folder (under `lib/python3.x/site-packages` to be exact). The virtual environment, when activated, alters
your _PATH_ so that the system looks for python and its packages within this folder hierarchy. 

#### Install the Software
The software is available on [PyPI](https://pypi.org/project/callattendant/). Install and update using `pip`:
```bash
# Using the virtual environment you use "pip" to install the software
pip install callattendant

# However, if your not using the virtual environment, you must use "pip3" on the Pi
pip3 install callattendant
```

If your not using the virtual environment, you may need to reboot or logoff/login to update the
`$PATH` for your profile in order to find and use the `callattendant` command.

***

## Operation

The __callattendant__ software includes a `callattendant` command to start the system. Run this command
the first time with the `--create-folder` option to create the initial data and files in the default
data folder: `~/.callattendant`. This is a hidden folder off the root of your home directory. You
can override this location with the `--data-path` option.

Command line options:
```
Usage: callattendant --config [FILE] --data-path [FOLDER]
Options:
-c, --config [FILE]       load a python configuration file
-d, --data-path [FOLDER]  path to data and configuration files
-f, --create-folder       create the data-path folder if it does not exist
-h, --help                displays this help text
```
Here are some examples of the `callattendant` command to start the system:
```bash
# Creating the default data folder with the default configuration
callattendant --create-folder

# Using the default configuration
callattendant

# Using the configuration file named `app.cfg` in the default location
callattendant --config app.cfg

# Using a customized config file in an alternate, existing location
callattendant --config myapp.cfg --data-path /var/lib/callattendant
```

After starting the system, you should see output of the form:
```
[Configuration]
  BLOCKED_ACTIONS = ('greeting',)
  BLOCKED_GREETING_FILE = resources/blocked_greeting.wav
  BLOCKED_RINGS_BEFORE_ANSWER = 0
  BLOCK_ENABLED = True
  BLOCK_NAME_PATTERNS = {'V[0-9]{15}': 'Telemarketer Caller ID'}
  BLOCK_NUMBER_PATTERNS = {}
  BLOCK_SERVICE = NOMOROBO
  DATABASE = data/callattendant.db
  DEBUG = False
  ENV = production
  PERMITTED_ACTIONS = ()
  PERMITTED_GREETING_FILE = resources/general_greeting.wav
  PERMITTED_RINGS_BEFORE_ANSWER = 4
  PHONE_DISPLAY_FORMAT = ###-###-####
  PHONE_DISPLAY_SEPARATOR = -
  ROOT_PATH = /home/pi/src/callattendant/callattendant
  SCREENED_ACTIONS = ('greeting', 'record_message')
  SCREENED_GREETING_FILE = resources/general_greeting.wav
  SCREENED_RINGS_BEFORE_ANSWER = 0
  SCREENING_MODE = ('whitelist', 'blacklist')
  TESTING = False
  VOICE_MAIL_GOODBYE_FILE = resources/goodbye.wav
  VOICE_MAIL_GREETING_FILE = resources/general_greeting.wav
  VOICE_MAIL_INVALID_RESPONSE_FILE = resources/invalid_response.wav
  VOICE_MAIL_LEAVE_MESSAGE_FILE = resources/please_leave_message.wav
  VOICE_MAIL_MENU_FILE = resources/voice_mail_menu.wav
  VOICE_MAIL_MESSAGE_FOLDER = data/messages
{MSG LED OFF}
Staring the Flask webapp
Running Flask webapp
 * Serving Flask app "userinterface.webapp" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
Modem COM Port is: /dev/ttyACM0
```

Make a few calls to yourself to test the service. The standard output will show the
progress of the calls. Then navigate to `http://<pi-address>|<pi-hostname>:5000` in a
web browser to checkout the web interface.

Press `ctrl-c` a couple of times to exit the system

### Web Interface
#### URL: `http://<pi-address>|<pi-hostname>:5000`
To view the web interface, simply point your web browser to port `5000` on your Raspberry Pi.
For example, in your Raspberry Pi's browser, you can use:
```
http://localhost:5000/
```
See the [[User Guide|User-Guide]] for detailed instructions on the web interface.

***

## Configuration
You can review the current configuration by navigating to the _Settings_ page by clicking on the "gear" icon in the main menu.
#### URL: `http://<pi-address>|<pi-hostname>:5000/settings`

To override the default configuration, edit the `app.cfg` file found in the default location (`~/.callattendant`).
Use an editor that provides Python syntax highlighting, like `nano`.  Then specify your configuration file when starting the 
__callattendant__. See the preceding [[Operation|home#operation]] for an example.

The following are select configuration elements that may be of interest. The default values are shown.

##### Block Enabled
Set to False to disable call blocking (for whatever reason)
```python
# BLOCK_ENABLED: if set to True, calls that fail screening will be blocked
BLOCK_ENABLED = True
```
##### Screening Mode
Determines whether the permitted (whitelist) and blocked (blacklist) number lists are processed:
```python
# SCREENING_MODE: A tuple containing: "whitelist" and/or "blacklist", or empty
SCREENING_MODE = ("whitelist", "blacklist")
```
##### Phone Display Separator
The character used to format phone numbers. This character is used in the phone display format string that follows.
```python
# Defines the character used to format phone numbers, e.g, a space (" "), hyphen ("-"), period (".") or blank ("").
PHONE_DISPLAY_SEPARATOR = "-"
```
##### Phone Display Format
The template used to format how phone numbers are displayed.  
```python
# PHONE_DISPLAY_FORMAT: Define the formatting of phone numbers in the various lists. You must use the
# separator character defined by PHONE_DISPLAY_SEPARATOR above in the format string.
#
#   The phone display format handles variable length phone numbers. Excess digits not included
#   in the format string are prepended to the number with a separator.
#   Define the format used for the majority of the phone numbers in your system.
#
#
#  Example: US -> 01234567890 formats as 0-123-456-7890  
#   PHONE_DISPLAY_FORMAT = "###-###-####"
#
#  Example: UK -> 01234567890 formats as 0123-456-7890
#   PHONE_DISPLAY_FORMAT = "####-###-####"
#
#  Example: FR -> 01234567890 formats as 01-234-56-78-90
#   PHONE_DISPLAY_FORMAT = "###-##-##-##"
#
#  Example: AU -> 01234567890 formats as 01-23-456-7890
#   PHONE_DISPLAY_FORMAT = "##-##-###-####"
#
#  Example: Raw -> 01234567890 formats as 01234567890
#  PHONE_DISPLAY_FORMAT = ""
#
PHONE_DISPLAY_FORMAT = "###-###-####"
```

##### Online Lookup Service
The name of the online service used to lookup robocallers and spam numbers. NomoRobo is the service used in the USA.
```python
#   Currently, only NOMOROBO is supported and it is for the USA. Areas outside the USA should set
#   to blank. When the online service is blank (disabled), only the blacklist and blocked
#   name/number patterns are used to block numbers.
#
#   Example: "NOMOROBO" (USA)  or "" (disabled).
BLOCK_SERVICE = "NOMOROBO"
```
##### Name Patterns
Blocks callers whose caller ID name match a regular expression:
```python
# BLOCK_NAME_PATTERNS: A regex expression dict applied to the CID names
# Example: {"V[0-9]{15}": "Telemarketer Caller ID", "O": "Unknown caller"}
BLOCK_NAME_PATTERNS = {"V[0-9]{15}": "Telemarketer Caller ID", }
```
##### Number Patterns
Blocks callers whose number match a regular expression, for example an area code:
```python
# BLOCK_NUMBER_PATTERNS: A regx expression dict applied to the CID numbers
# Example: {"P": "Private number",}
BLOCK_NUMBER_PATTERNS = {}
```
##### Blocked Actions
Determines what action is taken for a blocked caller:
```python
# BLOCKED_ACTIONS: A tuple containing a combination of the following actions:
#   "greeting", "record_message", "voice_mail"
# Example: No actions, just hang_up
#   BLOCKED_ACTIONS = ()
# Example: Play an announcement before hanging up
#   BLOCKED_ACTIONS = ("greeting", )
# Example: Record a message before hanging up, no key-press required
#   BLOCKED_ACTIONS = ("record_message", )
# Example: Option to record a message; key-press required to leave message
#   BLOCKED_ACTIONS = ("voice_mail", )
# Example: Play announcement and record a message; no key-press required
#   BLOCKED_ACTIONS = ("greeting", "record_message" )
# Example: Play announcement and voice mail menu; key-press required to leave message
#   BLOCKED_ACTIONS = ("greeting", "voice_mail" )
#
BLOCKED_ACTIONS = ("greeting", "voice_mail")
```
##### Audio WAV Files
Various audio wav files may be played to the caller. You can record your own
and specify them with these settings:
```python
# BLOCKED_GREETING_FILE: The wav file to be played to blocked callers
#   Example: "We're sorry, this call has been blocked by the Raspberry Pi
#           call attendant. To be unblocked, leave a message with your
#           justification to be unblocked."
BLOCKED_GREETING_FILE = "resources/blocked_greeting.wav"

# VOICE_MAIL_GREETING_FILE: The wav file played after answering: a general greeting
#   Example: "I'm sorry we missed your call..."
VOICE_MAIL_GREETING_FILE = "resources/general_greeting.wav"

# VOICE_MAIL_GOODBYE_FILE: The wav file play just before hanging up
#   Example:Goodbye"
VOICE_MAIL_GOODBYE_FILE = "resources/goodbye.wav"

# VOICE_MAIL_INVALID_RESPONSE_FILE: The wav file played on an invalid keypress
#   Example: "That was an invalid response..."
VOICE_MAIL_INVALID_RESPONSE_FILE = "resources/invalid_response.wav"

# VOICE_MAIL_LEAVE_MESSAGE_FILE: The wav file played before recording a message
#   Example: "Please leave a message"
VOICE_MAIL_LEAVE_MESSAGE_FILE = "resources/please_leave_message.wav"

# VOICE_MAIL_MENU_FILE: The wav file with message instructions, played after the greeting
#   Example: "Press 1 to leave a message..."
VOICE_MAIL_MENU_FILE = "resources/voice_mail_menu.wav"
```



