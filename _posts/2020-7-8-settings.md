---
layout: post
title: Settings to Enable/Disable RTT
---

As you may already know from my last week's blog post, Real-Time Texting is working great for single user chats. Even though working great, some users may not want to enable RTT and stick with traditional texting or they may just want to receive but not send; so this week I worked on the option to give them this choice. We concluded that there should be a three-state option for RTT, namely: 

    - `Send and Receive`
    - `Receive only`
    - `Off`

With the implementation of this option, sending and receiving `init` and `cancel` events are also now accounted for.

Right now the RTT setting is available in the settings dialogue of each conversation, but it will be shifted by text input field as a radio button menu in the future. The button would be similar to the encryption button.
