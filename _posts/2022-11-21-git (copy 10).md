---
layout: post
title: A perfect distro choice
subtitle: Choose a distro
tags: [question]
---

## What is called "distro"?
Distro full form is distribution. Here, the distro refers to GNU/Linux variety and flavors. Put your fun back in the real computing world, wander new features, or even better.

## Why did Windows fall apart?
As of today, most computers come with preinstalled Windows Operating System. However, Windows isn't a great OS or environment as a learning platform. Windows itself is closed-source software, which means you can't see the source code nor even know what's going on inside. Trying to reverse engineering the source code, may result in Microsoft can sue you for legal reasons. In fact, you don't own your copy of the Windows operating system, if you ever read the EULA (End User License Agreement), Microsoft clearly says,
> The software is licensed, not sold. Under this agreement, we grant you the right to install and run one instance of the software on your device (the licensed device), for use by one person at a time.
If you buy a Windows copy today, the same EULA will be applied, "The software is licensed, not sold", which means that you paid for Windows to get a copy of this piece of software, however, it's still not yours. legally owned by Microsoft and they completely reject the proof that you paid for this.

For particular reasons, the EULA license doesn't make any sense, in general, it's more like they'll take all of your money, but you can't protest about it. In my opinion, this isn't the right way to do business, especially for these big tech giants, they just want to sell their products no matter what. Not owning a single piece of software makes similar, "You don't use Windows, Windows uses you.".

## What do GNU/Linux licenses do about it?
GNU/Linux doesn't apply or have any kind of EULA license (EULA basically intended for closed source software) or based on any kind of proprietary license. Instead, it uses, free licenses, aka Open-source license(s). The most common and popular Open-source license is GNU Public License or GPL. Most GNU/Linux distros even the Linux kernel use GPL (v2.0) license, applying this license means, you're free to use this piece of software with some criteria.

In a nutshell, GPL says...
- The freedom to run the software for any purpose.
- The freedom to study the source code and change the software for any purpose.
- The freedom to share the software with others.
- The freedom to share your own modified versions of the software with others.

These are the main points of GPL. However, there are certain things you should know, (i) GPL requires you must distribute the source code. (ii) Forked or modified version must release the source code and the credits of the real author (iii) Same license. You can't modify GPL nor use any other license over it. (iv) If you used a piece of software that is under GPL, you must release the source code of the software where you used the code.

These certain rules are extremely good for open source. Because it not only blocks companies from suing a piece of popular open source project and making a proprietary distribution but also blocks some cheaters to use open source projects and use to own their software (for example using a small base code without giving author credits or not making it open source or both).

## What now, there are thousands of GNU/Linux distros
When talking about picking up a distro, lots of people even I also suffer to choose one of them and stick. However, I was able to make a list about it, after some distro hopping.

There are main three points, you must know before further hacking.
(i) Simple, lightweight, customizable.
(ii) Eye-candy UI, "quite" lightweight, customizable.
(iii) Easy to use, eye-candy, not lightweight in general, customizable.

After these, there's one thing you must know all GNU/Linux distros are pretty much the same. They all use Executable and Linking Format (formally ELF) for programs. File and folder structure and basically the same, nothing changes, file system mostly uses ext4 or ZFS by default (by the way, you can choose different file systems as long as they're not obsolete or not used as a general file system) Windows, however, can't be compared with this, they're totally different.

Below is the list, that I've made in mind of uses and comfortability.

[Arch Linux](https://archlinux.org/) - A simple, yet configurable from ground zero. Have AUR (Arch User Repository) where various people can put software pre-compiled. Recommended for programming, gaming, video editing, etc. It's a rolling release distro, might be unstable at some points.

[Debian](https://www.debian.org/) - A stable distro that is pre-configured by the installer, and attentionally follows open source culture. Have a large collection of software, drivers, etc. Even have "non-free" repos' for proprietary drivers, etc. Recommended for general uses, programming, and privacy-related work. It ships with quite an old version of the software, which isn't a problem at all. Debian also has an unstable branch, it's mainly for testing purposes, and has a rolling newer version of the software.

[Devuan](https://www.devuan.org/) - A stable distro which is a fork of Debian GNU/Linux, mainly because to remove systemd as an init system. If you know the fight about init systems then you might know how people follow UNIX philosophy, especially (do one thing and do it well). I've already written a post about this previously, where I try yo a little bit explain what's going on. If you want your system to be rock solid but without systemd (as you don't want to bother to remove systemd from Debian), Devuan is a great choice for you. One known issue is that, since Devuan is maintained by a very small community, there's a lack of package repos' that have great bandwidth, to fix it you can just follow [this](https://pastebin.com/uTMc2kTf). The recommendation is the same as Debian's. Don't do any sky jumping while installing it, as the installer is pretty straightforward.

[EndeavourOS](https://endeavouros.com/) - A rolling release-based distro, which is based on Arch Linux. Have an eye-candy look, with various desktop environments, so you don't have to configure anything, most of the time, it's ready to go GNU/Linux distro. However, a back draw is that it comes with a lot of packages that you may never go to use, so before picking up Endeavour, make sure you like "bloats" as previously written. Recommended for gamers, daily users, programmers, etc. Comes with almost every major desktop environment including KDE, GNOME, XFCE, Budgie, Cinnamon, LXDE, LXQT, Sway, i3, etc.

[Void Linux](https://voidlinux.org/) - A rolling release-based distro, which also offers quite stability towards newer software. Unlike Arch, it's probably the most lightweight GNU/Linux distro, that has XFCE preinstalled and all other stuff within ~900 MiB. This GNU/Linux distro is known for being lightweight and systemd free. It uses runit as an init system, and has both GLIBC and MUSL libraries for core libraries. Recommended for general uses, newer software within stable distro, programming, and privacy work. You've two options, base installer and XFCE preinstalled version, both are nice. If you're newer in Void Linux, try XFCE version.


## Which GNU/Linux distro would you pick up, or what distro do you use from these?
I use Devuan with XFCE DE. However, I also use LXDE sometimes during my hardware issues. Before I used Void Linux, however, I recently discovered a bug with lightdm-gtk3-greeter that the DE goes black after re-login in an idle state and that's where the bug came out, for that, I always need to restart either change TTY which was a pain, after this, it made me move on Devuan for stability reason.

It's not up to me to give a reason about what GNU/Linux you should use, I can only advise some distros which I think are somewhat better in packages and usage. 
