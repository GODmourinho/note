
hadoop jar /usr/lib/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-client-jobclient-2.7.1.jar TestDFSIO -write -nrFiles 10 -fileSize 1000


## HiBench

git clone https://github.com/intel-hadoop/HiBench.git


2. 打开hibench-master/bin/hibench-config.sh，在第27-30行中补上Hadoop目录位置：

HADOOP_EXECUTABLE= /usr/lib/hadoop/conf/bin/hadoop
HADOOP_CONF_DIR = /usr/lib/hadoop/conf
HADOOP_EXAMPLES_JAR=/usr/lib/hadoop/hadoop-examples-1.0.3-Intel.jar



cd ./HiBench/conf/
cp 99-user_defined_properties.conf.template 99-user_defined_properties.conf

```
  hibench.hadoop.home      The Hadoop installation location
  hibench.spark.home       The Spark installation location
  hibench.hdfs.master      HDFS master
  hibench.spark.master     SPARK master
```

<HiBench_Root>/bin/build-all.sh