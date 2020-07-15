---
layout: post
title: Settings Radio Button Menu
---

Till last week the option to change real-time text settings was available in the local settings dialogue. Since then I have been able to shift that option from local settings to beside the text input field as a radio button menu. This makes the settings option easily accessible.

Merely updating the setting on a single side would not be a good idea, for example, if a sender switches from `send and receive` option to `off` then it would still result in the peer sending RTT message stanza nodes in vain and using bandwidth for no reason. To counter issues like these the XEP- 0301 defines two events, `cancel` and `init`. In the above example, the sender sends `cancel` event when the setting is switched to `off` which directs the peer to also not send RTT message stanza nodes anymore. Similarly ‘init’ informs the peer that the sender has enabled RTT on their side.

There’s also a particular case where If the user has setting, `send and receive` and the peer switches to `off` then the setting is automatically switched to `receive only`, this results in the settings menu to pop up and display a label informing why the setting was changed by itself.

![_config.yml]({{ site.baseurl }}/images/settings_menu.png)

I also accounted for the `reset` event, with the whole text typed, until that moment to be sent and received every fixed interval. This helps in resolving out of sync issues that may arise while in the transmission of real-time text. This week also involved fixing of few small issues.
