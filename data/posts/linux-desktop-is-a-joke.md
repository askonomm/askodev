---
title: Linux desktop is a joke
slug: linux-desktop-is-a-joke
date: 2023-11-08
year: 2023
status: published
---

Yesterday I tried putting Linux on a spare laptop of mine. It's a 2023 model, not old, and I just wanted to squeeze a little extra performance out of the poor i5 processor it has. And thus, off I went, distro hunting. 

## Ubuntu

Probably the most known Linux distro is [Ubuntu](https://ubuntu.com/desktop). So I got the latest version, Ubuntu 23. Once installed and logged in, the OS told me that there are a bunch of updates I should install, so I did like it instructed me to do. After restarting the computer, like it told me to do after updates, I was no longer able to get into the OS because all I was greeted with was a black screen and a error `Malformed MSFT vendor event`, which upon Googling I found to apparently mean a million different things, none of which helped me get the operating system back working.

And honestly, if simple updates that the OS itself suggests completely bricks the OS, is it really meant for professional use? I don't think so. I don't want to _fix_ an OS, I want to use one.

## Linux Mint

I then tried [Linux Mint](https://linuxmint.com/). It's based on Ubuntu, so I didn't have high expectations, but people on the internet said it's the best beginner distro (whatever that means, is there some linux difficulty level like in games? I just want to use the damn operating system). 

Linux Mint installed fine, updates worked fine and I did not have odd, unexpected crashes here. That said, the operating system _looks_, as in, it's design, if you can call it that, is absolutely horrendous. Inconsistent font sizes across apps, inconsistent padding/spacing, some apps had a larger X button to close than others. I mean it literally looks like a professional designer had never touched the desktop environment (Cinnamon). 

Then, because my laptop is a modern device, it has a HiDPI display, but using it at 100% scaling makes things way too small to read, and at 200% makes things again too big to be usable. There is experimental support for fractional scaling, something which has existed in MacOS and Windows for years, so I turn it on, and it worked surprisingly well, except for the noticeable drop in OS performance and constant screen tearing. 

The horrible design and performance issues made me not want to use Linux Mint.

## Elementary OS

Arguably the only Linux distro that is developed with actual designers on the team. I was very tempted to use [Elementary OS](https://elementary.io/). Though it again is based on Ubuntu, so I was a little scared, but it seemed to work fine. What did not work fine however was the fact that it has no fractional scaling support at all, which meant it was completely unusable on HiDPI displays, and from googling it seemed the team was not interested in even fixing that? 

The recommended fix that the team had was to change the text size in settings, which worked fine to a degree, but left window title bar icons way too small to be easily clickable. Unfortunately fractional scaling support is pretty important on a HiDPI display and that meant this OS simply was not going to cut it.

## Installing applications is a nightmare

Installing apps on Linux is definitely an .. experience. While a lot of apps are available via the OS's own App Store, a ton more simply aren't. Going through the internet downloading apps I use often I encountered a variety of different ways apps distributed themselves. 

Some were .deb files, some were AppImage files, some were .tar.gz zip archives, others again where binary executables. Some you could install via the terminal, but then some would only have an outdated version available via the terminal leaving you the only opportunity of compiling the app yourself. Good luck doing that if you're not a programmer. 

## Conclusion

I truly don't think I've ever seen a more fragmented ecosystem. Nothing has polish, nothing has consistency, things that used to work suddenly need workarounds, simple OS updates break the OS, and so on. 

The Linux community itself is also delusional in thinking it is ready for prime-time, calling for the "year of the Linux desktop", and with no one agreeing on what ways a user should install things / use things / what distro to use / etc, it remains a steaming pile of shit that is more confusing than useful.

Linux's evangelists on Reddit seem to endlessly argue how fractional scaling supports is not needed, and that nobody uses HiDPI displays, despite HiDPI displays being the most sold displays out there (any 27inch 4k display, which are most of them, for example). Clearly they have never used any other OS, are unaware of the world around them, and live in their own little bubble. 

Maybe I'll try Linux desktop again in 10 years. Maybe by then it has attracted some actual designers to help out, and people who don't dismiss user needs that don't meet their worldview yet statistically are more common than their needs, so that actual progress could be made.