---
layout: post
title: Offshore Publish
author: Matthew Mazer
permalink: /scripts/offshore-publish.html
---
{% highlight bash %}
#!/usr/bin/perl -w

use File::Find;
use File::Basename;

$FROM_DIR = '/Users/autobuild/Public/TEST-BUILDS';
$TO_DIR   = '/Users/autobuild/Public/OffshoreMirror/TEST-BUILDS';

find sub {
    if (-d $_) {
	$dir_to_create = $File::Find::name;
	$dir_to_create =~ s/^$FROM_DIR/$TO_DIR/;
	if (! -d $dir_to_create ) {
	    mkdir ($dir_to_create, 0755) or die "Can't create directory $dir_to_create: $!\n";
	}
    }
    if (-f $_) {
	$link_target = $File::Find::name;
	$link_dir = dirname($link_target);
	$link_dir =~ s/$FROM_DIR/$TO_DIR/;
	$link_name = basename($link_target);
	if (! -l "$link_dir/$link_name") {
	    symlink ($link_target, "$link_dir/$link_name") or
		die "Can't make a link $link_dir/$link_name -> $link_target: $!\n ";
	}
    }
}, $FROM_DIR;

exit 0;
{% endhighlight %}
