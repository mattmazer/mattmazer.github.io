---
layout: post
title: Disk Usage Display
author: Matthew Mazer
permalink: /scripts/disk-usage.html
---

{% highlight csh %}
#!/bin/csh -f

#the df -k command gets piped to awk
#awk will look for the pattern "/dev" at the beginning of each line
#for those lines it will sum up the 3rd field (the used field)
#when it has completed it will print the summary (total used disk space)


df -k | awk '/^\/dev/ {sum=sum+$3} END {print "Total disk space used is: ", sum}' 
{% endhighlight %}


