##### Contents


***
## Web Interface

To view the __callattendant__ web interface, simply point your browser to your Raspberry Pi's port `5000`.
You can view the web interface from the Raspberry Pi's browser, and from any phone or computer that's on the 
same network as your Pi, e.g., `http://<address|hostname|localhost>:5000`

For example, in your Raspberry Pi's browser, you can use:
```
http://localhost:5000
```
And, for example, from your phone or computer, use your Raspberry Pi's host name or IP address:
```
http://<address>|<hostname>:5000
```

***

## The Call Log

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
![Call Log](https://github.com/emxsys/callattendant/blob/master/docs/call-log.png)

***

## Managing Callers
The _Manage Caller_ page is where you manage the caller's membership in the 
[[Permitted Numbers|User-Guide#viewing-permitted-numbers]] and/or 
[[Blocked Numbers|User-Guide#viewing-blocked-numbers]] lists. A caller can be
a member of both lists, however the _Permitted Number_ membership will have
precedence over the _Blocked Number_. This page is accessed by clicking/selecting
a caller on the [[Call Log|User-Guide#the-call-log]] page.
##### _Manage Callers example_
![Manage Caller](https://github.com/emxsys/callattendant/blob/master/docs/manage-caller.png)

***

## Managing Voice Messages
The _Messages_ page is where you listen to and/or delete voices messages left by blocked callers.
This page is accessed by selecting _Messages_ from the main menu.
##### _Messages example_
![Messages](https://github.com/emxsys/callattendant/blob/master/docs/messages.png)

***

## Viewing Permitted Numbers
The  _Permitted Numbers_ page is where you view the permitted numbers.
This page is accessed by selecting _Permitted_ from the main menu.
##### _Permitted Numbers example_
![Permitted Numbers](https://github.com/emxsys/callattendant/blob/master/docs/permitted-numbers.png)

***

## Viewing Blocked Numbers
The  _Blocked Numbers_ page is where you view the blocked numbers.
This page is accessed by selecting _Blocked_ from the main menu.
##### _Blocked Numbers example_
![Blocked Numbers](https://github.com/emxsys/callattendant/blob/master/docs/blocked-numbers.png)


