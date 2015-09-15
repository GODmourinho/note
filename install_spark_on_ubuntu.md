# Install Spark on Ubuntu

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

## Install Scala 2.10.5
wget http://downloads.typesafe.com/scala/2.10.5/scala-2.10.5.tgz
tar xzvf ./scala-2.10.5.tgz
mv scala-2.10.5 /usr/lib/

```
export SCALA_HOME=/usr/lib/scala-2.10.5
export PATH=$PATH:$SCALA_HOME/bin
```

## Install Spark 1.5
wget http://d3kbcqa49mib13.cloudfront.net/spark-1.5.0-bin-hadoop2.6.tgz
tar xzvf ./spark-1.5.0-bin-hadoop2.6.tgz
mv ./spark-1.5.0-bin-hadoop2.6/ /usr/lib/spark-1.5.0/

```
export SPARK_HOME=/usr/lib/spark-1.5.0
export PATH=$PATH:$SPARK_HOME/bin
```

## Test Spark
/usr/lib/spark-1.5.0/sbin/start-master.sh
/usr/lib/spark-1.5.0/sbin/start-slave.sh
/usr/lib/spark-1.5.0/bin/pyspark

```
rdd = sc.parallelize([1, 2, 3])
rdd.count()
```

## Setup cluster
Clone spark_master image to spark_slave1 and spark_slave2

cp /usr/lib/spark-1.5.0/conf/slaves.template /usr/lib/spark-1.5.0/conf/slaves

```
42.62.101.170
42.62.101.103
```

cp /usr/lib/spark-1.5.0/conf/spark-env.sh.template /usr/lib/spark-1.5.0/conf/spark-env.sh

```
export JAVA_HOME=/usr/lib/jvm/java-7-oracle/
export SCALA_HOME=/usr/lib/scala-2.10.5

export SPARK_MASTER_IP=42.62.101.165
#export SPARK_MASTER_IP=10.250.7.235
export SPARK_WORKER_MEMORY=2g
export SPARK_WORKER_INSTANCES=2
```

## Install ssh key
apt-get install -y openssh-server
ssh-keygen -t rsa
cat ~/.ssh/id_rsa.pub* >> ~/.ssh/authorized_keys
ssh-copy-id -i ~/.ssh/id_rsa.pub ntkhoa@192.168.85.136

## Add hostname to /etc/hosts

## Start automatically

Edit slaves.
Edit spark-env.sh

./sbin/start-all.sh

## Start manually

Edit the spark-env.sh and use ip in private network.

./sbin/start-master.sh
./sbin/start-slave.sh spark://42.62.101.165:7077

## Benchmark

apt-get install -y python-pip
pip install argparse
apt-get install -y git
git clone https://github.com/databricks/spark-perf.git

cp ./config/config.py.template ./config/config.py

```
SPARK_HOME_DIR = /usr/lib/spark-1.5.0
SPARK_CLUSTER_URL = "spark://%s:7077" % socket.gethostname()
SCALE_FACTOR = .05
SPARK_DRIVER_MEMORY = 512m
spark.executor.memory = 2g
```

Uncomment at least one SPARK_TESTS entry.