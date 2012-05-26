---
layout: post
title: "Hadoop initial configuration on OSX"
description: ""
category: 
tags: [hadoop101, hadoop]
---
{% include JB/setup %}

In this post we'll get hadoop confgured and copy a file to the hdfs filesystem. 

If you haven't already [installed the hadoop distribution](/2012/04/29/installing-hadoop-on-osx-lion/)
and [set up SSH keys](/2012/05/02/hadoop-osx-sshkey_setup/), please do so before proceeding. 

These instructions assume you are using the homebrew provided version of hadoop installed to `/usr/local/Cellar/hadoop/<version>`.  

##Setting up hadoop directories
We need to set a few variables and tell hadoop where to store files. Hadoop stores configuration information in XML files (`.xml`) and sources
envionment variables out of shell files (`.sh`) .  

Before we make any changes to the default settings, I like to take a git snapshot of the config directory. 
	
###(Optional) keep track of configuration settings in git
You can skip this part (it is not related to running hadoop), but you would be amazed at how little you remember what you changed during that last outage at 3AM. 

I like to keep track of any changes I make to configuration files using [git](http://git-scm.com/). Homebrew puts the configuration files in the `libexec/conf/` directory: 

	# cd /usr/local/Cellar/hadoop/<version>/libexec
	Initialized empty Git repository in /usr/local/Cellar/hadoop/1.0.1/libexec/conf/.git/
	# git init
	# git add .
	# git commit -m "Initial commit of default settings."
        [master (root-commit) 2540984] Initial commit of default settings.
 	16 files changed, 749 insertions(+), 0 deletions(-)
 	create mode 100644 capacity-scheduler.xml
 	create mode 100644 configuration.xsl
	create mode 100644 core-site.xml
	create mode 100644 fair-scheduler.xml
	create mode 100644 hadoop-env.sh
	create mode 100644 hadoop-metrics2.properties
	create mode 100644 hadoop-policy.xml
	create mode 100644 hdfs-site.xml
	create mode 100644 log4j.properties
	create mode 100644 mapred-queue-acls.xml
	create mode 100644 mapred-site.xml
	create mode 100644 masters
	create mode 100644 slaves
	create mode 100644 ssl-client.xml.example
	create mode 100644 ssl-server.xml.example
	create mode 100644 taskcontroller.cfg


###Fix Kerberos issue
The first file we will modify is `hadoop-env.sh`. This file sets environment variables which lets hadoop and other programs know where to find Java resources, the names of the master and slave nodes, and some other tuning options. Since we are just running on one node, we will leave everything unchanged except to fix Hadoop issue 7489. 
 
To fix the Jira issue [HADOOP-7489](https://issues.apache.org/jira/browse/HADOOP-7489)
Add the following line to your `hadoop-env.sh` file: 

	export HADOOP_OPTS="-Djava.security.krb5.realm= -Djava.security.krb.kdc="

###Configure data locations
In this example, I will use `/data/dfs` for our hadoop data, `/data/mapred` for mapreduce data, `/data/nn` for namenode files, and `/tmp/hdfs-${username}` for our hadoop tempfile location. 

In the homebrew installation, configuration files are located in `/usr/local/Cellar/hadoop/1.0.1/libexec/conf/`.

We need to update the `hdfs-site.xml` file and add the following between the `<configuration>` tags. 

The whole file should look like this: 

	<?xml version="1.0"?>
	<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
	
	<configuration>
	<property>
	 <name>dfs.data.dir</name>
	 <value>/data/dfs</value>
	</property>
	<property>
	  <name>dfs.name.dir</name>
	  <value>/data/nn</value>
	</property>
	</configuration>

Next, update `core-site.xml` to be similar to the following: 

	<?xml version="1.0"?>
	<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

	<configuration>
	  <property>
	  <name>hadoop.tmp.dir</name>
	  <value>/tmp/hdfs-${user.name}</value>
	  <description>A base for other temporary directories.</description>
	</property>
	<property>
	  <name>fs.default.name</name>
	  <value>hdfs://localhost:54310</value>
	  <description>The name of the default file system.  A URI whose
	  scheme and authority determine the FileSystem implementation.  The
	  uri's scheme determines the config property (fs.SCHEME.impl) naming
	  the FileSystem implementation class.  The uri's authority is used to
	  determine the host, port, etc. for a filesystem.</description>
	</property>
	</configuration>



Next update the `mapred-site.xml` file to look like this. 

	<?xml version="1.0"?>
	<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

	<configuration>
      <property>
	  <name>mapred.job.tracker</name>
	  <value>localhost:54311</value>
	  <description>The host and port that the MapReduce job tracker runs
	  at.  If "local", then jobs are run in-process as a single map
	  and reduce task.
	  </description>
	  </property>
	  <property>
	    <name>mapred.local.dir</name>
	    <value>/data/mapred/</value>
	  </property>
    </configuration>

###Create directories 
Now create the directories and set them to the account you want running hadoop. You can use the `whoami` account to 
get your id if you don't know it.  

`/data/dfs` will be the location of Hadoop data files. 
`/data/mapred` will hold 

         # sudo mkdir -p /data/ /data/dfs /data/mapred /data/nn  
         # sudo chown -R $(whoami)  /data/dfs /data/nn /data/mapred  

###Format the namenode 
You will only need to do this once. 

	# hadoop namenode -format  
	
	
###Start Hadoop
The `start-all.sh` script in `/usr/local/bin` will start all of the hadoop services:  
 
 
 	# start-all.sh 
 	
 	
###Create a test directory 
Run a simple test to see if your hdfs filesystems are working: 

	# hadoop fs -mkdir /gutenberg
	# hadoop fs -ls / 	

###Load a file into HDFS
We're going to load [Pride and Prejudice by Jane Austen](http://www.gutenberg.org/ebooks/1342) into our local hdfs filesystem in order to test functionality. 

    # curl http://mirrors.xmission.com/gutenberg/1/3/4/1342/1342.txt | hadoop fs -put - /gutenberg/1342.txt
    
    # hadoop fs -ls /gutenberg
    Found 1 items
	-rw-r--r--   3 steve supergroup     717599 2012-05-25 22:48 /gutenberg/1342.txt
    
###Run the wordcount example
We're now at the point where we can run our first hadoop example. A [word count application](http://wiki.apache.org/hadoop/WordCount) is included with the hadoop distribution. This command will create a directory `/output` in hdfs and put the results in it:


    # cd /usr/local/Cellar/hadoop/1.0.1/libexec
    # hadoop jar hadoop-examples*.jar wordcount /gutenberg /output
    
    ****hdfs://localhost:54310/gutenberg
	12/05/25 22:54:57 INFO input.FileInputFormat: Total input paths to process : 1
	12/05/25 22:54:57 INFO mapred.JobClient: Running job: job_201205252045_0001
	12/05/25 22:54:58 INFO mapred.JobClient:  map 0% reduce 0%
	12/05/25 22:55:13 INFO mapred.JobClient:  map 100% reduce 0%
	12/05/25 22:55:25 INFO mapred.JobClient:  map 100% reduce 100%

     …
      
     12/05/25 22:55:30 INFO mapred.JobClient:     Reduce input groups=13638
	 12/05/25 22:55:30 INFO mapred.JobClient:     Combine output records=13638
	 12/05/25 22:55:30 INFO mapred.JobClient:     Reduce output records=13638
	 12/05/25 22:55:30 INFO mapred.JobClient:     Map output records=124592


###Read the results file 
The output file will be called `part-r-00000` since we are only running on one node:

    # hadoop fs -ls /output 
     
    Found 3 items
	-rw-r--r--   3 steve supergroup          0 2012-05-25 22:55 /output/_SUCCESS
	drwxr-xr-x   - steve supergroup          0 2012-05-25 22:54 /output/_logs
	-rw-r--r--   3 steve supergroup     148319 2012-05-25 22:55 /output/part-r-00000
 
    # hadoop fs -cat /output/part-r-00000 | less
	"'After 1
	"'My    1
	"'Tis   2
	"A      12
	"About  2
	"Ah!    2
	"Ah!"   1
    ...

###Conclusion
Even though hadoop is primarily run on Linux clusters, having a version that runs on your Mac can make development and testing much more pleasant. 

In the next few posts, we'll set up additional components like hbase and pig. 

