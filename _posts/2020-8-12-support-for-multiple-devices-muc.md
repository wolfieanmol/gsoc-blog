---
layout: post
title: Support for Multiple Devices and MUCs
---

Since the last update, I have been able to implement cursor support for real-time text, support for multiple devices, and started with MUC implementation.

# Cursor Support

Since with the support of wait action elements, the received real-time texts are processed and displayed more realistically; keeping the typing patterns of the peer, the next step was to implement cursor support to display where the changes are taking place.

Implementing this was pretty straightforward, just insert '|' character in the latest position in the label and remove the old one if present.

# Support for Multiple Devices

With multiple device support, if a user is using multiple instances of Dino in separate devices then whatever is that they are typing in chat input will be synced with the other clients. This means that a user can start composing their text in one device and finish it in other.

This is done by checking if the bare JID of user account and the bare JID of message carbon stanza is the same for single user chat or by checking the resourcepart in case of MUCs. If the condition passes then just update the text input buffer with the rtt instead of creating an rtt element on the conversation view.

I faced two problems while implementing this:

First: I was using different conversations (conversation with peer JID and conversation with the same account JID) to store and retrieve action elements from Hash-Map.

Second: The chat input kept on updating in a loop for both the clients. The reason was that RTT generation is tied with updation of text input. So syncing text input resulted in the transmission of RTT which updated text input in other clients and so on the loop continued. This was resolved by using a bool variable, which does not let RTT generation in this scenario.

![_config.yml]({{ site.baseurl }}/images/multiple-device-support.gif)

# MUC support

For MUC only 3 Real-Time Text widgets would be displayed at a time. This is done to ensure that in case of highly active MUCs with multiple people typing at a time it does not cause any problems with user experience and to keep the UI clean and free of clutter.

If there are already more than three RTT widgets present then the priority of the new incoming JID and the previous ones present is checked based on MUC affiliation with the owner having a greater priority than admins and so on.

Other than that I resolved for removing typing notifications for those whose RTT is being displayed.


