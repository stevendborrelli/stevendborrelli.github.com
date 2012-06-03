---
layout: post
title: "OSX SSH key setup"
description: ""
category: 
tags: [hadoop, hadoop101, osx, lion, ssh ]
---
{% include JB/setup %}

Having completed the [hadoop installation from the last post](/2012/04/29/installing-hadoop-on-osx-lion/), we can now concentrate on getting it up and running by setting up ssh keys. 


##Quick Overview

* Enable 'Remote login' in Settings->Sharing
* run `ssh-keygen`
* `cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys2`
* `ssh localhost` to test the connection. 

If you are not familiar with setting up ssh keys, the rest of this post contains detailed instructions. If you have completed the above instructions, you can skip the rest of the post. 


##Setting up SSH connections 

[Secure shell](http://en.wikipedia.org/wiki/Secure_Shell) is used to create encrypted communication links between systems. Apple's OSX includes an SSH client and server. In order to use it, we have to enable remote login and then set up keys so that hadoop and other components can connect without needing to use a password. Hadoop mainly uses ssh for the start and stop scripts. 


####Turning on the SSH daemon
Open up the settings app. Under "Internet and Wireless", select the Sharing Icon. 

<img src="/assets/images/hadoop_tutorials/OSX_sharing_settings.png"/>

Next, make sure the "Remote Login" checkbox is selected. 

<img src="/assets/images/hadoop_tutorials/OSX_remote_login_settings.png"/>



####Generating SSH Keys
An ssh keypair will allow us to use ssh without requiring a password for every connection. We will generate a public key, which can be freely shared and copied to a system you want access to, and a private key, which is called your identification and is presented to remote servers. 

Run the `ssh-keygen` command. Supply your email and hit enter at the prompts:

	# ssh-keygen -C "you@your.email.com"
	Generating public/private rsa key pair.
	Enter file in which to save the key (/Users/hadoop/.ssh/id_rsa): 
	Enter passphrase (empty for no passphrase): 
	Enter same passphrase again: 
	Your identification has been saved in /Users/hadoop/.ssh/id_rsa.
	Your public key has been saved in /Users/hadoop/.ssh/id_rsa.pub.
	The key fingerprint is:
	2b:01:b6:37:48:f5:f0:a0:52:8e:37:d3:39:a3:55:09 you@your.email.com
	The key's randomart image is:
	+--[ RSA 2048]----+
	|    . E...       |
	|   + + B.        |
	|  o X * o        |
	|   = O o         |
	|    + + S        |
	|     . o .       |
	|      . .        |
	|       .         |
	|                 |
	+-----------------+

Please note anyone who has access to the `id_rsa` file will be able to access your system without needing a password. 



####Setting up SSH for public key login
In order to enable .ssh keys, append the contents of the `id_rsa.pub` public key to a file called `authorized_keys2`.  

	# cd ~/.ssh
	# cat "id_rsa.pub" >> authorized_keys2 
	
####Test the login
You should now be able to log into your local machine without a password. 

    # ssh localhost
    Last login: Wed May  2 08:51:03 2012 from localhost	
    # 


##Next Steps
Now that ssh keys are set up, we can configure and run programs like Hadoop, Hbase and MPI. 

The next post will detail getting Hadoop up and running. [Click to continue with Hadoop Configuration](/2012/05/03/hadoop-initial-configuration/index.html).  
