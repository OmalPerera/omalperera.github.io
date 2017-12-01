---
layout: post
title: Real-time Data Processing with Apache Kafka, Spark Streaming and Scala
categories: [bigdata]
tags: [apache, kafka, bigdata, scala, Real-Time-Processing, Spark, streaming]
description: Practical guide to connect Apache Kafka with Apache Spark using Scala, for real-time processing.
comments: true
---

Real-time processing! kind of a trending term that techie people talks & do things. So actually what are the components do we need to perform Real-time Processing. Apache Spark Streaming, Apache Kafka are key two components out of many that comes in to my mind.

Spark Streaming is built-in library in Apache Spark which is micro-batch oriented stream processing engine. There are other alternatives such as Flink, Storm etc.

As we discussed in above paragraph, Spark Streaming reads & process streams. So who provides these Streams to Spark ?. In that case, we use Apache kafka to accomplish this task.

But why? can't we use Direct streams via TCP sockets?. But the point is _`parallelism`_ . Kafka enables parallel streaming with a support named `"partition"` which is highly compatible to use with Spark’s `"partition"`.

I think, now it is clear why are we using spark with kafka. So let's look in to integrate these two components. Consider this as a starting point.

<br>


### My Development Environment ###

  - Spark version 	- 2.2.0
  - Scala version	  - 2.11.11
  - SBT version 	  - 0.13.16
  - Kafka version 	- 0.10.2.0
  - OS 				      - Mac OS (Unix based)

As mentioned the Spark [docs](https://spark.apache.org/docs/latest/index.html#downloading)
"Spark runs on Java 8+, Python 2.7+/3.4+ and R 3.1+. For the Scala API, **Spark 2.2.0 uses Scala 2.11. You will need to use a compatible Scala version (2.11.x)**."
so you better use any 2.11.x version of scala in order to avoid dependency problems.

Before continuing the project, probably you better double check your scala & sbt installations in your machine
{% highlight bash %}
$ scala -version
Scala code runner version 2.11.11 -- Copyright 2002-2017, LAMP/EPFL
{% endhighlight %}

{% highlight bash %}
$ sbt sbtVersion
[info] Updated file /root/.../project/build.properties: set sbt.version to 0.13.16
{% endhighlight %}

<br>

# Developing the Spark app with Scala #
<br>
**Note** : Dependencies for this project can be definitely change with the time. And also basically those dependencies depend on the scala version, Spark version, SBT version etc that you have installed in your system. So try to stick with above mentioned development environment or else follow up the error log & adjust the dependencies according to your development environment.
<br>

So let's start the journey to Real-time data processing with **kafka** , **spark** , & **scala** !

### Directory structure ###

first of all we have to arrange our project's directories & files in to a specific order which supports for sbt. Folder structure should be as follows.

{% highlight bash %}
./src
./src/main
./src/main/scala
./src/main/scala/kafkar.scala
./build.sbt
{% endhighlight %}

<br>

### build.sbt file ###

As we are doing our project with SBT, here is the sbt build file `build.sbt` , where we include all the dependencies needed for our project.

{% highlight bash %}
name := "kafkar"

version := "0.1"

scalaVersion := "2.11.11"

retrieveManaged := true

fork := true

libraryDependencies += "org.apache.spark" % "spark-streaming_2.11" % "2.2.0"

libraryDependencies += "org.apache.spark" % "spark-streaming-kafka-0-8_2.11" % "2.1.0"
{% endhighlight %}

<br>

### Integrating Spark Streaming and Apache Kafka ###

Here we are going to fetch data from kafka topic to our spark app. if you are absolute newbie to these area, I'm recommending you to google on _what is kafka?, how it works, what are kafka topics? what spark does?_ . I’m leaving it to you as a homework.

So this will be the code

{% highlight js %}
import org.apache.log4j.{Level, Logger}
import org.apache.spark.SparkConf
import org.apache.spark.streaming.kafka.KafkaUtils
import org.apache.spark.streaming.{Seconds, StreamingContext}


object kafkar {

  def main(args: Array[String]) {

    Logger.getLogger("org").setLevel(Level.OFF)
    Logger.getLogger("akka").setLevel(Level.OFF)

    println("program started")

    val conf = new SparkConf().setMaster("local[4]").setAppName("kafkar")
    val ssc = new StreamingContext(conf, Seconds(2))

    // my kafka topic name is 'test'
    val kafkaStream = KafkaUtils.createStream(ssc, "localhost:2181","spark-streaming-consumer-group", Map("test" -> 5))

    kafkaStream.print()
    ssc.start
    ssc.awaitTermination()

  }
}
{% endhighlight %}

You would need a multi-core machine (>= 2 cores) for spark-streaming to work while running in standalone mode. But it will work fine if you run it in local mode with master as local[4].

there are several Master Urls passed to Spark
  - **local** - Run Spark locally with one worker thread (no parallelism at all).
  - **local[K]** -	Run Spark locally with K worker threads (ideally, set this to the number of cores on your machine).
  - **local[* ]**	- Run Spark locally with as many worker threads as logical cores on your machine.
  - **spark://HOST:PORT** - 	Connect to the given Spark standalone cluster master.

are some of them

so we are now done with the spark app. prior to run our app, we have to make sure we have Up & running kafka topic with name of `test`. If not Spark app wont be able to consume the streams.

  <br>

--------------

<br>
# Configuring up Kafka broker #

If you don't have kafka installed in your environment, you can refer my post [Setting Up Apache Kafka locally](https://omalperera.github.io/general/bigdata/2017/11/10/Setting-Up-Apache-Kafka-localy.html) to setup it from the scratch.
if you have already installed kafka, we have to create a topic named `test` & start kafka producer.

  <br>


### Starting the Zookeeper Server ###

  - As you are now in the `kafka_2.10-0.10.2.0` directory (can be differ depends on your kafka version), execute the following command
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
# Running Spark Application with sbt #

We are all set for running the application. So open a terminal window & navigate to the project directory. Now we just want to compile the code with `sbt compile` & run it with `sbt run` on sbt console.

{% highlight bash %}
$ sbt compile

[info] Loading project definition from /root/xc/project
[info] Loading settings from build.sbt ...
[info] Set current project to kafkar (in build file:/root/xc/)
[info] Executing in batch mode. For better performance use sbt's shell
[success] Total time: 6 s, completed Dec 1, 2017 12:36:52 PM
{% endhighlight %}

{% highlight bash %}
$ sbt run

[info] Loading project definition from /root/xc/project
[info] Loading settings from build.sbt ...
[info] Set current project to kafkar (in build file:/root/xcity/)
[info] Running (fork) kafkar
[info] program started
[info] -------------------------------------------
[info] Time: 1512131872000 ms
[info] -------------------------------------------
[info] -------------------------------------------
[info] Time: 1512131873000 ms
[info] -------------------------------------------
[info] -------------------------------------------
[info] Time: 1512131874000 ms
[info] -------------------------------------------
{% endhighlight %}

<br>

We are done! Spark Streaming is now connected to Apache Kafka and consumes messages every 2 seconds. Leave it running and switch to kafka Producer terminal and enter some messages

{% highlight bash %}
omal@ubuntu-london:~/kafka_2.10-0.10.2.1# sudo bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test

OK,41,14,690,41,2014-11-13T09:15:00,3,32502142,158355
OK,36,19,690,36,2014-11-13T09:20:00,1,32502581,158355
OK,36,11,690,36,2014-11-13T09:25:00,0,32503023,158355
{% endhighlight %}

<br>

Switch to the terminal with Spark application running and see the message printed out.

{% highlight bash %}
[info] -------------------------------------------
[info] Time: 1512137948000 ms
[info] -------------------------------------------
[info] -------------------------------------------
[info] Time: 1512137949000 ms
[info] -------------------------------------------
[info] (null,OK,41,14,690,41,2014-11-13T09:15:00,3,32502142,158355)
[info] (null,OK,36,19,690,36,2014-11-13T09:20:00,1,32502581,158355)
[info] (null,OK,36,11,690,36,2014-11-13T09:25:00,0,32503023,158355)
[info] -------------------------------------------
[info] Time: 1512137950000 ms
[info] -------------------------------------------
[info] -------------------------------------------
[info] Time: 1512137951000 ms
[info] -------------------------------------------
{% endhighlight %}


Additionally you can visit Apache Spark web UI (most of the time http://localhost:4040/) to get more details, histograms on your Spark job.

<br>

#### Congratulations!  🎉   you have completed the mission

<br>

---
