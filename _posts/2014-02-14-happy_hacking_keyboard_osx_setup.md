---
tags: [Happy Hacking, Happy Hacking Keyboard, HHKB, Mac, Setup, Shortcuts]
category: "Programming Keyboards"
title: Happy Hacking Keyboard, OSX Configuration
---

A few months back, I set out to buy a new keyboard. I wanted something well-built and made for coding. I had no idea what I was getting myself into. The world of keyboards is a strange and wonderful corner of the Internet. There are as many choices out there as there are opinions. The one constant I found is that all keyboards aimed at the discerning keyboardist have mechanical switches. Like the keyboards from the 80's and 90's that have a distinctive click as you type, as opposed to the thin keys on a laptop. After a lot of research I ended up going with the [Happy Hacking Keyboard Pro 2](https://en.wikipedia.org/wiki/Happy_Hacking_Keyboard) (HHKB) by PFU Limited out of Japan. __This keyboard isn't going to be ideal for everyone, but those who like it (including myself), really like it__. The HHKB uses [Topre](http://deskthority.net/wiki/Topre_switch) mechanical switches. Again, loved by some but not by others. I have grown to really like the distinctive sound the keys make. After nearly a year with my new friend, I have a few thoughts to share:

First, a few notes about how I use my keyboard: I use a Mac. My text editor of choice is Vim. I chose the HHKB with no marks on the keycaps. Also long before I embarked on this journey I mapped my `Caps-Lock` to the control key. More on this later.

## Function Layer

The HHKB falls into the a category called a [60% keyboard]. This refers to the keyboard layout containing roughly 60% of the keys of a full keyboard. The reduced size is achieved by omitting the keypad, direction and F-keys (`F1`, `F2`...). The missing keys are accessed by a function key. Holding down this function key is like holding down a shift key, and gives some keys new actions. It's referred to as the function layer: when `Function` is held down a new layer of keys is available. All 60% keyboards rely on a function layer. When using the HHKB, holding down `Function` turns number keys into function keys, the `O , K L` become up, down, left, and right, respectively. And for Mac music-lovers `A S D F` become volume up, volume down, mute and eject.

## The HHKB Layout

Along with the function layer, the HHKB has other differences in the key layout. These differences are pretty minor and provide (in my mind) a noticeable performance gain. The dedicated `Caps-Lock` button is gone and in its place is the `Control` button. The missing F-keys leave the `Escape` key without a home, so it's one row down where the `` ~ `` usually is. That moves the `Tilde/Backtick` to the upper right of the keyboard to the left of the forward slash & pipe. The `Backspace/Delete` button is moved down one row.  Man, it sure does feel nice; not traveling as far to reach `Backspace` is a good thing. That's it - four buttons moved, not a huge adjustment. In fact, I switch between The Happy Hacking Keyboard and my laptop with no issues. However, mapping shortcuts that work seamlessly on both the HHKB and the standard laptop took some time.

### Aligning Shortcuts

One of my goals in setting up the HHKB was to minimize friction when swapping between my desktop and laptop. Muscle memory is a good thing, and I don't want to be hopelessly crippled on every other keyboard on the planet.

A few of my go-to shortcuts required rethinking for the new keyboard layout. I modified the laptop's key layout to match the available keys on the HHKB to create some consistency. I __remapped the caps-lock on the laptop to behave as a second `Control` key__ to match the HHKB's `command key` layout (Mac: `Preferences > Keyboard`, `Keyboard` tab. Click `Modifier Keys` on the lower right). Perhaps I'm biased, because I have had my caps-lock mapped to control on my laptop for years. As a programmer, I think this is a best practice to improve your typing performance. __Even if you use a standard keyboard, I highly recommend this.__ It's the way Unix keyboards were originally designed. Try it for a day and you'll wonder why `Caps-Lock` earned such valuable keyboard real-estate.

One shortcut I use a lot  is the ``Command + ` `` for switching between windows in an application. It took me a while to work out a replacement to this one. Built in to the Mac OS is an option to remap the ``Command + ` `` to `Command + Escape`. This lets me have the same binding for both keyboards. To do this' go to `Preferences > Keyboard > Shortcuts > Keyboard` and then re-assigning "Move focus to next window" to `Command + Escape`. Problem solved. :-)

## Dip-switches

The dip-switches are where you set the hardware settings. Here are my Mac specific settings:

|  1  |  2  |  3  |  4  |  5  |  6  |
| --- | --- | --- | --- | --- | --- |
| off | on  | on  | off | off | on  |

- Switch __one and two__ set the keyboard to Mac mode. This sets the media controls (volume `up` and `down`, `play`, and `pause`) on the function layer.
- Switch __three__ sets the `delete` button to backspace. I definitely want this.
- Switch __six__ is *wake mode*. Set this so the keyboard will wake up the computer from sleep.

## Wrapping Up

This is where I am with my HHKB Pro 2 at the moment. It's definitely a work in progress. I'd love to know what you think. If you have any other pro tips for getting the most out of the Happy Hacking Keyboard share 'em in the comments down below.

[60% keyboard]: http://deskthority.net/wiki/60%25
