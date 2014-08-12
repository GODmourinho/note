# Mesa: 全球备份, 近乎实时, 可拓展数据仓库

> 翻译自 [Mesa: Geo-Replicated, Near Real-Time, Scalable Data Warehousing](http://static.googleusercontent.com/media/research.google.com/en//pubs/archive/42851.pdf)

<div align="center">
Ashish Gupta, Fan Yang, Jason Govig, Adam Kirsch, Kelvin Chan, Kevin Lai, Shuo Wu, Sandeep Govind Dhoot, Abhilash Rajesh Kumar, Ankur Agiwal, Sanjay Bhansali, Mingsheng Hong, Jamie Cameron, Masood Siddiqi, David Jones, Jeff Shute, Andrey Gubarev, Shivakumar Venkataraman, Divyakant Agrawal
</div>

<div align="center">
Google, Inc.
</div>## 概述
Mesa是一个高度可拓展的分析数据仓库，它存储了与Google互联网广告业务相关的关键数据。Mesa是设计用于满足用户和系统复杂和具有挑战性的需求集，其中包括近乎实时的数据输入和查询，同时要实现大数据和查询集的高可用、可靠性、错误容忍性和可拓展性。特别的是，Mesa能处理PT级的数据，每秒完成数百万行数据的更新，每天服务几十亿次查询和万亿次数据读取操作。Mesa能够夸多个数据中心备份，并且在低延时提供一致的和可重复的请求响应，即使一整个数据中心垮了也没问题。这篇论文介绍了Mesa系统并报告了它所实现的性能和可拓展性。

## 1. 介绍

Google在多个地域运行着一个可拓展的广告平台，它每天为全球的用户提供数十亿广告服务。与每个广告相关的详细信息，例如目标的标准、给人留下印象和点击的数量等等，都会实时记录并处理。这些数据广泛地用在Google不同的用例上，包括报告、内部审计，分析，计费和预测。广告要在推广效果上获得良好的效果，必须通过与复杂的前端服务交互来解决对底层数据的在线和按需查询。Google的内部广告服务平台使用实时数据来确定预算和之前已经上线的广告性能，用以提高现在和未来广告服务的相关性。作为Google广告平台就需要继续拓展，而作为内部和外部用户则要求对他们广告行为更大的可视化，对更多细节和细粒度信息的需求导致了原始数据大小的急速增长。可拓展性和数据的重要性导致了针对处理、存储和查询的技术和可操作性的独特挑战。对这种数据存储的需求如下：

**原子性更新。**一个单独的用户行为可能导致对相关数据的多次更新，影响着数千个一致性的视图，这些视图由一些指标集组成（如点击和花费）而且跨越一系列维度（例如广告商和国家）。它绝不可能在部分更新完成时就查询到系统的某个状态。

**一致性和正确性。**由于商业和法律的原因，这个系统必须返回一直而且正确的数据。我们要求强一致性并且是可重复的，即使这个请求跨越了多个数据中心。
 
**可用性。**这个系统必须没有单点故障。在预计和没有预计到的维护和失败下也没有停机时间，这包括整个数据中心或者整个地域停运的影响。

**近乎实时的更新吞吐量。**这个系统必须支持持续的更新，不管是新的行数据还是对现有行的增量更新，在一秒内数百万行数据的顺序上实现更新。这个更新操作在几分钟内对跨越不同视图和数据中心的一致性请求都必须是有效的。
**查询性能。**这个系统必须支持对延时敏感的用户，为有低延时需求的用户和要求高吞吐的批处理用户服务。总而言之，这个系统必须支持单点查询在99%的情况下延时低于几百毫秒，并且总体的查询吞吐量一天要达到数万亿行。

**可拓展性。**这个系统必须能够随着数据量和请求数的增长而拓展。例如，它必须能够支持万亿行和PT级的数据。更新和查询的效率必须在这些数据增长得很大时维持住。

**在线数据和元数据转换。**为了支持新的特性和对现有数据粒度的改变，客户端通常要求改变数据Schema或者修改现有数据的值。这些改变必须不会影响到普通的查询和更新操作。

Mesa是Google对于这些技术性和可操作性难题的解决方案。即使这些需求的子集已经被现有的数据仓库解决了，Mesa是唯一同时为商业关键数据解决所有这些难题的。

Mesa是一个分布式、可备份并且高可用的结构化数据处理、存储和查询系统。Mesa处理由上层服务生成的数据，聚合然后持久化这些数据到内部，并且通过用户的查询提供数据的服务。即使这篇论文大部分都在讨论将Mesa应用在广告系统中，Mesa其实是一个通用的数据仓库解决方案，用以满足上述的所有需求。

Mesa综合了Google的基础架构和服务，例如Colossus（Google的下一代分布式文件系统）[22, 23]、BigTable[12]和MapReduce[19]。为了实现存储的可拓展性和可用性，数据进行水平分区和复制。更新操作将应用在单个表或者多个表的粒度上。为了实现在更新时一致而且可重复的查询，底层存储的数据都是有多个版本的。为了实现可拓展的更新，数据更新都是批量处理、同时分配一个新的版本号和周期性（例如每几分钟）合并到Mesa中。为了实现在多个数据中心之间更新的一致性，Mesa使用基于Paxos[35]的分布式同步协议。

大部分基于关系型技术和数据魔方[25]的商业数据仓库都不支持每隔几分钟连续的数据集成和聚合，同时不能对用户的查询提供几乎实时的响应。通常，这些解决方案与传统的企业氛围相关，他们不太频繁地将数据聚合到数据仓库，例如一天或者一周一次。相似的，没有Google处理大数据内部技术可以应用于此，例如BigTable[12]、Megastore[11]、Spanner[18]和F1[41]。BigTable不支持Mesa应用的原子性需求。同时Megastore、Spanner和F1（这三个都是针对在线的事务处理）能够在全球同步的数据中提供强一致性，但他们不支持Mesa客户端要求峰值更新吞吐量。然而，Mesa借鉴了基于Spanner的BigTable和Paxos技术用于存储和管理元信息。

最近的研究结果也要求数据分析和数据存储要能够动态拓展。Wong[49]已经开发了一个系统可以在云中提供大批量并行分析服务。然而，这个系统是设计与多租户的环境，它有大量用户和相对较小的数据痕迹。Xin[51]已经实现了Shark利用分布式共享内存来支持拓展数据分析。然而Shark只专注于内存中的处理和分析性查询。Athanassoulis[10]已经提出了MaSM（物化排序合并）算法，它可用于结合闪存存储设备来支持数据仓库的在线更新。

这篇论文的关键贡献在于：

* 我们展示了我们如何创建一个PT级数据仓库系统，它支持需要事务处理的ACID语义，并且能够拓展到好大的吞吐量来处理Google的广告指标。

* 我们介绍了一个版本管理系统，它通过批处理更新来实现更新操作的低延时和高吞吐量，查询操作也达到同样低延时高吞吐量的性能。

* 我们描述了一个高度可拓展的分布式架构，它在一个数据中心对于机器和网络失败可以弹性容忍。我们同样展示了解决数据仓库错误的全球同步架构。与我们的设计有所区别的是应用数据是通过在独立而且冗余的多个数据中心异步备份，然而只有关键的元数据是通过复制状态同步到所有副本。这种技术最小化了在多个数据中心管理副本的开销，同时提供了非常高的更新吞吐量。

* 我们展示了如果动态而有效地改变大量表的Schema，同时不影响已存在应用的正确性和性能。

* 我们描述了用于承受软硬件故障导致的问题和数据损坏的关键技术。

* 我们 描绘了维护一个可拓展系统的一些可操作性挑战，在正确性、一致性、性能强和新研究能做贡献的领域来提高最先进的技术。

论文其他部分的组织如下。第二部分描述Mesa得存储子系统。第三部分展示Mesa系统的架构和它的跨数据中心部署。第四部分展示Mesa的一些高级功能和特性。第五部分报告Mesa开发的经验，而第六部分报告Mesa生成环境下部署的指标。第七部分回顾了相关同坐，而第八部分总结这篇论文。

## 2. Mesa存储子系统

Mesa中的数据是持续生成的，它是Google最大而且最有价值的数据之一。对这些数据的分析查询有简单的查询，如“在特定一天某个特别广告商的广告有多少点击率？”，或者是更多查询的场景，如“在十月第一个的8点到11点显示在google.com针对美国地区使用移动设备的广告商有多少广告点击率？”在Mesa中数据本质是多维的，它包括在Google广告平台多维度的细微的数据。这些数据主要由两种属性组成：多维属性（我们成为键）和度量属性（我们成为值）。细微很多维度属性是分层的（并且甚至有多个层次等等，数据维度能以日/月/年或者财政的周/季/年来组织），一个单个数据局域这些维度层次可以聚合到多个物化视图中，它支持数据的向下聚合和向上聚合。一个稳定的数据仓库设计要求单个属性的存在是一致的，不管经过任何可能的方式进行物化和聚合。

![](image/Mesa1.png)

### 2.1 数据模型

In Mesa, data is maintained using tables. Each table has a table schema that specifies its structure. Specifically, a table schema specifies the key space K for the table and the corresponding value space V , where both K and V are sets. The table schema also specifies the aggregation func- tion F : V ×V → V which is used to aggregate the values corresponding to the same key. The aggregation function must be associative (i.e., F(F(v0,v1),v2) = F(v0,F(v1,v2) for any values v0 , v1 , v2 ∈ V ). In practice, it is usually also commutative (i.e., F(v0,v1) = F(v1,v0)), although Mesa does have tables with non-commutative aggregation func- tions (e.g., F(v0,v1) = v1 to replace a value). The schema also specifies one or more indexes for a table, which are total orderings of K.The key space K and value space V are represented as tu- ples of columns, each of which has a fixed type (e.g., int32, int64, string, etc.). The schema specifies an associative ag- gregation function for each individual value column, and F is implicitly defined as the coordinate-wise aggregation of the value columns, i.e.:## 引用
[1] HBase.http://hbase.apache.org/.  [2] LevelDB. http://en.wikipedia.org/wiki/LevelDB.  [3] MySQL. http:www.mysql.com.  [4] Pro ject Voldemart: A Distributed Database. http://www.project-voldemort.com/voldemort/.  [5] SAP HANA. http://www.saphana.com/welcome.[6] A. Abouzeid, K. Bajda-Pawlikowski, et al. HadoopDB: An Architectural Hybrid of MapReduce and DBMS Technologies for Analytical Workloads. PVLDB, 2(1):922–933, 2009.  [7] D. Agrawal, A. El Abbadi, et al. Efficient View Maintenance at Data Warehouses. In SIGMOD, pages 417–427, 1997.  [8] P. Agrawal, A. Silberstein, et al. Asynchronous View Maintenance for VLSD Databases. In SIGMOD, pages 179–192, 2009.  [9] M. O. Akinde, M. H. Bohlen, et al. Efficient OLAP Query Processing in Distributed Data Warehouses. Information Systems, 28(1-2):111–135, 2003.  [10] M. Athanassoulis, S. Chen, et al. MaSM: Efficient Online Updates in Data Warehouses. In SIGMOD, pages 865–876, 2011.  [11] J. Baker, C. Bond, et al. Megastore: Providing Scalable, Highly Available Storage for Interactive Services. In CIDR, pages 223–234, 2011.  [12] F. Chang, J. Dean, et al. Bigtable: A Distributed Storage System for Structured Data. In OSDI, pages 205–218, 2006.  [13] B. Chattopadhyay, L. Lin, et al. Tenzing A SQL Implementation on the MapReduce Framework. PVLDB, 4(12):1318–1327, 2011.  [14] S. Chaudhuri and U. Dayal. An Overview of Data Warehousing and OLAP Technology. SIGMOD Rec., 26(1):65–74, 1997.  [15] S. Chen, B. Liu, et al. Multiversion-Based View Maintenance Over Distributed Data Sources. ACM TODS, 29(4):675–709, 2004.  [16] J. Cohen, J. Eshleman, et al. Online Expansion of Largescale Data Warehouses. PVLDB, 4(12):1249–1259, 2011.  [17] B. F. Cooper, R. Ramakrishnan, et al. PNUTS: Yahoo!’s Hosted Data Serving Platform. PVLDB, 1(2):1277–1288, 2008.  [18] J. C. Corbett, J. Dean, et al. Spanner: Google’s Globally-Distributed Database. In OSDI, pages 251–264, 2012.  [19] J. Dean and S. Ghemawat. MapReduce: Simplified Data Processing on Large Clusters. Commun. ACM, 51(1):107–113, 2008.  [20] G. DeCandia, D. Hastorun, et al. Dynamo: Amazon’s Highly Available Key-value Store. In SOSP, pages 205–220, 2007.  [21] P. Deshpande, K. Ramasamy, et al. Caching Multidimensional Queries Using Chunks. In SIGMOD, pages 259–270, 1998.  [22] A. Fikes. Storage Architecture and Challenges. http://goo.gl/pF6kmz, 2010.  [23] S. Ghemawat, H. Gobioff, et al. The Google File System. In SOSP, pages 29–43, 2003.  [24] L. Glendenning, I. Beschastnikh, et al. Scalable Consistency in Scatter. In SOSP, pages 15–28, 2011.  [25] J. Gray, A. Bosworth, et al. Data Cube: A Relational Aggregation Operator Generalizing Group-By, Cross-Tabs and Sub-Totals. In IEEE ICDE, pages 152–159, 1996.  [26] H. Gupta and I. S. Mumick. Selection of Views to Materialize Under a Maintenance Cost Constraint. In ICDT, 1999.  [27] V. Harinarayan, A. Rajaraman, et al. Implementing Data Cubes Efficiently. In SIGMOD, pages 205–216, 1996.  [28] S. H ́eman, M. Zukowski, et al. Positional Update Handling in Column Stores. In SIGMOD, pages 543–554, 2010.  
[29] H. V. Jagadish, L. V. S. Lakshmanan, and D. Srivastava. Snakes and Sandwiches: Optimal Clustering Strategies for a Data Warehouse. In SIGMOD, pages 37–48, 1999.  [30] H. V. Jagadish, I. S. Mumick, et al. View Maintenance Issues for the Chronicle Data Model. In PODS, pages 113–124, 1995.  [31] A. Koeller and E. A. Rundensteiner. Incremental Maintenance of Schema-Restructuring Views in SchemaSQL. IEEE TKDE, 16(9):1096–1111, 2004.  [32] L. V. S. Lakshmanan, J. Pei, et al. Quotient cube: How to Summarize the Semantics of a Data Cube. In VLDB, pages 778–789, 2002.  [33] L. V. S. Lakshmanan, J. Pei, et al. QC-Trees: An Efficient Summary Structure for Semantic OLAP. In SIGMOD, pages 64–75, 2003.  [34] A. Lamb, M. Fuller, et al. The Vertica Analytic Database: C-Store 7 Years Later. PVLDB, 5(12):1790–1801, 2012.  [35] L. Lamport. The Part-Time Parliament. ACM Trans. Comput. Syst., 16(2):133–169, 1998.  [36] G. Lee, J. Lin, et al. The Unified Logging Infrastructure for Data Analytics at Twitter. PVLDB, 5(12):1771–1780, 2012.  [37] S. Melnik, A. Gubarev, et al. Dremel: Interactive Analysis of Web-Scale Datasets. PVLDB, 3(1-2):330–339, 2010.  [38] N. Roussopoulos, Y. Kotidis, et al. Cubetree: Organization of and Bulk Incremental Updates on the Data Cube. In SIGMOD, pages 89–99, 1997.  [39] K. Salem, K. Beyer, et al. How To Roll a Join: Asynchronous Incremental View Maintenance. In SIGMOD, pages 129–140, 2000.  [40] D. Severance and G. Lohman. Differential Files: Their Application to the Maintenance of Large Databases. ACM Trans. Database Syst., 1(3):256–267, 1976.  [41] J. Shute, R. Vingralek, et al. F1: A Distributed SQLDatabase That Scales. PVLDB, 6(11):1068–1079, 2013. [42] Y. Sismanis, A. Deligiannakis, et al. Dwarf: Shrinking the PetaCube. In SIGMOD, pages 464–475, 2002.  [43] D. Srivastava, S. Dar, et al. Answering Queries with Aggregation Using Views. In VLDB, pages 318–329, 1996. [44] M. Stonebraker, D. J. Abadi, et al. C-Store: A Column-oriented DBMS. In VLDB, pages 553–564, 2005. [45] A. Thusoo, J. Sarma, et al. Hive: A Warehousing Solution Over a Map-Reduce Framework. PVLDB, 2(2):1626–1629, 2009.  [46] A. Thusoo, J. Sarma, et al. Hive - A Petabyte Scale Data Warehouse Using Hadoop. In IEEE ICDE, pages 996–1005, 2010.  
[47] A. Thusoo, Z. Shao, et al. Data Warehousing and Analytics Infrastructure at Facebook. In SIGMOD, pages 1013–1020, 2010.  [48] R. Weiss. A Technical Overview of the Oracle Exadata Database Machine and Exadata Storage Server. Oracle White Paper. Oracle Corporation, Redwood Shores, 2012.   
[49] P. Wong, Z. He, et al. Parallel Analytics as a Service. In SIGMOD, pages 25–36, 2013.  [50] L. Wu, R. Sumbaly, et al. Avatara: OLAP for Web-Scale Analytics Products. PVLDB, 5(12):1874–1877, 2012.  [51] R. S. Xin, J. Rosen, et al. Shark: SQL and Rich Analytics at Scale. In SIGMOD, pages 13–24, 2013.  [52] J. Yang, K. Karlapalem, et al. Algorithms for Materialized View Design in Data Warehousing Environment. In VLDB, pages 136–145, 1997.  [53] Y. Zhuge, H. Garcia-Molina, et al. The Strobe Algorithms for Multi-Source Warehouse Consistency. In PDIS, pages 146–157, 1996.