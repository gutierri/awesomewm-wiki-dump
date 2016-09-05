---
title: Automounting fr
permalink: /Automounting/fr/
---

Dans les environnements de bureau célèbres tels que Gnome, KDE ou XFCE, le montage automatique des volumes est installé par défaut. Vous n’avez qu’à brancher votre clé USB pour accéder à vos données dans Nautilus, D3lphin ou Thunar. Puisqu’awesome n’est pas un environnement de bureau mais un gestionnaire de fenêtres, cette fonctionnalité n’est pas fournie par défaut.

Pour récupérer ce comportement, une application auto suffisante sans aucune dépendance qui fait très bien du montage automatique est disponible : [Ivman](http://ivman.sourceforge.net/)

Remarque.
*Le projet Ivman semble mort : la dernière version date de 2007. De plus, il ne semble plus fonctionner avec les dernières versions de Hal et D-Bus. Son successeur est [Halevt](http://www.nongnu.org/halevt/). Le fichier de configuration de Halevt est très proche de celui d’Ivman.*

__NOTOC__

Installation
------------

### Arch Linux

Sur Archlinux, vous pouvez installer Ivman depuis AUR :

[`http://aur.archlinux.org/packages.php?ID=18938`](http://aur.archlinux.org/packages.php?ID=18938)

### Debian et assimilées

Sur une distribution de type Debian, vous pouvez installer ce programme en tapant la ligne suivante en *root* dans une console virtuelle :

`aptitude install ivman`

### FreeBSD

Sur FreeBSD, vous pouvez l’installer en tapant dans une console virtuelle :

`su root -c pkg_add\ -r\ ivman`

### Gentoo Linux

Pour Gentoo Linux :

`# emerge sys-apps/ivman`
`# /etc/init.d/ivman start`

ou en l’ajoutant au runlevel :

`# rc-update -a ivman default`

Après l’avoir installé, ajoutez *ivman* à votre **~/.xinitrc**. Voici à quoi ça devrait ressembler.

`#!/bin/sh`
`ivman &`
`exec awesome`

Ou lancez simplement *ivman* et à vous les joies du montage automatique !