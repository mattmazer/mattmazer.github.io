---
layout: post
title: E-mail Address Counter
author: Matthew Mazer
permalink: /scripts/email-address-counter.html
---
{% highlight perl %}
#!/usr/local/bin/perl

use Email::Valid;

my $prompt="\nPlease enter the filename to be opened\:\nBy default, \"emailthis_log.dat\" is opened.\n\n";
print $prompt;

my $file=<STDIN>;   #get user input

# if the user does not enter anything for file name, a default file is opened
if ( $file eq "\n" || $file =~ /(^\s)+/)  {
    open (LOGFILE, "emailthis_log.dat") || die "Sorry, I cannot find emailthis_log.dat ($!)\n";
    
}

# otherwise, open the file the user inputted
else {		
    open (LOGFILE, $file) || die "Sorry, I cannot open file $file ($!)\n"; 
}

my $counter=0;       #counter for total e-mails
my $counter2=0;      #counter for unique e-mails

my $counterCom=0;
my $counterNet=0;
my $counterEdu=0;
my $counterOrg=0;
my $counterCountry=0;
my $counterOther=0;

#set contents of LOGFILE to array @file.  Every line of LOGFILE is an index in @file
my @file = <LOGFILE>;

#associative array (hash) to hold unique e-mail addresses.
my %emails;

#the foreach structure goes through each each index of @file and does the statements below

foreach $data (@file) {
    #split contents of @data by vertical bar
    @myArr=split(/\|/, $data);    
    
    #the splitted contents is in another array, so search that array for @ signs
    for($i=0;$i<=$#myArr;$i++) {
	$myArr[$i] =~ s/\s//g;  #get rid of any white space
	if($myArr[$i] =~ /\@/ && Email::Valid->address($myArr[$i])) {
	    
	    $emails{$myArr[$i]} = 1;
	    	    
	    #print "Found at $i:  $myArr[$i]\n\n";
	    $counter++;
	} #close if	
	
    } #close for
}  #close foreach

# This foreach prints out all of the unique e-mails. No doubles!
my @otherEmail;
my $num=0;	

my @foreign;
my $num1=0;

print "\n\n";
foreach $email (sort(keys(%emails))) {
    $counter2++;
    
    print "$email\n";
    if ($email =~ /\.com$/ || $email =~/\.COM$/) {
	$counterCom++;
    }
    elsif ($email =~ /\.net$/ || $email =~/\.NET$/) {
	$counterNet++;
    }
    elsif ($email =~ /\.edu$/ || $email =~/\.EDU$/) {
	$counterEdu++;
    }
    elsif ($email =~ /\.org$/ || $email =~/\.ORG$/){
	$counterOrg++;
    }
    elsif ($email =~ /\.(\w){2}$/) {
	$counterCountry++;
	$foreign[$num1]=$email;
	$num1++;
    }
    else {
	$counterOther++;
	$otherEmail[$num]=$email;
	$num++;
	
    }
}
print "---------------------------\n";
print "Total e-mails: $counter\n";
print "Unique e-mails: $counter2\n\n";	
print "Total e-mails from .com: $counterCom\n";
print "Total e-mails from .net: $counterNet\n";
print "Total e-mails from .edu: $counterEdu\n";
print "Total e-mails from .org: $counterOrg\n";
print "Total e-mails from foreign countries (or US school districts): $counterCountry\n";
for ($i=0;$i<=$#foreign;$i++) {
    print "     $foreign[$i]\n";
}

print "Total other e-mails: $counterOther\n";

for($i=0;$i<=$#otherEmail;$i++) {
    
    print "     $otherEmail[$i]\n";
	
}

close(LOGFILE);
{% endhighlight %}






































































