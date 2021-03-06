---
type: post0.3
title:  Spark Streaming
categories: SPARK-XAP-003
weight: 800
---


This section describes how to save computation results from the Spark Streaming application to the Data Grid.
There are two possible use cases where you may want to save computation results to the Data Grid, after `DStream` transformation or from the Spark driver.

# Saving DStream to Data Grid

To save a `DStream`, import implicit package and use `saveToGrid()` method, e.g.

{{%tabs%}}
{{%tab "Scala"%}}
```scala
import com.gigaspaces.spark.streaming.implicits._

val stream = TwitterUtils.createStream(ssc, None)
val hashTagStrings = stream.flatMap(status => status.getText.split(" ").filter(_.startsWith("#")))
val hashTags = hashTagStrings.map(tag => new HashTag(tag))
hashTags.saveToGrid()
```
{{%/tab%}}
{{%/tabs%}}

# Saving from Spark driver

Sometimes you may want to save computation results from the Spark driver. Use `saveToGrid()` or `saveMultipleToGrid()` methods available on the `SparkContext`, e.g.

{{%tabs%}}
{{%tab "Scala"%}}
```scala
topCountsStream.foreachRDD { rdd =>
  val topList = rdd.take(10)
  val topTags = new TopTags(topList.toMap)
  sc.saveToGrid(topTags)
}
```
{{%/tab%}}
{{%/tabs%}}
