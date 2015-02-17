---
layout: post
title: "Bundle for testing"
---

For the past couple of hours I have been running unit tests against some dummy XML data so I can write the parser in a TDD approach (yes because I'm cool). However, I found a lovely issue with bundles.

For those who don't know what bundles are, they are encapsulated folders which holds all the resources that you need without the need to know the relative or absolute path. Therefore you let the `NSBundle` class handle everything. *perfect*

However, with most of my iOS development prior, I only ever needed to use the main bundle, which can be involked as standard:

```
let url : NSURL? = NSBundle.mainBundle().urlForResource("foo", withExtension: "xml")
// will return either the path for the file so other methods can utilise it
// or a nil value if not found
```

This removes so much of the stress of handling paths yourself.

The issues start when you want to add a resource to the bundle however. You have to go through two steps before it will be there:

1. Allow the file to be accessible to the target
2. Add the file within the build phasing to Copy the Resources to the bundle.

So, this is all fine, and if you are a seasoned iOS developer, this should be standard. Buuuuut, what they don't tell you is that unit testing doesn't use the main bundle. Nope.

The following snippet will resolve the issue

```
let url : NSURL = NSBundle(forClass: self.classForCoder).URLForResource("foo", withExtension: "xml")
```


