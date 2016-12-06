---
title: "Downgrade from XCode 8 to XCode 7"
excerpt: "Downgrade from XCode 8 to XCode 7"
tags: 
- xcode8
- xcode7
- downgrade
modified: 2016-12-7
comments: true
categories:
- Post
---
{% include toc %}
In one of my recent project I developed a module in XCode 8 as I had upgraded to it. Later I came to know that our team is currently working on XCode 7. So if we integrate XCode 8 UI files i.e .storyboard & .xib, the main project will not compile and will show following error.

<figcaption>UI Files of XCode 8 opened in XCode 7</figcaption>
<a href="/images/XCode8ToXCode7Downgrade/ErrorOnXCode7ForXCode8Project.png"><img src="/images/XCode8ToXCode7Downgrade/ErrorOnXCode7ForXCode8Project.png"></a>
<figcaption>UI Files of XCode 8 opened in XCode 7 (2)</figcaption>
<a href="/images/XCode8ToXCode7Downgrade/ErrorOnXCode7ForXCode8Project2.png"><img src="/images/XCode8ToXCode7Downgrade/ErrorOnXCode7ForXCode8Project2.png"></a>

```
The document "Main.storyboard" requires XCode 8.0 or later.

This version does not support documents saved in XCode 8 format. Open this document with XCode 8.0 or later.
```

```
The document "(null)" requires XCode 8.0 or later.
```

Although it is strongly recommended to upgrade your XCode as soon as some stable version arrives, there can come certain situations where you have to downgrade it. If you are a developer like me who is trying to downgrade his project to support XCode 7 here are two simple ways.

# Using XCode 8:
If you have XCode 8 installed on your system, you can use this method to downgrade your storyboard files.
- Open .storyboard/.xib file in XCode 8.0
- On right side: Utility Area > File Inspecter > Interface Builder Document
- Choose 'XCode 7.x' for 'Opens in's' value. The process is shown in GIF below:

<figcaption>Downgrade to XCode 7 using XCode 8</figcaption>
<a href="/images/XCode8ToXCode7Downgrade/DowngradeToXCode7.gif"><img src="/images/XCode8ToXCode7Downgrade/DowngradeToXCode7.gif"></a>

# Using any text Editor
If you do not have access to XCode 8 at the moment you can use any of the text editors available to downgrade to XCode 7. So that you have no more build errors. 

Just open your .storyboard/.xib file in a text editor of your choice and remove following line:

```
<capability name="Document saved in the Xcode 8 format" minToolVersion="8.0"/>
```

<figcaption>Downgrade to XCode 7 using text editor</figcaption>
<a href="/images/XCode8ToXCode7Downgrade/ManuallyDowngradeToXCode7.png"><img src="/images/XCode8ToXCode7Downgrade/ManuallyDowngradeToXCode7.png"></a>

After removing this line you will be able to compile your project successfully.

To understand why we removed above line you can explore the changes once a UI file is saved in Xcode 8 format. If you will open .storyboard or .xib file on XCode 8 first time. It will show you a dialog as shown below to make these files compatible with XCode 8 document format.

<figcaption>Dialog to make storyboard file XCode8 compatible</figcaption> <a href="/images/XCode8ToXCode7Downgrade/StoryboardUpgradedToXCode8.png"><img src="/images/XCode8ToXCode7Downgrade/StoryboardUpgradedToXCode8.png"></a> 

<figcaption>Git Diff of storyboard file after upgrade to XCode 8</figcaption> <a href="/images/XCode8ToXCode7Downgrade/GitDiffAfterUpgradeToXCode8.png"><img src="/images/XCode8ToXCode7Downgrade/GitDiffAfterUpgradeToXCode8.png"></a>
