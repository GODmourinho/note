# GopherChina

## Go for artifact intelligent

Python is the ah-hoc language, not suitiable for large problem.

Use kubernetes for search engine in cluster mode.

Search billion data with million indexes within one second. Use modified wukon.

Search to get the users. Compute their data. Compare to get result.

Machine learning in one day is not enough because users change many times a day. We need online.

Go is not really good enough for machine learning. Cpp and python may be better.

All search data is in memory, just for speed.


## TiDB and TiKV

Use TiKV as Spanner and implement coprocessor there.

The performance of protobuf in 

Rust don't have gRPC. So just protobuf.

Cgo call has overhead and easy for memory leak.

Placement driver(the master) in spanner to know all regions to split, merge, rebalance and route.

Use MPP framework to pass the condition implement SQL layer.

Raft has the feture of configration change to rebalance data.

Use origin Datum to replace interface{} to support mysql type, which has 10 times performance.

Go is easy for concurrency, straming(pipeline to poll data), worker pool, connection pool.

pprof is damn greate to get profile.

Integrated with etcd and k8s.

Asked to sort interface with the alpha order.

Drop the proxy to build another database.

Use prefix or rebalance to seperate regions. Limit the slow request. Use container.

CocorckDB doesn't use push down so the SQL request is 10 times slower.

## Go in Baidu Front-end

Baidu has the united level 7 interface, support HTTP/HTTPS/SPDY/HTTP2

Redirect as stateless http requests

BFE is for redirect, rebalance, anti-affect, data anylics

The problems of C, hard to modify, debug and configure, not stable for updating

Sla of latency is from 1ms to 10ms.

One http request to 10 objects. 100000 objects to 1ms. Has 1 million http requests 400ms latency.

Use cgo. Reduce object numbers by merging small objects. Use object pool.

Use multiple child processes to accept request. Different time use different child process. Manually manage gc. Trigger gc one by one. Different child holds different connections.

Differenct processes listen the same port after Linux 3.0.

Go 1.5 still has millions second stop the world gc.

Use original test and pprofile. Expose process metrics by internal http service instead of log.

Baidu: golang lab, golang committee, golang good coder exam.

Golang panic recovery.

Expose http API to implement reload function.

## Go in object store

Null.

## Go for web development

The function of expose variables to json format. You can add new ones.

## Go at CoreOS

Go profile tool to benchmarking and profiling

Example: ./a -test.run=xxx -test.bench Full

The code is hard to understand so we just do profiling first

The IDE SublineTest

Online profiling with the port. Use sample so it costs less.

It's possible to profile memory and CPU.

## Go for mobile development

Use gomobile.

## Go profiling and benchmarking

Let's get back to the fist time we learn programming, we learn less and know more.

Use benchstat to compare the result of benchmark.

Read the artitle "How to write benchmakrs in Go".

Set the runtiem parameter to print all gc logs.

Generating garbage slows the problem and we should not concate two string to generate the third.

You should preallocate the capacity in memory if you know the length.

Always set timeout and don't start io operations without knowing how much time it will take.

Profile your code to identity the bottlenecks, do not guess.

## Go in AliCloud CDN

Use the traditional software like Squid, use lvs and haproxy.

Use LVM, Tengine and their own cache service, swift in cache servers.

From Hadoop to ODPS, which is developed by themself.

Use machine learning to predict the traffic then preallocate the policy.

We need monitor and metrics to know which one node is better and use it for others.

The first thing if you have performance bottlenet is monitoring then optimize the code.

Modify vcs.go to go get internal. Or monify gitlab to go get with the meta parameter. Or add new proxy.

Pass the request id for every reqeust. Collect log to anylics.


## Go in Xiami Website



 