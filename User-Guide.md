#### Contents
1. [[Installation|User-Guide#installation]]
2. [[Configuration|User-Guide#configuration]]
3. [[Operation|User-Guide#operation]]
4. [[Web Interface|User-Guide#web-interface]]

## Installation
This section describes how to install the hardware and the software.

### Hardware
Connecting the Raspberry Pi and modem to your home phone system...

### Software
#### Install
1. [Download the latest code](https://github.com/emxsys/callattendant/archive/master.zip) or choose a
[specific release](https://github.com/emxsys/callattendant/releases).
2. Unzip the downloaded version. The unzpped folder will be named `callattendant-master` or 
`callattendant-<version>` depending on what you downloaded. Here's how unzip it into your home folder, 
using the latest version (_master_) in the example:
```bash
cd
unzip ~/Downloads/callattendant-master.zip 
cd callattendant-master
```
You can rename the `callattendant-<version>` folder if you wish.

#### Removal
- Delete the `callattendant-<version>` folder and its contents.

#### Update
- If you are updating from a previous release, you should copy/move the `callattendant.db` file from the previous release to  `data/callattendant.db` in the new release.

***

## Configuration
Configuration file ...
```python
# SCREENING_MODE: A tuple containing: "whitelist" and/or "blacklist", or empty
SCREENING_MODE = ("whitelist", "blacklist")

# BLOCK_ENABLED: if True calls that fail screening will be blocked
BLOCK_ENABLED = True

# BLOCK_NAME_PATTERNS: A regex expression dict applied to the CID names
# Example: {"V[0-9]{15}": "Telemarketer Caller ID", "O": "Unknown caller"}
BLOCK_NAME_PATTERNS = {"V[0-9]{15}": "Telemarketer Caller ID", }

# BLOCK_NUMBER_PATTERNS: A regx expression dict applied to the CID numbers
# Example: {"P": "Private number",}
BLOCK_NUMBER_PATTERNS = {}

# BLOCKED_ACTIONS: A tuple containing a combination of the following actions:
#   "greeting", "record_message", "voice_mail".
#
# These actions are performed before hanging up.
#
# Note: the  "record_message", "voice_mail" actions are mutually exclusive.
# Also Note: A trailing comma is REQUIRED for a tuple with just one item
#
# Example: No actions, just hang_up
#   BLOCKED_ACTIONS = ()
#
# Example: Play an announcement before hanging up
#   BLOCKED_ACTIONS = ("greeting", )
#
# Example: Record a message before hanging up, no keypress required
#   BLOCKED_ACTIONS = ("record_message", )
#
# Example: Option to record a message; keypress required to leave message
#   BLOCKED_ACTIONS = ("voice_mail", )
#
# Example: Play announcment and record a message; no keypress required
#   BLOCKED_ACTIONS = ("greeting", "record_message" )
#
# Example: Play announcment and voice mail menu; keypress required to leave message
#   BLOCKED_ACTIONS = ("greeting", "voice_mail" )
#
BLOCKED_ACTIONS = ("greeting", "voice_mail")

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

## Web Interface
To view the __callattendant__ web interface, simply point your browser to port `5000` on your Raspberry Pi.
You can view the web interface from the Raspberry Pi itself and from any phone or computer that's on the 
same network as your Pi. Example URL:
```
http://<pi address|name>:5000
```

### The Call Log
The _Call Log_ is the main screen. It can be viewed by selecting _Calls_ from the main menu. 
It lists all the calls that have been screened by the __callattendant__.
The _Action_ column shows hows the __callattendant__ handled the call.
Clicking on a caller will take you to the [[Manage Caller|User-Guide#managing-callers]] page where you can manage the caller's
membership in the [[Permitted Numbers|User-Guide#viewing-permitted-numbers]] and/or 
[[Blocked Numbers|User-Guide#viewing-blocked-numbers]] lists.
##### _Action_
- __Permitted__: the caller is in the _Permitted Numbers_ list. The call was not blocked.
- __Blocked__: the caller is in the _Blocked Numbers_ list or was found to be a robocaller or other nuisance. The call was blocked.
- __Screened__: the caller was not found in the _Blocked Numbers_ list and did not appear as a robocaller or other nuisance. The call was not blocked.
##### _Call Log example_
![Call Log](https://github.com/emxsys/callattendant/blob/master/docs/call-log.png)

### Managing Callers
The _Manage Caller_ page is where you manage the caller's membership in the 
[[Permitted Numbers|User-Guide#viewing-permitted-numbers]] and/or 
[[Blocked Numbers|User-Guide#viewing-blocked-numbers]] lists. A caller can be
a member of both lists, however the _Permitted Number_ membership will have
precedence over the _Blocked Number_. This page is accessed by clicking/selecting
a caller on the [[Call Log|User-Guide#the-call-log]] page.
##### _Manage Callers example_
![Manage Caller](https://github.com/emxsys/callattendant/blob/master/docs/manage-caller.png)

### Managing Voice Messages
The _Messages_ page is where you listen to and/or delete voices messages left by blocked callers.
This page is accessed by selecting _Messages_ from the main menu.
##### _Messages example_
![Messages](https://github.com/emxsys/callattendant/blob/master/docs/messages.png)

### Viewing Permitted Numbers
The  _Permitted Numbers_ page is where you view the permitted numbers.
This page is accessed by selecting _Permitted_ from the main menu.
##### _Permitted Numbers example_
![Permitted Numbers](https://github.com/emxsys/callattendant/blob/master/docs/permitted-numbers.png)

### Viewing Blocked Numbers
The  _Blocked Numbers_ page is where you view the blocked numbers.
This page is accessed by selecting _Blocked_ from the main menu.
##### _Blocked Numbers example_
![Blocked Numbers](https://github.com/emxsys/callattendant/blob/master/docs/blocked-numbers.png)


