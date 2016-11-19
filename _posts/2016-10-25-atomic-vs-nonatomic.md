---
title: "atomic vs nonatomic properties in Objective-C"
excerpt: "In depth analysis of atomic and nonatomic properties."
tags: 
  - atomic
  - nonatomic
modified: 2016-10-25
comments: true
categories:
  - Post
---
{% include toc %}

The difference between atomic and non atomic properties remained a favourite question for interviewers. Today we will analyse these keywords in detail. These keyword define the characteristics of how these properties will behave once they will be accessed from multiple threads simultaneously.


# atomic (Default)
As the word explains itself, a single thread will be able to access the property at a given time. 
Explanation:
To explain more, only one thread will be able to access getter/setter of a property. Other threads have to wait until first thread releases get/set method. Let's say we have a property firstName which is atomic. 

~~~ shell
- Thread A => obj.firstName=@"A"
- Thread B => obj.firstName=@"B"
- Thread C => NSLog("First Name: %@",obj.firstName);
~~~ 

It will ensure that the value of property remains consistent throughout the lifecycle. By default all properties are atomic.

Example atomic property:

~~~ shell
@property (strong,atomic) NSString *firstName;
OR
@property (strong) NSString *firstName;
~~~ 


Sample getter setter:
This is how the methods of a property will look for an atomic property after 

~~~ shell
@synthesize firstName = _firstName;
-(NSString *)firstName{
    @synchronized (self) {
        return _firstName;
    }
}
-(void)setFirstName:(NSString *)firstName{
    @synchronized (self) {
        if(_firstName != firstName){
            [_firstName release];
            _firstName = [firstName retain];
        }
    }
}
~~~
Note: the Objective-C 2.0 specification, mentions that locks are used internally, but it doesn't specify exactly how. What you see above, is roughly what an atomic getter/setter would look like, but it might not be accurate.

*Pros:* Ensures that user gets a valid value and not some garbage

*Cons:* Slow, as it has code to ensure read write safety



# nonatomic
For non atomic properties multiple threads can access the properties simultaneously, so there is always a risk of inconsistency between values

Example:

~~~ shell
@property (strong,nonatomic) NSString *firstName;
~~~ 

Sample getter setter:
This is how the methods of a properties will look for an atomic property

~~~ shell
@synthesize firstName = _firstName;
-(NSString *)firstName{
    return _firstName;
}
-(void)setFirstName:(NSString *)firstName{
    if(_firstName != firstName){
        [_firstName release];
        _firstName = [firstName retain];
    }
}

~~~ 
*PROS:* Fast as compared to atomic properties, as there is no extra code to control access of multiple threads

*Cons:* You are not guaranteed whether or not you will receive a valid value

# TODO
- [] Thread safe vs Read/Write Safe
- [] When is code created for properties i.e how & getter and setter code is created?
- [] When to use atomic vs non-atomic some solid rules?
- [] Detailed Pros & Cons
- [] Multi-threading on Single CPU vs Multiple threads

# Further Reading
- [TMI #1: Objective-C Property Attributes](https://realm.io/news/tmi-objective-c-property-attributes/)
- [Ry’s Objective-C Tutorial - Properties](http://rypress.com/tutorials/objective-c/properties)
- [Apple's Docs](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/EncapsulatingData/EncapsulatingData.html)
- ['Gabriele Petronella's Answer for Explicit getters/setters for @properties (MRC)](http://stackoverflow.com/a/21802205/800848)
- [Memory and thread-safe custom property methods](http://www.cocoawithlove.com/2009/10/memory-and-thread-safe-custom-property.html)

