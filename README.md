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

do you understand what are the spark stages ?

<details><summary><b>Answer</b></summary>
As part of the DAG nodes, stages are created based on what operations can be per‐
formed serially or in parallel . Not all Spark operations can happen in a
single stage, so they may be divided into multiple stages.
</details>

<details><summary><b>Source</b></summary>
learningSpark2.0 - pag 28
</details>

### Theorical Question 4

do you understand the difference between spark operation actions and spark operation transformation ?

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