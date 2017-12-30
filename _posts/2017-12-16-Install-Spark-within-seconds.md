---
layout: post
title: Up & Run Apache Spark within seconds with all dependencies
categories: [bigdata]
tags: [apache, kafka, bigdata, scala, Real-Time-Processing, Spark, streaming, java, git]
description: Fully automated bash script for Spark installation in linux Dabian based Servers.
comments: true
---

[install-spark](https://github.com/OmalPerera/install-Spark) is a fully automated Spark installation bash script for linux Dabian based Servers.

Script will install following key components needes to install Real-time data processing with few clicks.

  - Full Update & Upgrade
  - install up-to-date JRE
  - install up-to-date JDK
  - install scala 2.11.11
  - install Apache kafka 2.10 0.10.2.1
  - install git
  - install sbt
  - install Apache spark spark-2.2.0 pre-built for Apache hadoop 2.7 & later
  - all the environmental + all the PATH variables

<br>


# Using the Script #

### 1. Granting the executing permission to the script ###

Navigate to the directory where the script is.

{% highlight bash %}
$ chmod +x install-spark.sh
{% endhighlight %}


### 2. Run the script ###

{% highlight bash %}
$ source install-spark.sh
{% endhighlight %}

<br>
<br>

# Customizing the Script #

In the script you can define the **versions** of libraries that you need to install.<br>

_eg: spark-2.2.0-bin-hadoop2.7 , scala-2.11.11_
<br>

Under the `# Default Application versions` define your prefered version.

{% highlight bash %}
# Default Application versions

SCALA_VERSION="scala-2.11.11"
SPARK_VERSION="spark-2.2.0-bin-hadoop2.7"
KAFKA_VERSION="kafka_2.10-0.10.2.1"
{% endhighlight %}

<br>

--------------
