---
layout: post
title: Log With Time Stamp
author: Matthew Mazer
permalink: /scripts/log-with-time-stamp.html
---
{% highlight perl %}
#!/usr/bin/perl

# Turn off buffering on STDOUT
$| = 1;

while ($line = <>) {
    ($sec,$min,$hour,$mday,$mon,$year,$wday,$yday,$isdst) = localtime();
    $now = sprintf ("[%04d-%02d-%02d %02d:%02d:%02d] ",
		    (1900+$year, $mon+1, $mday,
		     $hour, $min, $sec));
    print $now . $line;
}

exit 0;
{% endhighlight %}
