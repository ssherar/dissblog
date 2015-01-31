---
layout: post
title: "Continious Integration Woes"
---

For my dissertation, I want to do things right - especially as my major project will (hopefully) be used by health care professionals in Wales for the start. This means that I want to use Continious Integration (CI) for making sure any code I push doesn't break the build (a fall back  if you will).

I have always been a fan of [Travis CI](http://travis-ci.org) for my CI needs in the past, and with the Github education account, I thought it would be great to use the Private version of Travis CI to continiously build and test my code, which I am wary to open source - due to IPR not residing with me. So off I went, and ended up scraping it. Heres why (for all those googlers out there):

## Travis doesn't support Swift - yet

Well it *does*, but not formally. often when you build a new travis instance with `travis init`, you often get this as an error

```
 >>  travis init
 Main programming language used: |Swift|
 unknown language swift
```

Thanks, guys. It's not like swift has been out for nearly 7 months! To get around that, use `objective-c` as the language, so the code is built on a Mac VM with the latest XCode (which **does** have swift support)

## Travis Private are run in the most redicious way

Listen to this: Free travis accounts can build on the Mac VMs, as can the $129/month subscribers (*obviously*). Yet the Paid accounts which has been paid for through the Education pack from github **cannot**. Yes, you read that right. As a pseudo-paying member, you cannot use the Mac VMs, but open source can. Go figure.

After an abrupt discussion with the support member - apparently they are changing it, but wouldn't comment on the time frame or the reason why the restriction was in place. So I'm putting a guess in at 2016 and never.

## Bitrise to the rescue!

After some angry typing into google to find other CI services I could leverage, I managed to stumble upon [Bitrise](http://bitrise.io), which is a dedicated Mac CI service, with some really cool features such as workflows and archiving the code. Definitely puts Travis to shame... 
