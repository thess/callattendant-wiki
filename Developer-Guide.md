# Call Attendant Developer Guide
##### Contents
1. [[Software Project Setup|Developer-Guide#software-project-setup]]
1. [[Software Architecture|Developer-Guide#software-architecture]]
1. [[Software Development Plan|Developer-Guide#Software-Development-Plan]]
1. [[Additional Information|Developer-Guide#additional-information]]

***

## Software Project Setup
### Clone (or download) the Repository

You need a copy of this repository placed in a folder on your pi, e.g., `/pi/home/callattendant`. You can use a different folder if you want (I prefer `/home/pi/src/callattendant`). These instructions use the home folder just to keep them simple.

You can either clone this repository, or [download a zip file](https://github.com/emxsys/callattendant/archive/master.zip),
or download a specific release from [Releases](https://github.com/emxsys/callattendant/releases).

##### Clone with Git

Here's how clone the repository with `git` into your home folder. 

Using HTTPS
```bash
cd 
git clone https://github.com/emxsys/callattendant.git
cd callattendant
```

Using SSH (see [GitHub: About SSH](https://docs.github.com/en/github/authenticating-to-github/about-ssh))
```bash
cd 
git clone git@github.com:emxsys/callattendant.git
cd callattendant
```
##### Download Zip

If you [download the latest code](https://github.com/emxsys/callattendant/archive/master.zip) or a
specific release, the unzpped folder will be named `callattendant-master` or `callattendant-<release_tag>` 
depending on what you downloaded. You can rename it if you wish. Here's how unzip it into your home folder.

```bash
cd
unzip ~/Downloads/callattendant-master.zip 
cd callattendant-master
```

### Establish the Development Environment

The installation calls for Python3.x. 

#### Setup Virtual Environment

The following instructions create and activate a virtual environment named venv within the current folder:

```bash
# Install virtualenv - if not installed
sudo apt install virtualenv

# Create the virtual environment
virtualenv venv --python=python3

# Activate it
source venv/bin/activate
```

Now you're operating with a virtual Python. To check, issue the which command and ensure the output points to your virtual environment; and also check the Python version:
```bash
which python
# /home/pi/venv/bin/python

python --version
# Python 3.7.3
```

#### Install Packages

We've provided a requirements file called `requirements.txt`. Let's use it to
install the required packages. But first, navigate to the folder where the 
callattendant repository was placed, e.g., `/pi/home/callattendant`.

For the virtual environment:
```bash
$ pip install -r requirements.txt
```

In my install, lxml took a long time to build (because Raspberry Pi) but it's
worth the wait to get to that part of blocking spam calls. So chill and do
whatever it is you do while software builds.

### Initialization

There's a `__main__.py` file in the top-level package, so you can start the program using the package name. In the project directory, issue the following command:

```bash
python callattendant --help
# Usage: callattendant --config [FILE] --data-path [FOLDER]
# Options:
# -c, --config [FILE]		 load a python configuration file
# -d, --data-path [FOLDER]	 path to data and configuration files
# -f, --create-folder		 create the data-path folder if it does not exist
# -h, --help			 displays this help text
```

Running the software for the first time requires the creation of a data folder using the `--create-folder` option. The default location is `~/.callattendant`. You can override this location using the `--data-path` option.

```bash
python callattendant --create-folder
```
You should see output of the form:
```
Command line options:
  --config=None
  --data-path=None
  --create-folder=True

[Configuration]
  BLOCKED_ACTIONS = ('greeting', 'record_message')
  BLOCKED_GREETING_FILE = /home/pi/src/callattendant/callattendant/resources/blocked_greeting.wav
  BLOCKED_RINGS_BEFORE_ANSWER = 0
  BLOCK_ENABLED = True
  BLOCK_NAME_PATTERNS = {'V[0-9]{15}': 'Telemarketer Caller ID'}
  BLOCK_NUMBER_PATTERNS = {}
  DATABASE = callattendant.db
  DATA_PATH = /home/pi/.callattendant
  DB_FILE = /home/pi/.callattendant/callattendant.db
  DEBUG = False
  ENV = production
  PERMITTED_ACTIONS = ()
  PERMITTED_GREETING_FILE = /home/pi/src/callattendant/callattendant/resources/general_greeting.wav
  PERMITTED_RINGS_BEFORE_ANSWER = 4
  ROOT_PATH = /home/pi/src/callattendant/callattendant
  SCREENED_ACTIONS = ()
  SCREENED_GREETING_FILE = /home/pi/src/callattendant/callattendant/resources/general_greeting.wav
  SCREENED_RINGS_BEFORE_ANSWER = 0
  SCREENING_MODE = ('whitelist', 'blacklist')
  TESTING = False
  VOICE_MAIL_GOODBYE_FILE = /home/pi/src/callattendant/callattendant/resources/goodbye.wav
  VOICE_MAIL_GREETING_FILE = /home/pi/src/callattendant/callattendant/resources/general_greeting.wav
  VOICE_MAIL_INVALID_RESPONSE_FILE = /home/pi/src/callattendant/callattendant/resources/invalid_response.wav
  VOICE_MAIL_LEAVE_MESSAGE_FILE = /home/pi/src/callattendant/callattendant/resources/please_leave_message.wav
  VOICE_MAIL_MENU_FILE = /home/pi/src/callattendant/callattendant/resources/voice_mail_menu.wav
  VOICE_MAIL_MESSAGE_FOLDER = /home/pi/.callattendant/messages
{MSG LED OFF}
Starting the Flask webapp
Running Flask webapp
 * Serving Flask app "userinterface.webapp" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
```

Navigate to <pi_address> on port 5000 and you should see the Call Attendant web interface home page. Make a few calls to yourself to test the service.

### Unit Tests
`pytest` is used for unit testing. 

```bash
# Run from the top-level package in your project (your path may be different)
cd ~/callattendant/callattendant

# Runnning "python -m pytest ..." adds the current directory to the sys.path
python -m pytest ../tests
```

### Packaging Python Projects
Excerpts from https://packaging.python.org/tutorials/packaging-projects/

##### Build the distribution
```bash
# Make sure you have the latest versions of setuptools and wheel installed:
python -m pip install --user --upgrade setuptools wheel

# Now run this command from the same directory where setup.py is located:
python setup.py sdist bdist_wheel
```

##### Uploading to TestPyPI
Go to https://test.pypi.org/manage/account/#api-tokens and create a new API token; 

```bash
# You can use twine to upload the distribution packages. You’ll need to install Twine:
python -m pip install --user --upgrade twine

# Once intalled, run Twine to upload all of the archives under dist:
python -m twine upload --repository testpypi dist/*
```

##### Installing your newly uploaded package
You can use pip to install your package and verify that it works. Create a new virtualenv (see Installing Packages for detailed instructions) and install your package from TestPyPI:
```bash
python -m pip install --index-url https://test.pypi.org/simple/ --no-deps example-pkg-YOUR-USERNAME-HERE
```

***

## Software Architecture
This document provides a comprehensive architectural overview of the system, using a number of different architectural views to depict different aspects of the system. It is intended to capture and convey the significant architectural decisions which have been made on the system.

### Architectural Viewpoints
This section describes what software architecture is for the current system, and how it is represented. Of the Use-Case, Logical, Process, Deployment, and Implementation Views, it enumerates the views that are necessary, and for each view, explains what types of model elements it contains.

###### _Rational Unified Process 4+1 View_
![Alt text](https://github.com/emxsys/callattendant/blob/master/docs/design/images/RUP_41_View.png "RUP 4+1 View")

### Use Case View
This section contains use cases or scenarios from the use-case model if they represent some significant, central functionality of the final system, or if they have a large architectural coverage—they exercise many architectural elements or if they stress or illustrate a specific, delicate point of the architecture.

###### _Use Case Diagram_
![Alt text](https://github.com/emxsys/callattendant/blob/master/docs/design/images/Use_Case_View.png "Use Case Diagram")

### Logical View
This section describes the architecturally significant parts of the design model, such as its decomposition into subsystems and packages. And for each significant package, its decomposition into classes and class utilities. You should introduce architecturally significant classes and describe their responsibilities, as well as a few very important relationships, operations, and attributes.

###### _Class Diagram_
![Alt text](https://github.com/emxsys/callattendant/blob/master/docs/design/images/Logical_View.png "Logical View Diagram")

### Process View
This section describes the system's decomposition into lightweight processes (single threads of control) and heavyweight processes (groupings of lightweight processes). Organize the section by groups of processes that communicate or interact. Describe the main modes of communication between processes, such as message passing, interrupts, and rendezvous.

###### _Activity Diagram_
![Alt text](https://github.com/emxsys/callattendant/blob/master/docs/design/images/Process_View.png "Process View Diagram")

###### _Sequence Diagram_
![Alt text](https://github.com/emxsys/callattendant/blob/master/docs/design/images/Main_Sequence_Diagram.png "Main Sequence Diagram")

### Implementation View
This section describes the overall structure of the implementation model, the decomposition of the software into layers and subsystems in the implementation model, and any architecturally significant components.

###### _Component Diagram_
![Alt text](https://github.com/emxsys/callattendant/blob/master/docs/design/images/Implementation_View.png "Implementation Diagram")

### Deployment View
This section describes one or more physical network (hardware) configurations on which the software is deployed and run. It is a view of the Deployment Model. At a minimum for each configuration it should indicate the physical nodes (computers, CPUs) that execute the software and their interconnections (bus, LAN, point-to-point, and so on.) Also include a mapping of the processes of the Process View onto the physical nodes.
###### _Deployment Diagram_
![Alt text](https://github.com/emxsys/callattendant/blob/master/docs/design/images/Deployment_View.png "Deployment Diagram")

### Data View
This section contains a description of the persistent data storage perspective of the system. This section is optional if there is little or no persistent data, or the translation between the Design Model and the Data Model is trivial.

###### _Entity Relationship Diagram_
![Alt text](https://github.com/emxsys/callattendant/blob/master/docs/design/images/Data_View.png "Entity Relationship Diagram")

***

## Software Development Plan
The development plan's [phase objectives](https://github.com/emxsys/callattendant/projects?query=is%3Aopen+sort%3Acreated-asc) are captured in the GitHub projects.
### [Inception Phase](https://github.com/emxsys/callattendant/projects/1)
- [x] Iteration #I1: [v0.1](https://github.com/emxsys/callattendant/releases/tag/v0.1)
### [Elaboration Phase](https://github.com/emxsys/callattendant/projects/2)
- [x] Iteration #E1: [v0.2](https://github.com/emxsys/callattendant/releases/tag/v0.2)
### [Construction Phase](https://github.com/emxsys/callattendant/projects/3)
- [x] Iteration #C1: [v0.3](https://github.com/emxsys/callattendant/releases/tag/v0.3) Alpha
- [x] Iteration #C2: [v0.4](https://github.com/emxsys/callattendant/releases/tag/v0.4) Beta
- [x] Iteration #C3: [v0.5](https://github.com/emxsys/callattendant/releases/tag/v0.5) Release candidate
### [Transition Phase](https://github.com/emxsys/callattendant/projects/4)
- [x] Iteration #T1: [v1.0](https://github.com/emxsys/callattendant/releases/tag/v1.0)

***

## Additional Information


### Tools
#### SQLiteBrowser
You can use the SQLiteBrowser to examine and/or modify the __callattendant__ database.
```bash
sudo apt-get install sqlitebrowser
sqlitebrowser callattendant.db
```

### Raspberry Pi and US Robotics 5637 Modem Resources
- [US Robotics 5637 User Guide](https://support.usr.com/support/5637/5637-ug/ref_cmd_use.html)
- [USB Analog Modem with Raspberry Pi](https://iotbytes.wordpress.com/usb-analog-modem-with-raspberry-pi/)

The following blogs from [IoT Bytes by Pradeep Singh](https://iotbytes.wordpress.com/) were very useful 
for learning to how to program the Raspberry Pi and the US Robotics 5637 modem. His blog site has many 
Raspberry Pi resources. ___Thanks Pradeep!___

- [Incoming Call Details Logger with Raspberry Pi](https://iotbytes.wordpress.com/incoming-call-details-logger-with-raspberry-pi/)
- [Play Audio File on Phone Line with Raspberry Pi](https://iotbytes.wordpress.com/play-audio-file-on-phone-line-with-raspberry-pi/)
- [Record Audio from Phone Line with Raspberry Pi](https://iotbytes.wordpress.com/record-audio-from-phone-line-with-raspberry-pi/)



