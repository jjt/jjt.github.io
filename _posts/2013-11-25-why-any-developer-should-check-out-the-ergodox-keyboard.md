---
published: false
title: "The ErgoDox keyboard a year in, and why you should get one"
layout: post
---

![ErgoDox](https://d3jqoivu6qpygv.cloudfront.net/img_bucket/ergodox/_W3T2166.jpg)

The [ErgoDox][] is a split-hand ergonomic keyboard with mechanical switches and open source, layer-based firmware running on a [Teensy][] microcontroller. While other keyboards offer [dip-switches][codekeyboard] or GUI config tools, the firmware and layouts can be [built from source][0] on the command line or through a [layout configuration tool][]. Flashing a new build onto the ErgoDox is easy with the multi-platform [Teensy loader][]. And the hardware side is equally hackable, as [these][3] [examples][4] [show][5].

##What this all adds up to is a *highly configurable keyboard* that can be tailored to fit your *exact* needs.

Because of this flexibility, I was able to [program my own Colemak layout][mylayout] that adds a layer for common programming symbols on and near the home row, while enjoying a more ergonomic experience by allowing me to separate, rotate, and pitch the keyboard halves as needed. My layout also has a QWERTY layer for guests and for gaming, as I'm *farrrrr* too ~~lazy~~ efficient to remap keys in every game.

If you're not familiar with the concept of keyboard layers, don't fret; you switch between different keyboard layers likely hundreds of times an hour. A layer is simply a map from physical keys to characters. Switching between layers is often triggered by holding down a meta key, like shift. The shift key switches the keyboard from its "default" layer to the "uppercase" layer, which causes certain keys to output different characters (ex. "a" becomes "A", "3" becomes "#").

The ErgoDox firmware abstracts this idea to an arbitrary number of user-defined layers that can be triggered and toggled by any key. This is great for us developers, as we can fine tune our layouts to let our fingers be ~~lazy~~ efficient.

## On ~~laziness~~ efficiency

Take a look at the heat maps generated if we were to type out [Underscore 1.5.2](/assets/bower_components/underscore/underscore.js) with QWERTY, Colemak, Programmer Dvorak, ErgoDox QWERTY, ErgoDox Colemak and my custom ErgoDox Colemak layout:

###QWERTY
[![QWERTY](/assets/media/heat-qwerty.png)](/assets/media/heat-qwerty.png)

###Colemak
[![Colemak](/assets/media/heat-colemak.png)](/assets/media/heat-colemak.png)

###Programmer Dvorak
[![Programmer Dvorak](/assets/media/heat-programmer-dvorak.png)](/assets/media/heat-programmer-dvorak.png)

###ErgoDox QWERTY
[![ErgoDox QWERTY](/assets/media/heat-ergo-qwerty.png)](/assets/media/heat-ergo-qwerty.png)

###ErgoDox Colemak
[![ErgoDox Colemak](/assets/media/heat-ergo-colemak.png)](/assets/media/heat-ergo-colemak.png)

###ErgoDox Colemak JJT
[![ErgoDox Colemak JJT](/assets/media/heat-ergo-jjt.png)](/assets/media/heat-ergo-jjt.png)







## Love me, love my ErgoDox

This is my first mechanical keyboard after many happy years on [Thinkpad travel keyboards][2] and I've been enjoying the satisfying feel of the switches' tactile bump at the actuation point. I went with Cherry MX browns, which are very quiet at the point of actuation. They will click from the keys bottoming out and from releasing the key, however, but I think most non-chiclet keyboards will do so.

I managed to snag one of the last prototype boards and assembled the parts myself. Since it would have been expensive to produce a one-off case due in part to its [intricate plate layer][10], I opted for board-mount switches instead and just sat the PCBs right on fun foam. It worked very well - the foam distributed the weight along the PCB while letting the solder tails sink in.

![jjt-ergo-prototype.jpg](/assets/media/jjt-ergo-prototype.jpg)


[ErgoDox]: http://ergodox.org/
[Teensy]: http://www.pjrc.com/teensy/
[geekhack]: http://geekhack.org/
[Deskthority]: http://deskthority.net/
[layout configuration tool]: https://www.massdrop.com/ext/ergodox
[Teensy loader]: http://www.pjrc.com/teensy/loader.html
[codekeyboard]: http://codekeyboards.com/
[mylayout]: https://github.com/jjt/ergodox-firmware/blob/master/src/keyboard/ergodox/layout/colemak-symbol-mod.c
[Massdrop]: https://massdrop.com

[0]: https://github.com/benblazak/ergodox-firmware
[2]: http://www.ideacouture.com/blog/wp-content/uploads/2009/09/thinkpad-keyboard-beauty-1024x402.jpg
[3]: http://geekhack.org/index.php?topic=43709.0
[4]: http://farm4.staticflickr.com/3833/8943930400_c2e1f0b47e_z.jpg
[5]: http://geekhack.org/index.php?topic=46860.msg996709#msg996709
[6]: http://www.kinesis-ergo.com/images/cont-above-hands-blk630x390.jpg

[10]: http://i.imgur.com/cw4nX0w.png