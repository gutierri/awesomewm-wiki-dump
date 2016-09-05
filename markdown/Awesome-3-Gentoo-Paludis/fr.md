---
title: Awesome-3-Gentoo-Paludis fr
permalink: /Awesome-3-Gentoo-Paludis/fr/
---

Ajoutez le dépôt suivant à **/etc/paludis/repositories** dans un [fichier **.conf**](http://paludis.pioto.org/configuration/repositories.html) :

[`git://gitorious.org/thewtex-repo/mainline.git`](git://gitorious.org/thewtex-repo/mainline.git)

On peut maintenant trouver quelques ebuilds d’awesome 3 sur le dépôt de portage de Gentoo, mais ils sont masqués. Il vous faudra donc ajouter « x11-wm/awesome » dans votre fichier **/etc/paludis/package_unmask.conf**.

Mettez un mot-clé « x11-wm/awesome » dans **/etc/paludis/keywords.conf**, avec « ~amd64 » ou « ~x86 » si vous voulez la dernière version, out « \* » si vous voulez la dernière compilation du git :

`paludis --install --pretend awesome`
`paludis --install awesome`

[Category:Awesome3](/Category:Awesome3 "wikilink")