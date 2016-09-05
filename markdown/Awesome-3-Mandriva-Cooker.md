---
title: Awesome-3-Mandriva-Cooker
permalink: /Awesome-3-Mandriva-Cooker/
---

To get Awesome3 working on a Mandriva Cooker, you just need two things :

-   cairo recompiled with --enable-xcb
-   awesome 3.3

Every other dependency is on the mirrors, I assume you defined yours correctly.

RPMs for both of the previous items are available here :

-   <http://kenobi.mandriva.com/~shikamaru/cairo-xcb/>
-   <http://kenobi.mandriva.com/~shikamaru/awesome/>

To replace cairo if it's already installed you can do rpm -e --nodeps libcairo2 (lib64cairo2 if you're on x86_64).

I also suggest you to put /cairo/ in your /etc/urpmi/skip.list so that it does not get overwritten by an update.