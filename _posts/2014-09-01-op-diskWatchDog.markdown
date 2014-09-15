---
layout: post
title: Disk Watchdog 
author: Matthew Mazer
permalink: /scripts/disk-watchdog.html
---
{% highlight bash %}
#!/bin/bash

export PATH=${HOME}/Private/usr/local/bin:/usr/local/bin:/bin:/sbin:/usr/bin:/usr/sbin
#In order to fix hard link problem in DU module I edited the module to follow links by default! 

perl -I${HOME}/Private/usr/local/lib/perl5/site_perl \
    ${HOME}/Private/usr/local/bin/drivespace.pl "$@"

exit $?
{% endhighlight %}

Perl script drivespace.pl was created to be run on build machines to report drive space.  Using perl2exe the script was converted to an executable that is run on Windows Scheduler.

Usage for drivespace.pl
======================

default (no options) - show stats for all drives on screen.

<span style="font-family:Courier">
-t <threshold percentage> - mark drives that have reached threshold.<br/>
-t <threshold percentage> --to <address> [--from <address>] [--cc <address>] [--server <smtp address>]
[--verbose] – view smtp e-mail stats.<br />
--log <filename> - turns off STDOUT and writes report to a log file. <br />
--temp <path> - provide path for temporary file.  defaults to c:/temp

On each build box a directory, c:\rel, was created.  Usually, the c: drive is for system whereas d: is for data.  C:\engrel contains drivespace.exe and its log file.

Currently, the executable is set to run as follows:

Every 2 hours from 7:00am for 24 hours every day.

To set up Windows Scheduler:

1. Start → Programs → Accessories → System Tools → Scheduled Tasks
2. Click “Add Scheduled Task”
3. Choose the program to run.  Click ‘browse’ if program is not on the list.  
4. Choose frequency to run.
5. Choose time of day.
6. In this case, run as autobuild (with password).
7. If applicable, under the ‘advanced’ tab, enable ‘repeat task’.  In this case, drivespace is repeated every 24 hours for 2 hours.
8. Add comments if necessary.

