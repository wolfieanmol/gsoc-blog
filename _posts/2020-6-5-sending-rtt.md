---
layout: post
title: Sending RTT
---

In last week's blog, I introduced you to message comparison and an optimized algorithm to compare between two strings already being present in Python (which is an implementation of gestalt pattern matching,  an algorithm published in 1988 by Ratcliff and Obershelp).

This week I managed to implement the said algorithm in Vala to compare the current and previous versions of the message being typed and used it to generate the relevant action elements. The results produced are good with the correct action elements being generated.

I also implemented a queue to store these action elements and send all action elements present in the queue in scheduled intervals in a single RTT message stanza.

With that, I am mostly done with sending a basic version of real-time text and am able to send rtt message stanzas. There is still some work left for sending RTT before it becomes useable; like right now the event is always set to "new" and the rtt schedule method is always executed in a fixed time interval even when the user is not typing. This leads to Dino crashing eventually. These issues will be addressed this week.

This is the result produced by my build for sending rtt:

![_config.yml]({{ site.baseurl }}/images/rtt_message_send.png) 

For now, I'll be looking into receiving real-time texts.

That's it for this week.

