---
layout: guide
tags: [Happy Hacking, Happy Hacking Keyboard, HHKB, Mac, Setup, Shortcuts]
title: Happy Hacking Keyboard OSX Shortcuts Configuration
---

A few months back I took a trip down the very slippery slope of buying a mechanical keyboard. The intent is full of odd rabbit holes and this is one of them. From keyboard layout to switch design to the volume of the click. I ended up landing on the [Happy Hacking keyboard 2(HHKB)](https://en.wikipedia.org/wiki/Happy_Hacking_Keyboard) by PFU Limited out of Japan. It whats called a 60% keyboard. What that means is instead of dedicated function or direction(arrow) keys you hold down a function button and like holding shift this gives you the missing buttons. It uses [Topre](http://deskthority.net/wiki/Topre_switch) switches. Some people love em others don't. I have grown to really like the sound the keys make a I type. I have had mine for six months or so now and I love it.

When a keyboard uses a function button to get double or triple duty out of the available keys its often referred to as the function layer. All 60% keyboards make heavy use of a function layer. With the HHKB there is a few extra differences in the layout to keep in mind.

The main adjustment is the slightly different layout. The caps-lock button is gone. In it's place is the _control_ button.  With no more row of function keys the _escape_ key is with out a home and and is moved one down to where the tilde was. This is cool but that moves the tilde to the upper right of the keyboard with the forward slash next to it, in one from the right on the top row. That leaves the `Backspace/Delete` button down one row and man it sure does feel nice not to travel as far to reach it. Thats is it. Four buttons move, not a huge adjustment. In fact I switch between the Happy Hacking keyboard and my laptop with no real issue. For me Adjusting to the modified layout was not a big deal. Setting up some of the shortcuts to work across both the HKB and the standard laptop layout took me some time to work out.

## Aligning Keybindings(Shortcuts)

Rethink some of my go to shortcuts with the new layout in mind.  What I found was modifying some of the laptop keyboard keybinding to match the available keys on the Happy Hacking Keyboard I was able to bring some consistency. On the HHKB command key lives where the caps-lock is on the laptop. __The solution is to remap the caps-lock on the laptop to control.__ Go to `Preferences > Keyboard > Shortcuts > Keyboard` under the keyboard tab, in the lower right click `Modifier Keys`. Perhaps I'm biased on this one. I have had my caps-lock mapped to control on my laptop for years. As a programer I think this is the single best thing you can to do. __Even if you don't go the HHKB route I highly recommend this.__ Its the way unix keyboards were originally designed. Do this for a day and you'll why caps-lock got promoted to that valuable real-estate in the first place.

One shortcut that took me a little while to replace is the command + \` for
switching between windows in an application. Like swapping caps-lock for
command this is an easy fix, built in to Mac's os. __You can remap the
command + \` to command + escape.__ To do this go
to `Preferences > Keyboard > Shortcuts > Keyboard` and then re-assigning
"Move focus to next window" to Command + Escape". Problem solved. :-)

## Dip-switches

The dip-switches are where you setup the hardware settings. Below are mine,
they are Mac specific.

|  1  |  2  |  3  |  4  |  5  |  6  |
| --- | --- | --- | --- | --- | --- |
| off | on  | on  | off | off | on  |

Switch __one and two__ set the keyboard to mac mode. This give you media
controls like volume, play, and next inside the function layer.
Switch __three__ set the delete button to be backspace. We defiantly want this.
Switch __six__ is wake mode. Set this so keyboard will wake up the computer.
