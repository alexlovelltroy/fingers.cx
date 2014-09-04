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
headerstyle: plates
# headerstyle: presidencia
# headerstyle: statue
# headerstyle: unicorn
# headerstyle: yoda
# headerstyle: martenitsa
# headerstyle: cloud
# headerstyle: container
Title: Email in django is harder than it should be
Date: 2013-07-24
Category: programming
Tags: programming, python, django, email
Slug: email-is-hard
author: Alex Lovell-Troy
summary: Sending email well in django can be problematic, but it doesn't have to be.
---

# TLDR;
> Email in django isn't as easy as it should be.  I use [SES](https://github.com/hmarr/django-ses) with Celery as a backend and handle email content with [Class Based Emails](https://gist.github.com/alexlovelltroy/6074043) and thought I would share it.

In django, most of the [complication of email](/email-is-complicated.html) is hidden by the email module in core: [djang.core.email.EmailMessage](https://docs.djangoproject.com/en/dev/topics/email/#django.core.mail.EmailMessage)
which lets you deal with email as python objects rather than the ugly text
files they really are.  This is really based on the [python email module](http://docs.python.org/2/library/email) which handles MIME recursion
and headers.  Did you know that email messages can nest infinitely?  For sending email in django, the code is actually very simple:

    ```python
    from django.core.mail import EmailMultiAlternatives

    subject, from_email, to = 'hello', 'from@example.com', 'to@example.com'
    text_content = 'This is an important message.'
    html_content = '<p>This is an <strong>important</strong> message.</p>'
    msg = EmailMultiAlternatives(subject, text_content, from_email, [to])
    msg.attach_alternative(html_content, "text/html")
    msg.send()
    ```

This is readable and sensible, but storing the content in strings in the code
isn't a good idea.  Also, it's going to be tough to personalize this for each
user.  Django templates are made to solve that, right?  We use them for
the web part of our system to solve a similar problem.  But wait, those assume an [HTTP Response](https://docs.djangoproject.com/en/dev/ref/template-response/#simpletemplateresponse-objects).

Let's try anyway.  I'm sure we can just store the e-mail templates as models and make this easy, right?

    ```python
    from django.conf import settings
    from django.template import Template, Context
    from django.core.mail import EmailMultiAlternatives
    
    class TemplatedEmailMessage(models.Model):
        name = models.CharField(max_length=64, unique=True)
        description = models.CharField(max_length=512)
        subject = models.CharField(max_length=512)
        text_content = models.TextField()
        html_content = models.TextField(null=True, blank=True)
    
        def send(self, from_email=settings.DEFAULT_FROM_EMAIL, to_email=None, context_dict=None):
            if isinstance(to_email, list):
                to_addr = to_email
            else:
                to_addr=[to_email]
                
            msg = EmailMultiAlternatives(
                Template(self.subject).render(Context(context_dict)),
                Template(self.text_content).render(Context(context_dict)),
                from_email,
                to_addr
            )
            if self.html_content != "":
                tmpl = Template(self.html_content)
                msg.attach_alternative(
                    tmpl.render(Context(context_dict)),
                    "text/html"
                )
            msg.send()
    ```

That's more moving parts to maintain, and we're still not even pulling templates from the filesystem or logging the mail success/fail send rate.  We haven't even addressed delaying e-mail sending or using a different [backend](https://docs.djangoproject.com/en/dev/topics/email/#email-backends).  I use [django-ses](https://github.com/hmarr/django-ses).


I'm not the first one to see that this is a problem and work on it.

* [templated-emails](https://github.com/philippWassibauer/templated-emails) Templated E-mails for Django
* [django-email-extras](https://github.com/stephenmcd/django-email-extras) multipart and pgp encrypted e-mails
* [django-mailer](https://github.com/pinax/django-mailer/network) The venerable and splintered queue-based django-mailer
* [email tricks](http://stackful-dev.com/django-email-tricks-part-1.html) The stackful.io email tricks post

I like the class based views in django and use a similar approach when dealing with emails.

here's a first draft of making what I'm working on public

[Class Based Emails for Django](https://gist.github.com/alexlovelltroy/6074043)
<script src="https://gist.github.com/alexlovelltroy/6074043.js"></script>

By Alex Lovell-Troy
