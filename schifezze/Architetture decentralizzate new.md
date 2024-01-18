# sistemi P2P
I sistemi peer-to-peer P2P sono un'insieme di sistemi e applicazioni che usano delle risorse distribuite su una larga scala geografica per eseguire delle funzioni in modo decentralizzato. Spesso queste risorse sono disponibili su nodi che si trovano ai bordi della rete Internet.
La caratteristica delle risorse distribuite che compongono la rete p2p è che sono risorse condivise, in particolare si condividono file, spazio di storage, capacità di computazionale e banda.
Dunque l'idea alla base delle reti p2p è quella di dare e ricevere risorse da una comunità di nodi della rete p2p indipendenti detti ***peer***, localizzati tipicamente ai bordi della rete.
Questi sono dei peer proprio perché sono pari tra di loro: nei ruoli, nei privilegi e nelle responsabilità. Esistono dei sistemi p2p ibridi in cui alcuni nodi sono dei *superpeer*, cioè hanno più funzionalità rispetto ai semplici peer.
La caratteristica principale che identifica le reti p2p è la totale assenza di un controllo centralizzato: i peer si comportano sia come client che come server, dunque ricevono e forniscono risorse e servizi.
Infine, i sistemi p2p sono altamente distribuiti: queste reti possono essere composte da centinaia di migliaia di nodi, per cui si ottiene naturalmente un alto grado di ridondanza delle informazioni. Inoltre, queste reti sono altamente dinamiche: la loro struttura e il numero di nodi cambia costantemente nel tempo, dato che i nodi possono entrare e uscire liberamente.

La killer application delle reti p2p è la condivisione di contenuti e storage come file e streaming di video: eMule, gnutella, bittorrent,...
Altri usi delle reti p2p sono per la condivisione di risorse computazionali, per la telefonia audio/video e nelle blockchain e cryptovalute.

Gli aspetti difficili da gestire nei sistemi p2p sono:
- L'eterogeneità dei peer: essendo migliaia, i nodi possono essere molto diversi tra loro in termini di hardware e software;
- Garantire una scalabilità efficiente, facendo in modo che non ci siano colli di bottiglia;
- Come gestire aspetti di località dei nodi, ad esempio la locazione dei dati;
- Tolleranza ai guasti: capacità di mascherare i fallimento;
- Performance: come localizzare velocemente le risorse nella rete?
- Evitare il Free-riding, cioè evitare di avere nella rete quei nodi che ricevono senza dare nessun contributo alla rete;
- Privacy e anonimato: onion routing per le comunicazioni anonime;
- Gestione della fiduca e reputazione tra i peer: i nodi non si conoscono tra di loro, come possono fidarsi l'uno dell'altro?
- Attacchi di rete;
- Resilienza al fenomeno Churn, per cui i peer della rete entrano e escano liberamente dalla rete, quindi le risorse sono dinamicamente aggiunte e rimosse. Come fa il sistema a garantire l'operabilità del servizio?

In questa trattazione, il focus sarà su un sistema p2p di file sharing.
Le operazioni fondamentali di ciascun peer per poter condividere file nella rete sono:
1. **Bootstrap**: come un nuovo peer che vuole entrare nel sistema p2p scopre delle informazioni di contatto su altri peer che fanno già parte della rete (aka come un nuovo peer può entrare nella rete). Le soluzioni sono molteplici: usando una configurazione statica, oppure tenendo traccia dei nodi già esistenti nella rete (devo aver fatto parte della rete almeno una volta), oppure collegandosi ad un insieme di nodi ben noti all'esterno e sempre presenti nella rete. Non ci occupiamo di tale aspetto in modo approfondito;
2. **Ricerca delle risorse**: come localizzare le risorse nella rete p2p, si affronterà in modo approfondito;
3. **Acquisizione delle informazioni**: come ottenere le risorse localizzate all'interno della rete
## Reti di Overlay
Le reti che compongono i sistemi p2p sono comunemente dette reti logiche o *overlay network*.
Un *overlay network* è una rete logica che interconnette i peer che fanno parte del sistema. In particolare, è un astrazione di livello applicativo, dove i collegamenti di interconnessione sono *logici*, cioè non corrispondono ai collegamenti fisici: non c'è corrispondenza diretta tra la rete logica e la rete fisica sottostante.
La rete logica mette a disposizione un servizio di location che permette di fare il routing delle richieste delle risorse e di identificare chi sono i peer che le forniscono.
![[Pasted image 20231202120920.png]]
### Overlay routing
Con overlay routing, si intende la capacità del sistema p2p di trovare i percorsi per raggiungere le risorse condivise. A differenza del routing tradizionale, non si basa su indirizzi di rete dei nodi, ma riguarda le *risorse*.
Nel corso della trattazione verrà approfondita solo la parte di *routing* e non la parte di interazione tra i peer per ricevere le risorse localizzate.
### Altri servizi dell'overlay network
Oltre a fornire il routing delle risorse richieste, l'overlay network deve garantire le operazioni di inserimento, cancellazione e identificazione di nodi e risorse nella rete.
Le risorse vengono tipicamente identificate attraverso degli identificativi globali detti *Global Unique IDentifier (GUID)*, ottenuti applicando una funzione hash crittografica a una parte dello stato della risorsa (dunque a delle metainformazioni delle risorse).
Anche l'identificazione dei peer in modo univoco avviene per mezzo di funzioni hash crittografiche.
## Classificazione delle reti di overlay
La gestione delle risorse e dei nodi nelle reti p2p dipende fortemente dalla tipologia di overlay network adottata dal sistema. Le reti di overlay possono essere di due tipi: **strutturate** o **non strutturate**.
In una rete non strutturata, non c'è una struttura regolare e organizzata della rete.
In una rete strutturata, c'è una struttura ben definita della rete.
La presenza o meno di una struttura ben definita ha un impatto fondamentale su come avviene il routing: se non ho una struttura, si dovrà cercare la risorsa inondando la rete di messaggi; se la rete è strutturata, si potrà sfruttare la struttura della rete per trovare velocemente la risorsa, senza inondare la rete di messaggi.
### Overlay network non strutturate
In un sistema con overlay network non strutturato, la rete dei nodi viene costruita in modo randomico: si può rappresentare come un grafo aleatorio.
I nodi si collegano in modo arbitrario: ogni peer entra nella rete seguendo delle semplici regole locali, contattando un insieme di nodi vicini selezionati secondo una certa modalità.
Infine, non c'è controllo su dove le risorse vengono localizzate sui nodi.
L'obiettivo delle reti non strutturate è gestire delle reti in cui i nodi hanno un comportamento altamente dinamico: entrano ed escono spesso dalla rete.
I vantaggi di una rete non strutturata sono: la semplicità di manutenzione della rete, dato che l'inserimento e la cancellazione di nodi e risorse viene gestita in modo semplice e l'alta resilienza di tali reti in virtù della semplice gestione - difficilmente queste reti si rompono.
Lo svantaggio principale è l'alto costo di lookup delle risorse: localizzare le risorse risulta complicato per la mancanza di struttura (costo lineare nel numero di nodi).

Le reti non strutturate si possono classificare in base alla distribuzione dell'*indice risorse-peer*: è un indice che tiene traccia delle risorse possedute da ciascun nodo.
Questo indice può essere realizzato in tre diverse forme:
- **centralizzato**: un server mantiene completamente questo indice. Per fare lookup di una risorsa, bisogna contattare questo server centralizzato che ci fornisce la lista dei peer che possiedono la risorsa. In questa caso si parla di overlay network non strutturato e centralizzato (centralized unstructured network);
- **decentralizzato**: distribuire completamente questo indice tra tutti i peer che fanno parte della rete. In questo caso si parla di overlay network non strutturato e decentralizzato (decentralized unstructured network);
- **ibrido**: l'indice è distribuito tra un numero limitato di nodi, detti *superpeer*. In questo caso si parla di overlay netwrok non strutturato e semi-centralizzato (Hybrid unstructured network).
#### Centralized unstructured overlay
Nella unstructured overlay *centralizzata* è presente un'unico server, detto *directory server*, che mantiene l'indice peer-risorsa. Quando si vuole fare il lookup di una risorsa, si deve contattare il directory server fornendogli il nome della risorsa cercata; il server risponderà con la lista di tutti i peer che mantengono tale risorsa.
I vantaggi di avere un directory server centralizzato sono: la semplicità del lookup, dato che la ricerca è centralizzata su un singolo server, e il fatto che la risposta fornita dal directory server è definitiva, dato che conosce perfettamente le associazioni risorsa-peer.
Gli svantaggi comprendono la maggiore difficoltà di gestione di una directory centralizzata nel momento in cui risorse e peer sono in grande quantità, e il fatto che un singolo directory server agisce da bottleneck di prestazioni, oltre ad essere un single point of failure, sia tecnico che legale.
#### Decentralized unstructured overlay
Nella unstructured overlay *decentralizzata* si ha un'approccio completamente decentralizzato al lookup delle risorse, dunque ciascun peer ha solamente una visione locale della posizione delle risorse. Per fare il lookup delle risorse in queste reti, esistono diversi meccanismi. In particolare, si vedono il query flooding e il random walk.
##### Query Flooding
Quando un peer deve cercare una risorsa, invia una richiesta di ricerca della risorsa ai suoi peer vicini nell'overlay network. Ognuno di questi peer: risponde alla richiesta se possiede la risorsa, altrimenti la inoltra ai suoi peer vicini (escludendo il vicino dal quale ha ottenuto la richiesta). In questo modo la query si propaga e inonda la rete.
Per evitare che la query si propaghi all'infinito nella rete, si inserisce un meccanismo di TTL (Time to live) alla richiesta, in modo tale che dopo un certo numero di ritrasmissioni del messaggio, esso non venga più inoltrato.
Un altra ottimizzazione è quella di evitare percorsi ciclici: si può evitare assegnando a ciascuna query un id univoco, in modo tale che se un peer vede, tramite l'id, una richiesta più di una volta non la inoltra nuovamente.
Il costo di lookup tramite query flooding è $O(N)$ dove, $N$ è il numero di peer della rete.
![[Pasted image 20231202132254.png]]
Una volta individuato il peer che mantiene la risorsa richiesta, si hanno due opzioni per far conoscere questo peer al richiedente originale: contattandolo direttamente, facendo dunque un routing diretto tra il peer che mantiene la risorse e il peer che ha generato la richiesta; oppure facendo *backward routing*, per cui la risposta viene mandata tramite lo stesso percorso che ha seguito la richiesta a partire dal peer che l'ha generato.
Il backward routing ha il vantaggio che la risposta alla richiesta, dunque su quale nodo è localizzata una risorsa, viene recepita da tutti i nodi del percorso originale: questi peer possono tenere in cache tale risposta per rispondere più velocemente ai peer che fanno la query della stessa risorsa (tra cui loro stessi!).
Il query flooding porta con se una serie di svantaggi:
- **overhead di comunicazione**: infatti vengono inviati un gran numero di messaggi, e la maggior parte di essi sono inutili.
- **Alto costo di lookup**: lineare nel numero di peer, che possono essere centinaia di migliaia. Inoltre, settare un TTL non è risolutivo: qual'è il valore giusto per raggiungere la risorsa senza mandare *troppi* messaggi?
- **Falsi negativi**: se il valore di TTL è troppo basso, potrebbe accadere che tutti i nodi che mantengono la risorsa cercata non vengano raggiunti: nonostante la risorsa sia presente nella rete, la richiesta risponde che la risorsa non esiste.
- Relazione tra rete fisica e di overlay: infatti, l'algoritmo considera i vicini nella rete di overlay, ma non è detto che questi siano vicini nella rete fisica: potrebbero essere molto lontani fisicamente.
##### Random walk
Il meccanismo random walk standard prevede che il peer che vuole localizzare una risorsa, invia la richiesta ad uno dei suoi vicini scelgo randomicamente. Se questo peer vicino mantiene la risorsa risponde alla richiesta, altrimenti inoltra la richiesta ad un suo peer vicino scelto randomicamente. La procedura continua in questo modo fino a che non viene trovata la risorsa.
Rispetto al query flooding, si inviano molti meno messaggi e quindi si avrà minor overhead di comunicazione. Ma il fatto di inoltrare la richiesta ad un solo peer ogni volta, rende il tempo di lookup maggiore.
Per decrementare il tempo di lookup, si può pensare di inoltrare la query verso $k$ random walk simultaneamente. Inizialmente, si scelgono randomicamente $k$ peer vicini a cui inoltrare la richiesta, i quali inoltreranno la richiesta secondo la procedura standard del random walk. 
### Overlay network strutturato
Nelle reti di overlay strutturate, un peer può inoltrare le richieste di lookup usando un *certo insieme di informazioni* ben note riguardo gli altri peer della rete, date proprio dalla sua *natura strutturata*.
La struttura è data da certi vincoli posti su come le risorse e i peer si posizionano all'interno della rete, dando vita ad una **topologia** della rete di overlay: ad albero, ad anello, ipercubo o griglia.
Dunque l'obiettivo delle reti strutturate è quello di migliorare la scalabilità della rete, migliorando il tempo di lookup e riducendo il numero di richieste trasmesse nella rete (minor overhead di comunicazione) rispetto alle reti non strutturate.
In queste reti, il lookup delle risorse è implementato per mezzo di una ricerca **basata su chiave**, dunque le ricerche sono basate su un matching esatto delle risorse, possibile grazie alla struttura della rete che rende il *costo di lookup limitato*.
Lo svantaggio principale nelle reti strutturate è la maggior complessità nella gestione delle operazioni di inserimento e abbandono dei peer, dato che al variare dei nodi della rete bisogna mantenere la struttura. Anche questa complessità può essere però limitata.
#### Idee di base
- Ciascun peer è responsabile di un **sottoinsieme di risorse** e ha una conoscenza degli altri peer che *dipende* dalla struttura della rete;
- Ad ogni **risorsa** è assegnato un'**identificativo unico e globale (GUID)**;
- Ad ogni **peer** è assegnato un'**identificativo unico e globale (GUID)**;
- Queste GUID sono calcolate usando una **funzione hash crittografica**;
- Viene usato lo **stesso spazio di identificatori** sia per i peer, sia per le risorse: ciascun peer sarà responsabile delle risorse a lui *vicine* nello spazio di identificatori;
- Le richieste di lookup sono instradate verso i peer che hanno identificativo *vicino* all'identificativo delle risorse, dove la vicinanza dipende da una qualche *metrica di distanza*.
Il routing è basato sulle **Hash Table Distribuite (DHT)**: una hash table chiave-valore distribuita.
#### Distributed hash table
Una **Hash Table Distirbuita (DHT)** è una astrazione distribuita di una hash table classica che mappa delle chiavi a dei valori.
In una DHT, il lookup di una chiave avviene in modo simile ad una hash table classica: la chiave di una risorsa viene mappata ad un bucket contenente tale risorsa. La differenza sostanziale è che i bucket di una DHT sono **distribuiti** su molteplici peer, dunque nell'operazione di lookup con una certa chiave bisogna *trovare il peer che mantiene il bucket corrispondente*.
Dunque una DHT mantiene un'insieme di coppie (chiave $K$, valore $V$), dove $K$ rappresenta la chiave che identifica la risorsa contenuta in $V$ corrisponde al GUID della risorsa, ed espone le seguenti operazioni:
- `get(K)`: data una chiave $K$, restituisce il valore $V$ associato, prendendolo dal nodo che mantiene tale coppia;
- `put(K,V)`: inserisce nella DHT il valore $V$ nel nodo responsabile per la chiave (o identificativo di $V$) $K$;
- `remove(K):` rimuove la chiave $K$ e il valore $V$ associato.
![[Pasted image 20231202165217.png]]
Nel progettare una DHT si incontrano diversi ostacoli di progettazione;
- una DHT deve essere **decentralizzata**, dunque non deve esistere una autorità centrale che ha conoscenza completa della hash table;
- una DHT deve essere **scalabile** ed **efficiente**, dunque *veloce nelle operazioni di lookup* mantenendo un basso overhead di rete;
- una DHT deve essere **dinamica**, cioè in grado di gestire l'uscita e l'ingresso dei nodi: deve essere in grado di ridistribuire in modo opportuno le risorse tra i nodi, rimappando unicamente le risorse di un nodo uscente in (sperabilmente) un solo nodo e rimappando le risorse di un solo nodo nel caso di un nuovo nodo entrante.
##### Progettazione di una DHT
Il primo passo nella progettazione di una DHT è scegliere uno spazio di indirizzamento con cui mappare sia le risorse che i nodi, tramite una funzione hash crittografica; verrà applicata ai metadati delle risorse e dei nodi.
Dopodiché bisogna partizionare le risorse tra i nodi della rete. Ogni nodo gestisce una porzione delle risorse (o informazioni sulle risorse) memorizzate nella DHT: ad ogni nodo è assegnata una porzione contigua di chiavi e mantiene le informazioni sulle risorse mappate nella porzione di chiavi che gestisce.
Infine, il routing nella DHT deve avvenire in modo che data una risorsa con chiave $K$ venga assegnata al peer con identificatore *più vicino* alla $K$. Nel caso di Chord, questo peer sarà quello che gestisce la risorsa.
##### Problemi relativi alle DHT
Le accortezze da gestire nella progettazione di una hash table distribuita sono:
- Evitare che alcuni nodi gestiscano troppe risorse, distribuendo in maniera uniforme il numero di chiavi di cui ogni nodo è responsabile;
- Evitare il remapping di tutte le chiavi quando un peer entra o esce dalla rete.
Inoltre, bisogna considerare che le DHT cosi progettate supportano in modo efficiente solamente le **ricerche esatte**: dato che una risorsa è identificata unicamente dalla sua chiave, per fare il lookup di tale risorsa è necessario conoscere la chiave; dunque è facile fare query per matching esatto, ad esempio per nome della risorsa, più difficile e costoso implementare query per wildcard o range.
## Sistemi P2P basati su DHT: Chord
I sistemi P2P basati su DHT sono caratterizzati da essere **altamente scalabili** rispetto alla dimensione del sistema. Si indicherà con $N$ il numero di peer nella rete, che ne determinano la dimensione.
I diversi sistemi che usano DHT differiscono principalmente dallo spazio di indirizzamento (dunque dalla *topologia della rete*) e dalla selezione dei peer con i quali comunicare (cioè la *metrica di distanza* tra i peer).
In questa trattazione si esaminerà il protocollo detto **Chord**.
### Caratteristiche principali
Nel protocollo chiamato **Chord**, la rete viene strutturata su un anello che percorribile in senso *unidirezionale* (ad esempio, solo senso orario).
Lo spazio degli identificatori è dato da tutti i punti dell'anello considerandoli su una linea retta (si capisce bene dalla figura sotto, nella quale i punti tratteggiati sono risorse mentre i punti spessi sono peer).
Nodi e le risorse vengono mappati in questo spazio di indirizzamento usando il *consistent hashing*.
Ogni peer nella rete Chord è responsabile delle risorse associate alle chiavi comprese tra il suo identificativo e l'identificativo del peer che lo precede (nel senso di percorrenza dell'anello): in figura, il predecessore di 4 è 1, di 1 è 15, ecc.
Quindi, una risorsa con identificatore $K$ è gestito dal peer che ha l'identificatore più piccolo e maggiore o uguale a $K$: questo peer è detto **successore** della chiave $K$ - $\text{succ}(K)$, ed è il primo nodo che segue tale risorsa.
Dunque
$$\text{succ}(1) = 1, \quad \text{succ}(9)=12, \quad \text{succ}(13)=15.$$
La metrica di distanza è basata sulla differenza lineare tra gli identificatori.
![[Pasted image 20231202185625.png]]
### Consistent hashing
Il **consistent hashing** è una particolare tecnica di hashing per cui sia gli oggetti (le risorse) che i bucket (i peer) vengono **mappati uniformemente** nello **stesso spazio di identificatori** (l'anello) usando una funzione hash crittografica. Inoltre, ogni nodo **gestisce un'intervallo consecutivo** di chiavi hash, non delle chiavi sparse.
Il consistent hashing è *parte integrante di Chord* e ne garantisce *robustezza e performance elevate*:
1. Nel caso di ridimensionamento della DHT (con aggiunta o rimozione di un bucket) il mapping della maggior parte delle risorse rimarrà invariato. Quando un nodo abbandona la rete, solo le chiavi gestite da tale nodo che vengono rimappate; quando un nodo entra nella rete, solo le chiavi  del peer con identificatore più vicino a quello di tale nodo vengono rimappate.
![[Pasted image 20231202225931.png]]
2. Tutti i bucket mantengono *circa lo stesso numero di chiavi*: garantisce *bilanciamento del carico* tra i nodi della rete. 
### Routing in Chord
Per fornire una operazione di lookup efficiente, il routing in Chord si basa sul fatto che ogni peer mantiene una lista parziale di peer che sono progressivamente più distanti. Questa tabella è detta **finger table** e permette di implementare l'operazione di lookup con una complessità $O(\log{N})$.
#### Finger Table
Ogni peer della rete mantiene una tabella di routing con $m$ righe, dove $m$ è pari al numero di bit dello spazio di indirizzamento.
Sia $p$ un peer della rete e sia $FT_p$ la Finger Table del nodo $p$, allora
$$FT_{p}[i] = \text{succ}(p + 2^{i-1}) \mod{2^m}, \text{ con } 1 \leq i \leq m$$
quindi un nodo $p$ è in grado di contattare direttamente i nodi che gestiscono le risorse che si trovano a distanze pari a potenze di due da $p$. 
Guardando la figura della rete precedente, la FT del nodo 7 avrà le seguenti quattro righe:
$$
\begin{align}
FT_{7}[1] &= \text{succ}(7+1) = \text{succ}(7) = 12 \\
FT_{7}[2] &= \text{succ}(7+2) = \text{succ}(9) = 12 \\
FT_{7}[3] &= \text{succ}(7+4) = \text{succ}(11) = 12 \\
FT_{7}[4] &= \text{succ}(7+8) = \text{succ}(15) = 15
\end{align}
$$
Dunque ogni peer mantiene informazioni di una piccolo numero di nodi ($m$ righe) e conosce più nodi nelle vicinanze rispetto a nodi lontani.
Le finger table non sono sufficienti per rispondere direttamente ad una richiesta di lookup, infatti non si può determinare direttamente il successore di ogni chiave (il nodo responsabile di ogni risorsa).
Dunque bisogna implementare un algoritmo di routing che possa sfruttare le finger table per essere efficiente.
#### Algoritmo di routing
L'idea che l'algoritmo usa per abbattere il numero di inoltri della richiesta è quella di raggiungere la prossimità del peer che contiene la risorsa in pochi e grandi salti, e poi procedere gradualmente con piccoli passi in modo da trovare il peer esatto senza superarlo nella ricerca.
L'algoritmo data una chiave $K$ trova $\text{succ}(K)$ a partire dal nodo $p$:
- Se $K$ appartiene alla porzione di anello gestita da $p$, l'algoritmo termina;
- Se $K$ appartiene alla porzione di anello gestita dal peer successivo a $p$, dunque se $K$ è maggiore di $p$ ma minore di $FT_p[1]$, allora $p$ inoltra la richiesta al peer successivo;
- Altrimenti, $p$ inoltra la richiesta al nodo $q$ più distante da $p$ la cui chiave identificativa  è minore o uguale a $K$, dunque il nodo $q$ è quello nella $j$-esima riga di $FT_p$ per cui $$FT_{p}[j] \leq K < FT_{p}[j+1]$$
![[Pasted image 20231202234352.png]]
### Ingresso e uscita dei nodi in Chord
Si vede ora come Chord grazie alle sue caratteristiche, permette di gestire l'entrata e l'uscita dei nodi dalla rete in modo efficiente, aggiornando le finger table strettamente necessarie e riassegnando poche risorse, cioè quelle che si trovano intorno alla posizione in cui si inserisce o esce un peer.
Per gestire al meglio queste operazioni, ogni peer mantiene un puntatore al peer successore e al peer predecessore nella rete: questi puntatori verranno propriamente aggiornati nelle operazioni di entrata o uscita di un nodo.
![[Pasted image 20231202234926.png]]
#### Ingresso di un nodo
Quando un nodo con id pari a $p$ (ottenuto dal digest di metainformazioni sul nodo) entra nella rete di overlay, deve per prima cosa trovare il punto in cui deve posizionarsi nell'anello:
1. Il nodo $p$ chiede ad un peer di individuare il suo nodo successore nella rete $q = \text{succ}(p+1)$ nell'anello tramite l'operazione di lookup descritta prima;
2. Il nodo $p$ entra nella rete collegandosi con $q$, informandolo del suo ingresso;
3. Il nodo $p$ inizializza la sua Finger Table. Inizialmente contiene solo la prima riga con valore $q$ (ossia, il nodo successivo nell'anello);
4. Informa  il predecessore (ossia il predecessore di $q$ prima dell'entrata di $p$) di aggiornare la sua Finger Table. In particolare, la prima riga, che indica il nodo successivo nell'anello, dovrà contenere l'id del nodo $p$;
5. Trasferisce tutte le risorse gestite da $q$ che hanno chiave minore di $p$ sotto la sua gestione;
#### Uscita di un nodo
Quando un nodo con id pari a $p$ abbandona *volontariamente* la rete di overlay:
- Trasferisce tutte le risorse che gestisce al peer $q$ successivo nell'anello;
- Aggiorna il predecessore di $q$ che sarà pari al predecessore puntato da $p$;
- Aggiorna il successore del suo nodo predecessore che sarà pari a $q$; il predecessore aggiorna la prima riga della sua Finger Table sostituendo $p$ con $q$.
Può capitare che un nodo abbandoni la rete *involontariamente*, ad esempio per un guasto, per cui non vengono aggiornate tutte le informazioni coerentemente come avviene nell'abbandono volontario.
Per gestire l'abbandono improvviso della rete, ma anche per mantenere aggiornate tutte le Finger Table dopo eventi di join e leave, Chord prevede l'implementazione di una procedura di **stabilizzazione** dell'anello. Grazie a questa procedura, ciascun nodo ricontrolla e aggiorna la sua Finger Table periodicamente.
#### Tolleranza ai guasti in Chord
Anche in Chord, come in tutte le reti P2P, un nodo potrebbe subire un fallimento. Quando un nodo fallisce, oltre a rendere le Finger Table di alcuni nodi (o tutti) non aggiornate, le risorse gestite da quel nodo potrebbero sparire dalla rete. Per evitare ciò, si possono fare per ciascuna risorsa $R$ repliche, memorizzando ogni replica sugli $R-1$ nodi successivi nell'anello.
Per implementare queste replicazioni, un nodo deve conoscere molteplici successori: deve essere in grado di trovare il successore del suo successore del suo successore.... fare questo è facile solo se si ha una conoscenza completa di tutti i nodi.
Non avendo conoscenza di tutti i nodi, ad esempio un nodo $p$ può trovare il successore del suo successore $q$ facendo una operazione di lookup $\text{succ}(p+q+1)$ .
Quando un nodo rientra nella rete dopo un fallimento, deve controllare con i successori che avevano memorizzato le repliche delle sue risorse se ci sono stati aggiornamenti e ottenere la versione aggiornata delle risorse; inoltre, deve fare lo stesso con i predecessori sulle risorse che lui manteneva come backup.
La replicazione migliora in generale la robustezza della rete, rendendo meno grave il problema delle Finger Table non aggiornate.
### Summing up
#### Pros
- Chord è un protocollo semplice ed elegante;
- Garantisce il bilanciamento del carico grazie al consistent hashing, che distribuisce le chiave in modo uniforme su tutti i nodi
- Il sistema è scalabile: le operazioni di lookup, ingresso e uscita del nodo sono ragionevolmente efficienti ;
- Il sistema è robusto: grazie alla stabilizzazione periodica dell'anello, le finger table vengono periodicamente aggiornate riflettendo i cambiamenti nella rete;
#### Cons
- Nel protocollo la prossimità dei nodi a livello IP non è considerata: successore e predecessore di un nodo sono vicini soltanto nello spazio di indirizzamento, non è detto che siano vicini in termini di prossimità di rete. Non c'è corrispondenza nella vicinanza tra rete logica e rete fisica;
- Non è semplice supportare la ricerca delle risorse senza fare matching esatto;
- L'algoritmo di ottimizzazione di Chord originale non era corretto, sistemato circa dieci anni dopo.
## Sistemi P2P ibridi
Nei sistemi P2P ibridi, e in generale nelle architetture ibride, sono presenti degli elementi centralizzati che svolgono specifiche funzioni, in combinazione ad elementi che sono invece decentralizzati. L'obiettivo delle architetture ibride è quello di prendere i benefici della centralizzazione e i benefici della decentralizzazione.
In questa trattazione si vedono tre sistemi ibridi con diverso grado di decentralizzazione.
### Reti con super-peer
Le reti P2P con super-peer sono delle classiche reti P2P in cui viene in parte rotta la simmetria tra i peer eleggendo alcuni nodi della rete come **super-peer**, ossia dei nodi che hanno delle funzionalità in più rispetto a tutti gli altri nodi del sistema.
![[Pasted image 20231203123120.png]]
In questi sistemi la simmetria viene violata per renderli più efficienti e/o semplici da gestire. Ad esempio, i super-peer vengono utilizzati per implementare funzioni di routing sia in architetture strutturate che non strutturate: il routing non avviene tra tutti quanti i nodi del sistema, ma soltanto tra i supernodi. La funzione di lookup avviene dunque solo tra i super nodi: ogni peer normale fa riferimento ad un superpeer e quando vuole trovare una risorsa chiede al suo superpeer. Ogni superpeer ha una conoscenza delle risorse mantenute dai peer a lui collegati (un indice); se la risorsa non appartiene a nessuno di tali peer, il superpeer inoltra la richiesta agli altri superpeer usando gli stessi meccanismi utilizzati negli altri sistemi.
L'operazione di lookup tramite superpeer è più efficiente, dato che sono coinvolti meno nodi, ma è necessario affrontare alcune questioni: come scegliere i superpeer e quale meccanismo usare per associare peer a superpeer (in modo statico o dinamico?).
### BitTorrent
BitTorrent è un protocollo di condivisione file basato su un'architettura P2P non strutturata. I passi per cercare un determinato file $F$ sono i seguenti.
1. Utente clicca su un link da cui ottiene un file torrent che contiene il riferimento ad un *tracker*: un tracker è un server bittorrent che mantiene una lista accurata di quali sono i nodi attivi nella rete che hanno il file (o parti del file) $F$;
2. Il client BitTorrent contatta il tracker, da cui riceve una lista dei peer che hanno il file (o parti del file) $F$;
3. Il client BitTorrent scarica le parti del file $F$ dai diversi peer, entrando a far parte di uno *swarm* di downloader, cioè un insieme di peer che scaricano parti del file in parallelo e che allo stesso tempo distribuiscono i chunk scaricate verso gli altri peer dello swarm.
Dunque in bittorrent, il **tracker è il punto di centralizzazione** del sistema.
Inoltre il sistema BitTorrent implementa dei meccanismi volti ad incentivare lo scambio di dati tra i peer sia in ricezione che in condivisione, in particolare:
- Il meccanismo *rarest piece first* fa si che i peer scarichino prima i chunk del file che sono meno comuni nella rete, cioè quei chunk che solo pochi peer nella rete possiedono: questo ha lo scopo di rendere lo scambio dei file rispetto al fenomeno del churn dei nodi; se un chunk di file è condiviso da un solo nodo e questo abbandona la rete, nessuno può scaricare questo chunk e dunque nessuno può ottenere il file completo.
- Il meccanismo *tit-for-tat* fa si che i peer che non condividono risorse ricevono pochi dati dagli altri peer: questo ha lo scopo di evitare la presenza di nodi egoistici nella rete, che scaricano dati senza condividere risorse con gli altri nodi.
### Blockchain
Come dice il nome, una **blockchain** è una struttura dati distribuita rappresentata da una catena di blocchi collegati l'uno all'altro.
Questi blocchi possono contenere diverse risorse, ad esempio in Bitcoin (da cui nasce l'idea di blockchain) l'obiettivo è memorizzare transazioni finanziare all'interno dei blocchi.
Una blockchain è distribuita in una rete *untrusted*, nella quale i peer non si conoscono e non si fidano l'uno dell'altro.
L'obiettivo della blockchain è mantenere le transazioni in modo immutabile, garantendo che tutti i nodi che vanno leggere la blockchain vedano le stesse transazioni e che nessuno possa alterarne il contenuto in una situazione di mancanza di fiducia tra i peer.
L'idea della  blockchain è di avere una rete di nodi p2p dove ogni nodo replica il contenuto della blockchain e andare a leggere le transazioni memorizzate