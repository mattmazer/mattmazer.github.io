---
layout: post
title: Announce a Release
author: Matthew Mazer
permalink: /scripts/announce-release.html
---
{% highlight bash %}
#!/bin/bash

# announceRelease shell script
# send to automation@companyredacted.com.  
# cc to engineering@companyredacted.com

IAM=$0

MYDIR=`dirname $IAM`

ANNOUNCE_REL=${MYDIR}/announceRelease.pl

CC=release@companyredacted.com
SERVER=server.companyredacted.com
FROM=release@companyredacted.com
ROOT=~/Public/TEST-BUILDS
ROOTUNC="\\\\\autobuild\\Public\\TEST-BUILDS"
# FTP address hidden
FTP="255.255.255.0/TEST-BUILDS"

$ANNOUNCE_REL --verbose --from $FROM --server $SERVER --root $ROOT  --rootunc $ROOTUNC --ftp $FTP "$@"

{% endhighlight %}