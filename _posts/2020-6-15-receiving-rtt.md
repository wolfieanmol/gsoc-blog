---
layout: post
title: Receiving Real Time Texts
---

Since the last blog, I have been able to integrate receiving real-time text in Dino. For that, I have implemented a receiving queue that first stores all action elements received from incoming rtt message stanzas in order. A queue is better than directly processing received action elements as the latter approach is prone to syncing issues. I've also implemented a basic method to counter out of sync issues by ignoring further rtt messages from a sender if the sequence is disturbed and will be resumed only when the event received is new. (More work needs to be be done to manage out of sync issues in further weeks).

The action element stanza nodes in the queue are then processed one by one and the changes are then processed using string-builder. The real-time texts received look promising. Since there is no user interface right now, I'm using debug to see incremental changes in the message being received.

For sending RTT I managed to do the scheduling along with the user's typing action which is better than last week's always on scheduling.

So what's left now that I hope to accomplish in the next two weeks before the first evaluation? For one I will be implementing a user interface for single user chat; some code changes as suggested by Marvin (my mentor), handling issues and bugs, and error handling. With that, I hope to get basic real-time text working by then.

