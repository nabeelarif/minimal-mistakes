---
title: "synthesize vs dynamic in Objective-C"
excerpt: "In depth analysis of synthesize and dynamic"
tags: 
  - synthesize
  - dynamic
  - Objective C
modified: 2016-10-25
comments: true
categories:
  - Post
---
{% include toc %}

At the core of Object Orientation you should not access data directly, instead you should use accessor methods to get or set the data. In Objective-C @property directive is a simplified way in which developers can tell the compiler about the behavior required for the accessor methods, which is simple and brief.

Now the question arise, where are the accessor methods? This leads us to the key words which are our topic today i.e @synthesize and @dynamic.

# @synthesize

~~~ shell
@synthesize propertyName = _propertyName;
~~~  
@synthesize tells the compiler to take care of the accessor methods creation i.e it will generate the methods based on property description. It will also generate an instance variable to be used which you can specify as above, as a convention it starts with _(underscore)+propertyName.

# @dynamic

~~~ shell
@dynamic propertyName;
~~~  
mentioning @dynamic with a propertyName tells the compiler not to create accessor methods because user will provide the implementation dynamically in future. Apple allows its developers to provide method implementation at runtime using [Dynamic Method Resolution].


# Further Reading
* [Dynamic Method Resolution]
* [@synthesize vs @dynamic, what are the differences?]


[Dynamic Method Resolution]: https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/ObjCRuntimeGuide/Articles/ocrtDynamicResolution.html
[@synthesize vs @dynamic, what are the differences?]: http://stackoverflow.com/questions/1160498/synthesize-vs-dynamic-what-are-the-differences
