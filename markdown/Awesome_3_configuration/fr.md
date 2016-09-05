---
title: Awesome 3 configuration fr
permalink: /Awesome_3_configuration/fr/
---

Contexte
--------

Introduction de [Julien Danjou](/User:Jd "wikilink") :

« ***awesome 3** utilise maintenant un fichier de configuration fondé sur le langage [Lua](http://www.lua.org). La plupart des gens venant du monde d’awesome 2 risquent de râler car l’[ancien fichier de configuration](/Awesomerc/fr "wikilink") au format libconfuse a disparu, d’autres détesteront ça parce qu’ils n’ont jamais compris le fichier de configuration d’[ion](http://modeemi.fi/~tuomov/ion/), mais je l’ai fait quand même.*

*Pourquoi Lua ?*

''Parce qu’il offre plus de contrôle. Il offre un comportement conditionnel : avant, si on voulait qu’awesome fasse quelque chose quand il se passe quelque chose, on ne pouvait pas. Maintenant, on peut. On peut faire quasiment *tout* dans la liste des choses qu’un gestionnaire de fenêtres peut faire.'' »

Fichiers
--------

-   ~/.config/awesome/rc.lua
-   /etc/xdg/awesome/rc.lua

Bases en Lua
------------

On parle de Lua, alors on va d’abord apprendre le Lua. Si vous ne voulez pas, n’utilisez pas awesome 3 et arrêtez de lire maintenant. Ou alors, récupérez le fichier de configuration de l’archive **.tar** ou de quelqu’un, et bidouillez un peu ; ça devrait marcher même sans connaissance du langage Lua.

Pour ceux qui lisent encore, c’est super ! Lua est un langage *simple*. D’un autre côté, si vous n’êtes pas habitué du tout aux langages informatiques, *i. e.* si vous ne savez pas ce que sont des « objets », des « méthodes » et des « arguments », eh bien, vous serez perdus en lisant ce document, alors allez apprendre les bases et revenez !

C’est un peu élitiste : awesome 3 est conçu pour les utilisateurs avancés avec un minimum de connaissances informatiques, mais si vous êtes vraiment motivé, ce n’est pas pour autant que vous ne pourrez pas apprendre les bases pour configurer et contrôler awesome.

La meilleure façon d’apprendre le Lua est de passer une heure ou deux à lire le livre *[Programming In Lua](http://www.lua.org/pil/)*, qui est bien fait et qui vous donnera un aperçu de ce qu’il est possible de faire en Lua. N’hésitez pas à faire des marque-pages sur les notions importantes de *flow control*, d’*useful statements*, etc. Ça vous aidera : c’est toujours horrible de chercher la syntaxe d’une boucle « for » quand on essaie encore de taper des lignes de code.

Types d’objets awesome
----------------------

Pour les gens qui viennent du fin fond de l’espace intersidéral et qui n’utilisaient pas awesome, voici les objets de bases qu’awesome vous propose et que vous allez devoir manipuler très bientôt :

Écran
Il n’y a pas de vrai objet « écran » dans awesome, mais on parlera des écrans plus tard. Un écran est un moniteur physique branché à votre ordinateur. Ils sont représentés par leurs numéros, commençant à 1.

Client
Un client est une fenêtre. Ça doit être suffisamment clair.

Onglet
Un onglet est en gros comme un bureau ou un bureau virtuel, mais le concept est moins rigide.

Chaque client a au moins un onglet qui lui est assigné. Chaque écran a au moins un onglet.

À tout moment, sur un écran, vous pouvez voir n’importe quel onglet numéro untel. Ne regarder aucun onglet revient à cacher tous les clients. Regarder un ou plusieurs onglets revient à voir tous les clients qui ont ces onglets.

Widget
Les widgets sont des objets qui affichent des trucs sur l’écran. Il y a de nombreux types de widgets. Par exemple, il y a un widget « boîte de texte » qui affiche du texte à l’écran. Il y a aussi un widget « boîte à icone » qui affiche une icône à l’écran.

Barre de titre
Une barre de titre est une barre qui se trouve autour d’un client et qui lui est attaché. Vous pouvez mettre des widgets dessus.

Barre de statut
Une barre de statut est une barre qui est fixée au bord d’un écran. Vous pouvez mettre des widgets dessus.

Construisons notre interface
----------------------------

Pour construire notre interface, on doit utiliser les fonctions et méthodes d’awesome : *tout* est documenté dans la [documentation de l’interface de programmation (API) de Lua](http://awesome.naquadah.org/doc/api/) et dans la page de manuel **awesomerc(5)**. Lisez-les. Elles sont vos références de développement. À nouveau : lisez-les, tout est documenté dans cette page de manuel et la doc de Lua.

C’est quelle page de manuel, déjà ? … **awesomerc(5)**. Bien. Vous suivez.

*Voir aussi : [Pages de manuel](/Man_pages/fr "wikilink")*

### Création d’onglets

Si vous lancez awesome sans aucune opération Lua, vous n’aurez aucun onglet, ce qui est critique. awesome veut au moins un onglet.

Donc d’abord, on doit créer un onglet. Comment on fait ça ? On cherche « *tag()* » et sa documentation dans la page de manuel : c’est la fonction de création d’onglets. Ça dit que le paramètre de la fonction doit être un tableau avec au moins un attribut de nom. Faisons ça :

`mytagone = tag({ name = "one" })`

Là, on a déclaré un objet, « *mytagone* », qui est un objet « onglet », et son nom est « *one* ». Si on veut lui donner la disposition par défaut, on la rajoute sous la forme d’une paire de valeurs ou clés dans le tableau, comme expliqué dans la documentation.

`mytagtwo = tag({name = "two", layout = "floating" })`

Maintenant, on a un deuxième objet onglet, « *mytagtwo* » : son nom est « *two* », et par défaut il arrange les clients avec l’algorithme flottant, qui, comme son nom l’indique, laisse les clients flotter.

Créer des objets, c’est sympa, mais bon, pour le moment ça sert à rien. Souvenez-vous : chaque écran a au moins un onglet. Donc on doit ajouter les onglets qu’on vient de créer à notre premier écran. L’attribut *screen* de l’objet onglet fait ça pour nous : on a juste à lui donner le numéro de l’écran.

`mytagone.screen = 1`

On vient d’ajouter cet onglet à l’écran nº1. Si on a un deuxième écran (multi-écran : Zaphod, Xinerama ou XRandR sont identiques pour awesome), on peut ajouter notre deuxième onglet à cet écran :

`mytagtwo.screen = 2`

Et si on a trois écrans, on doit… Eh bien, vous avez sans doute déjà trouvé.

Si vous voulez savoir combien d’écrans vous avez, vous pouvez utiliser la fonction *screen.count()*.

Vous pouvez manipuler l’affichage d’un onglet avec l’attribut *selected*. Si vous voulez voir votre onglet « *one* » :

`mytagone.selected = true`

Si vous voulez le cacher, vous pouvez évidemment affecter la valeur à *false*.

Vous pouvez manipuler tous les paramètres de l’onglet avec ses attributs. Regardez dans la documentation.

### Manipulation des clients

Parfois, vous aurez des objets « client ». C’est très utile dans les fonctions *hook*, dont on parlera plus tard.

Un client est un objet qui a des méthodes lui aussi, comme les onglets et les autres. Décidons que la variable *c* est notre objet client.

Par exemple, si on veut qu’un client soit flottant, on peut utiliser l’attribut *floating* de cette façon :

`c.floating = true`

Certaines méthodes retournent des valeurs qui peuvent être utilisées plus tard pour analyse. Si l'on veut savoir si un client est flottant, on peut récupérer l’attribut *floating* :

`is_client_floating = c.floating`

La variable *is_client_floating* contient maintenant un booléen : *true* si le client flotte ou *false* sinon.

On peut combiner ces deux fonctions pour créer une fonction de bascule : cette fonction mettra l’état flottant du client à *true* s’il ne l’est pas, et à *false* si l’est.

`function client_floating_toggle(c)`
`    if c then`
`        c.floating = not c.floating`
`    end`
`end`

On peut aussi utiliser ça pour basculer un onglet à notre client :

`function client_tag_with_tag_one(c)`
`    if c then`
`        local t = c:tags()`
`        table.insert(t, one)`
`        c:tags(t)`
`    end`
`end`

### Création de widgets

Les widgets sont de petits objets qui peuvent être placés soit sur une barre de titre, soit sur une barre de statut. Comme pour les onglets, si vous ne les ajoutez nulle part, ils servent complètement à rien. Pour créer un widget, on utilise la fonction *widget()* :

`mytextbox = widget({ type = "textbox", name = "mytextbox" })`

*mytextbox* contient maintenant un objet « widget ». Vous pouvez utiliser différents attributs avec différents types de widgets ; ce qui suit paramètre le texte affiché par la boîte textuelle.

`mytextbox.text = "Hello, world!"`

### Création de barre de statut

Les barres de statut sons des conteneurs à widgets que vous pouvez mettre sur les bords haut, bas, gauche ou droite de l’écran.

D’abord, on crée une barre de statut :

`mystatusbar = statusbar({ position = "top", name = "mystatusbar" })`

*mystatusbar* est maintenant une variable qui contient un objet « barre de statut ». Comme on a mis la valeur de *position* à *top*, la barre sera placée en haut de l’écran. Évidemment, on ne l’a pas ajoutée à l’écran, donc elle ne sert à rien pour le moment. Réglons ça :

`mystatusbar.screen = 1`

Notre barre de statut est maintenant visible en haut de l’écran. Mais bon, elle ne fait rien, donc elle n’est pas très utile… Ajoutons des widgets !

`-- Crée une boîte textuelle`
`mytextbox = widget({ type = "textbox", name = "mytextbox" })`
`-- Paramètre le texte de la boîte textuelle`
`mytextbox.text = "Hello, world!"`
`-- Crée une barre de statut`
`mystatusbar = statusbar({ position = "top", name = "mystatusbar" })`
`-- Ajoute des widgets à la barre de statut`
`mystatusbar:widgets({ mytextbox })`
`-- Ajoute la barre de statut à l’écran nº1`
`mystatusbar.screen = 1`

Et *that’s all, folks!*. On a maintenant une barre de statut toute neuve en haut de l’écran, qui affiche « *Hello, world!* ». De cette façon, vous pouvez mettre en place une barre de statut avec de nombreux widgets avant de lancer awesome, dans votre fichier de configuration.

### Création de barre de titre

Comme dit plus tôt, les barres de titre sont comme des barres de statut, sauf qu’elles sont autour d’une fenêtre. Pour créer une barre de titre, c’est toujours la même rengaine :

`mytitlebar = titlebar({ position = "top" })`

Ensuite, vous voulez sans doute mettre des widgets dessus. Ça marche comme pour les barres de statut :

`mytitlebartitle = widget({ type = "textbox", name = "mytitlebartitle" })`
`mytitlebar:widgets({ mytitlebartitle })`

Une barre de titre doit être ajoutée à un objet « client ». On peut obtenir le client qui est actuellement au premier plan : c’est stocké dans la variable *client.focus*. C’est parti :

`-- On récupère le client au premier plan`
`c = client.focus`
`-- On met une barre de titre dessus`
`c.titlebar = mytitlebar`

Maintenant, le client au premier plan a une barre de titre sympa au-dessus de lui ! Mais il n’y a rien dessus. Ah si, attendez ! Vous vous souvenez, on a déjà ajouté un widget « boîte textuelle » dans la barre de titre. On peut donc maintenant afficher le nom du client dedans :

`mytitlebartitle.text = c.name`

L’attribut *name* est une chaîne de caractères contenant le titre du client, et on l’a mis dans le texte du widget. Maintenant, la barre de titre affiche le titre du client.

Souvenez-vous : une barre de titre est un objet, et un client est un autre objet. Donc on doit créer une barre de titre pour chaque client pour lequel vous voulez une barre de titre autour.

### Utiliser des *hooks*

Avec ce qu’on vient de voir, vous pouvez construire une interface sympa. Mais imaginez maintenant que vous voulez mettre à jour le titre qui s’affiche dans la barre de titre d’un client : vous ne pouvez pas. Attendez ! Voilà la solution : les *hooks*.

Avec les *hooks*, vous pouvez définir des fonctions qui sont appelées quand un évènement se produit. Essayons sur un exemple simple.

Quand un nouveau client arrive sur l’écran, awesome lance la fonction liée au *hook* « *manage* ». Cette fonction est appelée avec le nouveau client comme argument. Voici un exemple :

`function hook_manage(c)`
`    if c.name:find("mplayer") then`
`        c.floating = true`
`    end`
`end`

D’abord, on définit une fonction *hook_manage*, avec un argument qui s’appelle *c*, qui sera le client. Ensuite, on essaie de trouver la chaîne « mplayer » dans le nom du client, et si c’est le cas, on le rend flottant.

` awful.hooks.manage.register(hook_manage)`

Ensuite, avec cet appel, on définit que la fonction qui doit être appelée à l’arrivée de chaque client est *hook_manage*.

Il y a de nombreux *hooks* que vous pouvez utiliser et définir : focus, unfocus, manage, unmanage, mouseover, arrange, titleupdate, urgent, timer, etc. Reportez-vous à la page de manuel pour plus d’information.

### Exécuter des commandes et des scripts

Vous pouvez exécuter des commandes et des scripts depuis la configuration de base de Lua. voici un exemple de fonction qui prend une commande en paramètre et renvoie sa sortie. C’est très utile par exemple pour mettre des sorties de commandes dans des boîtes textuelles.

`-- Exécute « command » et renvoie sa sortie. On ne va sans doute pas`
`-- exécuter seulement des commandes avec une seule ligne de sortie.`
`function execute_command(command)`
`   local fh = io.popen(command)`
`   local str = ""`
`   for i in fh:lines() do`
`      str = str .. i`
`   end`
`   io.close(fh)`
`   return str`
`end`

Voici un exemple où l’on met **/proc/loadavg** dans *mytextbox* :

`mytextbox.text = " " .. execute_command("cat /proc/loadavg") .. " "`

Bibliothèques Lua d’awesome
---------------------------

[Awful](/Awful "wikilink")
La bibliothèque par défaut pour interagir avec awesome, comme les actions avec la souris ou les touches du clavier.

[Beautiful](/Beautiful/fr "wikilink")
Bibliothèque pour changer le thème d’apparence d’awesome.

[Eminent](/Eminent "wikilink")
Bibliothèque Lua qui vous permet de faire des onglets dynamiques. Les onglets dynamiques, dans ce contexte, signifie que le nombre d’onglets que vous avez n’est pas prédéfini ; vous pouvez donnez des noms d’onglets spécifiques, mais encore mieux par exemple : simplement aller sur l’onglet suivant quand vous êtes sur le dernier onglet vous fournit un onglet tout neuf, ce qui vous permet de vous adapter à de nombreuses situations sans changer votre fichier de configuration pour ajouter ou enlever un onglet. *Cette bibliothèque est obsolète : Shifty ci-dessous est plus flexible est plus à jour*

[Shifty](/Shifty/fr "wikilink")
Bibliothèque à jour de gestion d’onglets dynamiques et de clients.

[Naughty](/Naughty/fr "wikilink")
Bibliothèque de notification.

[Wicked](/Wicked/fr "wikilink")
Bibliothèque Lua pour awesome, fournissant plus de widgets ; par exemple des widgets MPD, l’utilisation du processeur, de la mémoire, etc.

[Category: Awesome3](/Category:_Awesome3 "wikilink")