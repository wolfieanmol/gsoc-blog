---
layout: post
title: Realistic RTT with wait
---

Since the last update, I have been able to make stale RTT disappear from UI and make Real-Time Text more realistic by matching the typing speed/pattern by accounting for the `wait` action element. Receiving wait and pausing RTT, in particular, took a lot of debugging and some refactoring of code.

# Removing Stale Messages

My initial approach in handling this was to simply create a `Timeout` when the RTT UI element is created. The Timeout would check if the current text on UI is the same as in RTT Builder every 5 seconds and delete it from UI if it was the case. But I quickly learned that this wouldn't work everytime and would end up deleting the UI element sometimes even when it wasn't stale.

So instead I created a timer that starts with the initialization of the UI element which is reset back to 0 whenever the RTT updated. Now instead of the Timeout checking for RTT builder, it checked if the time elapsed is more than 5 seconds and erases it from UI. This approach works great and stale messages are removed when the peer stops typing or there is a sync issue.

# Wait Action Element  

The wait action element records the time between key presses and transmits that interval to the receiver. This results in an accurate rendering of the Real-Time Text with time delays in between. This helps the receiver see the typing mood of their peer. This also made it possible to transmit RTT every 700 ms instead of 300 ms used before without resulting in bursty rendering on the receiver side.

Sending 'wait' was pretty straight forward:

Create a Timer -> Send elapsed time between two key presses -> Reset timer -> Repeat

But receiving and pausing was very much mind-boggling. I started (knowing that it would fail) with `Thread.usleep` to pause the RTT generation, as expected it paused the whole UI.

So I went with making `schedule_receiving` method to async and yield nap when there was a need to pause. Well, it still did not work at all. So after hours of debugging, I realized that the reason could be that the `rtt_received` signal is invoked with every RTT message stanza which results in the creation of a new `Idle` loop every time. I tried to not start a new `Idle` if there was already one running using the returned uint. Again, didn't work, instead, it resulted in the wrong rendering of RTT. 

"Hmmm, maybe only invoke the signal when RTT with `new` event is received and continue Idle till the queue is emptied? Surely that would work". Nope, now it resulted in just displaying the latest typed character. "HUH WHAT!!".

Finally, I asked Marvin and came to know that Idle doesn't support async methods. `async_method.begin()` finishes instantly even though the code is finished sometimes later, this results in Idle to keep on running if the bool returned is true. He suggested to return false every time in idle and create a new one after `schedule_receiving` is finished if needed be. 

Even after that, I needed to figure out how to not start different Idle at the same time as using the returned uint was not working correctly when used in a method. Finally, I decided that Idle should only start when initially the queue is empty. If there is already an active element present in the queue then there's no need to start another Idle, as the previous one runs till the queue is emptied.

With that wait is working well and the received RTT looks more realistic and smoother. Now for this week I think of accounting for cursor support on RTT and encrypting RTT.

