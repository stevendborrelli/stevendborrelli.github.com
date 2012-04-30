---
layout: post
title: "Installing Hadoop on OSX Lion"
description: ""
category: 
tags: [hadoop, hadoop101]
---
{% include JB/setup %}

The various [Apache Hadoop](http://hadoop.apache.org) projects have become a popular solution to the problem of storing and processing large amounts of data in a distributed fashion. While most of the Hadoop install documentation centers around Linux, it is fairly simple to get most of the components running on an OSX system using [homebrew](http://mxcl.github.com/homebrew/).

 

##Requirements 

For this install have two requirements: Java and the Homebrew package manager. Both are straightforward to install onto your system. 

####Java 
Hadoop and most of its supporting software is written in Java. There is a very good chance your Mac already has it installed.


To check the Java installation open a terminal and run the command `java -fullversion`. You should see output similar to the following:  

	# java -fullversion
    java full version "1.6.0_31-b04-415"

If you don't have Java installed, you will get a `command not found` error. Get [Java for OS X Lion](http://support.apple.com/kb/DL1515) from Apple. 
   
####Homebrew 
   
Homebrew is an easy easy way to install open source packages on OSX. There is a strong community behind the project, so there are good odds a recent version of the software you are looking for is available. 

Install homebrew using the instructions at the [Homebrew github](https://github.com/mxcl/homebrew/wiki/installation) site, or just run the command below:

	# /usr/bin/ruby -e "$(/usr/bin/curl -fksSL https://raw.github.com/mxcl/homebrew/master/Library/Contributions/install_homebrew.rb)"

##Installing Hadoop

####Checking homebrew for the Hadoop package
First, let's check brew to see what version of Hadoop is packaged. The `brew info` command will let us know the most recent version available: 

	# brew info hadoop
	hadoop 1.0.1
	http://hadoop.apache.org/common/
	/usr/local/Cellar/hadoop/1.0.1 (264 files, 57M) *
	https://github.com/mxcl/homebrew/commits/master/Library/Formula/hadoop.rb

	==> Caveats
	In Hadoop's config file:
  	/usr/local/Cellar/hadoop/1.0.1/libexec/conf/hadoop-env.sh
	$JAVA_HOME has been set to be the output of:
  	/usr/libexec/java_home

One of the nice things about the Homebrew Hadoop package is that hadoop environment file has already been updated with OSX's Java location. The `brew versions` command will allow you to see all the previous versions that have been packaged. 


####Install Hadoop 
Another nice feature of Homebrew is that we don't need to install things as root. To install a package, run `brew install <package name>`. 

	# brew install hadoop 	
	==> Downloading http://www.apache.org/dyn/closer.cgi?path=hadoop/core/hadoop-1.0.1/hadoop-1.0.1.tar.gz
	==> Best Mirror http://mirrors.gigenet.com/apache/hadoop/core/hadoop-1.0.1/hadoop-1.0.1.tar.gz
	######################################################################## 100.0%
	==> Caveats
	In Hadoop's config file:
  	/usr/local/Cellar/hadoop/1.0.1/libexec/conf/hadoop-env.sh
	$JAVA_HOME has been set to be the output of:
  	/usr/libexec/java_home
	==> Summary
	/usr/local/Cellar/hadoop/1.0.1: 264 files, 57M, built in 3 seconds

Note that Hadoop has been installed to `/usr/local/Cellar/hadoop/<version>`. Homebrew creates the following symlinks in `/usr/local/bin: 

	lrwxr-xr-x  1 steve  admin    33 Apr 29 22:54 hadoop -> ../Cellar/hadoop/1.0.1/bin/hadoop
	lrwxr-xr-x  1 steve  admin    43 Apr 29 22:54 hadoop-config.sh -> ../Cellar/hadoop/1.0.1/bin/hadoop-config.sh
	lrwxr-xr-x  1 steve  admin    43 Apr 29 22:54 hadoop-daemon.sh -> ../Cellar/hadoop/1.0.1/bin/hadoop-daemon.sh
	lrwxr-xr-x  1 steve  admin    44 Apr 29 22:54 hadoop-daemons.sh -> ../Cellar/hadoop/1.0.1/bin/hadoop-daemons.sh
	lrwxr-xr-x  1 steve  admin    30 Apr 29 22:54 rcc -> ../Cellar/hadoop/1.0.1/bin/rcc
	lrwxr-xr-x  1 steve  admin    36 Apr 29 22:54 slaves.sh -> ../Cellar/hadoop/1.0.1/bin/slaves.sh
	lrwxr-xr-x  1 steve  admin    39 Apr 29 22:54 start-all.sh -> ../Cellar/hadoop/1.0.1/bin/start-all.sh
	lrwxr-xr-x  1 steve  admin    44 Apr 29 22:54 start-balancer.sh -> ../Cellar/hadoop/1.0.1/bin/start-balancer.sh
	lrwxr-xr-x  1 steve  admin    39 Apr 29 22:54 start-dfs.sh -> ../Cellar/hadoop/1.0.1/bin/start-dfs.sh
	lrwxr-xr-x  1 steve  admin    52 Apr 29 22:54 start-jobhistoryserver.sh -> ../Cellar/hadoop/1.0.1/bin/start-jobhistoryserver.sh
	lrwxr-xr-x  1 steve  admin    42 Apr 29 22:54 start-mapred.sh -> ../Cellar/hadoop/1.0.1/bin/start-mapred.sh
	lrwxr-xr-x  1 steve  admin    38 Apr 29 22:54 stop-all.sh -> ../Cellar/hadoop/1.0.1/bin/stop-all.sh
	lrwxr-xr-x  1 steve  admin    43 Apr 29 22:54 stop-balancer.sh -> ../Cellar/hadoop/1.0.1/bin/stop-balancer.sh
	lrwxr-xr-x  1 steve  admin    38 Apr 29 22:54 stop-dfs.sh -> ../Cellar/hadoop/1.0.1/bin/stop-dfs.sh
	lrwxr-xr-x  1 steve  admin    51 Apr 29 22:54 stop-jobhistoryserver.sh -> ../Cellar/hadoop/1.0.1/bin/stop-jobhistoryserver.sh
	lrwxr-xr-x  1 steve  admin    41 Apr 29 22:54 stop-mapred.sh -> ../Cellar/hadoop/1.0.1/bin/stop-mapred.sh
	lrwxr-xr-x  1 steve  admin    42 Apr 29 22:54 task-controller -> ../Cellar/hadoop/1.0.1/bin/task-controller


##Next Steps

In the next post, I'll detail setting up SSH keys and configuring your new Hadoop installation. 
