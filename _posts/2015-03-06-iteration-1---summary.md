---
layout: post
title: "Iteration 1 - summary"
---

It's been a while since I've updated this blog (nearly 2 weeks!), so let's get down to what's happened... :) 

## Swift 1.2

My last blog post was around XCode 6.3 and Swift 1.2. Since then, Apple has released a new beta version of XCode 6.3b2. This was a mostly welcomed release, bar a couple of legacy issues I have with Apple in general.

Take for example this code in Swift 1.2 beta 1 and below:

```swift
let splitString : [String] = split(formedString, { $0 == "." })
```

This is common syntax for many developers, especally from the old Objective-C way of `^{}` for creating anonymous function definitons. However, they removed support for that in favour of 

```swift
let splitString : [String] = split(formedString) { $0 == "." }
```

While I agree that the second code example is far more readable, Apple should atleast keep that syntax in legacy to not break applications out of the box!

## SQLite databases & Frameworkless Frameworks

With the move to Swift 1.2 and the difficulty of searching for drugs in a tree data structure (even if it is possible), I started using the [SQLite.swift](https://github.com/stephencelis/SQLite.swift), a **wonderfull** typesafe wrapper around the libsqlite3.dylib! However, this was making me tear out my hair for a while.

The solution around this was to import the code in myself, and using a Bridging header, import the header files. This also means that it is iOS 7 ready as 7 doesn't support linked frameworks unfortunately (again legacy apple!!!).

## Colour, Colour, Colour


This has been a very successful portion of my time, including the prospect of an open source small library that can be imported! This stems from the fact that `UIColor` is still in the 80s - unfortunately. 

My first issue was the fact that some of the data coming in was hexadecimal values (i.e. #336699). At this time, `UIColor` doesn't support hexadecimals nicely, and there is no inbuilt way to resolve this. This forced me to write a couple of functions to help me with this:

```swift
/**
Returns a UIColor object from a Hexadecimal string with a solid colour

i.e. 

UIColorFromRGB("#FF0000") == UIColor.redColor()
UIColorFromRGB("#0000FF") == UIColor.blueColor()
UIColorFromRGB("#GGGGGG") == UIColor.blackColor()
UIColorFromRGB("#Hello") == UIColor.blackColor()

Adapted from http://stackoverflow.com/a/12397366

:param: color The hexadecimal string representation of the string including the #
:returns: a UIColor, UIColor.blackColor() if the casting fails
*/
func UIColorFromRGB(color: String) -> UIColor {
    return UIColorFromRGB(color, 1.0)
}
 
/**
Returns a UIColor object from a Hexadecimal string with an adjustable alpha channel

i.e.
UIColorFromRGB("#FF0000", 1.0) == UIColor.redColor()
UIColorFromRGB("#0000FF", 1.0) == UIColor.blueColor()
UIColorFromRGB("#GGGGGG", 1.0) == UIColor.blackColor()
UIColorFromRGB("#Hello", 1.0) == UIColor.blackColor()

Adapted from http://stackoverflow.com/a/12397366

:param: color The hexadecimal string representation of the string including the #
:param: alpha The transparency, represented as a Double between 0 and 1
:returns: a UIColor, UIColor.blackColor() if the casting fails
*/
func UIColorFromRGB(color: String, alpha: Double) -> UIColor {
    assert(alpha <= 1.0, "The alpha channel cannot be above 1")
    assert(alpha >= 0, "The alpha channel cannot be below 0")
    var rgbValue : UInt32 = 0
    let scanner = NSScanner(string: color)
    scanner.scanLocation = 1
    
    if scanner.scanHexInt(&rgbValue) {
        let red = CGFloat((rgbValue & 0xFF0000) >> 16) / 255.0
        let green = CGFloat((rgbValue & 0xFF00) >> 8) / 255.0
        let blue = CGFloat(rgbValue & 0xFF) / 255.0
        
        return UIColor(red: red, green: green, blue: blue, alpha: CGFloat(alpha))
    }
    
    return UIColor.blackColor()
}
```

(available from [https://gist.github.com/ssherar/deb2536df5f51acc3910](https://gist.github.com/ssherar/deb2536df5f51acc3910))

This means that you can now pass this function a hexadecimal colour and it will return a UIColor as standard! Fantastic-o!

Another issue was the fact that some of the colours were being sent down as human readable words (i.e. red, cyan, magenta, blue), as per the (HTML spec)[http://www.w3.org/TR/html401/types.html#h-6.5], so a conversion enum was created to handle this as lazily as possible (as in, allow UIColor to handle all the standard colours it has implemented, and use `UIColorFromRGB` to handle the outside cases).

Last issue came from animation. The problem was if `UINavigationBar.barTintColor` was set to `nil`, it wouldn't be able to fade in as expected, bar expand from the left hand corner. A quick fix meant that setting the tint colour to white before triggering the animation. The snippets can be found [here](https://gist.github.com/ssherar/d43a8bd97742a3ff0787)

