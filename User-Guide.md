##### Contents


***
## Web Interface
### URL: `http://<pi-address>|<pi-hostname>:5000`

##### _Dashboard | Home page examples on an IPad Pro and Pixel2 phone_
![Dashboard - Small](https://github.com/emxsys/callattendant/blob/master/docs/dashboard-responsive.png)
---
The __callattendant__'s web interface uses a responsive design: you can review your calls and messages on your phone,
tablet or computer.

To view the web interface, simply point your browser to port `5000` on your Raspberry Pi.
If you haven't used _ports_ before, you simply append the port number, prefixed with a colon
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
You can view the web interface from any phone or computer that's on
the same network as the Pi.

***

## Dashboard | Home
##### _Dashboard | Home page on IPad_
![Dashboard - Small](https://github.com/emxsys/callattendant/blob/master/docs/dashboard-ipad.png)



***

## The Call Log | Call History

The _Call Log_ is the main screen. It can be viewed by selecting _Calls_ from the main menu. 
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
Call details
##### _Viewing Call example on phone_
![Call Log](https://github.com/emxsys/callattendant/blob/master/docs/view-call-pixel2.png)
***

## Managing Callers
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
The _Messages_ page is where you listen to and/or delete voices messages left by blocked callers.
This page is accessed by selecting _Messages_ from the main menu.
##### _Messages example_
![Messages](https://github.com/emxsys/callattendant/blob/master/docs/messages-ipad.png)

***

## Viewing Permitted Numbers
The  _Permitted Numbers_ page is where you view the permitted numbers.
This page is accessed by selecting _Permitted_ from the main menu.
##### _Permitted Numbers example_
![Permitted Numbers](https://github.com/emxsys/callattendant/blob/master/docs/permitted-numbers-ipad.png)

***

## Viewing Blocked Numbers
The  _Blocked Numbers_ page is where you view the blocked numbers.
This page is accessed by selecting _Blocked_ from the main menu.
##### _Blocked Numbers example_
![Blocked Numbers](https://github.com/emxsys/callattendant/blob/master/docs/blocked-numbers-ipad.png)


