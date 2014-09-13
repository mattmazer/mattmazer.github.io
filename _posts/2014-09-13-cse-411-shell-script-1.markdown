---
layout: post
title: Display Primary Group For User
author: Matthew Mazer
permalink: /scripts/primary-group-user.html
---
{% highlight csh %}
#!/bin/csh -f               
#indicate that file is a shell script and what shell should be used to interpret it

#if the user did not enter a command line argument (a username) then print message and exit

if("$1" == "") then
echo "This program requires a command line argument which should be a username"
exit
endif 


#have grep filter out passwd file to return line with the command line ($1) entry.
#in other words, grep will return the line with the username on it  
#then pipe it to awk to print out the 4th field which is the primary group number

set var1 = `grep $1 /etc/passwd | awk -F: '{print $4}'`

#if the variable var1 is empty then that means that there was no username in the passwd file
#the script will then print an error message to user and exit

if ($var1 == "") then
echo "Sorry, you have entered an invalid username"

else
#awk -F: '/'$var1'/ {print $1}' /etc/group  #this was the original, very simple and not specific

#this awk searches for the pattern "at the beginning of the line look for any amount of characters followed by a : then look for any amount of characters followed by a : and then search for the variable", which is the primary group number.
#In other words, it will search the 3rd field of each row (primary group number) of the group file.
awk -F: '/^.*:.*:'$var1':/ {print $1}' /etc/group
endif
{% endhighlight %}
