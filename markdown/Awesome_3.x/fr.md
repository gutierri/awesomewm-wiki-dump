---
title: Awesome 3.x fr
permalink: /Awesome_3.x/fr/
---

**Ceci est la page de manuel de awesome version 3.4.1. Elle est utilisable aussi pour une grande part avec les versions précédentes 3.x, parce qu'il y a peu de différences ; par exemple, les commandes Mod4 + w (Ouvre le menu principal) dans la version 3.3 et Mod4 + n (Minimise la fenêtre) dans la version 3.4.1 ont été ajoutées. Néanmoins, vous pouvez consulter spécifiquement leurs pages si besoin est.**

<H1>
AWESOME

</H1>
<H2>
NOM

</H2>
awesome - gestionnaire de fenêtres awesome

<H2>
SYNOPSIS

</H2>
<B>awesome</B> \[<B>-v</B> | <B>--version</B>\] \[<B>-h</B> | <B>--help</B>\] \[<B>-c</B> | <B>--config</B> <I>FILE</I>\] \[<B>-k</B> | <B>--check</B>\]

<H2>
DESCRIPTION

</H2>
<B>awesome</B> est un gestionnaire de fenêtres pour X. Il gère les fenêtres avec diverses mises en page, par exemple flottante ou juxtaposée. Toutes les mises en page peuvent être appliquées dynamiquement, pour optimiser l'environnement pour l'application utilisée et la tâche en cours d'exécution.

Avec la mise en page juxtaposée, les fenêtres sont gérées avec une zone maître et une zone d'empilage. La zone maître contient les fenêtres qui nécessitent le plus d'attention, tandis que la zone d'empilage contient toutes les autres fenêtres. Avec la mise en page flottante, les fenêtres peuvent être redimensionnées et déplacées à la demande. Les fenêtres de dialogue sont toujours gérées d'une manière flottante, quelle que soit la mise en page en cours. Les mises en forme spirale ou décroissante sont des modes spéciaux de mise en forme juxtaposée, où la zone d'empilage est présentée en spirale pour la première, ou comme une fractale rectangulaire pour la seconde.

Dans awesome les fenêtres sont groupées en onglets. Chaque fenêtre peut être affectée à un ou plusieurs onglets. En sélectionnant plusieurs onglets on peut afficher toutes les fenêtres affectées à ces onglets. <B>awesome</B> peut contenir des petites boîtes graphiques qui peuvent afficher tout ce que vous désirez : tous les onglets disponibles, la mise en page courante, le titre des fenêtres visibles, du texte, etc.

<H2>
OPTIONS

</H2>
<B>-v</B>, <B>--version</B>

<DL COMPACT>
<DT>
<DD>
Affiche la version de awesome sur la sortie standard.

</DL>
<B>-h</B>, <B>--help</B>

<DL COMPACT>
<DT>
<DD>
Affiche l'aide.

</DL>
<B>-c</B>, <B>--config</B> <I>FILE</I>

<DL COMPACT>
<DT>
<DD>
Définit un autre fichier de configuration à la place de <I>$XDG_CONFIG_HOME/awesome/rc.lua</I>.

</DL>
<B>-k</B>, <B>--check</B>

<DL COMPACT>
<DT>
<DD>
Vérifie la syntaxe du fichier de configuration.

</DL>
<H2>
ASSOCIATIONS AVEC LA SOURIS (PAR DÉFAUT)

</H2>
<H3>
Navigation

</H3>
<B>Bouton1</B> sur le nom de l'onglet

<DL COMPACT>
<DT>
<DD>
Affiche le contenu de l'onglet.

</DL>
<B>Bouton4</B>, <B>Bouton5</B> sur le nom de l'onglet

<DL COMPACT>
<DT>
<DD>
Affiche le contenu de l'onglet précédent ou suivant.

</DL>
<B>Bouton4</B>, <B>Bouton5</B> sur un onglet vide

<DL COMPACT>
<DT>
<DD>
Affiche le contenu de l'onglet précédent ou suivant.

</DL>
<B>Bouton1</B>, <B>Bouton3</B>, <B>Bouton4</B>, <B>Bouton5</B> sur le symbole de mise en page

<DL COMPACT>
<DT>
<DD>
Affiche la mise en page précédente ou suivante.

</DL>
<H3>
Changement dans la mise en page

</H3>
<B>Mod4 + Bouton1</B> sur le nom de l'onglet

<DL COMPACT>
<DT>
<DD>
Déplace la fenêtre courante vers cet onglet.

</DL>
<B>Mod4 + Bouton3</B> sur le nom de l'onglet

<DL COMPACT>
<DT>
<DD>
Ajoute ou retire la fenêtre courante à cet onglet.

</DL>
<B>Bouton3</B> sur le nom de l'onglet

<DL COMPACT>
<DT>
<DD>
Ajoute ou retire le contenu de cet onglet à la vue courante.

</DL>
<B>Mod4 + Bouton1</B> sur la fenêtre courante avec déplacement de la souris

<DL COMPACT>
<DT>
<DD>
Déplace la fenêtre courante sur l'écran.

</DL>
<B>Mod4 + Bouton3</B> sur la fenêtre courante avec déplacement de la souris

<DL COMPACT>
<DT>
<DD>
Redimensionne la fenêtre courante sur l'écran.

</DL>
<H2>
ASSOCIATIONS AVEC LES TOUCHES DU CLAVIER (PAR DÉFAUT)

</H2>
<H3>
Contrôle du gestionnaire de fenêtres

</H3>
<B>Mod4 + Contrôle + r</B>

<DL COMPACT>
<DT>
<DD>
Redémarre le gestionnaire de fenêtres <B>awesome</B>.

</DL>
<B>Mod4 + Majuscule + q</B>

<DL COMPACT>
<DT>
<DD>
Quitte le gestionnaire de fenêtres <B>awesome</B>.

</DL>
<B>Mod4 + r</B>

<DL COMPACT>
<DT>
<DD>
Affiche une invite de ligne de commande dans la barre de statut.

</DL>
<B>Mod4 + x</B>

<DL COMPACT>
<DT>
<DD>
Affiche une invite de code lua dans la barre de statut.

</DL>
<B>Mod4 + Retour</B>

<DL COMPACT>
<DT>
<DD>
Lance un émulateur de terminal.

</DL>
<B>Mod4 + w</B>

<DL COMPACT>
<DT>
<DD>
Ouvre le menu principal.

</DL>
<H3>
Clients

</H3>
<B>Mod4 + Majuscule + r</B>

<DL COMPACT>
<DT>
<DD>
Réaffiche la fenêtre focalisée.

</DL>
<B>Mod4 + m</B>

<DL COMPACT>
<DT>
<DD>
Maximize la fenêtre de l'application.

</DL>
<B>Mod4 + n</B>

<DL COMPACT>
<DT>
<DD>
Minimise la fenêtre de l'application.

</DL>
<B>Mod4 + f</B>

<DL COMPACT>
<DT>
<DD>
Affiche la fenêtre de l'application en plein écran.

</DL>
<B>Mod4 + Majuscule + c</B>

<DL COMPACT>
<DT>
<DD>
Ferme la fenêtre focalisée.

</DL>
<B>Mod4 + t</B>

<DL COMPACT>
<DT>
<DD>
Marque la fenêtre de l'application.

</DL>
<H3>
Navigation

</H3>
<B>Mod4 + j</B>

<DL COMPACT>
<DT>
<DD>
Focalise sur la fenêtre suivante.

</DL>
<B>Mod4 + k</B>

<DL COMPACT>
<DT>
<DD>
Focalise sur la fenêtre précédente.

</DL>
<B>Mod4 + u</B>

<DL COMPACT>
<DT>
<DD>
Focalise sur la première fenêtre urgente.

</DL>
<B>Mod4 + Flèche Gauche</B>

<DL COMPACT>
<DT>
<DD>
Affiche l'onglet précédent.

</DL>
<B>Mod4 + Flèche Droite</B>

<DL COMPACT>
<DT>
<DD>
Affiche l'onglet suivant.

</DL>
<B>Mod4 + 1-9</B>

<DL COMPACT>
<DT>
<DD>
Affiche le contenu de l'onglet 1 à 9.

</DL>
<B>Mod4 + Contrôle + j</B>

<DL COMPACT>
<DT>
<DD>
Focalise sur l'écran d'affichage suivant et y déplace la souris.

</DL>
<B>Mod4 + Contrôle + k</B>

<DL COMPACT>
<DT>
<DD>
Focalise sur l'écran d'affichage précédent et y déplace la souris.

</DL>
<B>Mod4 + échappement</B>

<DL COMPACT>
<DT>
<DD>
Affiche l'onglet sélectionné précédemment.

</DL>
<H3>
Changement dans la mise en page

</H3>
<B>Mod4 + Majuscule + j</B>

<DL COMPACT>
<DT>
<DD>
Intervertit la fenêtre courante avec la suivante.

</DL>
<B>Mod4 + Majuscule + k</B>

<DL COMPACT>
<DT>
<DD>
Intervertit la fenêtre courante avec la précédente.

</DL>
<B>Mod4 + o</B>

<DL COMPACT>
<DT>
<DD>
Déplace le client vers l'écran suivant.

</DL>
<B>Mod4 + h</B>

<DL COMPACT>
<DT>
<DD>
Diminue la largeur de la fenêtre-maître de 5%.

</DL>
<B>Mod4 + l</B>

<DL COMPACT>
<DT>
<DD>
Augmente la largeur de la fenêtre-maître de 5%.

</DL>
<B>Mod4 + Majuscule + h</B>

<DL COMPACT>
<DT>
<DD>
Augmente de 1 le nombre de fenêtres-maître.

</DL>
<B>Mod4 + Majuscule + l</B>

<DL COMPACT>
<DT>
<DD>
Diminue de 1 le nombre de fenêtres-maître.

</DL>
<B>Mod4 + Contrôle + h</B>

<DL COMPACT>
<DT>
<DD>
Augmente de 1 le nombre de colonnes pour les fenêtres non-maître.

</DL>
<B>Mod4 + Contrôle + l</B>

<DL COMPACT>
<DT>
<DD>
Diminue de 1 le nombre de colonnes pour les fenêtres non-maître.

</DL>
<B>Mod4 + Espace</B>

<DL COMPACT>
<DT>
<DD>
Affiche la mise en page suivante.

</DL>
<B>Mod4 + Majuscule + Espace</B>

<DL COMPACT>
<DT>
<DD>
Affiche la mise en page précédente.

</DL>
<B>Mod4 + Contrôle + Espace</B>

<DL COMPACT>
<DT>
<DD>
Rend la fenêtre flottante, ou fixe si elle est déjà flottante.

</DL>
<B>Mod4 + Contrôle + Retour</B>

<DL COMPACT>
<DT>
<DD>
Intervertit la fenêtre focalisée avec la fenêtre-maître.

</DL>
<B>Mod4 + Contrôle + 1-9</B>

<DL COMPACT>
<DT>
<DD>
Ajoute ou supprime le contenu de l'onglet 1 à 9 à l'onglet courant.

</DL>
<B>Mod4 + Majuscule + 1-9</B>

<DL COMPACT>
<DT>
<DD>
Déplace la fenêtre courante dans l'onglet 1 à 9.

</DL>
<B>Mod4 + Majuscule + Contrôle + 1-9</B>

<DL COMPACT>
<DT>
<DD>
Ajoute ou retire la fenêtre courante à l'onglet 1 à 9.

</DL>
<B>Mod4 + Majuscule + F1-9</B>

<DL COMPACT>
<DT>
<DD>
Déplace les applications marquées vers l'onglet 1 à 9.

</DL>
<H2>
PERSONNALISATION

</H2>
<B>awesome</B> peut être personnalisé en créant un fichier spécifique <I>$XDG_CONFIG_HOME/awesome/rc.lua</I>.

<H2>
SIGNAUX

</H2>
<B>awesome</B> peut être redémarré en lui envoyant un signal SIGHUP.

<H2>
VOIR AUSSI

</H2>
<B>awesomerc</B>(5) <B>awesome-client</B>(1)

<H2>
BOGUES

</H2>
Bien sûr il n'y a aucun bogue dans <B>awesome</B> ! Il peut seulement y avoir des comportements inattendus....

<H2>
AUTEURS

</H2>
Julien Danjou &lt;julien@danjou.info\[1\]&gt; et d'autres personnes.

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

<H2>
TRADUCTION

</H2>
Ce document est une traduction, réalisée par Jean-Luc Duflot <jl POING duflot CHEZ laposte POING net> le 9 novembre 2009.

L'équipe de traduction a fait le maximum pour réaliser une adaptation française de qualité. La version anglaise la plus à jour de ce document est toujours consultable via la commande : LANGUAGE=en man awesome. N'hésitez pas à signaler à l'auteur ou au traducteur, selon le cas, toute erreur dans cette page de manuel.

[Category:Awesome3](/Category:Awesome3 "wikilink")