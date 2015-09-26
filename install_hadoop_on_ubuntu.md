# Install Hadoop on Ubuntu

## Install Java 7
sudo apt-get purge -y openjdk*
add-apt-repository ppa:webupd8team/java
apt-get update -y
apt-get install -y oracle-java7-installer

```
export JAVA_HOME=/usr/lib/jvm/java-7-oracle/
export JRE_HOME=$JAVA_HOME/jre
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin
```

## Add hadoop user

sudo useradd -m hadoop -s /bin/bash
sudo passwd hadoop
sudo adduser hadoop sudo

## Add ssh key

ssh-keygen -t rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

## Install hadoop

wget http://mirror.bit.edu.cn/apache/hadoop/common/stable2/hadoop-2.7.1.tar.gz
tar xzvf ./hadoop-2.7.1.tar.gz
sudo mv hadoop-2.7.1 /usr/lib/hadoop

```
export HADOOP_HOME=/usr/lib/hadoop
export PATH=$PATH :$HADOOP_HOME/bin
```

## Test standalone mode

cd /usr/lib/hadoop/
mkkdir input
cp ./etc/hadoop/*.xml ./input/
./bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-*.jar grep input output 'dfs[a-z.]+'
cat ./output/*
rm -R ./output

## Setup pesudo mode

vim /usr/lib/hadoop/etc/hadoop/core-site.xml

```
<configuration>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>file:/usr/lib/hadoop/tmp</value>
        <description>Abase for other temporary directories.</description>
    </property>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```

vim /usr/lib/hadoop/etc/hadoop/hdfs-site.xml

```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:/usr/lib/hadoop/tmp/dfs/name</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:/usr/lib/hadoop/tmp/dfs/data</value>
    </property>
</configuration>
```

bin/hdfs namenode -format

sbin/start-dfs.sh

jps

Go to http://localhost:50070

## Test pesudo mode

bin/hdfs dfs -mkdir -p /user/hadoop
bin/hdfs dfs -mkdir input
bin/hdfs dfs -put etc/hadoop/*.xml input
bin/hdfs dfs -ls input

bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-*.jar grep input output 'dfs[a-z.]+'
bin/hdfs dfs -cat output/*
rm -R ./output
bin/hdfs dfs -get output output
cat ./output/*
bin/hdfs dfs -rm -r /user/hadoop/output

sbin/stop-dfs.sh

## Setup cluster mode

sudo vim /etc/hostname

```
hadoop-master
```

sudo vim /etc/hosts

```
127.0.0.1	localhost
192.168.0.2     hadoop-master
192.168.0.5     hadoop-slave
```

cd /usr/lib/hadoop/etc/hadoop

vim slaves

```
hadoop-slave
```

vim core-site.xml

```
<configuration>
<property>
    <name>fs.defaultFS</name>
    <value>hdfs://hadoop-master:9000</value>
</property>
<property>
    <name>hadoop.tmp.dir</name>
    <value>file:/usr/lib/hadoop/tmp</value>
    <description>Abase for other temporary directories.</description>
</property>
</configuration>
```

vim hdfs-site.xml

```
<configuration>
<property>
    <name>dfs.namenode.secondary.http-address</name>
    <value>hadoop-master:50090</value>
</property>
<property>
    <name>dfs.namenode.name.dir</name>
    <value>file:/usr/lib/hadoop/tmp/dfs/name</value>
</property>
<property>
    <name>dfs.datanode.data.dir</name>
    <value>file:/usr/lib/hadoop/tmp/dfs/data</value>
</property>
<property>
    <name>dfs.replication</name>
    <value>1</value>
</property>
</configuration>
```

cp mapred-site.xml.template mapred-site.xml 

```
<configuration>
<property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
</property>
</configuration>
```

vim yarn-site.xml

```
<configuration>
<property>
    <name>yarn.resourcemanager.hostname</name>
    <value>hadoop-master</value>
</property>
<property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
</property>
</configuration>
```

scp ./core-site.xml hadoop@hadoop_slave:/usr/lib/hadoop/etc/hadoop/
scp ./hdfs-site.xml hadoop@hadoop_slave:/usr/lib/hadoop/etc/hadoop/
scp ./mapred-site.xml hadoop@hadoop_slave:/usr/lib/hadoop/etc/hadoop/
scp ./yarn-site.xml hadoop@hadoop_slave:/usr/lib/hadoop/etc/hadoop/

cd /usr/lib/hadoop/
bin/hdfs namenode -format
sbin/start-dfs.sh
sbin/start-yarn.sh

## Test cluster mode

bin/hdfs dfsadmin -report

bin/hdfs dfs -mkdir -p /user/hadoop
bin/hdfs dfs -mkdir -p input
bin/hdfs dfs -put etc/hadoop input 
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.1.jar grep input output 'dfs[a-z.]+'
bin/hdfs dfs -cat output/*

/usr/local/hadoop/sbin/mr-jobhistory-daemon.sh start historyserver

Go to http://master:8088/cluster
