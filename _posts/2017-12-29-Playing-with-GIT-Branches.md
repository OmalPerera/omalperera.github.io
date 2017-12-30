---
layout: post
title: Playing with GIT Branches
categories: [git]
tags: [git, branch, remote, local, git pull, git fetch]
description: Silly things you need to know about git, when dealing with branches.
comments: true
---

Once you clone a git repository from github, you may need to figure out what other branches really exist on the remote repository so you can `pull` them down and `checkout` them, `merge` them into your local branches. It is not a hard task if you are using GitHub Desktop. But much better to know if you interacting with command line.  
<br>
For the sake of better understanding, lets look into an example of cloning a remote repository into your local machine.
<br>
{% highlight bash %}
$ git clone https://github.com/OmalPerera/sample-project.git
$ cd sample-project
{% endhighlight %}
<br>

#### To check the local branches in your repository

{% highlight bash %}
$ git branch

* master
{% endhighlight %}
<br>

#### To list all existing Branches

{% highlight bash %}
$ git branch -av

* master                cc30c0f Initial commit
  remotes/origin/HEAD   -> origin/master
  remotes/origin/dev    cc30c0f Initial commit
  remotes/origin/master cc30c0f Initial commit
{% endhighlight %}
<br>

#### Create a local tracking branch in order to work on it

{% highlight bash %}
$ git checkout <dev>

  Branch dev set up to track remote branch dev from origin.
  Switched to a new branch 'dev'
{% endhighlight %}
<br>

#### Fetching all remote branches at once

{% highlight bash %}
$ git pull --all
{% endhighlight %}

Below I have explained the difference between `git pull` & `git fetch`

<br>

#### Switching between branches

{% highlight bash %}
$ git checkout <branch>
{% endhighlight %}
<br>

#### Creating a new tracking branch based on a remote branch

{% highlight bash %}
$ git checkout --track <remote/branch>
{% endhighlight %}
<br>

#### Deleting local branch

{% highlight bash %}
$ git branch -d <branch>
{% endhighlight %}

<br><br>

---------
### `git pull` vs `git fetch`
<br>
#### git pull
Git tries to automate your work for you. Since it is context sensitive, git will merge any pulled commits into the branch you are currently working in. pull automatically merges the commits without letting you review them first. You may run into frequent conflicts if you are mot managing your branches in a proper way.

<br>
#### git fetch
Git gathers any commits from the particular branch that do not exist in your current branch & saves them in your local repository. But it doesn't merge them with your current branch. This is particularly useful if you need to keep your repository up to date, but are working on something that might break if you update your files.
<br>
<img src="https://i.imgur.com/w4sr7bp.png" alt="pull vs fetch diagram" style="width: 400px;"/>

<br>

---------
### Explaining `Downstream` & `UpStream`
<br>
In terms of source control, you're `downstream` when you copy (clone, checkout, etc) from a repository. Information flowed `downstream` to you.

<br>
When you make changes, you usually want to send them back `upstream` so they make it into that repository so that everyone pulling from the same source is working with all the same changes.
[Source](https://stackoverflow.com/questions/2739376/definition-of-downstream-and-upstream)




<br>

--------------
