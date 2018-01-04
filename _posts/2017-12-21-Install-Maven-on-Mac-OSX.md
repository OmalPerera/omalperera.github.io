---
layout: post
title: Install Maven on Mac OSX
categories: [HowTo]
tags: [maven]
description: Beginners way to install Maven on Mac OSX
comments: true
---

First of all lets check for Maven in your system, because some versions of OS X came with Apache Maven 3 built in installed.

{% highlight bash %}
$ mvn -version
{% endhighlight %}

if you are getting a response as follows.

{% highlight bash %}
Apache Maven 3.5.2 (138edd61fd100ec658bfa2d307c43b76940a5d7d; 2017-10-18T13:28:13+05:30)
Maven home: /usr/share/maven
Java version: 1.8.0_60, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "mac os x", version: "10.12.5", arch: "x86_64", family: "mac"
{% endhighlight %}

Congratulations!! you don't want to install Maven, You already have it.

<br>
But if you are getting a message like this, you can go ahead with the tutorial

{% highlight bash %}
-bash: mvn: command not found
{% endhighlight %}

--------

## Installing Maven

<br>
### Step I - Downloading Maven
Download maven from [maven.apache.org](http://maven.apache.org/download.cgi)

<br>
### Step II - Extract the tar.gz
Navigate to the directory where `tar.gz` file downloaded & extract it to `/usr/share/maven`. You can use following terminal commands to exact directly in to `/usr/share/maven`.

Remember to change the maven version in the command!.

{% highlight bash %}
sudo tar -xvf apache-maven-3.5.2.tar.gz -C /usr/share/
cd /usr/share/
sudo mv apache-maven-3.5.2 maven
{% endhighlight %}

<br>
### Step III - Update the HOME & PATH variables
{% highlight bash %}
export M2_HOME=/usr/share/maven
export PATH=$PATH:$M2_HOME/bin
{% endhighlight %}


--------

Okay! now check the previous command again

{% highlight bash %}
$ mvn -version
{% endhighlight %}

if you are getting a response as follows.

{% highlight bash %}
Apache Maven 3.5.2 (138edd61fd100ec658bfa2d307c43b76940a5d7d; 2017-10-18T13:28:13+05:30)
Maven home: /usr/share/maven
Java version: 1.8.0_60, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "mac os x", version: "10.12.5", arch: "x86_64", family: "mac"
{% endhighlight %}


Congratulations! 🎉   you have correctly configured the Maven in your Mac!

<br>

--------------
