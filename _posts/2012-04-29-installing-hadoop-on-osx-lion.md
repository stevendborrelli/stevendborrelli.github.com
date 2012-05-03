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

One of the nice things about the Homebrew Hadoop package is that Hadoop environment file (hadoop-env.sh) has already been updated to conform with OSX's Java location. The `brew versions` command will allow you to see all the previous versions that have been packaged. 


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

Note that Hadoop has been installed to `/usr/local/Cellar/hadoop/<version>`. Homebrew creates the following symlinks in `/usr/local/bin`: 

	/usr/local/bin/hadoop -> /usr/local/Cellar/hadoop/1.0.1/bin/hadoop
	/usr/local/bin/hadoop-config.sh -> /usr/local/Cellar/hadoop/1.0.1/bin/hadoop-config.sh
	/usr/local/bin/hadoop-daemon.sh -> /usr/local/Cellar/hadoop/1.0.1/bin/hadoop-daemon.sh
	/usr/local/bin/hadoop-daemons.sh -> /usr/local/Cellar/hadoop/1.0.1/bin/hadoop-daemons.sh
	/usr/local/bin/rcc -> /usr/local/Cellar/hadoop/1.0.1/bin/rcc
	/usr/local/bin/slaves.sh -> /usr/local/Cellar/hadoop/1.0.1/bin/slaves.sh
	/usr/local/bin/start-all.sh -> /usr/local/Cellar/hadoop/1.0.1/bin/start-all.sh
	/usr/local/bin/start-balancer.sh -> /usr/local/Cellar/hadoop/1.0.1/bin/start-balancer.sh
	/usr/local/bin/start-dfs.sh -> /usr/local/Cellar/hadoop/1.0.1/bin/start-dfs.sh
	/usr/local/bin/start-jobhistoryserver.sh -> /usr/local/Cellar/hadoop/1.0.1/bin/start-jobhistoryserver.sh
	/usr/local/bin/start-mapred.sh -> /usr/local/Cellar/hadoop/1.0.1/bin/start-mapred.sh
	/usr/local/bin/stop-all.sh -> /usr/local/Cellar/hadoop/1.0.1/bin/stop-all.sh
	/usr/local/bin/stop-balancer.sh -> /usr/local/Cellar/hadoop/1.0.1/bin/stop-balancer.sh
	/usr/local/bin/stop-dfs.sh -> /usr/local/Cellar/hadoop/1.0.1/bin/stop-dfs.sh
	/usr/local/bin/stop-jobhistoryserver.sh -> /usr/local/Cellar/hadoop/1.0.1/bin/stop-jobhistoryserver.sh
	/usr/local/bin/stop-mapred.sh -> /usr/local/Cellar/hadoop/1.0.1/bin/stop-mapred.sh
	/usr/local/bin/task-controller -> /usr/local/Cellar/hadoop/1.0.1/bin/task-controller


##Next Steps

In the next couple of posts, I'll detail [setting up SSH keys](/2012/04/29/installing-hadoop-on-osx-lion). Then we'll configure and run your new Hadoop installation. 
