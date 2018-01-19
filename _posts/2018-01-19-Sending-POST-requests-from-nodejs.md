---
layout: post
title: Sending POST requests from Node.js
categories: [Node.js]
tags: [node.js, javaScript, POST]
description: How to send post requests from Node.js
comments: true
---

Hello all of you! from this article we are going to explore how to send POST requests from a Node.js Application. Node application using here is very much simple & very easy to understand.
<br>

For the easy coding we are using [`requests`](https://www.npmjs.com/package/request) npm package to send the requests. In `requests` npm package you can find many easy http method implementations

  - _request.get()_: Defaults to method: "GET".
  - _request.post()_: Defaults to method: "POST".
  - _request.put()_: Defaults to method: "PUT".
  - _request.patch()_: Defaults to method: "PATCH".
  - _request.del() / request.delete()_: Defaults to method: "DELETE".
  - _request.head()_: Defaults to method: "HEAD".
  - _request.options()_: Defaults to method: "OPTIONS".

<br>
### So lets directly go into the code

{% highlight ruby %}

var http       = require('http');
var express    = require('express');
var request    = require('request');
var app        = express();

var server = http.createServer(app);
var port = 9000;

//Starting the server
server.listen(port);
console.log('Server Started on port ' + port);

//POST request
request.post(
  'http://localhost:8090/api/users',
  { json: { key1: 'value1', key2: 'value2' } },
  function (error, response, body) {
    if (!error && response.statusCode == 200) {
      console.log(body);
    }
  }
);

{% endhighlight %}
<br>

------

### To run the code snippet

**Step 1** - run `npm init` to initiate a command line questionnaire that will conclude with the creation of a package.json in the directory
{% highlight bash %}
$ npm init
{% endhighlight %}
<br>

**Step 2** - You first need to install related npm packages using `npm install <package-name>` command
{% highlight bash %}
$ npm install body-parser
$ npm install express
$ npm install request
{% endhighlight %}
<br>

**Step 3** Run the js file
{% highlight bash %}
$ node file-name.js
{% endhighlight %}
<br>

--------------
