
## Setup cluster mode

ssh root@42.62.101.103 -p 1024
ssh root@42.62.101.103 -p 1025
ssh root@42.62.101.103 -p 1026

## Use official tools

hadoop jar /usr/lib/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-client-jobclient-2.7.1.jar TestDFSIO -write -nrFiles 10 -fileSize 1000

## HiBench

git clone https://github.com/intel-hadoop/HiBench.git

vim conf/99-user_defined_properties.conf

```
hibench.hadoop.home             /usr/lib/hadoop/bin/hadoop
hibench.hdfs.master             hdfs://localhost:9000
```


```
  hibench.hadoop.home      The Hadoop installation location
  hibench.spark.home       The Spark installation location
  hibench.hdfs.master      HDFS master
  hibench.spark.master     SPARK master
```

<HiBench_Root>/bin/build-all.sh