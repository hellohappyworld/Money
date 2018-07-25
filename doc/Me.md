![](img/20180725160816.png)

答：redeuceByKey:reduceByKey在结果发送至reducer之前会对每个mapper在本地进行merge,有点类似于在MapReduce中的combiner。这样做的好处在于，在map端进行一次reduce之后，数据量会大幅度减少，从而减少传输，保证reduce端能够更快的进行结果计算。

![](img/20180725161237.png)

groupByKey:groupByKey会对每个RDD中的value值进行聚合形成一个序列(Iterator),此操作发生在reduce端，所以势必会造成所有的数据通过网络进行传输，造成不必要的浪费，同时数据量十分大的时候，还会造成oom（内存溢出）。

因此，大量数据的reduce操作时候建议使用reduceByKey，不仅可以提高速度，还可以防止使用groupByKey造成的内存溢出问题。