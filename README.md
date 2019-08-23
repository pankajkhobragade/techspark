#Also read :
1) <a url=./dataframe> how to loop through the dataframe</a>
# techspark

<h2> What is the difference between spark.sql.shuffle.partitions and spark.default.parallelism?</h2>

From the answer here, spark.sql.shuffle.partitions configures the number of partitions that are used when shuffling data for joins or aggregations.

spark.default.parallelism is the default number of partitions in RDDs returned by transformations like join, reduceByKey, and parallelize when not set explicitly by the user. Note that spark.default.parallelism seems to only be working for raw RDD and is ignored when working with dataframes.

If the task you are performing is not a join or aggregation and you are working with dataframes then setting these will not have any effect. You could, however, set the number of partitions yourself by calling df.repartition(numOfPartitions) (don't forget to assign it to a new val) in your code.

Example of passing value whil submitting job:  
./bin/spark-submit --name FireServiceCallAnalysisDataFramePartitionTest --master yarn --deploy-mode cluster --executor-memory 2g --executor-cores 2 --num-executors 2 --conf spark.sql.shuffle.partitions=23 --conf spark.default.parallelism=23 --class com.treselle.fscalls.analysis.FireServiceCallAnalysisDF /data/SFFireServiceCall/SFFireServiceCallAnalysis.jar /user/tsldp/FireServiceCallDataSet/Fire_Department_Calls_for_Service.csv

To change the settings in your code you can simply do:

sqlContext.setConf("spark.sql.shuffle.partitions", "300")

sqlContext.setConf("spark.default.parallelism", "300")
