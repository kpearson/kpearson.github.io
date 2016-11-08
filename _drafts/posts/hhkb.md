---
tags: [Happy Hacking, Happy Hacking Keyboard, HHKB, Mac, Setup, Shortcuts]
category: "Programming Keyboards"
title: Happy Hacking Keyboard, OSX Configuration
---

A few months back I set out to buy a new keyboard. I wanted something well built and made for coding. I had no idea what I was getting my self into. The world of keyboards is a strange and wonderful corner of the internet. There are as many choices out there as there are opinions. The one constant I found is, all keyboards aimed at the discerning keyboardist have mechanical switches. Like the keyboards form the 80s and 90s that have a distinctive click as you type. As a posed to the thin keys on a laptop. After a lot of research I ended up going with the [Happy Hacking Keyboard Pro 2](https://en.wikipedia.org/wiki/Happy_Hacking_Keyboard) (HHKB) by PFU Limited out of Japan. __This keyboard isn't going to be ideal for everyone, but those who like it, (including myself), really like it__. The HHKB uses [Topre](http://deskthority.net/wiki/Topre_switch) mechanical switches. Again, loved by some and not by others. I have grown to really like the distinctive sound the keys make. After nearly a year with my new friend I have a few thoughts to share.

Before I go any further, a couple notes about me and how I use my keyboard. I use a Mac. My text editor of choice is Vim. I chose the HHKB with no marks on the keycaps. Also long before I embarked on this journey I mapped my `caps lock` mapped to control. More on this later.

## Function Layer
The HHKB falls into the a category called a [60% keyboard]. This refers to the keyboard layout containing ruffly 60% of the keys of a full keyboard. The reduced size is achieve by omitting the ten key pad as well as the direction and function keys. The missing keys are accessed with the use of a function key. Holding down the function key is like the holing down the shift key, giving the some keys new actions. This is often referred to as the function layer. As if when the function button is help down a new layer of keys is available. All 60% keyboards make heavy use of a function layer. In the HHKB holding down `function` turns the number keys into function keys, the `O , K L` become up, down, left, and right. Also, on a Mac `A S D F` become volume up, volume down, mute and eject.

## The HHKB Layout
Along with the function layer the HHKB has a few additional differences in the key layout. The differences are actually fairly slight and provide (in my mind) a noticeable performance gain. The dedicated `caps-lock` button is gone. In it's place is the `control` button. Without the the row of function keys the `escape` key is without a home and is moved one row down to where the `` ~ `` was. This is cool, but that moves the tilde to the upper right of the keyboard with the forward slash next to it, one in from the right on the top row. That leaves the `Backspace/Delete` button, which is moved down one row, and man, it sure does feel nice. Not traveling as far to reach `Backspace` is a good thing. That's it. Four buttons moved, not a huge adjustment. In fact, I switch between the Happy Hacking keyboard and my laptop with no issues. For me, adjusting to the modified key layout was not a big deal. On the other hand, mapping shortcuts that worked across both the HHKB and the standard laptop layout with a minimal amount of friction took some time.

## Aligning Shortcuts
My goal when setting up my computer with the HHKB is to minimize the inconsistencies between it and my laptop. Mainly because muscle memory is a good thing and I don't want hopelessly crippled on every other  keyboard on the planet.

I found that a few of my go-to shortcuts required some rethinking with the new layout in mind. By Modifying the laptop's key layout to match the available keys on the Happy Hacking Keyboard I was able to create some consistency. The HHKB _command key_ lives where the caps-lock is on most laptops. The solution is to __remap the caps-lock on the laptop to behave as a second `control` key.__ To do this on a Mac go to `Preferences > Keyboard`. Under the `Keyboard` tab, in the lower right, click `Modifier Keys`. Perhaps I'm biased on this one. As I mentioned above, I have had my caps-lock mapped to control on my laptop for years. As a programer I think this is the single best performance modification you can to do. __Even if you don't go the HHKB route I highly recommend this.__ Its the way Unix keyboards were originally designed. Do this for a day and you'll wonder why `caps-lock` ever got promoted to such valuable keyboard real-estate in the first place.

One shortcut I use a lot  is the ``command + ` `` for switching between windows in an application. It took me a while to work out a replacement to this one. Built in to the Mac os is an option to remap the ``command + ` `` to `command + escape`. This lets me have the same binding for both keyboards. To do this go to `Preferences > Keyboard > Shortcuts > Keyboard` and then re-assigning "Move focus to next window" to `Command + Escape`. Problem solved. :-)

## Dip-switches

The dip-switches is where you set the hardware settings. Here are my Mac specific.

|  1  |  2  |  3  |  4  |  5  |  6  |
| --- | --- | --- | --- | --- | --- |
| off | on  | on  | off | off | on  |

- Switch __one and two__ set the keyboard to mac mode. This give you media controls (volume up and down, play, and pause on the function layer.
- Switch __three__ set the delete button to be backspace. I defiantly want this.
- Switch __six__ is *wake mode*. Set this so keyboard will wake up the computer.

## Wrapping up

this is where i am with my hhkb pro 2 at the moment. its definitely a work in progress. i'd love to know what you think. if you have any other pro tips for getting the most out of the happy hacking keyboard share em in the comments down below.

[60% keyboard]: http://deskthority.net/wiki/60%25
