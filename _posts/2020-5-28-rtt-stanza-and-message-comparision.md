---
layout: post
title: RTT stanza and message comparision
---

It's been a week and a half since I started working On Real-Time Text. Firstly I have established a framework to generate RTT message stanza and send them over the network. This includes generating relevant action elements as per requirement.

My initial goal is to get a basic real-time text model for single chat users. For that, I have planned a series of steps which are as follows:

![_config.yml]({{ site.baseurl }}/images/rtt_send.jpg){:height="50%" width="50%"} ![_config.yml]({{ site.baseurl }}/images/rtt_receive.jpg){:height="50%" width="50%"}

One of the important steps to achieve that is message comparison.
This step will produce a difference between the previous text stored in the buffer and the new one being typed. This text difference will be translated in the form of action elements that will be sent in the <rtt/> element. 

A correct algorithm is required to produce the right action elements. An inefficient message comparison will lead to either generation of many action elements that could lead to bandwidth issues or action elements that will make real-time texts to appear unnatural. 

I found a JavaScript implementation of the XEP-0301. The message comparison method there is not efficient for our purposes. For instance, if someone makes an edit in between their text then it would first erase everything from that point and replace it with new text. This will seem unnatural to the end-user who is receiving RTT.

As planned before I was thinking in terms of either a linear scan to compare changes or some minimum edit sequence algorithm.

Fortunately, in my search, I found that Python has a standard library called 
difflib which offers exactly what I want for my purpose. The algorithm behind is basically a version of Longest Common Subsequence (LCS) and produces a good result that is more "human friendly", unlike traditional LCS.
Here's a small script I ran in python inspired from [this StackOverflow answer](https://stackoverflow.com/questions/774316/python-difflib-highlighting-differences-inline/788780#788780):

![_config.yml]({{ site.baseurl }}/images/diffpy.png)

And the result:

![_config.yml]({{ site.baseurl }}/images/diffres.png)

Pretty neat, right!

So right now I’m looking over at implementing something like this for dino In Vala. Instead of color-coded text to show the difference the Vala code will call methods that will generate insert and erase action elements.

