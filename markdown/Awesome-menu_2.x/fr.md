---
title: Awesome-menu 2.x fr
permalink: /Awesome-menu_2.x/fr/
---

'''Ceci est la page de manuel de awesome-menu version 2.3.4, valide pour toutes les versions 2.3.x. Elle est utilisable aussi pour une grande part avec les versions précédentes 2.0.x et 2.1.x, mais, pour les quelques différences qu'il y a, veuillez consulter les pages de manuel de ces versions. '''

<H1>
AWESOME-MENU

</H1>
Section: (1)

<H2>
NOM

</H2>
awesome-menu - système de menu de awesome

<H2>
SYNOPSIS

</H2>
awesome-menu \[-c config\] \[-e command\]

<title>
<H2>
DESCRIPTION

</H2>
awesome-menu est un outil qui fait apparaître un menu sur votre écran ; par la saisie au clavier il vous permet de chercher dans une liste de complètement par défaut, ou dans une liste de complètement contenant vos propres fichiers.

<H2>
UTILISATION

</H2>
Par défaut, awesome-menu lit le complètement à partir de l'entrée standard. Si rien n'est lu, la liste de complètement est construite à partir des fichiers du répertoire courant de travail.

Pour construire un menu avec tous les fichiers exécutables de /usr/bin :

<DL COMPACT>
<DT>
<DD>
    ls /usr/bin | awesome-menu -e 'exec ' 'Execute'

</DL>
Pour construire le même menu et lancer les programmes dans une fenêtre de terminal :

<DL COMPACT>
<DT>
<DD>
    ls /usr/bin | awesome-menu -e 'xterm -e exec ' 'Execute in terminal'

</DL>
Pour obtenir une invite de connection ssh :

<DL COMPACT>
<DT>
<DD>
    cut -d' ' -f1 ~/.ssh/known_hosts | cut -d, -f1 | awesome-menu -e 'xterm -e ssh ' 'ssh to:'

</DL>
Si vous ne spécifiez pas l'option -e, le résultat sera envoyé sur la sortie standard. Vous pouvez faire quelquechose comme ça :

<DL COMPACT>
<DT>
<DD>
    gzip "$(awesome-menu '"File to gzip')"

</DL>
<H2>
OPTIONS

</H2>
-c config

<DL COMPACT>
<DT>
<DD>
Utilise le fichier de configuration "config" à la place de $HOME/.awesomerc.

</DL>
-e command

<DL COMPACT>
<DT>
<DD>
Commande à exécuter. Le résultat est ajouté à la fin de cette commande (comme xargs).

</DL>
<title>
<DL COMPACT>
<DT>
<DD>
Affiche "title". On l'utilise aussi pour identifier une section de menu dans le fichier awesomerc.

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

L'équipe de traduction a fait le maximum pour réaliser une adaptation française de qualité. La version anglaise la plus à jour de ce document est toujours consultable via la commande : LANGUAGE=en man awesome-menu. N'hésitez pas à signaler à l'auteur ou au traducteur, selon le cas, toute erreur dans cette page de manuel.

[Category:Awesome2](/Category:Awesome2 "wikilink")