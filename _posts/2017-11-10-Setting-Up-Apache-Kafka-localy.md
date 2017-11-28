---
layout: post
title: Setting Up Apache Kafka locally
categories: [general, bigdata]
tags: [apache, kafka, bigdata]
description: Sample placeholder post.
---

This Guide is for the linux based OS & directories in the Kafka setup can be slightly different in the windows setup.
Mainly we deal with the `/bin` folder.

  - for mac     - `kafka_2.10-0.10.2.0/bin/`
  - for Windows - `kafka_2.10-0.10.2.0\bin\windows\`

<br>

**Note** : Every command must be executed in a separate terminal or console except for creating the topic and ending a server.

<br>

### Step 1 - Download the source code & extract it ###

  - Download Apache kafka from [here](https://www.apache.org/dyn/closer.cgi?path=/kafka/1.0.0/kafka_2.10-0.10.2.0.tgz)

  - Navigate to the downloaded directory & `untar` the kafka_2.10-0.10.2.0.tar
  {% highlight bash %}
  tar -xzf kafka_2.10-0.10.2.0.tgz
  cd kafka_2.10-0.10.2.0
  {% endhighlight %}

<br>

### Step 2 - Starting the Server ###

  - You first need to start a ZooKeeper serve, since Kafka uses Zookeeper for the tasks such as Electing a controller, Topic configuration, ACLs & for many more.

  - As you are now in the `kafka_2.10-0.10.2.0` directory execute the following command
  {% highlight text %}
  bin/zookeeper-server-start.sh config/zookeeper.properties
  {% endhighlight %}

  - Starting the Kafka Server
  {% highlight bash %}
  bin/kafka-server-start.sh config/server.properties
  {% endhighlight %}

  <br>

### Step 3 - Creating a Topic ###

  - Creating a topic located at zookeeper at the `localhost:2181` named `mytopic` with a `single partition` & `single replica`.
  {% highlight bash %}
  bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic mytopic
  {% endhighlight %}

  - To ensure that our topic is created, execute following command
  {% highlight bash %}
  bin/kafka-topics.sh --list --zookeeper localhost:2181
  {% endhighlight %}

  <br>

### Step 4 - Running the Producer ###

  - This is for feed the `mytopic` Topic (do it in a separate console)
  {% highlight bash %}
  bin/kafka-console-producer.sh --broker-list localhost:9092 --topic mytopic
  {% endhighlight %}

  - You can send messages to the Kafka cluster from the console even except the standard file inputs. just  type the message in the console.
  {% highlight bash %}
  This is a Sample message
  {% endhighlight %}

  <br>

### Step 5 - Running the Consumer ###

  - This is for listen to the producer at that port (do it in a separate terminal).
  - Following command will listens for the topic inputs and outputs
    {% highlight bash %}
    bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic mytopic
    {% endhighlight %}

  - If you type messages into the producer terminal you should see them appear in the consumer terminal.

  <br>

### Step 6 - Stopping the Kafka Server ###
{% highlight bash %}
bin/kafka-server-stop.sh  
{% endhighlight %}

  <br>

### Step 7 - Stopping the Zookeeper Server ###
{% highlight bash %}
bin/zookeeper-server-stop.sh
{% endhighlight %}
