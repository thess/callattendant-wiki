#### Contents
1. [[Software Development Plan|Developer-Guide#Software-Development-Plan]]
2. [[Software Architecture|Developer-Guide#software-architecture]]
3. [[Software Project|Developer-Guide#software-project]]

***
## Overview
TODO

## Software Development Plan
The development plan's [phase objectives](https://github.com/emxsys/callattendant/projects?query=is%3Aopen+sort%3Acreated-asc) are captured in the GitHub projects.
### [Inception Phase](https://github.com/emxsys/callattendant/projects/1)
- [x] Iteration #I1: [v0.1](https://github.com/emxsys/callattendant/releases/tag/v0.1)
### [Elaboration Phase](https://github.com/emxsys/callattendant/projects/2)
- [x] Iteration #E1: [v0.2](https://github.com/emxsys/callattendant/releases/tag/v0.2)
### [Construction Phase](https://github.com/emxsys/callattendant/projects/3)
- [x] Iteration #C1: [v0.3](https://github.com/emxsys/callattendant/releases/tag/v0.3)
- [x] Iteration #C2: [v0.4](https://github.com/emxsys/callattendant/releases/tag/v0.4)
- [ ] Iteration #C3: [v0.5](https://github.com/emxsys/callattendant/issues?q=is%3Aopen+is%3Aissue+milestone%3A%22Release+0.5%22)
### [Transition Phase](https://github.com/emxsys/callattendant/projects/4)
- [ ] Iteration #T1: [v1.0](https://github.com/emxsys/callattendant/issues?q=is%3Aopen+is%3Aissue+milestone%3A%22Release+1.0%22)

## Software Architecture
### Architectural Viewpoints
###### _Rational Unified Process 4+1 View_
![Alt text](https://github.com/emxsys/callattendant/blob/master/docs/images/RUP_41_View.png "RUP 4+1 View")

### Use Case View
###### _Use Case Diagram_
![Alt text](https://github.com/emxsys/callattendant/blob/master/docs/images/Use_Case_View.png "Use Case Diagram")

### Logical View
###### _Class Diagram_
![Alt text](https://github.com/emxsys/callattendant/blob/master/docs/images/Logical_View.png "Logical View Diagram")

### Process View
###### _Activity Diagram_
![Alt text](https://github.com/emxsys/callattendant/blob/master/docs/images/Process_View.png "Process View Diagram")

###### _Sequence Diagram_
![Alt text](https://github.com/emxsys/callattendant/blob/master/docs/images/Main_Sequence_Diagram.png "Main Sequence Diagram")

### Implementation View
###### _Component Diagram_
![Alt text](https://github.com/emxsys/callattendant/blob/master/docs/images/Implementation_View.png "Implementation Diagram")

### Deployment View
###### _Deployment Diagram_
![Alt text](https://github.com/emxsys/callattendant/blob/master/docs/images/Deployment_View.png "Deployment Diagram")

### Data View
###### _Entity Relationship Diagram_
![Alt text](https://github.com/emxsys/callattendant/blob/master/docs/images/Data_View.png "Entity Relationship Diagram")

***

## Software Project
### Download Repository

You need a copy of this repository placed in a folder on your pi, e.g., `/pi/home/callattendant`.
You can either clone this repository, or [download a zip file](https://github.com/emxsys/callattendant/archive/master.zip),
or download a specific release from [Releases](https://github.com/emxsys/callattendant/releases).

##### Clone with Git

Here's how clone the repository with `git` into your home folder:

```bash
cd 
git clone https://github.com/emxsys/callattendant.git
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

### Setup

The installation calls for Python3.X.

#### Setup Virtual Environment
###### *Optional*

For development purposes, you might be best served by setting up a virtual environment.
If you intend to simply install and run the **callattendant** on a dedicated Raspberry Pi, 
you can skip this step and proceed with [Install Packages](#install-packages).

The following instructions create a virtual environment named _python3_ within the current
folder:

```bash
sudo apt install virtualenv

virtualenv python3 --python=python3

source python3/bin/activate
```

Now you're operating with a virtual Python. To check, issue the following
command:

```bash

$ which python
```

You should see output of the form:

```
(python3) pi@raspberryi:~/testing $ which python
/home/pi/testing/python3/bin/python
```

To make sure you're on 3.x as requested, issue:

```bash

$ python --version
```

You should see output of the form:

```
(python3) pi@raspberrypi:~/testing $ python --version
Python 3.7.3
```

#### Install Packages

We've provided a requirements file called `requirements.txt`. Let's use it to
install the required packages. But first, navigate to the folder where the 
callattendant repository was placed, e.g., `/pi/home/callattendant`.

```bash
$ pip install -r requirements.txt
```

In my install, lxml took a long time to build (because Raspberry Pi) but it's
worth the wait to get to that part of blocking spam calls. So chill and do
whatever it is you do while software builds.

### Initialization

In the `callattendant/src` directory, issue the following command:

```bash
python callattendant.py
```

You should see output of the form:

```
(python3) pi@raspberrypi:~/testing/callattendant/src $ python callattendant.py
CallLogger initialized
Blacklist initialized
Whitelist initialized
Modem COM Port is: /dev/ttyACM0
 * Serving Flask app "userinterface.webapp" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)

```

Navigate to <pi_address> on port 5000 and you should see the home page. Make a few calls to
yourself to test the service.


### Tools
You can use the SQLiteBrowser to examine and/or modify the __callattendant__ database.
```bash
sudo apt-get install sqlitebrowser
sqlitebrowser callattendant.db
```


### US Robotics 5637 Modem
- See IoT Bytes [USB Analog Modem with Raspberry Pi](https://iotbytes.wordpress.com/usb-analog-modem-with-raspberry-pi/)
- [US Robotics 5637 User Guide](https://support.usr.com/support/5637/5637-ug/ref_cmd_use.html)