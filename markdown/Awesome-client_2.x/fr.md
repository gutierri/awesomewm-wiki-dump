---
title: Awesome-client 2.x fr
permalink: /Awesome-client_2.x/fr/
---

'''Ceci est la page de manuel de awesome-client version 2.3.4, valide pour toutes les versions 2.3.x. Elle est utilisable aussi pour une grande part avec les versions précédentes 2.0.x et 2.1.x, mais, pour les quelques différences qu'il y a, veuillez consulter les pages de manuel de ces versions. '''

<H1>
AWESOME-CLIENT

</H1>
Section: (1)

<H2>
NOM

</H2>
awesome-client - interface en ligne de commande du gestionnaire de fenêtres awesome

<H2>
SYNOPSIS

</H2>
awesome-client

<H2>
DESCRIPTION

</H2>
awesome-client est l'interface en ligne de commande pour awesome. Il communique avec awesome à travers une interface de connexion située dans le répertoire HOME de l'utilisateur.

<H2>
UTILISATION

</H2>
Pour déterminer quelle interface de connexion doit être utilisée, awesome-client lit la variable d'environnement DISPLAY. Il lit les commandes à partir de l'entrée standard.

Lorsque vous pipez de multiples lignes dans awesome-client, une ligne vide purge les lignes déjà collectées dans awesome, avec une exécution immédiate.

Le format de la commande est : numéro_d'écran commande argument

Par exemple, pour changer le texte dans la boîte de texte d'une barre d'état dans l'écran 0, vous pouvez faire :

<DL COMPACT>
<DT>
<DD>
    echo 0 widget_tell <statusbar-name> <textbox-name> text Hello, world | awesome-client

</DL>
Pour changer une image d'icône dans l'écran 1, vous pouvez faire :

<DL COMPACT>
<DT>
<DD>
    echo 0 widget_tell <statusbar-name> <iconbox-name> image /home/user/image.jpg | awesome-client

</DL>
Pour afficher l'onglet numéro 3 dans l'écran 1 :

<DL COMPACT>
<DT>
<DD>
    echo 1 tag_view 3 | awesome-client

</DL>
Pour rendre maître la fenêtre focalisée dans l'écran 0 :

<DL COMPACT>
<DT>
<DD>
    echo 0 client_zoom | awesome-client

</DL>
<H2>
VOIR AUSSI

</H2>
awesome(1) awesomerc(5)

<H2>
AUTEUR

</H2>
Julien Danjou &lt;julien@danjou.info\[1\]&gt;

<H2>
WWW

</H2>
<I><http://awesome.naquadah.org></I>

<H2>
NOTES

</H2>
<DL COMPACT>
<DT>
1. julien@danjou.info

<DL COMPACT>
<DT>
<DD>
courriel : [mailto:julien@danjou.info](mailto:julien@danjou.info)

</DL>
<H2>
TRADUCTION

</H2>
Ce document est une traduction, réalisée par Jean-Luc Duflot <jl POING duflot CHEZ laposte POING net> le 2 novembre 2008.

L'équipe de traduction a fait le maximum pour réaliser une adaptation française de qualité. La version anglaise la plus à jour de ce document est toujours consultable via la commande : LANGUAGE=en man awesome-client. N'hésitez pas à signaler à l'auteur ou au traducteur, selon le cas, toute erreur dans cette page de manuel.

[Category:Awesome2](/Category:Awesome2 "wikilink")