# winutils
Windows binaries for Hadoop versions 

These are built directly from the same git commit used to create the official ASF releases; they are checked out
and built on a windows VM which is dedicated purely to testing Hadoop/YARN apps on Windows. It is not a day-to-day
used system so is isolated from driveby/email security attacks.

Note that I am a Hadoop committer: I have nothing to gain by creating malicious versions of these binaries. If I wanted to run anything on your systems, I'd be able to add the code into Hadoop itself, or its build process.

I'm moving to `gpg -armor` signing of binaries; the GPG key used is on the [MIT key server](https://pgp.mit.edu/pks/lookup?op=vindex&search=0xA92454F9174786B4)

## release process


in `hadoop-trunk`

Check out the git commit ID voted on as the final release of the ASF artifacts.

```
mvn clean
mvn package -Pdist -Dmaven.javadoc.skip=true -DskipTests

```

This creates a distribution, with the native binaries under hadoop-dist\target


1. create dir winutils/$VERSION
1. copy bin\* to winutils/$VERSION
1. rm stuff you don't want
1. add the rest
1. GPG sign, add .ASC files.
1. commit
1. push
Hive Performance Tuning:
For operating large volumes of data, Systems and their processes should function efficiently for achieving accuracy in less time. Systems with underutilizing its modules with the different configurations also degrade the performance leads to in underutilized processing capacity and ends up with in efficient and lagging outputs.
Normally any performance degrading bottlenecks are identified during load testing and performance testing, these issues are commonly rectified through a process of performance tuning. Performance tuning is a process of analyzing these specific configuration bottlenecks present in the hive engines and internal map reduce events for utilizing RAM, memory, CPU, IO data and execution engines effectively.
Understanding Hive query life cycle and probable causes of data lags:
Hive Query—triggersßà (Hive metadata+hive execution engines+spetial RDBMS+ Internal map_reduce-Jobs) ßàHDFS
1) Lot of hardware(Datanodes+exicution engines+disk writing+RAM+cachees)+many tasks(Hive metadata +hive execution engines+ special RDBMS)+DFS environment storage.
2) Instance opertions
3) Spill over of cache to disk data  operations
4) Replicative data process.
5) Intermediate data +IO operations, Network traffic redundancy.
 
For achieving efficient system’s utilization problems, Performance tuning is required to solve problems with
1)Too many small files and reducers configuration
Copying small files to staging tables and direct insert in to target table which makes the number of files and reducers cost equal.
2) Adjusting split activities with the variable sizes-as per the type of data using in the execution environment.
3) Compressed data IO operations and internal network decrypt and encrypt engines with codec,DZip, bzip2..etc
Ex: serdy: lazy simple surdy, set org.apache.hadoop.io.compress.default.codec
Hive environment performance tuning causes and operations:
1)Optimizing Hive queries
a) enabling the data compression: compressed output, intermediate data
b) Optimized joins by setting auto map join enabled we can optimize the time for small tables to join the big tables with less time in every process engine in the mapping phase and data read time of small data tables at each data node
c) enabling auto map join, avoid skew joins provide the operation of join in Map phase itself.
d) Bucketed Map join enabling makes the table with bucketed column for easy operations  
2) Avoiding Global sorting in Hive:
Order by class fetch the results by setting the number of reducers to one. For the large data sets it makes the retrieving and processing time more. Instead of order by we can use sort by which makes the each file with one reducer.
3) Added execution engines:
High performance and additional processing engines like tez improves the performance by 300% just by setting the property
Example:
Set hive.execution.engine=tez;
4)Avoiding LIMIT operator:
Scans the entire file for sorting some records instead by enabling hive.limit.optimize.enable= true scans the final one and passes files for operation.
5) Enabling parallel execution:
For completing the overall job quickly, internal operations involved in the stages like map reduce stage, sampling stage, merge stage, limit stage default execution engine operates in one on one but if enabling parallel process some independent jobs can be executed in parallel to execute the query which intern decrease query fetch time.
6)Map reduce strict mode:
7) Single reduce for Multi group by functional query which set the single map reduce job by combining multiple group by function in a typical nested query.
8)Controlling the number of parallel reducer tasks and making it fixed value saves processor for additional works.
9)Enabling query based cost optimization with the number of joins to perform and degree of parallelism
10) Using the optimized record Columnar (ORC File) format which improve the performance of hive queries, by having columnar format of storing and which makes the less retrieving operational time.
11) Massive network traffic suppression by implementing compressed map output in map reduce tasks and implementing intermediate combiner jobs in query and internal map reduce operation.
Balancing reducer load: common performance issue might encounter is unbalanced reducer tasks: one or more reducer takes most of the output from mapper and ran extremely long compare to other reducers.
Reduced by Implementing a better hash function in Partitioner. class.
 
 
 
 
 
 
