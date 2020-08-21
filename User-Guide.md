##### Contents
1. [[Overview|User-Guide#overview]]
1. [[Navigation|User-Guide#navigation]]
1. [[Home Screen|User-Guide#home-screen]]
1. [[Viewing Call History|User-Guide#viewing-call-history]]
1. [[Viewing Calls|User-Guide#viewing-calls]]
1. [[Managing a Caller|User-Guide#managing-a-caller]]
1. [[Managing Voice Messages|User-Guide#managing-voice-messages]]
1. [[Managing Permitted Numbers|User-Guide#managing-permitted-numbers]]
1. [[Managing Blocked Numbers|User-Guide#managing-blocked-numbers]]


***
## Overview
### URL: `http://<pi-address>|<pi-hostname>:5000`
To view the web interface, simply point your web browser to port `5000` on your Raspberry Pi.
If you haven't used _ports_ before, you simply append the port number, prefixed with a colon,
to the web address, e.g., `:5000`.

For example, in your Raspberry Pi's browser, you can use:
```
http://localhost:5000
```
Here's a example using a typical IP address on a home network (your Raspberry Pi's address may be different):
```
http://192.168.1.254:5000
```
At my home, my Pi's host hame is `pi-blocker`, so I use this address:
```
http://pi-blocker:5000
```
Following is an example of the home page you will see:
##### _Home page examples on an IPad Pro and Pixel2 phone_
![Dashboard - Small](https://github.com/emxsys/callattendant/blob/master/docs/dashboard-responsive.png)

The __callattendant__'s web interface uses a responsive design: you can review your calls and messages on your phone,
tablet or computer.

You can view the web interface from any phone or computer that's on
the same network as the Pi.

***
## Navigation
The __callattendant__ uses a consistent menu across the entire application. The menu adapts to your screen size.  
##### _Main Menu_
![Main Menu](https://github.com/emxsys/callattendant/blob/master/docs/callattendant-navbar.png)

##### _Main Menu expanded on phone_
![Main Menu](https://github.com/emxsys/callattendant/blob/master/docs/callattendant-navbar-pixel2.png)

* **Call Attendant** displays the Home page / Dashboard. This menu item is always in view.
* **Calls** displays the Call Log / Call History page.
* **Permitted** displays the page for managing the membership in the Permitted Numbers list.
* **Blocked** displays the page for managing the membership in the Blocked Numbers list.
* **Messages** displays the page for playing and managing voice messages left by callers.
* **Help** displays this Wiki User Guide
* **Search** displays the call history for the given phone number

***

## Home Screen
### URL: `http://<pi-address>|<pi-hostname>:5000`
##### _Dashboard | Home page on IPad_
![Dashboard - Small](https://github.com/emxsys/callattendant/blob/master/docs/dashboard-ipad.png)

The Dashboard is the home page for the application. This screen provides metrics and convenient 
access to the last 10 calls.

---
#### Statistics
![Dashboard](https://github.com/emxsys/callattendant/blob/master/docs/dashboard-statistics.png)

Here we have some overall statistics since the Call Attendant was installed.
* **Calls processed** is the overall number of calls received. This is also a link to the Call Log.
* **Calls blocked** is the number of calls blocked by the Call Attendant.
* **Percent Blocked** is the percentage of calls that were blocked. This is also a link to the Calls per Day graph.

---
#### New Messages
![Dashboard](https://github.com/emxsys/callattendant/blob/master/docs/dashboard-messages-waiting.png)

This button appears when there are new voices messages waiting to be played. Clicking this button
will take you to the Messages page.

---
#### Recent Calls
![Dashboard](https://github.com/emxsys/callattendant/blob/master/docs/dashboard-recent-calls.png)

This section shows you the last 10 calls received.

* The Phone Numbers in the Caller column are links to the View Call page, where you can get all the details about the call.
* The Actions column shows you what action was taken by the Call Attendant, include whether a voice message was recorded, and whether it has been played or not.
* The color of the rows indicates whether the caller is currently in the Permitted Numbers list (green) or Blocked Numbers list (red).

---
#### Calls per Day
![Dashboard](https://github.com/emxsys/callattendant/blob/master/docs/dashboard-calls-per-date.png)

This stacked bar graph displays the number of calls per day for the last 30 days. 

---
#### Top Permitted Callers
![Dashboard](https://github.com/emxsys/callattendant/blob/master/docs/dashboard-top-permitted-callers.png)

This list shows the top permitted and screened callers (aka allowed callers) since the application was installed.

---
#### Top Blocked Callers
![Dashboard](https://github.com/emxsys/callattendant/blob/master/docs/dashboard-top-blocked-callers.png)

This list shows the top blocked callers (aka denied callers) since the application was installed.

***
## Viewing Call History
### URL: `http://<pi-address>|<pi-hostname>:5000/calls`

The _Call Log_ shows your entire call history. It can be viewed by selecting _Calls_ from the main menu. 
It lists all the calls that have been screened by the __callattendant__.
The _Action_ column shows how the __callattendant__ handled the call.
Clicking on a caller will take you to the [[Manage Caller|User-Guide#managing-callers]] page where you can manage the caller's
membership in the [[Permitted Numbers|User-Guide#viewing-permitted-numbers]] and/or 
[[Blocked Numbers|User-Guide#viewing-blocked-numbers]] lists.
##### _Action_
- __Permitted__: the caller is in the _Permitted Numbers_ list. The call was not blocked.
- __Blocked__: the caller is in the _Blocked Numbers_ list or was found to be a robocaller or other nuisance. The call was blocked.
- __Screened__: the caller was not found in the _Blocked Numbers_ list and did not appear as a robocaller or other nuisance. The call was not blocked.

##### _Call Log example_
![Call Log](https://github.com/emxsys/callattendant/blob/master/docs/call-log-ipad.png)

***
## Viewing Calls
### URL: `http://<pi-address>|<pi-hostname>:5000/calls/view/<call-number>`

##### _Viewing Call example on phone_
![Call Log](https://github.com/emxsys/callattendant/blob/master/docs/view-call-pixel2.png)
***

## Managing Callers
### URL: `http://<pi-address>|<pi-hostname>:5000/caller/manage/<call-number>`
The _Manage Caller_ page is where you manage the caller's membership in the 
[[Permitted Numbers|User-Guide#viewing-permitted-numbers]] and/or 
[[Blocked Numbers|User-Guide#viewing-blocked-numbers]] lists. A caller can be
a member of both lists, however the _Permitted Number_ membership will have
precedence over the _Blocked Number_. This page is accessed by clicking/selecting
a caller on the [[Call Log|User-Guide#the-call-log]] page.
##### _Manage Callers example_
![Manage Caller](https://github.com/emxsys/callattendant/blob/master/docs/manage-caller-pixel2.png)

***

## Managing Voice Messages
### URL: `http://<pi-address>|<pi-hostname>:5000/messages`
The _Messages_ page is where you listen to and/or delete voices messages left by blocked callers.
This page is accessed by selecting _Messages_ from the main menu.
##### _Messages example_
![Messages](https://github.com/emxsys/callattendant/blob/master/docs/messages-ipad.png)

***

## Viewing Permitted Numbers
### URL: `http://<pi-address>|<pi-hostname>:5000/permitted`
The  _Permitted Numbers_ page is where you view the permitted numbers.
This page is accessed by selecting _Permitted_ from the main menu.
##### _Permitted Numbers example_
![Permitted Numbers](https://github.com/emxsys/callattendant/blob/master/docs/permitted-numbers-ipad.png)

***

## Viewing Blocked Numbers
### URL: `http://<pi-address>|<pi-hostname>:5000/blocked`
The  _Blocked Numbers_ page is where you view the blocked numbers.
This page is accessed by selecting _Blocked_ from the main menu.
##### _Blocked Numbers example_
![Blocked Numbers](https://github.com/emxsys/callattendant/blob/master/docs/blocked-numbers-ipad.png)


