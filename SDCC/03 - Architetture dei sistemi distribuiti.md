# Distributed System Architectures

I sistemi distribuiti sono caratterizzati da software complessi, in cui le componenti sono per definizione sparse in diverse macchine. E' dunque necessario che questi sistemi siano ben organizzati. Un modo di vedere i sistemi distribuiti è quello di visualizzare da una parte l'organizzazione logica delle componenti software, e dall'altra l'effettiva realizzazione fisica.
## Architettura software vs architettura di sistema

- **Architettura software**: si intende l'organizzazione logica e l'interazione delle componenti software che costituiscono il sistema distribuito o l'applicazione distribuita. Dunque l'architettura software dice come le varie componenti del sistema distribuito sono organizzate e come queste possono (e devono) interagire tra di loro.
- **Architettura di sistema**: si intende la istanziazione (incluso il deployment) finale della architettura software sul sistema distribuito a disposizione. Dunque, come collocare le componenti software sulle risorse del sistema.
	- Ad esempio, come posso collocare 100 microservizi sulle 6 macchine a disposizione (placement di una app distribuita).
- Di solito il compito del deployment finale è a cura di un layer middleware (kubernetes): studiamo come collocare i vari "pod" sui nodi del cluster su cui kubernetes viene eseguito.
- Questo posizionamento potrebbe non essere ottimale per tutta la durata del servizi: sono dunque necessari meccanismi che sono in grado di reinstanziare componenti su altri nodi del sistema. Studieremo meccanismi che ci permettono di fare migrazione di container o vm mentre in esecuzione.

## Stili architetturali per i sistemi distribuiti
Si analizzano i principali stili architetturali per la progettazione di sistemi distribuiti.
Essi verranno definiti in termini di **componenti** e **connettori**:
- **Componente**: una unità modulare con interfacce richieste e fornite ben definite, che è sostituibile all'interno del suo ambiente.
	- Un interfaccia ben definita e immutabile è necessaria per poter sostituire al volo componenti software nel sistema distribuito senza dover interrompere il servizio.
- **Connettore**: un meccanismo che permette a componenti diverse di interagire tra di loro, fungendo da mediatore nelle comunicazioni, e quindi nella cooperazione e coordinazione. Esempi di connettori sono i meccanismi di chiamate a procedura remota o una coda per lo scambio di messaggi.
Dunque descrivere uno stile architetturale bisogna specificare come i componenti sono connessi, come si scambiano dati e come componenti e connettori sono configurati in un sistema o applicazione.
Gli stili architetturali principali sono tre
- Stile architetturale a livelli;
- Stile architetturale orientato ai servizi;
- Stile architetturale publish-subscribe.

### Stile architetturale a livelli
I componenti sono organizzati in livelli, o **layer** (con il termine layer si intende nello specifico un livello _logico_).
Generalmente la componente nel layer $L_i$  può invocare le componenti ai livelli sottostanti $L_j: j<i$ , tramite scambio di messaggi. Solo in casi eccezionali una componente può fare invocazioni verso livelli più alti nello stack.
In particolare, le richieste cominciano dal layer più alto e vengono propagate nei livelli sottostanti, mentre le risposte risalgono i layer nello stack.
Separazione dei compiti a livello di componente
#### Application layering
Lo stile a livelli viene utilizzato spesso in applicazioni web, e solitamente sono presenti tre layer: layer di presentazione (applicazione, il client insomma), layer di processing o logico (nel quale avvengono le operazioni principali),e il layer dei dati (spesso separato in layer di persistenza e database).

### Service oriented style
Lo stile orientato ai servizi consiste in una collezione di entità separate ed indipendeti, dove ogni entità incapsula un servizio, oggetto o microservizio,
Lo stile orientato ai servizi include i seguenti "sottostili":
- Stile architetturale basato su oggetti;
- Stile architetturale basato su microservizi;
- Stile architetturale RESTful.
#### Object-based style
Ogni componente corrisponde ad un oggetto, il quale incapsula i dati e offre metodi che lavorano su tali dati.
- Si hanno naturalmente proprietà di incapsulamento e information hiding, riducendo la complessità nella gestione;
- Le componenti, ossia gli oggetti, garantiscono un'alta riusabilità tra diverse applicazioni;
- Un oggetto può essere un wrapper per un'unità di tipo legacy;
La comunicazione tra queste componenti avviene per mezzo di chiamate a procedura remota
Comunicazione avviene tramite chiamata a procedura remota. Dunque, la locazione degli oggetti e dei loro metodi invocabili è del tutto trasparente al chiamante.
#### Microservices style
Un nuovo stile architetturale emergente per applicazioni distribuite che struttura un'applicazione come una collezione di servizi lascamente accoppiati tra loro.
Sono pensati per avere delle applicazioni che sono effettivamente scalabili;
Questo stile definisce come costruire, gestire ed aggiornare architetture per mezzo di piccole unità self contained.
- Modularità: decomposizione dell'applicazione in un insieme di servizi deployabili in modo indipendente, che siano lascamente accoppiati e cooperativi e che possano essere rapidamente deployati e scalati.
#### Restful style
In questo stile architetturale, il sistema distribuito è concepito come una collezione di risorse, individualmente gestite dalle componenti;
REST: utilizzo dei metodi del protocollo http per definire quali sono le operazioni possibili nelle varie componenti.
Le risorse sono identificate tramite URI, dunque le componenti espongono delle interfacce uniformi.
Infine, le interazioni sono tipicamente **stateless** (senza stato): lo stato deve essere trasferito dai client ai server.
Le operazioni REST usano i 4 metodi HTTP: GET, PUT, POST e DELETE.

### S3
servizio di storage basato e organizzato in chiavi;
Gli oggetti(file) sono memorizzati in buckets(directory):-
S3 è flat, non c'è una gerarchia di directory ma è simulata a livello logico attribuendo ai nomi una struttura a directory. Un oggetto memorizzato in S3 è referenziato da un unico URI.
Le operazioni avvengono tramite http rest ( uno dei meccanismi per accedere ad S3).

## Decoupling (disaccoppiamento)

Una dipendenza stretta tra componenti introduce delle limitazioni;
Soluzione: lasciamo comunicare le componenti in modo indiretto per mezzo di un intermediario, creando una netta separazione tra i ruoli di coordinazione e computazione.
Il disaccoppiamento rende le applicazioni più flessibili e permette di definire stili architetturali che permettono una migliore distribuzione, scalabilità ed elasticità.
Si definiscono diversi tipi di disaccoppiamento:
- **disaccoppiamento rispetto allo spazio**: i componenti dell'applicazione non devono conoscersi tra di loro per comunicare. Qualcuno in mezzo farà da intermediario nella comunicazione
- **disaccoppiamento rispetto al tempo**: le componenti che interagiscono tra di loro non devono essere presenti nel momento in cui avviene una comunicazione.
- **disaccoppiamento di sincronizzazione**: le componenti che interagiscono non devono aspettarsi l'una con l'altra: chi invoca può continuare a fare altro, dunque le componenti non si bloccano reciprocamente.
Grazie al disaccoppiamento, i sistemi distribuiti possono essere flessibile mentre gestiscono i cambiamenti, e dunque forniscono servizi più elastici.
Naturalmente, il decoupling provoca inevitabilmente dell'overhead tra le componenti del sistema distribuito.

L'introduzione del decoupling ha comportato alla progettazione di diversi stili architetturali in cui le componenti comunicano in modo indiretto. In particolare si hanno tre stili:
- **event-driven**;
- **data-oriented** ;
- **publish-subscribe**;

### Stile event-driven
In questa architettura le componenti comunicano attraverso un bus degli eventi.
Con evento si intende un cambiamento significativo di stato, come ad esempio un cambio di temperatura, o l'apertura di una porta.
In particolare:
- Le componenti pubblicano degli eventi, nel senso di renderli monitorabili da parte di altre componenti;
- Le componenti possono iscriversi ad eventi pubblici ai quali sono interessati;
- Le componenti ricevono le notifiche degli eventi a cui sono iscritti;
La comunicazione in questa architettura è basata sullo scambio di messaggi, inoltre è anonima, asincrona e di tipo multicast (la notifica di un event viene mandata a tutte le componenti connesse all'event bus).
In questa architettura, sono ben implementati il disaccoppiamento temporale e di sincronizzazione.

### Stile data-oriented
In questa architettura la comunicazione tra le componenti avviene attraverso uno spazio di memoria condiviso passivo (qualche volta (pro)attivo). Dunque i dati vengono aggiunti e rimossi da questo spazio dalle varie componenti che vi accedono.
Le API rese disponibili sono solitamente per la lettura, la scrittura, il pop di un dato e varianti di esse.
Se lo spazio di memoria è attivo, allora le modifiche vengono notificate alle componenti.
Ovviamente, è necessario gestire la concorrenza, per non rendere i dati inconsistenti alle componenti che vi accedono.
Questo stile garantisce ovviamente disaccoppiamento spaziale, dato che non è necessario conoscere le componenti per comunicare, visto che i dati (messaggi) vengono scritti in una memoria condivisa.
Viene garantito anche il disaccoppiamento di sincronizzazione.

### Stile publish-subscribe
In questa architettura le componenti si dividono in due gruppi: le componenti **publisher**, o producer, che generano degli eventi da pubblicare ma che non sono interessati ad inviare notifiche direttamente a dei consumer, o subscriber; le componenti consumer o subscribers si registrano (subscribe) agli eventi ai quali sono interessati e vengono notificati quando si ha un occorrenza di tali eventi.
Dunque tra i publisher e i subscriber è presente un middleware publish-subscribe, che si occupa di gestire gli eventi pubblicati e le iscrizioni effettuate da componenti a tali eventi.
In particolare, le componenti publisher pubblicano per mezzo del middleware degli eventi, mentre le componenti subscriber utilizzano il middleware per scoprire e iscriversi (o disiscriversi) agli eventi pubblicati: il middleware si occuperà di notificare i subscriber quando ci sono occorrenze di eventi.
Grazie al middleware publisher-subscriber, si ottiene il disaccoppiamento completo tra le componenti.

Il problema principale da risolvere è come effettuare il matching degli eventi, cioè scegliere quali eventi notificare ai subscriber in base alle loro sottoscrizioni.

Il matching tra sottoscrizioni e eventi avviene basandosi su topic o basandosi su contenuto. Assumiamo che gli eventi sono descritti da una coppia (attribute, value).
- **sottoscrizione basata su topic**: il subscriber specifica i valori di attributo a cui è interessato. Lo svantaggio di questa soluzione è che l'espressività per fare matching è limitata;
- **sottoscrizione basata su contenuto**: il subscriber specifica che l'attributo che gli interessa appartiene ad un certo range di valori (una sorta di filtro). Lo svantaggio si trova nella difficoltà implementativa, che può portare a problemi di scalabilità; vedremo kafka, che è un sistema topic-based e ci permette di definire dei topic fissi e gli eventi quando vengono pubblicati vengono inseriti in questi topic, che possono essere seguiti dai subscriber.

### Scelta dello stile architetturale

Come per ogni cosa, **non c'è una soluzione migliore dell'altra!** Lo stesso problema può essere risolto usando stili architetturali differenti.

La scelta dipende dai requisiti extra-funzionali dell'applicazione:
- costi (effort dello sviluppo, se i miei sw dev hanno dimestichezza con un certo stile, mi costa di meno implementarlo)
- scalabilita
- performance
- affidabilità
- e chi piu ne ha piu ne metta.

### Architettura di un sistema distribuito
L'architettura di un sistema distribuito è l'istanziazione di una applicazione o sistema distribuito sui nodi dell'infrastruttura che sono a disposizione.
Scegliere come viene fatto il deployment delle componenti nel sistema. Ad esempio nel caso di applicazioni a microservices si può decidere in fase di istanziazione di fare il mapping 1 microservice 1 container oppure piu microservices in un singolo container.
Decidere come questi componenti interagiscono tra di loro e come dove farne il deployment, cioè scegliere i nodi su cui posizionare le componenti dell'applicazione.
Tipi di architetture di sistema:
- Architetture centralizzate;
- Architetture decentralizzate;
- Architetture ibride.

## Architetture di sistema centralizzate

Il modello classico di architettura centralizzata è il modello **client-server** (non distribuito), dove sono presenti i servers che offrono dei servizi a dei client, i quali usano questi servizi. Naturalmente, client e server possono trovarsi su macchine distinte.
Nel modello i client fanno richieste ad un server, e il server invia una risposta al client. Questa comunicazione tra client e server è basata su scambio di messaggi e spesso è implementata in modo sincrono e bloccante: questo comporta uno stretto accoppiamento tra client e server per la comunicazione.
Dato che è un corso di sistemi distribuiti, ci chiediamo come è possibile distribuire questa architettura di base sul nostro sistema composto da più host.
Questo avviene realizzando una architettura di tipo client-server **multitier**, cioè con più livelli fisici. In particolare, dati i livelli logici che caratterizzano ad esempio uno stile architetturale a livelli, mappiamo questi livello logici (layer) in livelli fisici (tier) distribuiti
Generalmente le architetture di questo tipo sono composte da due o tre tier, e possono avere configurazioni diverse, in base a come si devono distribuire i layer che ricordiamo essere raggruppati in:
- layer di **presentazione**;
- layer **logico** (o business);
- layer dei **dati**;

![[img_01.png]]

Questo tipo di architettura multi-tier corrisponde ad una distribuzione verticale dell'applicazione, in cui essa viene divisa in più livelli logici e ognuno di questi livelli è eseguito su livelli fisici differenti.
Replicando su più nodi ciascuno dei componenti che si trovano sui livelli fisici, si ottiene una distribuzione orizzontale: distribuiamo ciascun livello logico su molteplici server. Utilizzando un load balancer per distribuire il carico tra i server, si ottiene un sistema più potente e stabile.
#### Esempio: Web application in AWS

Schema architetturale di una web application il cui deployment viene fatto sull'infrastruttura di amazon.

