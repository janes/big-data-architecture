# big-data-architecture

**Key architecture layers**

*   **File Systems**- Distributed file systems which provide storage, fault tolerance, scalability, reliability, and availability.
*   **Data Stores**– Evolution of application databases into [Polyglot](http://bigbe.su/lectures/2014/15.3.pdf) storage with application specific databases instead of one size fits all. Common ones are Key-Value, Document, Column and Graph.
*   **Resource Managers**– provide resource management capabilities and support schedulers for high utilization and throughput.
*   **Coordination**– systems that manage state, distributed coordination, consensus and lock management.
*   **Computational Frameworks**– a lot of work is happening at this layer with highly specialized compute frameworks for Streaming, Interactive, Real Time, Batch and Iterative Graph (BSP) processing. Powering these are complete computation runtimes like [BDAS](http://datasys.cs.iit.edu/events/CCGrid2014/CCGrid-May25-Stoica.pdf) (Spark) & Flink.
*   **DataAnalytics **–Analytical (consumption) tools and libraries, which support exploratory, descriptive, predictive, statistical analysis and machine learning.
*   **Data Integration**– these include not only the orchestration tools for managing pipelines but also metadata management.
*   **Operational Frameworks** – these provide scalable frameworks for monitoring & benchmarking.

### **Architecture Evolution**

The modern data architecture is evolving with a goal of reduced latency between data producers and consumers. This consequently is leading to real time and low latency processing, bridging the traditional batch and interactive layers into hybrid architectures like Lambda and Kappa.

*   [Lambda](http://jameskinley.tumblr.com/post/37398560534/the-lambda-architecture-principles-for) - Established architecture for a typical data pipeline. [Mor](http://lambda-architecture.net/)e details.
*   [Kappa](http://radar.oreilly.com/2014/07/questioning-the-lambda-architecture.html)– An alternative architecture which moves the processing upstream to the Stream layer.
*   [SummingBird](http://www.vldb.org/pvldb/vol7/p1441-boykin.pdf)– a reference model on bridging the online and traditional processing models. 

Before you deep dive into the actual layers, here are some general documents which can provide you a great background on NoSQL, Data Warehouse Scale Computing and Distributed Systems.

*   [_Distributed Systems Fundamentals_](http://github.com/aphyr/distsys-class)
*   [Data center as a computer](http://www.morganclaypool.com/doi/pdf/10.2200/S00516ED2V01Y201306CAC024)– provides a great background on warehouse scale computing.
*   [_NoSQL Databases: a Survey and Decision Guidance_](http://medium.baqend.com/nosql-databases-a-survey-and-decision-guidance-ea7823a822d#.liya5zc1d)
*   [NOSQL Data Stores](http://www.cattell.net/datastores/Datastores.pdf)– background on a diverse set of key-value, document and column oriented stores.
*   [NoSQL Thesis](http://www.christof-strauch.de/nosqldbs.pdf)– great background on distributed systems, first generation NoSQL systems.
*   [Large Scale Data Management](http://webdocs.cs.ualberta.ca/~lengdong/papers/JCST14.pdf)- covers the data model, the system architecture and the consistency model, ranging from traditional database vendors to new emerging internet-based enterprises. 
*   [Eventual Consistency](http://grail.csuohio.edu/~sschung/cis612/hadoopjoin_sigmod2010.pdf)– background on the different consistency models for distributed systems.
*   [CAP Theorem](http://www.cs.berkeley.edu/~rxin/db-papers/CAP.pdf)– a nice background on CAP and its evolution.
*   [_Critique on CAP Theorem_](http://jvns.ca/blog/2016/11/19/a-critique-of-the-cap-theorem/)

There also has been in the past a fierce debate between traditional Parallel DBMS with Map Reduce paradigm of processing. Pro [parallel DBMS](http://database.cs.brown.edu/sigmod09/benchmarks-sigmod09.pdf) ([another](http://database.cs.brown.edu/papers/stonebraker-cacm2010.pdf)) paper(s) was rebutted by the pro [MapReduce](http://www.cs.princeton.edu/courses/archive/spr11/cos448/web/docs/week10_reading2.pdf) one. Ironically the  Hadoop community from then has come full circle with the introduction of MPI style shared nothing based processing on Hadoop - [SQL on Hadoo](http://www.vldb.org/pvldb/vol7/p1295-floratou.pdf)p. 

### **File Systems**

As the focus shifts to low latency processing, there is a shift from traditional disk based storage file systems to an emergence of in memory file systems - which drastically reduces the I/O & disk serialization cost. Tachyon and Spark [RDD](http://www.cs.berkeley.edu/~matei/papers/2012/nsdi_spark.pdf) are examples of that evolution.

*   [Google File System](http://static.googleusercontent.com/media/research.google.com/en/us/archive/gfs-sosp2003.pdf)- The seminal work on Distributed File Systems which shaped the Hadoop File System.
*   [Hadoop File System](http://zoo.cs.yale.edu/classes/cs422/2014fa/readings/papers/shvachko10hdfs.pdf)– Historical context/architecture on evolution of HDFS.
*   [Ceph File System](http://ceph.com/papers/weil-ceph-osdi06.pdf)– An [alternative](http://www.usenix.org/legacy/publications/login/2010-08/openpdfs/maltzahn.pdf) to HDFS. 
*   [Tachyon](http://www.cs.berkeley.edu/~haoyuan/papers/2014_socc_tachyon.pdf)– An in memory storage system to handle the modern day low latency data processing.

File Systems have also seen an evolution on the file formats and compression techniques. The following references gives you a great background on the merits of row and column formats and the shift towards newer nested column oriented formats which are highly efficient for Big Data processing. Erasure codes are using some innovative techniques to reduce the triplication (3 replicas) schemes without compromising data recoverability and availability.  

*   [Column Oriented vs Row-Stores](http://db.csail.mit.edu/projects/cstore/abadi-sigmod08.pdf)– good overview of data layout, compression and materialization.
*   [RCFile](http://web.cse.ohio-state.edu/hpcs/WWW/HTML/publications/papers/TR-11-4.pdf)– Hybrid PAX structure which takes the best of both the column and row oriented stores.
*   [Parquet](http://github.com/Parquet/parquet-mr/wiki/The-striping-and-assembly-algorithms-from-the-Dremel-paper)– column oriented format first covered in Google’s Dremel’s paper.
*   [ORCFile](http://web.cse.ohio-state.edu/hpcs/WWW/HTML/publications/papers/TR-14-2.pdf)– an improved column oriented format used by Hive.
*   [Compression](http://www.ijritcc.org/IJRITCC%20Vol_2%20Issue_3/A%20Survey%20on%20Compression%20Algorithms%20%20in%20Hadoop.pdf)– compression techniques and their comparison on the Hadoop ecosystem.
*   [Erasure Codes](http://web.eecs.utk.edu/~plank/plank/papers/Login-2013.pdf)– background on erasure codes and techniques; improvement on the default triplication on [Hadoop](http://anrg.usc.edu/~maheswaran/Xorbas.pdf) to reduce storage cost.

### **Data Stores**

Broadly, the distributed data stores are classified on ACID & BASE stores depending on the continuum of strong to weak consistency respectively. BASE further is classified into KeyValue, Document, Column and Graph - depending on the underlying schema & supported data structure. While there are multitude of systems and offerings in this space, I have covered few of the more prominent ones. I apologize if I have missed a significant one...

**BASE**

**Key Value Stores**

*   [Dynamo](http://www.cs.ucsb.edu/~agrawal/fall2009/dynamo.pdf) – key-value distributed storage system
*   [Cassandra](http://www.cs.cornell.edu/projects/ladis2009/papers/lakshman-ladis2009.pdf) – Inspired by Dynamo; a multi-dimensional key-value/column oriented data store.
*   [Voldemort](http://static.usenix.org/events/fast/tech/full_papers/Sumbaly.pdf) – another one inspired by Dynamo, developed at LinkedIn.

**Column Oriented Stores**

*   [BigTable](http://static.googleusercontent.com/media/research.google.com/en/us/archive/bigtable-osdi06.pdf) – seminal paper from Google on distributed column oriented data stores.
*   [HBase](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.303.752&rep=rep1&type=pdf) – while there is no definitive paper , this provides a good overview of the technology.
*   [Hypertable](http://www.hypertable.com/collateral/whitepaper-hypertable-architecture.pdf) – provides a good overview of the architecture.

**Document Oriented Stores**

*   [CouchDB](http://media.readthedocs.org/pdf/couchdb/latest/couchdb.pdf) – a popular document oriented data store.
*   [MongoDB](http://s3.amazonaws.com/info-mongodb-com/MongoDB_Architecture_Guide.pdf) – a good introduction to MongoDB architecture.

**Graph**

*   [Neo4j](http://info.neotechnology.com/rs/neotechnology/images/GraphDatabases.pdf) – most popular Graph database.
*   [Titan](http://s3.thinkaurelius.com/docs/titan/0.9.0-M2/) – open source Graph database under the Apache license.

**ACID**

I see a lot of evolution happening in the open source community which will try and catch up with what Google has done – 3 out of the prominent papers below are from Google , they have solved the globally distributed consistent data store problem.

*   [Megastore](http://www.cidrdb.org/cidr2011/Papers/CIDR11_Paper32.pdf) – a highly available distributed consistent database. Uses Bigtable as its storage subsystem.
*   [Spanner](http://static.googleusercontent.com/media/research.google.com/en/us/archive/spanner-osdi2012.pdf) – Globally distributed synchronously replicated linearizable database which supports SQL access.
*   [MESA](http://www.vldb.org/pvldb/vol7/p1259-gupta.pdf) – provides consistency, high availability, reliability, fault tolerance and scalability for large data and query volumes.
*   [CockroachDB](http://github.com/cockroachdb/cockroach/blob/master/docs/design.md) – An open source version of Spanner (led by former engineers) in active development.

**Resource Managers**

While the first generation of Hadoop ecosystem started with monolithic schedulers like YARN, the evolution now is towards hierarchical schedulers (Mesos), that can manage distinct workloads, across different kind of compute workloads, to achieve higher utilization and efficiency.

*   [YARN](http://54e57bc8-a-62cb3a1a-s-sites.googlegroups.com/site/2013socc/home/program/a5-vavilapalli.pdf?attachauth=ANoY7co94J9PVjpjD5GD4z-S8e1O7YrLsqHssH7aeFReTJaoOBLbvLhq9HeDNb-PQz2jQvPUeQbDjJa2bctooZz5_zHCKWXAKZrYqAy_mVCLIQqU0Cc-sNQBHOJNsUTyVPfEdpHQ5yoIGVdIzoCnQwsFjbSX2ztS9b0OBNI2SjDCdvLE7Hsi5ktJINChoFa7w0ELgFvir4sEAJaL-G1qgmUglhOjVjHgwXYsqHH7FOPXrTVC-csZelo%3D&attredirects=0) – The next generation Hadoop compute framework. [YARN TimeLine 2](http://hadoop.apache.org/docs/r3.0.0-alpha1/hadoop-yarn/hadoop-yarn-site/TimelineServiceV2.html) – uses a more scalable distributed writer architecture and a scalable backend storage, separates collection of data from serving of data to better scale the cluster.   
*   [Mesos](http://people.csail.mit.edu/matei/papers/2011/nsdi_mesos.pdf) – scheduling between multiple diverse cluster computing frameworks.

These are loosely coupled with schedulers whose primary function is schedule jobs based on scheduling policies/configuration.

**Schedulers**

*   [_Aurora_](http://aurora.apache.org/) _generic service scheduler that runs as a framework on top of Mesos._
*   [_Apache REEF_](http://www.markusweimer.com/files/pub/2015/2015-SIGMOD.pdf) _- a library for developing portable applications for cluster resource managers such as YARN or Mesos._
*   [Capacity Scheduler](http://hadoop.apache.org/docs/stable1/capacity_scheduler.pdf) - introduction to different features of capacity scheduler. 
*   [FairShare Scheduler](http://www.valleytalk.org/wp-content/uploads/2013/03/fair_scheduler_design_doc.pdf) - introduction to different features of fair scheduler.
*   [Delayed Scheduling](http://www.eecs.berkeley.edu/Pubs/TechRpts/2009/EECS-2009-55.pdf) - introduction to Delayed Scheduling for FairShare scheduler.
*   [Fair & Capacity schedulers](http://arxiv.org/ftp/arxiv/papers/1207/1207.0780.pdf) – a survey of Hadoop schedulers.

### **Coordination**

These are systems that are used for coordination and state management across distributed data systems.

*   [Paxos](http://research.microsoft.com/en-us/um/people/lamport/pubs/paxos-simple.pdf) – a simple version of the [classical](http://research.microsoft.com/en-us/um/people/lamport/pubs/lamport-paxos.pdf) paper; used for distributed systems consensus and coordination. 
*   [Chubby](http://static.googleusercontent.com/media/research.google.com/en/us/archive/chubby-osdi06.pdf) – Google’s distributed locking service that implements Paxos.
*   [Zookeeper](http://www.usenix.org/legacy/event/usenix10/tech/full_papers/Hunt.pdf) – open source version inspired from Chubby though is general coordination service than simply a locking service 
*   [_Raft_](http://raft.github.io/raft.pdf) _Consensus Algorithm_
*   [_NoPaxos_](http://www.usenix.org/system/files/conference/osdi16/osdi16-li.pdf) _– simplifies by Replacing Consensus with Network Ordering_

### **Computational Frameworks**

The execution runtimes provide an environment for running distinct kinds of compute. The most common runtimes are

[Spark](http://www.usenix.org/system/files/login/articles/zaharia.pdf) – its popularity and adoption is challenging the traditional Hadoop ecosystem.

[Flink](http://events.linuxfoundation.org/sites/events/files/slides/flink_apachecon_small.pdf) – very similar to Spark ecosystem; strength over Spark is in iterative processing.

The frameworks broadly can be classified based on the model and latency of processing

**Batch**

[MapReduce](http://static.googleusercontent.com/media/research.google.com/en/us/archive/mapreduce-osdi04.pdf) – The seminal paper from Google on MapReduce.

[MapReduce Survey](http://www.cs.arizona.edu/~bkmoon/papers/sigmodrec11.pdf) – A dated, yet a good paper; survey of Map Reduce frameworks.

**Iterative (BSP)**

*   [Pregel](http://kowshik.github.io/JPregel/pregel_paper.pdf) – Google’s paper on large scale graph processing
*   [Giraph](http://researcher.ibm.com/researcher/files/us-heq/Large%20Scale%20Graph%20Processing%20with%20Apache%20Giraph.pdf) - large-scale distributed Graph processing system modelled around Pregel
*   [GraphX](http://amplab.cs.berkeley.edu/wp-content/uploads/2014/02/graphx.pdf) - graph computation framework that unifies graph-parallel and data parallel computation.
*   [Hama](http://csl.skku.edu/papers/CS-TR-2010-330.pdf) - general BSP computing engine on top of Hadoop
*   [Open source graph processing ](http://www.vldb.org/pvldb/vol7/p1047-han.pdf) survey of open source systems modelled around Pregel BSP.
*   [_GraphTau_](http://www.cs.columbia.edu/~lierranli/publications/GraphTau-GRADES2016.pdf) _is a new programming model for graph computation on time-evolving graphs which is built on top of Apache Spark._

**Streaming**

[Stream Processing](http://www.ucviden.dk/portal/files/26907191/Survey_of_real_time_processing_systems_for_big_data_Draft_.pdf) – A great overview of the distinct real time processing systems .

[_Streaming Data Architecture Overview_](http://info.lightbend.com/rs/558-NCX-702/images/COLL-ebook-Fast-Data-Architectures-for-Streaming-Applications-Lightbend.pdf) _- O'Reilly report on the state of stream processing._

*   [_Apache Apex_](http://events.linuxfoundation.org/sites/events/files/slides/ApacheBigDataEU2016IntroApacheApex_0.pdf) _- a stream processing platform submitted by data torrent leverages the resource management capabilities provided by YARN. It is built upon the Apache Malhar library which offers blocks(operators) for stream processing._
*   [_Twitter Heron_](http://pdfs.semanticscholar.org/e847/c3ec130da57328db79a7fea794b07dbccdd9.pdf) _– exposes the same API interface as storm, however improves upon it to have higher scalability, better debugability, better performance, and easier to manage. Builds capabilities like backpressure, replaces Nimbus with Aurora scheduler._
*   [_GearPump_](http://downloads.typesafe.com/website/presentations/GearPump_Final.pdf) _– An Akka based real-time streaming engine._
*   [Samza](http://www.jfokus.se/jfokus15/preso/ApacheSamza.pdf)  - stream processing framework from LinkedIn
*   [Spark Streaming](http://people.csail.mit.edu/matei/papers/2013/sosp_spark_streaming.pdf) – introduced the micro batch architecture bridging the traditional batch and interactive processing.

**Interactive**

*   [Dremel](http://www.vldb.org/pvldb/vldb2010/papers/R29.pdf) – Google’s paper on how it processes interactive big data workloads, which laid the groundwork for multiple open source SQL systems on Hadoop.
*   [Impala](http://www.cidrdb.org/cidr2015/Papers/CIDR15_Paper28.pdf) – MPI style processing on make Hadoop performant for interactive workloads.
*   [Drill](http://wiki.apache.org/incubator/DrillProposal?action=AttachFile&do=get&target=Drill+slides.pdf) – A open source implementation of Dremel.
*   [Shark](http://www.cs.berkeley.edu/~matei/papers/2012/sigmod_shark_demo.pdf) – provides a good introduction to the data analysis capabilities on the Spark ecosystem.
*   [Shark](http://people.csail.mit.edu/matei/papers/2013/sigmod_shark.pdf) – another great paper which goes deeper into SQL access.
*   [Dryad](http://www.news.cs.nyu.edu/~jinyang/sp07/papers/dryad.pdf) – Configuring & executing parallel data pipelines using DAG.
*   [Tez](http://www.datanubes.com/mediac/ApacheTezPrimer.pdf) – open source implementation of Dryad using YARN.
*   [BlinkDB](http://www.cs.berkeley.edu/~sameerag/blinkdb_eurosys13.pdf) - enabling interactive queries over data samples and presenting results annotated with meaningful error bars
*   [_Apache Geode_](http://events.linuxfoundation.org/sites/events/files/slides/ApacheConBigData%20-%20Introducing%20Apache%20Geode%20-%20final.pdf) _– is an in-memory, distributed database with strong consistency built to support scalable low latency transactional applications._
*   [_Apache Kudu_](http://kudu.apache.org/kudu.pdf) _- Fast processing of OLAP workloads that operates on columnar storage which provides high-throughput on both sequential (such as HDFS) and random (such as HBase or Cassandra) workloads simultaneously._

**RealTime**

*   [Druid](http://static.druid.io/docs/druid.pdf) – a real time OLAP data store. Operationalized time series analytics databases
*   [Pinot](http://github.com/linkedin/pinot/wiki/Architecture) – LinkedIn OLAP data store very similar to Druid. 

**Data Analysis**

The analysis tools range from declarative languages like SQL to procedural languages like Pig. Libraries on the other hand are supporting out of the box implementations of the most common data mining and machine learning libraries.

**Tools**

*   [Apache Zeppelin](http://events.linuxfoundation.org/sites/events/files/slides/Zeppelin_ApacheCon2015_0.pdf) – for interactive data analytics ad visualization   
*   [Pig](http://infolab.stanford.edu/~olston/publications/sigmod08.pdf) – Provides a good overview of Pig Latin.
*   [Pig](http://paperhub.s3.amazonaws.com/a7b584c04b61fabb8d10333e91989120.pdf) – provide an introduction of how to build data pipelines using Pig.
*   [Hive](http://infolab.stanford.edu/~ragho/hive-icde2010.pdf) – provides an introduction of Hive.
*   [Hive](http://www.vldb.org/pvldb/2/vldb09-938.pdf) – another good paper to understand the motivations behind Hive at Facebook.
*   [Phoenix](http://phoenix.apache.org/presentations/OC-HUG-2014-10-4x3.pdf) – SQL on Hbase.
*   [Join Algorithms for Map Reduce](http://www.ispras.ru/proceedings/docs/2012/23/isp_23_2012_285.pdf) – provides a great introduction to different join algorithms on Hadoop. 
*   [Join Algorithms for Map Reduce](http://grail.csuohio.edu/~sschung/cis612/hadoopjoin_sigmod2010.pdf) – another great paper on the different join techniques.

**Machine Learning**

*   [MLlib](http://stanford.edu/~rezab/sparkworkshop/slides/xiangrui.pdf) – Machine language library on Spark.
*   [SparkR](http://files.meetup.com/3138542/SparkR-meetup.pdf) – Distributed R on Spark framework.
*   [Mahout](http://openresearch.baidu.com/u/cms/www/201210/30144944cqmu.pdf) – Machine learning framework on traditional Map Reduce.
*   [_Apache SystemML_](http://researcher.watson.ibm.com/researcher/files/us-ytian/systemML.pdf) _– open sourced by IBM, it allows a developer to write a single machine learning algorithm and automatically scale it up using Spark or Hadoop._
*   [_Apache SAMOA_](http://melmeric.files.wordpress.com/2014/10/samoa-scalable-advanced-massive-online-analysis.pdf) _Scalable Advanced Massive Online Analysis is an open source platform - both a framework and a library - for mining big data streams. It supports the most common machine learning tasks such as classification, clustering, and regression._
*   [_Apache HiveMall_](http://myui.github.io/publications/hivemall.pdf) _- Hive scalable machine learning library that runs on Apache Hive, Spark and Pig._

**Data Integration**

Data integration frameworks provide good mechanisms to ingest and outgest data between Big Data systems. It ranges from orchestration pipelines to metadata framework with support for lifecycle management and governance.

**Ingest/Messaging**

[Flume](http://blogs.apache.org/flume/entry/flume_ng_architecture) – a framework for collecting, aggregating and moving large amounts of log data from many different sources to a centralized data store.

[Sqoop](http://cwiki.apache.org/confluence/download/attachments/27361435/Cecho_Ting_SqoopBigDataTechCon.pdf?version=1&modificationDate=1366107169000&api=v2)– a tool to move data between Hadoop and Relational data stores.

[Kafka](http://notes.stephenholiday.com/Kafka.pdf) – distributed messaging system for data processing

**ETL/Workflow**

*   [_Apache Nifi_](http://nifi.apache.org/docs.html) _- data distribution and processing system; provides a way to move data from one place to another, making routing decisions and transformations as necessary along the way._
*   _Apache Beam – An open source version of_ [_Google’s Cloud DataFlow_](http://cloud.google.com/dataflow/) _–_ [_FlumeJava_](http://static.googleusercontent.com/media/research.google.com/en/pubs/archive/35650.pdf) _&_ [_MillWheel_](http://static.googleusercontent.com/media/research.google.com/en/pubs/archive/41378.pdf) _- which unifies the model for batch and streaming data processing (_[_uber-API_](http://www.infoworld.com/article/3056172/application-development/apache-beam-wants-to-be-uber-api-for-big-data.html) _for big data**).**_
*   [_Apache Airflow_](http://airflow.incubator.apache.org/)_ **-** author, schedule and monitor workflows._
*   [Crunch](http://events.linuxfoundation.org/sites/events/files/slides/Simplifying%20Big%20Data%20with%20Apache%20Crunch.pdf) – library for writing, testing, and running MapReduce pipelines.
*   [Falcon](http://public-repo-1.hortonworks.com/HDP-LABS/Projects/Falcon/2.0.6.0-76/FalconHortonworksTechnicalPreview.pdf) – data management framework that helps automate movement and processing of Big Data.
*   [Cascading](http://smokinn.com/files/cascading_notes/cascading.pdf) – data manipulation through scripting.
*   [Oozie](http://oozie.apache.org/docs/4.2.0/index.html) – a workflow scheduler system to manage Hadoop jobs.

**Metadata**

*   [HCatalog](http://cwiki.apache.org/confluence/display/Hive/HCatalog+UsingHCat) - a table and storage management layer for Hadoop.
*   [_Apache Atlas_](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.5.3/bk_data-governance/content/ch_hdp_data_governance_overview.html) _- designed to exchange metadata, track lineage with other tools and processes within and outside of the Hadoop stack - enables enterprises to effectively and efficiently meet their compliance requirements and allows integration with the whole enterprise data ecosystem._

**Security**

*   [_Hadoop Security Design_](http://carfield.com.hk:8080/document/distributed/hadoop-security-design.pdf) _– seminal paper which captures key aspects of Hadoop design._
*   [_Apache Metron_](http://cwiki.apache.org/confluence/display/METRON/Metron+Wiki) _- is a cyber security application framework that provides organizations the ability to ingest, process and store diverse security data feeds to detect anomalies._
*   [_Apache Knox_](http://public-repo-1.hortonworks.com/HDP-LABS/Projects/Knox/1.3.3.0-59/KnoxTechnicalPreview.pdf) _- is the Web/REST API Gateway solution for Hadoop. It provides a single access point to access all of Hadoop resources over REST. It acts as a virtual firewall enforcing authentication and usage policies on inbound requests and blocking everything else._
*   [_Apache Ranger_](http://files.meetup.com/19917255/Apache%20Ranger%20Meetup.pdf)_—is a policy administration tool for Hadoop clusters. It includes a broad set of management functions, including auditing, key management, and fine grained data access policies across HDFS, Hive, YARN, Solr, Kafka and other modules._
*   [_HDFS Encryption_](http://issues.apache.org/jira/secure/attachment/12660368/HDFSDataatRestEncryption.pdf)_— HDFS offers 'transparent' encryption embedded within the Hadoop file system._
*   [_Apache Sentry_](http://events.linuxfoundation.org/sites/events/files/slides/ApacheSentry2015_0.pdf) _- fine-grained authorization to data stored in Apache Hadoop. Enforces a common set of policies across multiple data access path in Hadoop_
*   [_Apache Eagle_](http://7xnz4l.com1.z0.glb.clouddn.com/Arch022.pdf) _- is a distributed real-time monitoring and alerting engine which helps in identifying security and performance issues instantly on big data platforms like Hadoop, Spark etc._

**Serialization**

[ProtocolBuffers](http://homepages.lasige.di.fc.ul.pt/~vielmo/notes/2014_02_12_smalltalk_protocol_buffers.pdf) – language neutral serialization format popularized by Google. [Avro](http://mil-oss.org/resources/mil-oss-wg3_an-introduction-to-apache-avro_douglas-creager.pdf) – modeled around Protocol Buffers for the Hadoop ecosystem.

### **Operational Frameworks**

Finally the operational frameworks provide capabilities for metrics, benchmarking and performance optimization to manage workloads.

**Monitoring Frameworks**

*   [OpenTSDB](http://opentsdb.net/overview.html) – a time series metrics systems built on top of HBase.
*   [Ambari](http://issues.apache.org/jira/secure/attachment/12559939/Ambari_Architecture.pdf) - system for collecting, aggregating and serving Hadoop and system metrics

**Benchmarking**

*   [_NDBench_](http://techblog.netflix.com/2016/09/netflix-data-benchmark-benchmarking.html) _- open-source project from Netflix which is used benchmark data systems like Cassandra, Redis, and Elasticsearch for throughput and latency._
*   [YCSB](http://research.ijais.org/volume7/number8/ijais14-451229.pdf) – performance evaluation of NoSQL systems.
*   [GridMix](http://hadoop.apache.org/docs/stable1/gridmix.pdf) – provides benchmark for Hadoop workloads by running a mix of synthetic jobs
*   [Background](http://arxiv.org/ftp/arxiv/papers/1402/1402.5194.pdf) on big data benchmarking with the key challenges associated.


**Anil Madan**
(https://www.linkedin.com/pulse/100-open-source-big-data-architecture-papers-anil-madan)
