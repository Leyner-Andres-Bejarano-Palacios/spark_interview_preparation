# spark_interview_preparation
Throughout my career, I have used spark for a lot of data analysis activities, but when I have to do a spark interview sometimes I don't find the best words for explaining concepts sometimes I shock when they ask me to do an exercise live, so I created this repo to record my journey, my preparation for future interviews.

If you want access to the sources, such as books, links or pieces of code, feel free to reach out.

## Theorical Questions Section

### Theorical Question 1

In a Spark application what is the driver program and does it get access to resources such as the the Spark executors ?

<details><summary><b>Answer</b></summary>
Spark application consists of a driver program that is responsible for orchestrating parallel operations on the Spark cluster. The driver accesses the distributed components in the cluster—the Spark executors and cluster manager—through a SparkSession .
</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 2
</details>

### Theorical Question 2

Why is spark faster than hadoop mapReduce ?

<details><summary><b>Answer</b></summary>
Spark provides in-memory storage for intermediate computations, making it much
faster than Hadoop MapReduce.
</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 4
</details>

### Theorical Question 3

Do you understand what are the spark stages ?

<details><summary><b>Answer</b></summary>
As part of the DAG nodes, stages are created based on what operations can be per‐
formed serially or in parallel . Not all Spark operations can happen in a
single stage, so they may be divided into multiple stages.
</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 28
</details>

### Theorical Question 4

Do you understand the difference between spark operation actions and spark operation transformation ?

<details><summary><b>Answer</b></summary>
Transformations, as the name suggests, transform a Spark DataFrame
into a new DataFrame without altering the original data, giving it the property of
immutability. Put another way, an operation such as select() or filter() will not
change the original DataFrame; instead, it will return the transformed results of the
operation as a new DataFrame.

All transformations are evaluated lazily. That is, their results are not computed immediately, but they are recorded or remembered as a lineage. A recorded lineage allows Spark, at a later time in its execution plan, to rearrange certain transformations, coalesce them, or optimize transformations into stages for more efficient execution. Lazy evaluation is Spark’s strategy for delaying execution until an action is invoked or data is “touched” (read from or written to disk).

An action triggers the lazy evaluation of all the recorded transformations.

![Image](img/exampleTransformationsActions.png "example spark trasnsformations and actions")

![Image](img/TransformationsActions.png "trasnsformations and actions")
</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 28
</details>


### Theorical Question 5

Do you understand the difference between wide and narrow transformations ?

<details><summary><b>Answer</b></summary>
Transformations can be classified as having either narrow dependencies or wide
dependencies. Any transformation where a single output partition can be computed
from a single input partition is a narrow transformation. For example, in the previous

code snippet, filter() and contains() represent narrow transformations because
they can operate on a single partition and produce the resulting output partition
without any exchange of data.

However, groupBy() or orderBy() instruct Spark to perform wide transformations,
where data from other partitions is read in, combined, and written to disk. Since each partition will have its own count of the word that contains the “Spark” word in its row of data, a count ( groupBy() ) will force a shuffle of data from each of the executor’s partitions across the cluster. In this transformation, orderBy() requires output fromother partitions to compute the final aggregation.
</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 30
</details>

### Theorical Question 6

Why would you use RDD instead of dataframes ?

<details><summary><b>Answer</b></summary>
There are two possible reasons:

• Are using a third-party package that’s written using RDDs

• Want to precisely instruct Spark how to do a query
</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 75
</details>

### Theorical Question 7

Where does spark store metadata like the schema, description, table name, data‐
base name, column names, partitions, physical location where the actual data resides,
etc.

<details><summary><b>Answer</b></summary>
Tables hold data. Associated with each table in Spark is its relevant metadata, which is information about the table and its data: the schema, description, table name, database name, column names, partitions, physical location where the actual data resides, etc. 

All of this is stored in a central metastore.

Instead of having a separate metastore for Spark tables, Spark by default uses the
Apache Hive metastore, located at /user/hive/warehouse, to persist all the metadata
about your tables. However, you may change the default location by setting the Spark
config variable spark.sql.warehouse.dir to another location, which can be set to a
local or external distributed storage.
</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 89
</details>

### Theorical Question 8

Do you know the difference between managed and unmanaged tables

<details><summary><b>Answer</b></summary>
Spark allows you to create two types of tables: managed and unmanaged. For a managed table, Spark manages both the metadata and the data in the file store. This could be a local filesystem, HDFS, or an object store such as Amazon S3 or Azure Blob. 

For an unmanaged table, Spark only manages the metadata, while you manage the data
yourself in an external data source such as Cassandra.

With a managed table, because Spark manages everything, a SQL command such as
DROP TABLE table_name deletes both the metadata and the data. With an unmanaged
table, the same command will delete only the metadata, not the actual data.
</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 90
</details>


### Theorical Question 9

Do you know what views are ?

<details><summary><b>Answer</b></summary>
In addition to creating tables, Spark can create views on top of existing tables. Views can be global (visible across all SparkSession s on a given cluster) or session-scoped (visible only to a single SparkSession ), and they are temporary: they disappear after your Spark application terminates.

Creating views has a similar syntax to creating tables within a database. Once you create a view, you can query it as you would a table. The difference between a view and a table is that views don’t actually hold the data; tables persist after your Spark application terminates, but views disappear.
</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 92
</details>

### Theorical Question 10

Do you know the difference between global and temporary views ?

<details><summary><b>Answer</b></summary>
The difference between temporary and global temporary views being subtle, it can be a
source of mild confusion among developers new to Spark. A temporary view is tied
to a single SparkSession within a Spark application. In contrast, a global temporary
view is visible across multiple SparkSession s within a Spark application. 

Yes, you can create multiple SparkSession s within a single Spark application—this can be handy, for example, in cases where you want to access (and combine) data from two different SparkSession s that don’t share the same Hive metastore configurations.
</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 92
</details>

### Theorical Question 11

Do you know why you don't specify an schema when you are working with a parquet file ?

<details><summary><b>Answer</b></summary>
Unless you are reading from a streaming data source there’s no need to supply the
schema, because Parquet saves it as part of its metadata.
</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 98
</details>

### Theorical Question 12

Do you think serializing and deserialing data would make Dataset or Dataframe give a greater performance ?

<details><summary><b>Answer</b></summary>
In “DataFrames Versus Datasets” on page 74 in Chapter 3, we outlined some of the
benefits of using Datasets—but these benefits come at a cost. As noted in the
preceding section, when Datasets are passed to higher-order functions such as fil
ter() , map() , or flatMap() that take lambdas and functional arguments, there is a
cost associated with deserializing from Spark’s internal Tungsten format into the JVM
object.

Compared to other serializers used before encoders were introduced in Spark, this
cost is minor and tolerable. However, over larger data sets and many queries, this cost accrues and can affect performance.
</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 170
</details>

### Theorical Question 13

How would you change the properties/config files of spark ?

<details><summary><b>Answer</b></summary>
There are three ways you can get and set Spark properties. The first is through a set of configuration files. In your deployment’s $SPARK_HOME directory (where you installed
Spark), there are a number of config files: conf/spark-defaults.conf.template, conf/
log4j.properties.template, and conf/spark-env.sh.template. Changing the default values in these files and saving them without the .template suffix instructs Spark to use these new values.

![Image](img/ChangingConfigFiles.png "Changing Config Files")

![Image](img/ChangingConfigFilesOption3.png "Changing Config Files Option3")

</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 173
</details>

### Theorical Question 14

How would you avoid job failures due to resource starvation or gradual performance degradation ?

<details><summary><b>Answer</b></summary>

When you specify compute resources as command-line arguments to spark-submit ,
as we did earlier, you cap the limit. This means that if more resources are needed later as tasks queue up in the driver due to a larger than anticipated workload, Spark cannot accommodate or allocate extra resources.
If instead you use Spark’s dynamic resource allocation configuration, the Spark driver can request more or fewer compute resources as the demand of large workloads flows and ebbs. 
In scenarios where your workloads are dynamic—that is, they vary in their
demand for compute capacity—using dynamic allocation helps to accommodate sud‐
den peaks.
One use case where this can be helpful is streaming, where the data flow volume may
be uneven. Another is on-demand data analytics, where you might have a high vol‐
ume of SQL queries during peak hours. Enabling dynamic resource allocation allows
Spark to achieve better utilization of resources, freeing executors when not in use and acquiring new ones when needed.

![Image](img/increasingResources.png "increasing Resources")

![Image](img/increasingResourcesPart2.png "increasing Resources Part 2")

</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 177
</details>


### Theorical Question 15

Do you understand the difference between cache() and persist() ?

<details><summary><b>Answer</b></summary>

![Image](img/cachingData.png "caching Data")

![Image](img/cachingDataPart2.png "caching Data Part 2")

![Image](img/cachingDataPart3.png "caching Data Part 3")

</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 177
</details>

### Theorical Question 16

When should you cache and not to cache data ?

<details><summary><b>Answer</b></summary>

![Image](img/whenCache.png "when cache")

</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 187
</details>

### Theorical Question 17

When do you use broadcast hash join ?

<details><summary><b>Answer</b></summary>

![Image](img/whenToUseBroadCastJoin.png "when To Use BroadCast Join")

</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 188
</details>


## Practical Questions Section

### Practical Question 1

Could you explain line by line what the next piece of code is doing ?

![Image](img/practicalImg1_part1.png "practical Img1_part1")

![Image](img/practicalImg1_part2.png "practical Img1_part2")

![Image](img/practicalImg1_part3.png "practical Img1_part3")

![Image](img/practicalImg1_part4.png "practical Img1_part4")

![Image](img/practicalImg1_part5.png "practical Img1_part5")

![Image](img/practicalImg1_part6.png "practical Img1_part6")

![Image](img/practicalImg1_part7.png "practical Img1_part7")

![Image](img/practicalImg1_part8.png "practical Img1_part8")

![Image](img/practicalImg1_part9.png "practical Img1_part9")

<details><summary><b>Answer</b></summary>

![Image](img/practicalImg1_explanation_part1.png "practicalImg1_explanation_part1")

![Image](img/practicalImg1_explanation_part2.png "practicalImg1_explanation_part*-2")

![Image](img/practicalImg1_explanation_part3.png "practicalImg1 explanation_part3")

</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 36
</details>

### Practical Question 2

How would you execute the program ?

<details><summary><b>Answer</b></summary>
$SPARK_HOME/bin/spark-submit mnmcount.py data/mnm_dataset.csv
</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 37
</details>

### Practical Question 3

Do you understand what this line is doing ?

blogsDF.select(expr("Hits * 2")).show(2)

<details><summary><b>Answer</b></summary>
It is creating a columns where all of the values of the column "Hits" are multiplied by 2, and it is only showing the first 2 rows
</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 55
</details>


### Practical Question 4

Do you know how to create a managed and an unmanaged spark table ?

<details><summary><b>Answer</b></summary>

![Image](img/creatingManagedTable.png "creating Managed Table")

![Image](img/creatingUnManagedTable.png "creating UnManaged Table")

</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 91
</details>

### Practical Question 5

How would you view metadata in spark ?

<details><summary><b>Answer</b></summary>

![Image](img/viewingMetadata.png "viewing Metadata")

</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 93
</details>
