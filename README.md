# Implement Hadoop on AWS:

# 1. Deploy on AWS EC2 
# 2. Set 1 NameNode and 3 dataNodes
# 3. Authorization Setup:
	`
	sudo cp /etc/ssh/ssh_host_rsa_key ~/.ssh/id_rsa
	sudo cp /etc/ssh/ssh_host_rsa_key.pub ~/.ssh/id_rsa.pub (change to ubuntu@ in last ) 
	sudo cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys (change to ubuntu@ in last)
	sudo chown -R ubuntu:ubuntu /home/ubuntu/.ssh
	chmod 700 /home/ubuntu/.ssh
	chmod 600 /home/ubuntu/.ssh/authorized_keys
	` 
# 4. Environment Setup

   ## Install Java

	
	`
		sudo apt-get update
		sudo apt install default-jre
		sudo apt-get install default-jdk 
		cd~/
	`

   ## Download hadoop
	`	
		wget https://mirror.its.dal.ca/apache/hadoop/common/hadoop-3.4.0/hadoop-3.4.0.tar.gz
		sudo tar xzf hadoop-3.4.0.tar.gz
		sudo mv hadoop-3.4.0 /usr/local/hadoop
		sudo chown -R ubuntu /usr/local/hadoop/
	`
   ## Environmental Varialbe Setup:
	`
		chmod 777 ~/.profile
		vi ~/.profile
		export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64 #should specify java version
		export HADOOP_HOME=/usr/local/hadoop PATH="$HOME/bin:$HOME/.local/bin:$JAVA_HOME/bin:$HADOOP_HOME/bin:$PATH"
		source ~/.profile
		which java
		which hadoop
	`
   ## Hadoop Script Setup
	`
		cd /usr/local/hadoop/etc/hadoop/
	`
   ### hadoop-env.sh
	`export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64` #should specify java version

   ### core-site.xml
		`
		<configuration> 
			<property>
				<name>fs.default.name</name>
				<value>hdfs://<namenode_private_IP>:9000</value> 
			</property>
			<property>
				<name>hadoop.tmp.dir</name> 
				<value>/home/ubuntu/hadooptmp</value>
				<description>A base for other temporary directories.</description>
			</property> 
		</configuration>
		`

   ### hdfs-site.xml
		`
		<configuration> 
			<property>
				<name>dfs.replication</name>
				<value>3</value> 
			</property> 
			<property>
				<name>dfs.namenode.name.dir</name>
				<value>/usr/local/hadoop/hdfs/namenode</value> 
			</property>
			<property>
				<name>dfs.namenode.data.dir</name>
				<value>/usr/local/hadoop/hdfs/datanode</value> 
			</property>
		</configuration>
		`

   ### mapred-site.xml

		`
		<configuration>
	        <property>
	                <name>yarn.app.mapreduce.am.env</name>
	                <value>HADOOP_MAPRED_HOME=/usr/local/hadoop</value>
	        </property>
	        <property>
	                <name>mapreduce.map.env</name>
	                <value>HADOOP_MAPRED_HOME=/usr/local/hadoop</value>
	        </property>
	        <property>
	                <name>mapreduce.reduce.env</name>
	                <value>HADOOP_MAPRED_HOME=/usr/local/hadoop</value>
	        </property>
		</configuration>
		`

   ### yarn-site.xml

		`
		<configuration> 
			<property>
				<name>yarn.nodemanager.aux-services</name>
				<value>mapreduce_shuffle</value> 
			</property>
			<property>
				<name>yarn.nodemanager.auxservices.mapreduce.shuffle.class</name>
				<value>org.apache.hadoop.mapred.ShuffleHandler</value> 
			</property>
			<property>
				<name>yarn.resourcemanager.hostname</name> 
				<value><namenode_private_IP></value>
				<description>The hostname of the Resource Manager.</description>
			</property> 
		</configuration>
		`

   ## Hadoop Master-slaves Setup

		namenode:
		`
		vi /usr/local/hadoop/etc/hadoop/masters
		`

		datanodes:
		`
		cd /usr/local/hadoop/etc/hadoop/
		touch masters
		vi slaves
			`
				datanodes's private-ip
			`
		`

## create hdfs file system

		On namenode:
		`/usr/local/hadoop/bin/hdfs namenode -format`

## start hdfs and yarn

		Hdfs is responsible for maintaining hdfs distributed file system and yarn for containers and resource management.

		`/usr/local/hadoop/sbin/start-dfs.sh` 
		`/usr/local/hadoop/sbin/start-yarn.sh`

## verify if run successfully
		`hdfs dfsadmin -report`

## run example to verify if it start successfully
		`hadoop jar /usr/local/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.4.0.jar pi 10 1000`
		should specify version


# Implement MapReduce
	
## MapReduce Test
	1. Copy and Paste mapper.py and reducer.py for testing
	2. download sample file for testing
		`wget https://www.gutenberg.org/files/20417/20417-8.txt`
	3. test:
		`cat 20417-8.txt | ./mapper.py | sort -k1,1 | ./reducer.py`

	4. shell show the word, count pair 

## Implement MapReduce via Hadoop

	1. download text:
		`
		 mkdir ~/test_txt
		 cd ~/test_text
	     wget http://www.gutenberg.org/files/5000/5000-8.tx
		 wget http://www.gutenberg.org/files/4300/4300-0.txt
         wget http://www.gutenberg.org/cache/epub/20417/pg20417.txt
        `
    2. copy local file to hadoop filesystem
    	`hdfs dfs -copyFromLocal ~/test_text /user/ubuntu/gutenberg`

    3. start map-reduce job:

    	`
    		hadoop jar /usr/local/hadoop/share/hadoop/tools/lib/hadoop-streaming-3.4.0.jar -file /home/ubuntu/mapper.py -mapper /home/ubuntu/mapper.py -file /home/ubuntu/reducer.py -reducer /home/ubuntu/reducer.py -input /user/ubuntu/gutenberg/* -output /user/ubuntu/gutenberg-output
    	`









