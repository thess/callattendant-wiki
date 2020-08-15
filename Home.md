# Call Attendant

#### Contents
1. [[Overview|home#overview]]
2. [[Installation|home#installation]]
3. [[Configuration|home#configuration]]
4. [[Operation|home#operation]]

#### Other pages
* [[User Guide|User-Guide]]
* [[Developer Guide|Developer-Guide]]
* [[Advanced|Advanced]]

## Overview
The Call Attendant (__callattendant__) is a python-based, automated call attendant that runs on a lightweight Raspberry Pi or other Linux-based system. Coupled with a modem, it provides a call blocker and voice messaging system that can screen callers and block robocall and scams from your landline.

### Hardware
- Raspberry Pi 3B+ or better
- US Robotics 5637 Modem

##### _Raspberry Pi 3B+ and USR5637 modem_
![Raspberry Pi and USR5637 Modem](https://github.com/emxsys/callattendant/raw/master/docs/raspberry_pi-modem.jpg)

***

## Installation
This section describes how to install the hardware and the software.

### Hardware
* The USR5637 Modem is connect to your Raspberry Pi via USB
* Your home phone system is connected to the USR5637 Modem via the RJ11 connection to a wall jack,
or to a splitter connected to a wall jack and shared with a phone, or a splitter connected to your Telco modem
and shared with the house phone wiring.
* Your Raspberry Pi is connected to your home network via wireless or an RJ45 network connection.

##### Schematic
![Hardware Connections](https://github.com/emxsys/callattendant/blob/master/docs/images/Deployment_View.png)

_TODO: Raspberry PI LED connections_

### Software

#### Step 1. Download
1. [Download the latest code](https://github.com/emxsys/callattendant/archive/master.zip) or choose a
[specific release](https://github.com/emxsys/callattendant/releases).
2. Unzip the downloaded version. The unzipped folder will be named `callattendant-master` or 
`callattendant-<version>` depending on what you downloaded. Here's how unzip it into your home folder, 
using the latest version (_master_) in the example:
```bash
cd
unzip ~/Downloads/callattendant-master.zip 
cd callattendant-master
```
You can rename the `callattendant-<version>` folder if you wish.

#### Step 2. Install Dependencies

We've provided a requirements file called `requirements.txt`. Let's use it to
install the required packages. But first, navigate to the folder where the 
callattendant repository was placed, e.g., `/pi/home/callattendant`.

```bash
$ pip3 install -r requirements.txt
```

#### Step 3. If Updating 

If you are updating from a previous release, you should copy/move the `callattendant.db` file from the previous release to  `data/callattendant.db` in the new release.

***

## Configuration
To override the default configuration, copy the `src/app.example` file to a new file, e.g. `src/app.cfg` and edit its contents.
Use an editor that provides Python syntax highlighting, like `nano`.  Then use your configuration file when starting the 
__callattendant__. See [[Starting the Call Attendant|User-Guide#starting-the-call-attendant]] for an example.

Following are select configuration elements that may be of interest, the default values are shown:

##### Block Enabled
Set to False to disable call blocking (for whatever reason)
```python
# BLOCK_ENABLED: if True calls that fail screening will be blocked
BLOCK_ENABLED = True
```
##### Screening mode
Determines whether the permitted (whitelist) and blocked (blacklist) number lists are processed.
```python
# SCREENING_MODE: A tuple containing: "whitelist" and/or "blacklist", or empty
SCREENING_MODE = ("whitelist", "blacklist")
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
#   "greeting", "record_message", "voice_mail".
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
##### Audio WAV files
Various audio wav files may be played to the caller. You can record your own
and specify them with these settings:
```python
# BLOCKED_GREETING_FILE: The wav file to be played to blocked callers.
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
#   Example: "That was an invalid response.."
VOICE_MAIL_INVALID_RESPONSE_FILE = "resources/invalid_response.wav"

# VOICE_MAIL_LEAVE_MESSAGE_FILE: The wav file played be before recording a message
#   Example: "Please leave a message"
VOICE_MAIL_LEAVE_MESSAGE_FILE = "resources/please_leave_message.wav"

# VOICE_MAIL_MENU_FILE: The wav file with message instructions, played after the greeting
#   Example: "Press 1 to leave a message..."
VOICE_MAIL_MENU_FILE = "resources/voice_mail_menu.wav"
```

***

## Operation
### Starting the Call Attendant
To start the system, run the Python 3 `callantendant.py` program with an optional configuration file. 
Here are a couple of examples assuming you installed the __callattendant__ under the home folder on your
Raspberry Pi:

##### _Using the default configuration_
```bash
cd ~
cd callattendant<-version>
python3 src/callattendant.py
```
##### _Using a configuration file named app.cfg_
```bash
cd ~
cd callattendant<-version> 
python3 src/callattendant.py --config app.cfg
```


