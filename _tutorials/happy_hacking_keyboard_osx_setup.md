---
tags: [Happy Hacking, Happy Hacking Keyboard, HHKB, Mac, Setup, Shortcuts]
date: 2014-02-14 00:00:00 -0800
category: "Programming Keyboards"
title: Happy Hacking Keyboard, OSX Configuration
---

A few months back I set out to buy a new programming keyboard. I ended up going with the [Happy Hacking Keyboard Pro 2](https://en.wikipedia.org/wiki/Happy_Hacking_Keyboard) (HHKB) by PFU Limited out of Japan. __This keyboard isn't going to be ideal for everyone, but those who like it, (including myself), really like it__. It is a [60% keyboard](http://deskthority.net/wiki/60%25). A 60% keyboard is reduced in size by omitting the ten key pad as well as the direction keys. Access to some of those keys is restored by the use of a function key. The HHKB uses [Topre](http://deskthority.net/wiki/Topre_switch) mechanical switches. Again, loved by some and not by others. I have grown to really like the sound the keys make. I have had mine for several months now. My goal when setting up my computer with the HHKB is to minimize the inconsistencies between it and my laptop. Mainly because muscle memory is a good thing and I don't want to be that guy that needs to have "my special" keyboard.

## Function Layer
Before we go any further we need to go over the function layer on a keyboard. __One way keyboard makers get double or triple duty out of the available keys is with a function button__. When held down it gives the keys a new action. This is often referred to as the function layer. All 60% keyboards make heavy use of a function layer. In the HHKB holding down `function` turns the number keys into function buttons, the `O , K L` become up, down, left, and right. Also, on a Mac `A S D` become volume up, volume down, and mute. Along with the function layer the HHKB has a few additional differences in the key layout.

## Adjusting to the Layout
The differences are actually fairly slight and provide (in my mind) a noticeable performance gain. The dedicated caps-lock button is gone. In it's place is the _control_ button. Without the the row of function keys the _escape_ key is without a home and is moved one row down to where the `` ~ `` was. This is cool, but that moves the tilde to the upper right of the keyboard with the forward slash next to it, one in from the right on the top row. That leaves the `Backspace/Delete` button, which is moved down one row, and man, it sure does feel nice. Not traveling as far to reach `Backspace` is a good thing. That's it. Four buttons moved, not a huge adjustment. In fact, I switch between the Happy Hacking keyboard and my laptop with no issues. For me, adjusting to the modified key layout was not a big deal. On the other hand, mapping shortcuts that worked across both the HHKB and the standard laptop layout with a minimal amount of friction took some time.

## Aligning Keybindings (Shortcuts)
I found that a few of my go-to shortcuts required some rethinking with the new layout in mind. Modifying some of the laptop's keybindings to match the available keys on the Happy Hacking Keyboard I was able to create some consistency. The HHKB _command key_ lives where the caps-lock is on the laptop. The solution is to __remap the caps-lock on the laptop to behave as a second control key.__ Go to `Preferences > Keyboard`. Under the `Keyboard` tab, in the lower right, click `Modifier Keys`. Perhaps I'm biased on this one. I have had my caps-lock mapped to control on my laptop for years. As a programer I think this is the single best thing you can to do. __Even if you don't go the HHKB route I highly recommend this.__ Its the way Unix keyboards were originally designed. Do this for a day and you'll wonder why caps-lock ever got promoted to such valuable keyboard real-estate in the first place.

One shortcut I use a lot  is the ``command + ` `` for switching between windows in an application. It took me a while to work out a replacement to this one. Built in to the Mac os is an option to remap the ``command + ` `` to `command + escape`. This  lets me ave the same binding for both keyboards. To do this go to `Preferences > Keyboard > Shortcuts > Keyboard` and then re-assigning "Move focus to next window" to `Command + Escape`. Problem solved. :-)

## Dip-switches

The dip-switches are where you set the hardware settings. Here are my Mac specific.

|  1  |  2  |  3  |  4  |  5  |  6  |
| --- | --- | --- | --- | --- | --- |
| off | on  | on  | off | off | on  |

- Switch __one and two__ set the keyboard to mac mode. This give you media controls (volume up and down, play, and pause on the function layer.
- Switch __three__ set the delete button to be backspace. I defiantly want this.
- Switch __six__ is *wake mode*. Set this so keyboard will wake up the computer.

## Wrapping up

This is where I am with my HHKB pro 2 at the moment. Its definitely a work in progress. I'd love to know what you think. If you have any other pro tips for getting the most out of the Happy Hacking Keyboard share em in the comments down below.
