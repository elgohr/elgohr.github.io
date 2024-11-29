---
title: "Maven Latest in Continuous Integration"
date: 2017-12-27
draft: false
tags:
  - Maven
  - CI/CD
  - Concourse
description: "Howto use maven latest in CI/CD"
toc: false
---

`Disclaimer: This article requires a high automated test coverage. If you haven't got this, you should probably start your journey at http://blog.cleancoder.com/`

In the past few weeks I played with the idea of self-driving, zero-maintenance jobs.
Let's call them Herbie-Jobs 🚗  

The idea of using CI/CD pipelines inside [Concourse](https://concourse.ci/) or other tools, to update components isn't that new.
Nevertheless I asked myself whether this could also be useful in daily software-development.  

I remembered my sadness, when Maven dropped the LATEST-feature in [Maven3.x](https://cwiki.apache.org/confluence/display/MAVEN/Maven+3.x+Compatibility+Notes#Maven3.xCompatibilityNotes-PluginMetaversionResolution).
Maybe they had a sixth sense, looking at the joy of `npm install` today.

But why did I like this feature?

There are probably some components, which are really stable and people don't go there very often to perform changes (never change a beloved, stable system).  
This behavior leads to rotten code, because you often end up in a  _Ok, now we need to change this component. Let's upgrade the libraries from the last century_-situation.  

I would like to do the housekeeping more often, but steady.

Recently I also made contributions to [Concourse](https://concourse.ci/), in this way I took a dive into [Go](https://golang.org/). [Go](https://golang.org/) has some great features for [code generation](https://blog.golang.org/generate).

So why don't use this approach and make Maven LATEST reproducible?  

Of course I started with a fresh project in my IDE (What else?).
But after a while I remembered the [maven versions plugin](http://www.mojohaus.org/versions-maven-plugin/).
It ships with Maven, so why not use it?
The plugin helps you to update the versions in your POM.xml to latest(release/snapshot/edge) and modifies your POM.xml  

The plugin can be used by executing  
```bash
mvn versions:update-parent
mvn versions:use-latest-releases
```

In this case `versions:update-parent` updates the POM.xml-parent version (if there is one) and `versions:use-latest-releases` updates the dependencies.

Create a pipeline looking like  
```
(Checkout-Git) -> (Run mvn versions) -> (Commit-Git)
```  
In this way you will always have a reproducible build, using your desired latest versions, which are updated automatically by Herbie. And automatic code-generation [tastes a bit like Google](https://cacm.acm.org/magazines/2016/7/204032-why-google-stores-billions-of-lines-of-code-in-a-single-repository/fulltext), so why don't give it a try?  

Hint: As seen above, I didn't say anything about the trigger of the pipeline. I would try it out as a team, how much time you could spend on eventually new incompatibilities and when.