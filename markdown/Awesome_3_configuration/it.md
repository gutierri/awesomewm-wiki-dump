---
title: Awesome 3 configuration it
permalink: /Awesome_3_configuration/it/
---

Ambiente
========

**awesome 3** utilizza un file di configurazione basato sul linguaggio [Lua](http://www.lua.org). Molte persone che utilizzavano awesome 2 piangeranno quando vedranno che il vecchio file in libconfuse è sparito, altri odieranno awesome 3 perchè non riescono a capire il file di configurazione [ion](http://modeemi.fi/~tuomov/ion/).

Perchè Lua? Perchè offre più controllo. Permette di controllare più aspetti: stavate cercando di far fare ad awesome qualcosa in un determinato evento, e non eravate in grado di farlo. Ora siete in grado di fare **qualsiasi cosa** (nel campo di quello che può fare un window manager).

File
====

-   ~/.config/awesome/rc.lua
-   /etc/xdg/awesome/rc.lua

Basi di Lua
===========

Abbiamo parlato di Lua; quindi prima di tutto imparate Lua. Non volete? Allora fate a meno di usare awesome 3 e smettete di leggere. (Oppure procuratevi il file di configurazione dall'archivio dei sorgenti o da qualcuno, e dategli una ritoccata, perchè non potete fare altro senza la conoscenza del lua).

Per le persone che stanno ancora leggendo, Lua è un linguaggio **semplice**. In altre parole, se non avete familiarità coi linguaggi di programmazione, i.e. se non conoscete *oggetti*, *metodi* e *parametri*, beh, vi troverete un poco spaesati leggendo questa pagina; andate a imparare un po' di basi della programmazione e tornate!

Non sto chiedendo geni dell'informatica: awesome 3 è ideato per le persone che hanno un minimo di *background* informatico. Comunque qualsiasi persona, se abbastanza motivata, può imparare le basi che servono a configurare e controllare awesome.

Il miglior modo per imparare Lua è quello di dedicare un paio d'ore alla lettura di [Programming In Lua](http://www.lua.org/pil/), che da un'anteprima di quello che è possibile realizzare in Lua. Non esitare ad aggiungere ai Preferiti una pagina così importante: parla di controllo del flusso, espressioni, ecc. Questo dovrebbe aiutare : è una pessima idea cercare a proposito della sintassi "for" quando stai scrivendo linee di codice.

Tipi di oggetti in awesome
==========================

Per le persone che non hanno mai usato awesome, qui sono spiegati gli oggetti messi a disposizione, che è necessario manipolare.

Screen
------

Non c'è un vero e proprio schermo in awesome, ma parleremo dopo degli oggetti di questo tipo. Uno *screen* è un monitor fisico collegato al vostro computer. Sono rappresentati da un indice che parte da 1.

Client
------

Un *client* è una finestra. Credo di essere stato chiaro.

Tag
---

Un *tag* è qualcosa come un workspace/desktop, anche se il concetto è meno rigido. Ogni client ha almeno un tag assegnato. Ogni screen ha almeno un tag. In qualsiasi momento, su uno screen, poteve vedere un qualsiasi numero di tag. Facendo a meno di osservare almeno un tag, nasconderai tutti i client. Osservando uno o più tag contemporaneamente, saranno visibili tutti i client che ogni tag possiede.

Widget
------

I *widget* sono oggetti che visualizzano qualcosa su uno screen. Ci sono molti tipi di widget. Per esempio c'è il tipo textbox che stampa un testo su uno screen. Ci sono anche i widget iconbox che visualizzano un'icona su uno screen.

Titlebar
--------

Una *titlebar* è una barra che è intorno ad un client e attaccata ad esso. Potete inserire dei widget in essa.

Statusbar
---------

Una *statusbar* è una barra fissata al bordo di uno screen. Anche qui, potete inserire dei widget all'interno.

Costruiamo l'interfaccia
========================

Per costruire la nostra interfaccia, dobbiamo usare le funzioni e i metodi messi a disposizione da awesome: **qualsiasi cosa** è riportata nella [Documentazione delle API Lua](http://awesome.naquadah.org/doc/api/) e nel manuale di awesomerc(5). Leggeteli. Rappresentano la documentazione alla quale dovete riferirvi. Vi ripeto: leggeteli; qualsiasi cosa è documentata in quella pagina di manuale o nella luadoc.

Quale pagina di manuale? ... awesomerc(5). Ottimo. Mi stavate seguendo.

Creare i tag
------------

Se avviate awesome senza esegure alcuna operazione con lua, vi ritroverete con 0 tag; questo è fatale. Awesome ha bisogno di almeno un tag.

Per prima cosa, dobbiamo creare un tag. Come possiamo farlo? Vediamo i tag() nella documentazione, e la funzione per creare tag nella pagina del manuale. Dice che il parametro della funzione dev'essere una tabella con almeno un nome come parametro. Facciamolo.

`mytagone = tag({ name = "one" })`

*mytag* è un nuovo oggetto di tipo tag, il suo nome è *one*. Se vogliamo impostargli un layout di default, dobbiamo aggiungere una chiava/valore all'interno della tabella, come spiegato nella documentazione.

`mytagtwo = tag({name = "two", layout = "floating" })`

Ora, noi abbiamo un secondo tag: il suo nome è *two*, e di default lui gestisce i client con lo schema floating, che come suggerisce il nome, rende i client liberi.

Creare oggetti è divertente, ma per il momento privo di utilità. Ricorda ogni screen deve avere almeno un tag. Quindi dobbiamo aggiungere questi tag che abbiamo creato al nostro primo screen. L'attributo *screen* di un tag lo fa per noi, dobbiamo solo assegnare il numero dello screen che desideriamo.

`mytagone.screen = 1`

Ora abbiamo aggiunto il tag allo screen \#1. Se abbiamo un secondo screen (multi-head: Zaphod, Xinerama o XRandR sono identici per awesome), dobbiamo aggiungere un secondo tag a questo screen:

`mytagtwo.screen = 2`

E se avessimo 3 screen, dobbiamo... beh, spero abbiate capito il concetto.

Se volete conoscere quanti screen avete, potete usare la funzione *screen.count()*.

Potete gestire la visualizzazione del tag con l'attributo *selected*. Se volete vedere il tag *one*:

`mytagone.selected = true`

Se volete nasconderlo, ovviamente dovete settare il valore su *false*.

Potete gestire tutte le impostazioni del tag con i suoi attributi. Guardate la documentazione,

Gestire i client
----------------

Qualche volta, vorrete prendere gli oggetti di tipo client. Sono veramente utili nelle funzioni hook, delle quali ne parleremo dopo.

Un client è un oggetto con dei metodi, come i tag e altro. Decidiamo che la variabile *c* è il nostro oggetto client. Per esempio, se vogliamo rendere un client libero dobbiamo usare l'attributo *floating* in questo modo:

`c.floating = true`

Alcuni metodi ritornano valori che possono essere usati dopo per delle analisi. Se vogliamo sapere se un client è libero possiamo prendere l'attributo *floating*:

`is_client_floating = c.floating`

La variabile *is_client_floating* ora contiene un valore booleano: *true* se il client è libero o *false* se non lo è.

Possiamo combinare queste 2 funzioni per creare una funzione: questa funzione imposterà lo stato libero del client su true se non lo è già, oppure su falso se lo è.

`function client_floating_toggle(c)`
`    if c then`
`        c.floating = not c.floating`
`    end`
`end`

Possiamo anche usare questa funzione per spostare un client su un determinato tag:

`function client_tag_with_tag_one(c)`
`    if c then`
`        local t = c:tags()`
`        table.insert(t, one)`
`        c:tags(t)`
`    end`
`end`

Creare i widget
---------------

I widget sono piccoli oggetti che possono essere disposti nelle titlebar o nelle statusbar. Come i tag, se non li aggiungete da qualche parte, sono completamente inutili. Per creare un widget usate la funzione *widget()*:

`mytextbox = widget({ type = "textbox", name = "mytextbox" })`

Ora *mytextbox* contiene un oggetto di tipo textbox. Potete usare diversi attributi con diversi tipi di widget, il prossimo inserisce un testo in una textbox.

`mytextbox.text = "Hello, world!`

Creare statusbar
----------------

Le statusbar sono contenitori di widget che possono essere disposti in alto, in basso, a sinistra o a destra di uno screen.

Primo, create la statusbar:

`mystatusbar = statusbar({ position = "top", name = "mystatusbar" })`

*mystatusbar* è una variabile che contiene un oggetto statusbar. Abbiamo dato a *position* il valore *top*, che posizionerà la barra in alto allo screen. Ovviamente, non l'abbiamo aggiunta allo screen, per ora è un oggetto inutile. Quindi facciamo:

`mystatusbar.screen = 1`

La nostra statusbar sarà ora visibile in alto allo screen. Bene, non fa niente ma è davvero utile. Aggiungiamo qualche widget!

`-- Crea una textbox`
`mytextbox = widget({ type = "textbox", name = "mytextbox" })`
`-- Imposta un testo nella textbox`
`mytextbox.text = "Hello, world!"`
`-- Crea una statusbar`
`mystatusbar = statusbar({ position = "top", name = "mystatusbar" })`
`-- Aggiunge le widgets alla statusbar`
`mystatusbar:widgets({ mytextbox })`
`-- Aggiunge la statusbar allo screen #1`
`mystatusbar.screen = 1`

E voilà. Ora abbiamo una statusbar nuova di zecca in alto allo schermo che visualizza *Hello, world!*. Usando questo metodo, potete settare un bel po' di widget sulla statusbar prima di avviare awesome, nel vostro file di configurazione.

Creare titlebar
---------------

Come detto prima, le titlebar sono come le statusbar ma le titlebar si trovano intorno ad una finestra. Per creare una titlebar, sapete cosa fare:

`mytitlebar = titlebar({ position = "top" })`

Quindi, probabilmente vorreste inserire dei widget all'interno. Funziona come nelle statusbar:

`mytitlebartitle = widget({ type = "textbox", name = "mytitlebartitle" })`
`mytitlebar:widgets({ mytitlebartitle })`

Una titlebar dev'essere aggiunta a un *oggetto client*. Possiamo ottenere la finestra in primo piano: è contenuta nella variabile *client.focus*. Facciamo questo:

`-- Prendi la finestra in primo piano`
`c = client.focus`
`-- Imposta una titlebar per quella finestra`
`c.titlebar = mytitlebar`

Ora, la finestra in primo piano ha una carina titlebar in alto! Ma non c'è nulla all'interno. No, aspetto! Ricorda, nelle titlebar noi possiamo sempre aggiungere una *widget di tipo textbox*. Possiamo ora stampare il nome del client su di essa:

`mytitlebartitle.text = c.name`

L'attributo *name* è una stringa che contiene il nome del client, e noi inseriamo il testo in una widget. Ora la titlebar visualizza il titolo del client.

Ricorda: una titlebar è un oggetto, e un client è un altro oggetto. Vuol dire che dovrai creare una titlebar per ogni client a cui vuoi assegnarla.

Usare gli hooks
---------------

Con quello che abbiamo visto prima, potete costruire una bella interfaccia. Comunque, immagine di voler aggiornare il titolo visualizzato nella titlebar di un client: non potete. Aspetta! Qui c'è la soluzione: gli hooks.

Con gli hooks, potete definire funzioni che sono richiamate al seguito di un evento. Provate con un esempio.

Quando un nuovo client appare nello screen, awesome esegue la funzione collegata *gestita* dall'hook. Questa funzione è chiamata passando il nuovo client come argomento. Questo è un esempio:

`function hook_manage(c)`
`    if c.name:find("mplayer") then`
`        c.floating = true`
`    end`
`end`

In primo luogo, abbiamo definito una funzione *hook_manage*, con un argomento chiamato *c*, che sarà il cliente. Quindi, cerchiamo di trovare la stringa *"mplayer"* nel nome del client, e se c'è, impostiamo lo stato libero al client.

` awful.hooks.manage.register(hook_manage)`

Poi, con quella chiamta, definiamo che la funzione chiamata per ogni nuovo client che arriva è *hook_manage*.

Ci sono molti hooks che potete usare e definire: focus, unfocus, manage, unmanage, mouseover, arrange, titleupdate, urgent, timer, … Consultate il manuale per altre informazioni.

Creare le combinazioni di tasti
-------------------------------

A volte può essere utile mappare alcune azioni ad una serie di tasti, che sono spesso più facili da premere rispetto Mod-shift-ctrl-x. Nell'esempio seguente è possibile digitare Ctrl-f e DOPO premere "a" invece di premere tutti i tasti nello stesso momento.

` -- Crea una lista di scorciatoie da tastiera che possono essere aggiunte quando viene premuto Ctrl-f`
` keybind_ctrl_f = {} `
`  `
` function keychain_ctrl_f_add() `
`     for k, v in pairs(keybind_ctrl_f) do `
`         v:add() `
`     end `
` end `
` `
` function keychain_ctrl_f_remove() `
`     for k, v in pairs(keybind_ctrl_f) do `
`         v:remove() `
`     end `
` end `
` `
` table.insert(keybind_ctrl_f, keybinding({}, "a", function () `
`     mytextbox.text = "You pressed a!"`
`     keychain_ctrl_f_remove() `
` end)) `
` `
` table.insert(keybind_ctrl_f, keybinding({ "Mod4" }, "b", function () `
`     mytextbox.text = "You pressed Mod4 + b!"`
`     keychain_ctrl_f_remove() `
` end)) `
` `
` table.insert(keybind_ctrl_f, keybinding({}, "Escape", keychain_ctrl_f_remove)) `
` `
` keybinding({ "Control" }, "f", keychain_ctrl_f_add):add()`

Eseguire comandi e script
-------------------------

Potete eseguire comandi e script inseriti nella configurazione base di lua. Qui un esempio di funzione che prende come parametro un comando e ritorna un risultato. Questa è utile per esempio per visualizzare qualcosa nelle textbox.

    -- Esegui il comando e ritorna il suo risultato. Probabilmente non volete eseguire
    -- comandi che hanno una sola linea come output
    function execute_command(command)
       local fh = io.popen(command)
       local str = ""
       for i in fh:lines() do
          str = str .. i
       end
       io.close(fh)
       return str
    end

Qui un esempio che inserisce /proc/loadavg in mytextbox:

     mytextbox.text = " " .. execute_command("cat /proc/loadavg") .. " "

Librerie Lua di awesome
=======================

awful
-----

[awful](/awful "wikilink") è la libreria predefinita per interagire con awesome, come le azioni del mouse o le scorciatoie da tastiera.

beautiful
---------

[Beautiful](/Beautiful "wikilink") è la libreria per gestire il tema di awesome.

eminent
-------

[Eminent](/Eminent "wikilink") è una libreria Lua che permette di gestire i tag dinamicamente. Con gestire i tag dinamicamente intendo dire che il numero di tag che avrete non è predefinito, potete assegnare specifici nomi ai tag, ma semplicemente andando nel prossimo tag o nell'ultimo potrete aggiungere un tag, permettendovi di adattarvi alla situazione senza cambiare la vostra configurazione per avere più o meno tag.

wicked
------

[Wicked](/Wicked "wikilink") è una libreria Lua per awesome che fornisce molti widget, come il widget per MPD, utilizzo della CPU, memoria, ecc.

[Category:Awesome3](/Category:Awesome3 "wikilink")