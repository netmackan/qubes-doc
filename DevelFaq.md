---
layout: wiki
title: DevelFaq
permalink: /wiki/DevelFaq/
---

Qubes Developers FAQ
====================

### Q: Why does dom0 need to be 64-bit?

Often it is more difficult to exploit a bug on the x64 Linux than it is on x86 Linux (e.g. ASLR is sometimes harder to get around). While we designed Qubes with the emphasis on limiting any potential attack vectors in the first place, still we realize that some of the code running in Dom0, e.g. our GUI daemon or xen-store daemon, even though it is very simple code, might contain some bugs. Plus currently we haven't implemented a separate storage domain (which is planned only for Release 2), so also the disk backends are in Dom0 and are "reachable" from the VMs, which adds up to the potential attack surface. So, having faced a choice between 32-bit and 64-bit OS for Dom0, it was almost a no-brainer, as the 64-bit option provides some (little perhaps, but still) more protection against some classes of attacks, and at the same time do not have any disadvantages (except that it requires a 64-bit processor, but all systems on which it makes sense to run Qubes, e.g. that have at least 3-4GB memory, they do have 64-bit CPUs anyway).

### Q: Why do you use KDE in Dom0? What is the roadmap for Gnome support?

There are very few things that are KDE-specific and thus it should not be a big problem to port add Gnome support to Qubes (in Dom0 of course). Those KDE-specific things are:

-   Qubes requires KDM (KDE Login Manager), rather than GDM, for the very simple reason that GDM doesn't obey standards and start ```/usr/bin/Xorg``` instead of ```/usr/bin/X```. This is important for Qubes, because we need to load a special "X wrapper" (to make it possible to use Linux usermode shared memory to access Xen shared memory pages in our App Viewers -- see the sources [​here](http://qubes-os.org/gitweb/?p=mainstream/gui.git;a=tree;f=shmoverride;h=75133ddcdad0c6a59e630f005569bb8c758b67c5;hb=HEAD)). So, Qubes makes the ```/usr/bin/X``` to be a symlink to the Qubes X Wrapper, which, in turn, executes the ```/usr/bin/Xorg```. This works well with KDM (and would probably also work with other X login managers), but not with GDM. If somebody succeeded in makeing GDM to execute ```/usr/bin/X``` instead of ```/usr/bin/Xorg```, we would love to hear about it!

-   We're planning to patch the KDE's Window Manager (KWin) to draw window decorations in the color of the specific AppVM's label. Currently we draw frames in the [AppViewers?](/wiki/AppViewers), but it would be a better user experience to also see the title bars having the color of the VM's label. Most likely this should also be possible to be done on GNOME's Window Manager too, but as we have very litmited resources, we are forced to pick only one Window Manager, and we decided to pick KDE (just because we like it). If somebody were willing to send the code for GNOME, we would be more that happy to incorporate it into mainstream repo.

### Q: What is the recommended build environment?

Fedora 12 Linux, 64-bit, with most of the regular development packages installed.