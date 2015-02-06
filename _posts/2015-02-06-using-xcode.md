---
layout: post
title: "Using XCode"
---

XCode is a relatively nice piece of kit for iOS development. It works much like the Microsoft Visio Studio set for building on a specific platform - and like Microsoft, you can see that they've put a lot of effort into the UI, making things very *visual* when developing iOS applications. It definitely takes the hassle out making really **nice** looking applicaitons! But all that time spent on the UI has got it's draw backs.

The most annoying of which is the Source Code Management (SCM). XCode employs git for everything it does in version control (which I stupidly happy about). However, I often create and manage repos on the commandline, dodging the incredibly outdated UI for commiting data in XCode (one of my only UI gripes). XCode does link in with this git repo (after all, it should be standardised right?) and shows dirty files. But, the real issue happens when habit happens and I change branch in the terminal -
filtering branches, merging upstream changes etc - I go back to XCode and /et voilia/ it has crashed. **IT ABSOLUTELY HATES `git chechout <branch>`**. WHY XCODE WHY?

Another intermitant issue with XCode on Yosemite is Full screen mode - randomly when testing your code an emulator/device, it will just close. Not crash, not error out or freeze, it will **close**. Why would you do this to me apple?
