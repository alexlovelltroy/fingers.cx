---
layout: post
# headerstyle: ishtar
# headerstyle: alps
# headerstyle: bench
# headerstyle: door
# headerstyle: eagle
# headerstyle: flower
# headerstyle: hawk
# headerstyle: home
# headerstyle: ishtar
# headerstyle: midgets
headerstyle: mountain
# headerstyle: network
# headerstyle: plates
# headerstyle: presidencia
# headerstyle: statue
# headerstyle: unicorn
# headerstyle: yoda
# headerstyle: martenitsa
# headerstyle: cloud
# headerstyle: container
title: deploying django for free
date: 2013-06-26
category: programming
tags: programming, python, django, ops
slug: deploy-django-for-free
author: Alex
summary: Staying within the free tier on AWS for django deployment
---

### TLDR;
> Deploying a scalable django application on AWS is possible within the free tier and the secrets are the same as any other devops problem.  Cache, Cache Cache...

## Do as little as possible between request and response
Django does all its work between the time when you request the webpage and when it returns the response.  On a responsive website, that can't be more than half a second.  For things to feel instantaneous, you're looking at ~200ms.  That's not much time.  And, if we're using an AWS micro instance, that means we need to really limit per-request computation.
The whole point of a dynamic site is to show updated information as soon as it changes.  So, you're going to have to do something between request and response.
## async execution with djcelery with amazon SQS
[Celery](http://celeryproject.org) is the queue-solution of choice for django developers everywhere.  I use it with SQS.
## assets on amazon S3 using django-storages and staticfiles
## send mail through amazon SES
Sending mail is a blocking operation as it opens a socket and holds it open while transmitting.  In most cases, it isn't a problem, but when it is, it will crash your site.  Offload that to a queue using SQS and/or SES. SES also has the benefit of doing DKIM and SPF well.
## cache everything with amazon Elasticache
Running large, scalable applications means adding caching everywhere you can.  Elasticache can cache just about anything and you set the timeout on each item.  It's memcache for the cloud and it's really [[fast.]]
## data in RDS (amazon manages it)
