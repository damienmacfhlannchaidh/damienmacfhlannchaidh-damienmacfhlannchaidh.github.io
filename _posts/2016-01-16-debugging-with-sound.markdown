---
layout: post
title: 'Programming Note: Debugging Using Sound'
categories: Programming
excerpt: 'I recently found myself debugging an app that called a method frequently but without any defined pattern. I wanted to know each time it was called, but I grew tired of using `NSLog` as it was totally polluting my output console with garbage.'
---

**Headphones may be required for this technique**

I recently found myself debugging an app that called a method frequently but without any defined pattern. I wanted to know each time it was called, but I grew tired of using `NSLog` as it was totally polluting my output console with garbage.

So I set about seeing what XCode could do to help. Here is the line of code that I wanted to monitor calls to:

![breakpoint1](/images/blogs/breakpoint1.png "XCode Breakpoint 1")
*(Running gag: yes it was a Core Data method!)*

So after I set my breakpoint, I «Alt-Cmd-Mouse Click» on the breakpoint to bring up the breakpoint configuration dialog, like so:

![breakpoint2](/images/blogs/breakpoint2.png "XCode Breakpoint 2")

I then choose a “Sound” action and used “Ping”. Select the “Automatically continue after evaluating” checkbox. This is important to not have the debugger pause your app every time the breakpoint is triggered.

![breakpoint3](/images/blogs/breakpoint3.png "XCode Breakpoint 3")

Now, I get a quiet ping sound every time the method was hit. In this case, every time a save operation was triggered on a `NSManagedContext`. Everyone around you can tell you’re debugging!

Debugging is all about feedback, and now I can use my ears as well as my eyes. Try it out, it might very well work for you too!

[This post on medium](https://medium.com/@dglancy/debugging-using-sound-31bec4daaec7)