---
layout: post
title: "-[UIViewController viewDidUnload] not Called when Received a Memory Warning on iOS 5"
date: 2013-08-13 18:19
comments: true
categories: [iOS, memory warning, UIViewController]
---

<p>
Wat? iOS 5? Are you serious?
</p>
<p>
Yea, I know that your shining iPhone 5 is running the latest iOS 7 beta nowdays. But still there are customers haveing some app we wrote previously running on their legacy devices. Especially considering that the booming market of jailbroken devices in China, dropping support for legacy OS is not a easy decision anyway. 
</p>
<p>
Long story short. Recently we found some UIViewController subclasses in our app don't unload their views when they received a memory warning. After diagnosis, it turns out that only some of view controllers instantiated without a XIB file behave abnormally.
</p>
<!-- more -->
<p>
UIViewController's view loading mechanism is well documented in the <a href="http://developer.apple.com/library/ios/featuredarticles/ViewControllerPGforiPhoneOS/ViewLoadingandUnloading/ViewLoadingandUnloading.html#//apple_ref/doc/uid/TP40007457-CH10-SW2">View Controller Programming Guide for iOS</a>. It instantiates its view in loadView method either by loading it from a associated XIB/storyboard file or creating it programmatically. Actually there are three different cases:
</p>
<ul>
<li>view controller associated with a XIB/storyboard file
</li>
<li>view controller without a XIB/storyboard file but overriding loadView method to create a view programmatically
</li>
<li>view controller without a XIB/storyboard file and not overriding loadView method
</li>
</ul>


<p>
In the third case, UIViewController will create an empty UIView object and assigned it to its view property. But for some unknown reason, it doesn't unload it when received a memory warning. Therefore your viewDidUnload method will certainly not get called. This is a weird and undocumented behavior.
</p>
<p>
After overriding loadView method to create and set view property, all our view controllers behave correctly now.
</p>