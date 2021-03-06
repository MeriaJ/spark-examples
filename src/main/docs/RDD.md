## Why RDD ?

In Spark, the programming paradigm remained the same: Map-Reduce. Spark, at its core is a 
implementation of Map-Recue paradigm. The nature of problems it can solve also is almost 
same as that of Hadoop Map-reduce. Whats make it different is the speed at which it can execute 
and the caching mechanism which avoids disk IOs whenever possible. 

Now, the question is why RDD (Resilient Distributed Dataset) abstraction ?
In Hadoop Map-Reduce two one job commnicate to the other by writing data into HDFS and reading from there.
Even if we are not interested to persist the intermediate results in a data flow having multiple map-reduce jobs
we have to do it (or the the framework will do it) because that the only way jobs can communicates each other.
But it has got advantages, HDFS will take care of fault tolerance and high availability of your datsets.
As I said earlier Spark try to avoid Disk IOs whenever possible. ( its important to not that `whenever`. Becaus ethe spark
caching mechanism is not only in memory. It can vary depending on the storage level you choose and may include DISK IOs)
In order to avoid disk IOs we need spark need to cache the intermediate results in memory. This demands whole new 
mechanisms for tault tolerance. You should be abler to guess, why ? in Hadoop map-reduce, the file system
HDFS handle it. Once the data in written all is taken by HDFS, the processing framework has nothing to worry
about it.
 
In HDFS, the file blocks are replicated and stored in different machines for fault tolerance. One more thing to note here is that
you have serialisation overhead in all these operations too. Think of the same strategy with memory. Its costly to replicate your data in memory
both in terms of storage and netwrok transfer, we need better solution. (Note: even if its costly, we would like to do it at times and park has provision for that as well).
 
The better soution Spark came up with is RDD. RDD is a read-only, partitioned collection of records. RDDs can only be created 
through functional operations( in other words deterministic operations) on other RDDs or from files systems like HDFS which
are reliable persistent storage.The abtraction of RDD holds meta information including the previous RDD, transformation apllied to the
prevous RDD, the list of partitions and their loications etc. This make RDD capable of reconstructiong the any partition 
in case of a failure, most of the time just re-computing the lost partition. This property make its efficient for batch processing.

I would strongly encourage you to read the RDD white paper, its [here](https://www.usenix.org/system/files/conference/nsdi12/nsdi12-final138.pdf)

### The RDD Abstraction



