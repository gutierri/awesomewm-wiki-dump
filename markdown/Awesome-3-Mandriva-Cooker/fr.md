---
title: Awesome-3-Mandriva-Cooker fr
permalink: /Awesome-3-Mandriva-Cooker/fr/
---

Pour avoir awesome 3 sur Mandriva Cooker, vous n’avez qu’à recompiler Cairo avec l’argument *--enable-xcb* et à installer awesome 3.3.

En supposant que vos miroirs sont correctement configurés, vous aurez les dépendances nécessaires pour installer les **.rpm** des deux éléments cités ci-dessus :

-   <http://kenobi.mandriva.com/~shikamaru/cairo-xcb/>
-   <http://kenobi.mandriva.com/~shikamaru/awesome/>

Si Cairo est déjà installé sur votre système, vous pouvez exécuter :

`rpm -e --nodeps libcairo2`

en remplaçant « libcaro2 » par « lib64cairo2 » si vous êtes sur une architecture x86_64.

Il est également mieux de mettre */cairo/* dans votre fichier **/etc/urpmi/skip.list** pour que cette version de Cairo ne soit pas écrasée par une éventuelle mise à jour.