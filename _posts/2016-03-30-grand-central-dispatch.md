---
title: "Dispatch Objects in GCD"
excerpt: "An overview of Dispatch object's life cycle"
tags: 
  - gcd
  - grand central dispatch
modified: 2016-03-30
comments: false
categories:
  - Post
---
{% include toc %}

Grand Central Dispatch (GCD) is a technology by Apple to manage multiple cores of hardware at system level. As GCD is built at lower level and closer to unix system, it can manage muliple cores and developer do not have to worry about it. GCD provides a set of APIs which a developer can use to submit code blocks (dispatch objects) to it, after that GCD can do its magic.

# Dispatch Object lifecycle
When compiled with Objective-C compiler dispatch objects behave like normal Objective-C objects. They are reatained and then released at the end of their lifecycle automatically in ARC. 

# Further Readings
- [Grand Central Dispatch (GCD) Reference](https://developer.apple.com/library/ios/documentation/Performance/Reference/GCD_libdispatch_Ref/)