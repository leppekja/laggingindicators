---
title: "Replacing a mouse with a pen and tablet?"
date: "2024-04-22"
categories: 
  - "accessibility"
  - "review"
tags: 
  - "handwriting"
  - "reviews"
  - "tablet"
  - "technology"
  - "writing"
---

These are some thoughts about using a [Wacom Intuos Wireless Pen and Tablet](https://estore.wacom.com/en-us/wacom-intuos-s-bluetooth-black-us-ctl4100wlk0.html) to complement a mouse and keyboard over the past few weeks. I made this purchase in an effort to decrease forearm and finger pain from overuse working at a desk. Previously, I've been using the Logitech MX Master 3 For Business mouse, and I was hoping that switching to a pen and tablet would limit some of the forearm rotation that was causing me pain. While I put the mouse to the side, I am still using a Glove80 keyboard, as well as Windows Voice Access.

### Overall

I wouldn't recommend it unless you need it. Odd compatibility issues, not easy to switch between input devices, and difficult to accomplish some tasks with a less accurate input device. Tablet itself is good enough if you want to test it out. Handwriting recognition was excellent.

### Set-up

Unpacking and setting up the tablet was easy, but appropriately configuring Windows and all of the applications took some time.

The tablet and pen configuration is done inside Wacom's software, and overrode the Windows settings. It is not the most intuitive software and figuring out what I should do - or what I should do - was kind of tricky. My initial settings change was just using a portion of the tablet to reflect the entire monitor, so I didn't have to move my wrist so much. I also updated a pen button to open the on screen handwriting panel.

Choosing Mouse mode should be done on an application basis. I found Mouse mode less intuitive when working across applications. It made it much more difficult to actually write as well, so I stayed in Pen mode, which maps the edges of the tablet to the edges of your screen.

There are definitely tradeoffs between Pen mode and Mouse mode when using a tablet. With Pen mode, I was forced to only use a portion of the tablet because I simply couldn't reach to cover all of the screen. It can also be kind of tricky when you move the pen away and then put it back. The mouse will jump to wherever your pen is on the screen. Plus, to reach the entire screen, sometimes I have to contort my wrist to really reach the lower left corner, for example, even when only using part of the tablet.

Not that you can add application specific settings. but I don't particularly want to add custom settings for every app that I use.

I tried a different couple places on the tablet to portion my screen too. At first I tried a corner, then I tried the middle bottom, and then I ended up just switching to the middle of the tablet. My actual writing area was quite small, and I only used very little of the tablet. It's also a bit odd to get used to because in general, when you're writing by hand, you're either moving the paper or your forearm as you write across the page. Here you don't need to do that.

The default onscreen writing pad is too small on the screen; I had to adjust it to be larger in the Personalization Settings menu. It's also tricky to just stay in the space of the onscreen writing pad, especially if you are using a smaller area of the Wacom tablet.

### Experience Using It

I use Windows 11. Some applications will require overrides; for example, Excel kept drawing in the spreadsheet, but also would refuse to show the Draw Ribbon where it is possible to enter "mouse mode", even with the Draw tab enabled in the Ribbon menu. I had to update the Default pen Settings in the advanced menu to always use the pen to interact with the cells like a mouse.

For some reason, working in VSCode was awful, especially in an Jupyter notebook. The mouse would disappear entirely, and I wasn't able to scroll in file using the scroll bar for more than a cell or two. Text and handwriting did work in regular .py files, but not in Jupyter notebooks.

Fields marked as password entry don't let me use handwriting as an input. The onscreen keyboard will pop up, but without an option to switch to Ink.

#### Windows Ink

Even though Windows Ink isn't part of this tablet, without it, the tablet would be useless, so it is important to consider what built-in option exists on your OS for handwriting recognition. I haven't done any research on custom solutions or tablets that come with their own replacement software.

Windows Ink advertises writing directly into fields; outside of the Start menu and Settings applications, this rarely works for most apps. Instead, I had to always enable the onscreen keyboard, then select the handwriting option.

So this introduces a bit of friction because I have to select the text input field, then click a button to open up the writing pad, then properly place my pen into the writing pad. Not a big deal, but it is something to get used to. I would like it better if it was like Windows Snip, for screenshots, where upon selecting a text box the screen "greyed out" and I could write anywhere on the tablet as input. But this is not the case, and you have to write within a onscreen writing pad.

I tried to turn on "tip-up sensitivity" to solve this, but upon exiting the Wacom menu, the selection would always revert back to disabled. So whatever that check box should do, it does not do.

Also, I did find that the recognized words scroll too slowly as I write. I sort of find myself running out of room to type and have to wait for the screen to catch up. That said, I thought the handwriting recognition was phenomenal. Very often what I would write appeared illegible on the screen, but Windows perfectly translated it.

The editing tools are okay. For example, deleting a word is just a scribble. But using the pad as a complement to a keyboard, where my primary usage of the pen would be to replace a mouse, e.g., selecting and highlighting a word, means the pen editing tools are ineffective.

#### Tablet

Sometimes, if I didn't lift the pen up from the tablet enough, even though it definitely wasn't touching, a line would still appear on the screen in between letters. I don't see an option to control the "lift distance".

Precision mode creates a box on your screen roughly around the current position of your pen and slows down the movement of your cursor. But to enable and disable precision mode, you have to touch a button. So you essentially need to enable precision mode as a button on your pen, otherwise you can't leave it without very, very slowly moving your pen to whatever button you have on your computer.

So this leads to a point where you sort of run out of buttons on the pen. Precision mode, left click and open the handwriting input onscreen, I think, is the ideal combo for the pen - but I only have 2 buttons to spare.

I mapped the "Express keys", buttons at the top of the writing pad, pretty quickly. One to control+find, and two to control+copy, +paste, with the last on Enter. With the buttons at the top of the pad, it can be sort of difficult to reach up, so I'm glad I went with the smallest version of the tablet. Adding precision mode to one of these buttons isn't helpful, because to click the button, my pen moves, so precision mode is created in the wrong place on the screen.

I'm not sure if this is a setting, but the pen does sort of stick to the edge of the screen, sort of like snapping to a grid line. It's not awful, but it is noticeable.

Occasionally scrolling, and the click by pressing the pen tip to the tablet, would just randomly not work. I can't quite consistently reproduce it. Switching apps or even just moving the pen away for a few seconds would often seem to help it figure itself out.

### Build Quality

The pen definitely feels like the cheapest part of the tablet. I don't like that the nib of the pen is loose, and I think the pen is too light overall. I definitely prefer the [iPad pen](https://www.apple.com/apple-pencil/), which feels much more solid. The button keys on the pen are also a bit loose, and they can almost sort of get stuck sometimes, or catch on the edge. I also don't care for how the pen feels against the tablet, slightly scratchy against a hard surface.

It can take a bit of time to get used to the sensitivity. Because the pen is so light, even though I feel like I'm not moving my hand, I'm still making tiny movements. The normal mouse wouldn't necessarily respond to this, because it's just heavier. So it's harder to move a very tiny area quickly, and oftentimes, where I hover the pen is different from where I actually push down.

Trying to hit a specific mark while pressing an on-pen button (e.g., a click and drag action) also changes where you're holding the pen tip to as well by just a little bit, and sometimes that makes the difference between missing or making your target.

I couldn't really notice to any input lag difference between wired and Bluetooth.

I ended up using a mouse pad to lean my wrist on, and putting the tablet on top of the main part of the mouse pad. The tablet is very thin, but the mouse pad helps with comfort as the arms of my chair are a bit higher.

### Other Notes

I do think that text-to-voice quicker than using the pen. I started writing this article with the pen, and then quickly switched to voice. Either way, the real slowdown is always editing. Typing is much more precise and easier to edit in real-time.

I did keep a mouse right by me. I wouldn't say I found myself reaching for it too much unless I was in Excel or VSCode. But it did become slightly more annoying if I needed to type something out, because then I'd have to put down the pen and move my hands to the keyboard. I wasn't really aware of how much I drag and drop as an action, which also becomes trickier with the pen.

Space on the desk becomes an issue too as well - I'm very happy that I got the smallest writing pad. If you're only using this pad in the context of daily admin work, the small version is great - if I did upgrade, I don't think it would be to get a bigger size. Also, I use a keyboard drawer in the desk, and I found that I had to pull out the drawer quite a lot, because my pen would hit the top of the desk when I was trying to write. I had the writing pad in the middle of each side of the Glove80.

I did get the cheapest tablet just cause I wanted to test it out. Having used it, I would appreciate multi-touch capabilities alongside the pen. Zoom in and out would be great, plus just replacing clicks and double clicks with a tap.

### Updates

May 2024

- I think that the Bluetooth disconnects too quickly if not using the tablet.

- [I ran into this issue with the jittering mouse](https://www.reddit.com/r/wacom/comments/k44hku/wacom_intuos_pro_medium_pen_creates_two_jittering/); restarting the driver solved it. This happened after when I returned home from using a dual screen set up while traveling.
