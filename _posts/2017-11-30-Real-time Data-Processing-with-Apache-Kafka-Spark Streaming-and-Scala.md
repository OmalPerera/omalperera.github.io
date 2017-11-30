---
layout: post
title: Real-time Data Processing with Apache Kafka, Spark Streaming and Scala
categories: [general, bigdata]
tags: [apache, kafka, bigdata, scala, Real-Time-Processing, Spark, streaming]
description: Practicle guide to connect Apache Kafka with Apache Spark using Scala, for real-time processing. 
---

### *************should change ###
Mainly we deal with the `/bin` folder.

  - for mac\t- `kafka_2.10-0.10.2.0/bin/`
  - for Windows - `kafka_2.10-0.10.2.0\bin\windows\`


### My Development Environment ###

  - Spark version 	- 2.2.0
  - Scala version	- 2.11.11
  - SBT version 	- 0.13.16
  - Kafka version 	- 
  - OS 				- Mac OS (Unix based)

  888888888888 quote the spark is designed to work with scala 2.11.x

<br>

# Developing the Spark app with Scala #
<br>
**Note** : Dependencies for this project can be definitely change with the time. And also basically those dependencies depends on the scala version, Spark version, SBT version etc that you have installed in your system. So try to stick with above mentioned development environment or else follow up the error log & adjust the dependencies according to your development environment.

<br>

So lets start the journey to Real-time data processing with **kafka** , **spark** , & **scala** !

### build.sbt file ###

As we are doing our project with SBT, here is the sbt build file `build.sbt` , where we include all the dependencies needed for our project.

{% highlight bash %}
  name := "RealTimeDataProcessing"

  version := "0.1"

  scalaVersion := "2.11.11"

  resolvers += "Spark Packages Repo" at "http://dl.bintray.com/spark-packages/maven"

  dependencyOverrides += "com.fasterxml.jackson.core" % "jackson-core" % "2.8.7"
  dependencyOverrides += "com.fasterxml.jackson.core" % "jackson-databind" % "2.8.7"
  dependencyOverrides += "com.fasterxml.jackson.module" % "jackson-module-scala_2.11" % "2.8.7"

  libraryDependencies += "org.apache.spark" % "spark-core_2.11" % "2.2.0"
  libraryDependencies += "org.apache.spark" % "spark-sql_2.11" % "2.2.0"
  libraryDependencies += "org.apache.spark" % "spark-streaming_2.11" % "2.2.0"
  libraryDependencies += "org.apache.spark" % "spark-mllib-local_2.11" % "2.2.0"
  libraryDependencies += "dibbhatt" % "kafka-spark-consumer" % "1.0.12"
  libraryDependencies += "org.apache.kafka" % "kafka_2.11" % "1.0.0"
  libraryDependencies += "org.apache.spark" % "spark-streaming-kafka-0-8_2.11" % "2.1.0"
{% endhighlight %}

<br>

### Integrating Spark Streaming and Apache Kafka ###

Here we are going to fetch data from kafka topic to our spark app. if you are absoulute newbie to these area, I'm recomending you to google on _what is kafka?, how it works, what are kafka topics? what spark does?_ . I’m leaving it to you as a homework.

So this will be the code

{% highlight js %}
import org.apache.spark.SparkConf
import org.apache.spark.streaming.kafka.KafkaUtils
import org.apache.spark.streaming.{Seconds, StreamingContext}


object kafkaConsumer {

  def main(args: Array[String]) {

    println("program started")

    val conf = new SparkConf().setMaster("local[*]").setAppName("kafkaConsumer")

    //app consumes messages every 5 seconds from kafka
    val ssc = new StreamingContext(conf, Seconds(5))

    // my kafka topic name is 'test'
    val kafkaStream = KafkaUtils.createStream(ssc, "localhost:2181","spark-streaming-consumer-group", Map("test" -> 5))

    kafkaStream.print()
    ssc.start
    ssc.awaitTermination()

  }
}
{% endhighlight %}

lets identify what the code consist of.
8
8explain the keywords
8
8
8
8
8

so we are now done with the spark app. prior to run our app, we have to make sure we have Up & running kafka topic with name of `test`. If not Spark app wont be able to consume the streams.

  <br>

--------------

<br>
# Configuring up Kafka broker #

If you dont have kafka installed in your environment, you can refer my post [88888888888](8888888 link 88888888) to setup it from the scratch.
if you have already installed kafka, we have to create a topic named `test` & start kafka producer.

  <br>


### Starting the Zookeeper Server ###

  - As you are now in the `kafka_2.10-0.10.2.0` directory (can be differ depends on youy kafka version), execute the following command
  {% highlight bash %}
  sudo bin/zookeeper-server-start.sh config/zookeeper.properties
  {% endhighlight %}

  - Starting the Kafka Server
  {% highlight bash %}
  sudo bin/kafka-server-start.sh config/server.properties
  {% endhighlight %}

  <br>

### Creating topic ###

  - Creating a topic located at zookeeper at the `localhost:2181` named `test` with a `single partition` & `single replica`. (do it in a separate terminal).
  {% highlight bash %}
  sudo bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
  {% endhighlight %}

  - To ensure that our topic is created, execute following command
  {% highlight bash %}
  bin/kafka-topics.sh --list --zookeeper localhost:2181
  {% endhighlight %}

  <br>

### Running the Producer ###

  - This is for feed the `test` Topic (do it in a separate console)
  {% highlight bash %}
  bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
  {% endhighlight %}

  - You can send messages to the Kafka cluster from the console even except the standard file inputs. just type the message in the console. These messages will be consumed by our spark app.
  {% highlight bash %}
  This is a Sample message
  {% endhighlight %}

  <br>

### Running the Consumer ###

  - This is for listen to the producer at that port (do it in a separate terminal).
  - Following command will listens for the topic inputs and outputs
    {% highlight bash %}
    bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic test
    {% endhighlight %}

  - If you type messages into the producer terminal you should see them appear in the consumer terminal.

  <br>

Now Kafka broker is ready to go. Now its time to run our Spark application.

  <br>

--------------

<br>
# Running Spark Application #

