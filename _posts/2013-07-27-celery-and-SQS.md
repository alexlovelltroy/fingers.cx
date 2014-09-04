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
# headerstyle: mountain
# headerstyle: network
# headerstyle: plates
# headerstyle: presidencia
headerstyle: statue
# headerstyle: unicorn
# headerstyle: yoda
# headerstyle: martenitsa
# headerstyle: cloud
# headerstyle: container
title: Using celery with Amazon SQS
date: 2013-07-27
category: programming
tags: programming, python, django, ops, SQS
Author: Alex
Summary: The documentation for using SQS with celery isn't great.  This post should help.
---

I found the documentation lacking.  Here's what works for me.

    BROKER_USER = AWS_ACCESS_KEY_ID
    BROKER_PASSWORD = AWS_SECRET_ACCESS_KEY
    BROKER_TRANSPORT = 'sqs'
    # Other docs suggest you should try this.  It didn't work for me
    #BROKER_URL = 'sqs://%s:%s@' % (BROKER_USER, BROKER_PASSWORD)
    #BROKER_HOST = BROKER_PASSWORD
    BROKER_TRANSPORT_OPTIONS = { 
            'region': 'us-west-2',
    }
    CELERY_DEFAULT_QUEUE = 'production_queue'
    CELERY_QUEUES = { 
        CELERY_DEFAULT_QUEUE: {
            'exchange': CELERY_DEFAULT_QUEUE,
            'binding_key': CELERY_DEFAULT_QUEUE,
        }
    }

Here's a list of other answers I found while searching:

* [Tobias's instructions at caktusgroup](http://www.caktusgroup.com/blog/2011/12/19/using-django-and-celery-amazon-sqs/) - An almost complete answer
* [Official Docs](http://docs.celeryproject.org/en/latest/getting-started/brokers/sqs.html) - Not written for those without experience
