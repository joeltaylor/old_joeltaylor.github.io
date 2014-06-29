---
layout: post
title: "Bootstrap-sass"
date: 2014-06-19 18:59:40 -0400
comments: true
categories: Rails
---

<p>I use Bootstrap so that I can focus on developing as opposed to designing. It's
all fun and games till your entire layout breaks for "no reason".</p>

<p>I was experimenting with different versions of Bootstrap-sass and ended up using
version 3.1.1. Despite having run
```
bundle update
```
and restarting the server,
my app was using the wrong version of Bootstrap (v.2.1.2).</p>

<p>Simple fix. Shut down the server and run:
```
rake tmp:clear
```</p>

<p>I'm a bit ashamed it took me more than 5 minutes to figure this out.</p>
