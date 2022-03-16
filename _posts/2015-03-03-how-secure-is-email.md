---
layout: post
title: How secure is email
---

Yesterday I was in the process of booking something and was asked to email
my credit card details to confirm my payment. Given that I trust the recipient
not to abuse my card should I have complied?

The simple answer is **no**.

It is not difficult to imagine that there is plenty of talented hackers on the
internet living in the developing world resenting the West and stealing away
for the lack of better opportunities.

Emails were originally plain-text messages passed around open for anyone to
intercept. Things have changed now and emails are often encrypted.  However the
encryption is not endpoint to endpoint encryption, it's a point-to-point
encryption, meaning an email keeps getting decrypted and encrypted while it is
passed around. For this chain to stay uncompromised every node should use
encryption.

A more complex answer is: as long as you use https web-client[^1], the
recipient is using https web-client[^2] and the recipient's email server
supports TLS encryption then the answer is yes, i.e. your email *most likely*
won't be intercepted.

How can one check that the recipient's email provider supports encryption?

The following works on linux. First find the list of servers the email will be
delivered to using the domain name of the email:

~~~ bash
dig crowninn.nl +short mx | cut -d' ' -f2

> mx.crowninn.nl.
> relay.crowninn.nl.
> mail.crowninn.nl.
~~~

And now for each server above  we need to make sure the server supports TLS:

~~~ bash
telnet mail.crowninn.nl 25
ehlo crowninn.nl

> 250-crowninn.nl Hello [178.79.182.236]
> 250-TURN
> 250-SIZE
> 250-ETRN
> 250-PIPELINING
> ...
> 250 OK
~~~

There is no "250-STARTTLS" output in the above hence sending emails to
@crowninn.nl is insecure!

Every server from the previous output should support TLS because when an email
is sent to the domain any server could be chosen.

Thanks for reading this far have a vaguely relevant xkcd comic to entertain you:

![What happens when someone really needs your data](http://imgs.xkcd.com/comics/security.png)

[^1]: or secure smtp
[^2]: or secure pop/imap
