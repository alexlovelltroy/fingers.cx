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
Title: Email is complicated
Date: 2013-07-24
Category: programming
Tags: programming, email
Slug: email-is-complicated
author: Alex Lovell-Troy
summary: Email standards date back to 1982 in RFC form and can be a challenge to understand or follow as a software developer especially when dealing with international e-mail and forwards.  Here are the relevant standards.
---

Email standards are actually a set of overlapping standards going back to [1982](https://tools.ietf.org/html/rfc822). 
To really understand the standards, you'll need to read all the relevant IETF standards.

* [822](https://tools.ietf.org/html/rfc822) - STANDARD FOR THE FORMAT OF ARPA INTERNET TEXT MESSAGES
* [2045](https://tools.ietf.org/html/rfc2045) - Multipurpose Internet Mail Extensions (MIME) Part One: Format of Internet Message Bodies
* [2231](https://tools.ietf.org/html/rfc2231) - MIME Parameter Value and Encoded Word Extensions: Character Sets, Languages, and Continuations
* [2822](https://tools.ietf.org/html/rfc2822) - Internet Message Format
* [6532](https://tools.ietf.org/html/rfc6532) - Internationalized Email Headers

Forwarded e-mails are not well defined.  Here's what the various RFCs have to say about it:

### RFC 822
>
>4.2.  FORWARDING
>
>Some systems permit mail recipients to  forward  a  message,
>retaining  the original headers, by adding some new fields.  This
>standard supports such a service, through the "Resent-" prefix to
>field names.
>
>Whenever the string "Resent-" begins a field name, the field
>has  the  same  semantics as a field whose name does not have the
>prefix.  However, the message is assumed to have  been  forwarded
>by  an original recipient who attached the "Resent-" field.  This
>new field is treated as being more recent  than  the  equivalent,
>original  field.   For  example, the "Resent-From", indicates the
>person that forwarded the message, whereas the "From" field indi-
>cates the original author.
>
>Use of such precedence  information  depends  upon  partici-
>pants'  communication needs.  For example, this standard does not
>dictate when a "Resent-From:" address should receive replies,  in
>lieu of sending them to the "From:" address.
>
>Note:  In general, the "Resent-" fields should be treated as con-
>taining  a  set  of information that is independent of the
>set of original fields.  Information for  one  set  should
>not  automatically be taken from the other.  The interpre-
>tation of multiple "Resent-" fields, of the same type,  is
>undefined.
>
>In the remainder of this specification, occurrence of  legal
>"Resent-"  fields  are treated identically with the occurrence of
>fields whose names do not contain this prefix.


### RFC 2822
>
>3.6.6. Resent fields
>
>Resent fields SHOULD be added to any message that is reintroduced by
>a user into the transport system.  A separate set of resent fields
>SHOULD be added each time this is done.  All of the resent fields
>corresponding to a particular resending of the message SHOULD be
>together.  Each new set of resent fields is prepended to the message;
>that is, the most recent set of resent fields appear earlier in the
>message.  No other fields in the message are changed when resent
>fields are added.
>
>Each of the resent fields corresponds to a particular field elsewhere
>in the syntax.  For instance, the "Resent-Date:" field corresponds to
>the "Date:" field and the "Resent-To:" field corresponds to the "To:"
>field.  In each case, the syntax for the field body is identical to
>the syntax given previously for the corresponding field.
>
>When resent fields are used, the "Resent-From:" and "Resent-Date:"
>fields MUST be sent.  The "Resent-Message-ID:" field SHOULD be sent.
>"Resent-Sender:" SHOULD NOT be used if "Resent-Sender:" would be
>identical to "Resent-From:".
>
>resent-date     =       "Resent-Date:" date-time CRLF
>
>resent-from     =       "Resent-From:" mailbox-list CRLF
>
>resent-sender   =       "Resent-Sender:" mailbox CRLF
>
>resent-to       =       "Resent-To:" address-list CRLF
>
>resent-cc       =       "Resent-Cc:" address-list CRLF
>
>resent-bcc      =       "Resent-Bcc:" (address-list / [CFWS]) CRLF
>
>resent-msg-id   =       "Resent-Message-ID:" msg-id CRLF
>
>Resent fields are used to identify a message as having been
>reintroduced into the transport system by a user.  The purpose of
>using resent fields is to have the message appear to the final
>recipient as if it were sent directly by the original sender, with
>all of the original fields remaining the same.  Each set of resent
>fields correspond to a particular resending event.  That is, if a
>message is resent multiple times, each set of resent fields gives
>identifying information for each individual time.  Resent fields are
>strictly informational.  They MUST NOT be used in the normal
>processing of replies or other such automatic actions on messages.
>
>Note: Reintroducing a message into the transport system and using
>resent fields is a different operation from "forwarding".
>"Forwarding" has two meanings: One sense of forwarding is that a mail
>reading program can be told by a user to forward a copy of a message
>to another person, making the forwarded message the body of the new
>message.  A forwarded message in this sense does not appear to have
>come from the original sender, but is an entirely new message from
>the forwarder of the message.  On the other hand, forwarding is also
>used to mean when a mail transport program gets a message and
>forwards it on to a different destination for final delivery.  Resent
>header fields are not intended for use with either type of
>forwarding.
>
>The resent originator fields indicate the mailbox of the person(s) or
>system(s) that resent the message.  As with the regular originator
>fields, there are two forms: a simple "Resent-From:" form which
>contains the mailbox of the individual doing the resending, and the
>more complex form, when one individual (identified in the
>"Resent-Sender:" field) resends a message on behalf of one or more
>others (identified in the "Resent-From:" field).
>
>Note: When replying to a resent message, replies behave just as they
>would with any other message, using the original "From:",
>"Reply-To:", "Message-ID:", and other fields.  The resent fields are
>only informational and MUST NOT be used in the normal processing of
>replies.
>
>The "Resent-Date:" indicates the date and time at which the resent
>message is dispatched by the resender of the message.  Like the
>"Date:" field, it is not the date and time that the message was
>actually transported.
>
>The "Resent-To:", "Resent-Cc:", and "Resent-Bcc:" fields function
>identically to the "To:", "Cc:", and "Bcc:" fields respectively,
>except that they indicate the recipients of the resent message, not
>the recipients of the original message.
>
>The "Resent-Message-ID:" field provides a unique identifier for the
>resent message.

By Alex Lovell-Troy
