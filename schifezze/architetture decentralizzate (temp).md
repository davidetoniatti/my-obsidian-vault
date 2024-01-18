### Arch decentralizzate
Sistemi peer-to-peer: elevato grado di decentralizzazione.
Insieme di sistema di applicazioni che usano risorse distribuite su larga scala geografica per eseguire delle funzioni in modo decentralizzato. Spesso i nodo sono sui bordi della rete.
Risorse condivise nei sistemi p2p: file, storage, banda, potenza computazionale.
Tutti i nodi sono pari (peer) tra di loro nei ruoli, nei privilegi e nelle responsabilità. Sono indipendenti tra loro e localizzati ai bordi della rete.
Hybrid p2p: ci sono dei nodi che hanno più ruoli e funzionalità, e sono detti superpeer.
Non c'è controllo centralizzato: il controllo è distribuito, dunque ogni peer si comporta sia da client che da server.
Le reti p2p sono altamente distribuite, alta ridondanza

Compiti fondamentali di un nodo p2p
1. bootstrap: come un nuovo peer che vuole partecipare alla rete scopre delle informazioni di contatto su altri peer che fanno già parte della rete. Soluzioni: configurazione statica, tenere traccia dei nodi che in usi precedenti so che facevano parte della rete, avere nella rete dei nodi sempre presenti e che sono noti all'esterno.
2. Resource lookup: come trovare le risorse, ossia chi possiede il file che voglio ottenere
3. Resource retriveal: come recuperare il file in modo da ottenerlo

P2P overlay: la rete logica che connette i peer che fanno parte del network. i link logici tra i nodi non corrispondono ai link fisici tra i nodi della rete.
rispetto al routing tradizionale, il routing di una p2p riguarda le risorse

Risorse identificate tramite GUID, ottenuta tramite hash di uno stato della risorsa
necessario anche identificare i peer che fanno parte della rete,anche qui id ottenuti tramite hash functions.
Due architetture fondamentali di reti p2p: overlay net non strutturare e overlay net strutturate
Rete logica non strutturata: non è costruita seguendo delle regole che ne determinano la topologia. I peer sono connessi in modo arbitrario. Un peer che entra nella rete segue delle regole molto semplici per accedervi.
Svantaggio principale legato ad un elevato costo computazionale di lookup.
Vantaggi: semplice da gestire nelle operazioni di aggiunta di nodi e risorse nella rete; molto resistente a guasti.
Tre architetture:
- funzione di lookup gestita centralmente da una directory centrale (server centralizzato):  dato id della risorsa, restituisce i peer che possiedono quella risorsa.
- funzione di lookup decentralizzata: la tabella di lookup è distribuita tra tutti quanti i peer della rete (flooding delle richieste).
- sistema ibrido: il routing delle risorse avviene solo tra i superpeer.

## query flooding
Ciascun peer che cerca una risorsa, chiede a tutti i suoi vicini che conosce, cioè ad un hop nell overlay net. Se il peer possiede la risorsa, risponde, altrimenti inoltra la richiesta di lookup a tutti i suoi vicini. Per evitare che la rete venga inondata dei messaggi: assegnamo alla query di lookup un TTL che viene decrementata ad ogni hop della richiesta.
2 evitare che una stessa query passi piu volte in un ciclo. id univoco della query, cosi non riprocessa query gia processate.
costo di lookup lineare al nro di nodi della rete.
backward rooting: il nodo che possiede la risorsa inoltra la risposta sullo stesso percorso che aveva seguito la richiesta. In questo modo tutti i nodi del cammino sanno dove si trova la risorsa.

## random walk
Caso particolare del query flooding. Il nodo che origina la richiesta sceglie u.a.r uno solo dei suo vicini e gli inoltra la richiesta, questo avviene in modo ricorsivo fintantoché la risorsa non viene trovata.
Vantaggio: traffico di messaggi ridotto;
Svantaggio: tempo di lookup aumenta;
Come diminuire il tempo di lookup? Scegliamo k nodi casuali con k parametro arbitrario, creando k random walks.

## rete logica strutturata
Topologia ben definita: le piu comuni sono anello e ipercubo.
Obiettivo delle reti strutturate: ridurre il costo di lookup.
Sono applicate in modo efficienti nel momento in cui va cercata una risorsa identificata da una chiave.
Svantaggio: dato il vincolo nella topologia, l'ingresso di nuovi nodi nella rete deve essere implementata in modo coerente.
Idee di base:
- ciascun peer è responsabile di un sottoinsieme di risorse e ha una conoscenza dei peer che dipende dalla struttura della rete. chor: ciascun peer conosce bene i nodi vicini e conosce poco i nodi piu lontani.
- ogni risorsa ha un GUID
- ogni peer ha un GUID
- GUID computed as hash functions
- stesso spazio degli identificatori per risorse e peers
- instradare la richiesta al peer il cui GUID è il più vicino rispetto al GUID della risorsa.
Il routing avviene usando una struttura dati distribuita: DHT (Distributed Hash Function).

designing dht
Le risorse sono partizionate tra i nodi e ogni nodo gestisce una porzione di risorse salvate nella dht. ad ogni nodo sono assegnate una porzione contigua di chiave e mantiene informazioni di risorse mappate nella sua porzione di chiave;
routing nel dht: data K, si mappa nel guid del nodo piu vicino a K;

Problematiche:
Ciascun nodo deve gestire piu o meno la stessa quantità di chiavi
Rimappare completamente le chiavi di un nodo quando esce dalla rete: quando abbiamo un cambiamento, l'operazione di remapping sia confinata il piu possibile;
Usiamo il consistent hashing per risolvere questi problemi.

Le dht supportano direttamente solo il matching esatto, dunque per trovare la risorsa bisogna fornire la chiave esatta.
Dunque è difficile e costoso supportare query + complesse, tipo query con wildcard o intervallo.

dht-based p2p differiscono da: definizione dello spazio degli identificatori e dunque la topologia; la selezione dei peer con cui comunicare (distant metric)

## Chord
lookup in $O(\log{n})$ ; scalabile xk ciascun nodo mantiene uno stato di dimensione $O(\log{n})$; algoritmo robusto: sopravvive a failure che riguardano tanti nodi.
I nodi sono mappati come un anello usando il consistent hashing, percorso in senso unidirezionale (orario).
Sia i nodi che le risorse vengono mappate sullo spazio di indirizzamento identificate da una chiave 
un nodo è responsabile delle sue risorse e quelle che vengono dopo il nodo che lo precede.
Consistent hashing: gli oggetti nei bucket (risorse) sia i bucket stessi (nodi) usano lo stesso spazio di indirizzamento e ciascun nodo gestisce una porzione contigua di chiavi. fondamentale per robustezza e performance:
1. nel caso di ridimensionamento della rete, la maggior parte delle chiavi viene mappata allo stesso bucket come prima: cambiamenti solo a livello locale.
2. ciascun nodo riceve un numero simile di chiavi: buona distribuzione del carico sui nodi.

Routing: finger table, tabella di routing che ciascun peer mantiene di dimensione logn
