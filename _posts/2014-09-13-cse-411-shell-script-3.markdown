---
layout: post
title: Creating a Mail Alias File
author: Matthew Mazer
permalink: /scripts/create-mail-alias-file.html
---
{% highlight perl %}
#!/usr/bin/perl

#open armstrong password file and give it a perl file handle called ARMSTRONG
#note changed path to a local path for my purposes
open (ARMSTRONG, "passwd.armstrong") || die "Sorry, cannot open /util/passwd.armstrong.";

#open hadar password file and give it a perl file handle called HADAR
#note changed path to a local path for my purposes
open (HADAR, "passwd.hadar") || die "Sorry, cannot open /util//passwd.hadar.";

#open the file to write to, called aliases.  Perl will refer to it as TODOCFILE
open (TODOCFILE, ">aliases") || die "Sorry, I cannot open a file to write to (aliases)";

#each index in array armsList will be one line of the armstrong passwd file
@armsList=<ARMSTRONG>;

#each index in array hadaList will be one line of the hadar passwd file
@hadaList=<HADAR>;

#counters for loops to get usernames
$counter=0;
$counter1=0;

#get usernames from armstrong passwd file
foreach $data (@armsList) {
    @myArr=split(/\:/, $data);
    @namesArray[$counter]=@myArr[0];
    $counter++;
    
}

#get usernames from hadar passwd file
foreach $data1 (@hadaList) {
    @myArrHad=split(/\:/,$data1);
    @namesArrayHad[$counter1]=@myArrHad[0];
    $counter1++;

}

#check if user is in both armstrong and hadar
#foreach $data5(@namesArray) {  #this line follows old logic, was not that good.  Instead of using a nested foreach loop I put a for loop in the foreach loop so I can use Perl's splice method.

#Because users common to both armstrong and hadar will be aliased to hadar, I removed the common names from the armstrong list.  That way, when the loop is done I will have two unique lists.  I know this works because there is 1279 total users on hadar and 3916 users on armstrong.  Adding these two yields 5195 but subtract out the common users to get an answer of 4737.  There are 4737 users in the alias file. 

foreach $data4 (@namesArrayHad) {                  #for each user in hadar
    for($i=0; $i<=$#namesArray; $i++) {            #set up for loop to check against
	if ($data4 eq $namesArray[$i]) {           #if they are equal
	    splice(@namesArray,$i,1,());           #remove from armstrong list   
     
#this was old stuff not needed anymore but kept for reference
#printf(TODOCFILE "%s:%s\@hadar.cse.buffalo.edu\n",$data4,$data4);
#print ("Match at $data4 and $data5\n");

	}  #close if
    }  #close for
}   #close foreach
	
#actually write the alias file
foreach $data10(@namesArray) {
    printf(TODOCFILE "%s:%s\@armstrong.cse.buffalo.edu\n",$data10,$data10);
}

foreach $data11(@namesArrayHad) {
    printf(TODOCFILE "%s:%s\@hadar.cse.buffalo.edu\n",$data11,$data11);
}

#close file handles
close(TODOCFILE);  
close(ARMSTRONG);
close(HADAR);

#exit program
exit(0);
{% endhighlight %}
