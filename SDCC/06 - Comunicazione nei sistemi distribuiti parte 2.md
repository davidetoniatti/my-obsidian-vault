## Comunicazione persistente
La comunicazione tramite RPC migliora la trasparenza alla distribuzione, nascondendo tutta la parte di programmazione delle socket; ma è un meccanismo transiente, dunque le entità client e server devono essere entrambe presenti affinché la comunicazione possa aver luogo.
Utilizzando un middleware orientato ai messaggi, si ottiene un miglioramento nel disaccoppiamento nel tempo e nello spazio delle componenti dell'applicazione distribuita e nella sua flessibilità, grazie al supporto persistente per la memorizzazione dei messaggi offerto dal middleware. Questo pattern di comunicazione è usato specialmente nelle applicazioni a microservizi (spesso, si usano sia RPC che sistemi per comunicazione persistente come le code di messaggi).
I middleware orientati ai messaggi fanno uso generalmente di uno di questi due pattern: code di messaggi o publish-subscribe, con i due relativi sistemi middleware ovvero sistemi a code di messaggi e sistemi pub/sub.
### Pattern coda di messaggi
Una coda di messaggi è un'entità che permette di memorizzare in modo temporaneo dei messaggi fintantoché non vengono estratti (dunque cancellati) dalla coda e processati.
Possono leggere più destinatari (consumer) dalla coda di messaggi e ciascun messaggio è consegnato ad un singolo consumer, dunque il pattern di comunicazione è one-to-one.
Inoltre, le code dei messaggi sono tipicamente unidirezionali: nel caso di request-response si utilizzano due code.
Le code di messaggi si possono anche utilizzare per fare scheduling di task tra più consumer che rappresentano le componenti che vanno ad eseguire i task.
![[SDCC/imgs/06-img01.png|center|700]]
La tipica interfaccia di un sistema a coda di messaggi è la seguente.
- **put**: invio non bloccante
	- Appende un messaggio nella coda specificata;
- **get**: ricezione bloccante
	- Se la coda specificata vuota, il processo rimane bloccato in attesa fintantoché non viene inserito un messaggio in coda;
	- Variante: attesa di un messaggio specifico nella coda;
- **poll**: ricezione non bloccante
	- check della coda specificata e riceve un messaggio se disponibili, mai bloccante
- **notify**: ricezione non bloccante
	- Installazione di un handler con funzione di callback che viene automaticamente chiamata quando un messaggio di interesse viene inserito nella coda specificata.
### Pattern  publish-subscribe
In questo caso, i componenti dell'applicazione che devono produrre dei messaggi (publisher) li pubblicano in modo asincrono sul sistema pub/sub, mentre le componenti dell'applicazione che sono interessate a ricevere eventi o messaggi (subscriber) possono sottoscriversi al sistema pub/sub rispetto a dei topic.
L'implementazione più usuale è quella di un sistema pub/sub topic based, cioè basato su argomenti a cui i vari messaggi fanno riferimento, vedremo in dettaglio Kafka.
Rispetto al pattern a coda di messaggi, con il pattern publish-subscribe lo stesso messaggio può essere consegnato a molteplici consumer.
A livello logico, il servizio pub/sub deve tenere traccia dei subscriber interessati ai topic che il sistema rende disponibili.
![[SDCC/imgs/06-img04.png]]
Molti consumer possono iscriversi ad un topic con o senza applicazione di filtro, e le sottoscrizioni sono collezionate da una componente che fa dispaching degli eventi, cioè che si occupa di indirizzare gli eventi a tutti i subscriber interessati.
Il vantaggio dei sistemi pub/sub è il disaccoppiamento completo tra le componenti che pubblicano informazioni e quelle che sono interessate a ricevere informazioni.
Il sistema pub/sub è una generalizzazione del sistema a code di messaggi.

L'interfaccia generica di un sistema pub/sub è la seguente.
- `publish(event)`: per pubblicare un evento
	- Gli eventi possono essere di qualunque tipo di dato, in base anche al supporto delle implementazioni per i diversi linguaggi di programmazione.
- `subscribe(filter expr, notify_cb, expiry) -> sub handle`: per sottoscriversi ai topic di interesse
	- Si possono esprimere dei filtri sui messaggi, ed è necessario specificare la funzione di callback che verrà attivata per gestire il messaggio una volta che è disponibile nel sistema pub/sub. Inoltre si specifica una scadenza alla registrazione del topic.
	- Viene ritornato un handler della sottoscrizione
- `unsubscribe(sub handle)`: per disiscriversi da un topic;
- `notify_cb(sub_handle,event)`: chiamata dal sistema pub/sub per notificare al client che c'è un evento su cui c'è un matching.
### Middleware orientato ai messaggi: funzionalità
Il middleware orientato ai messaggi si occupa di funzionalità di indirizzamento, per smistare correttamente i messaggi dai mittenti ai destinatari; deve rendere la comunicazione disponibile e spesso offre funzionalità di trasformazione dei formati dei messaggi. Infatti chi comunica potrebbe usare formati differenti.

![[SDCC/imgs/06-img05.png]]
### Semantiche di comunicazione nella ricezione in MOM
Quando il producer manda il messaggio, quali sono le garanzie che il messaggio venga consegnato? I sistemi MOM offrono le seguenti semantiche.
#### At-least-once delivery
Il middleware deve consegnare il messaggio *almeno* una volta: affinché il mittente sappia che il messaggio è stato consegnato almeno una volta, il destinatario deve mandare un messaggio di acknowledge ogniqualvolta riceve un messaggio. A questo punto il mittente rimanda il messaggio se dopo un certo tempo non riceve alcun messaggio di ack. Naturalmente l'applicazione deve essere idempotente, perché è possibile che riceva la stesso messaggio più di una volta, ad esempio quando il messaggio di acknowledge viene perso nella trasmissione.

![[SDCC/imgs/06-img06.png]]
#### Exactly-once delivery
In questo caso, i meccanismi devono garantire che un messaggio sia consegnato esattamente una sola volta al destinatario. Dunque è necessario identificare i messaggi duplicati e filtrarli, assegnando un identificativo univoco ad ogni messaggio. Inoltre è necessario che i componenti del sistema middleware siano tolleranti a guasti.
![[SDCC/imgs/06-img07.png]]
#### Transaction-based delivery
Una variante dell'exatly once, in cui i messaggi vengono cancellati dalla coda soltanto nel momento in cui vengono ricevuti con successo certamente da un destinatario. Questo avviene tramite la partecipazione del sistema MOM e del destinatario ad una transizione, garantendo le proprietà ACID.

![[SDCC/imgs/06-img08.png]]
#### Timeout-based delivery
Garantire che il messaggio sia cancellato dalla coda quando è stato ricevuto e processato con successo *almeno* una volta: garanzia un po più forte dell'at-least-once.
Questa semantica è implementata in Amazon SQS. L'idea è che il messaggio, una volta consegnato, non viene cancellato immediatamente ma diventa invisibile ad altri mittenti, per un periodo di tempo definito da un timeout di visibilità. Se il destinatario processa il messaggio correttamente, manda un acknowledge al sistema MOM e a questo punto il messaggio viene cancellato. Se destinatario non manda l'acknowledge entro il timeout, allora il messaggio torna ad essere visibile, e dunque potrà essere recapitato da un altro destinatario.

![[SDCC/imgs/06-img09.png]]
### Routing dei messaggi
#### Modello generale
In generale, le code sono gestite da *gestori delle code (queue managers)*. Ogni host del sistema ha un queue manager, e le applicazioni possono solamente estrarre ed inserire messaggi in code locali all'host su cui si trova. Il gestore della coda è responsabile del routing dei messaggi verso altre code.
![[SDCC/imgs/06-img02.png|center|600]]
Dunque l'insieme dei gestori delle code va a formare una *rete di overlay*, nella quale possono esserci dei gestori che operano soltanto come router, cioè dei gestori che non sono direttamente collegati a componenti applicative, ma fanno solamente da instradatori per i messaggi che coinvolgono altre code.
![[06-img26.png|center|500]]
Il meccanismo di routing nella rete di overlay dei gestori delle code può essere configurata due modi:
1. Per mezzo di tabelle di routing configurate staticamente. Semplice ma si perde di flessibilità e adattamento;
2. Utilizzare dei gestori di code che effettuano routing in modo dinamico, mappando dinamicamente i nomi delle code alla loro effettiva locazione. Garantisce flessibilità e adattamento ma complica il routing.
### Trasformazione dei messaggi
Spesso è necessario convertire il formato dei messaggi; questa attività è fatta da una componente detta **message broker**.
Il message broker è un entità che fa parte del middleware di messaggistica, in grado di gestire l'eterogeneità dell'applicazione. Questa componente è in grado di applicare trasformazioni sui messaggi tramite un insieme di regole e plugin di conversione. Per essere scalabile e affidabile, il message broker deve essere implementato in modo distribuito.
![[06-img27.png|center|600]]
### Amazon Simple Queue Service (SQS)
**Amazon Simple Queue (SQS)** è un servizio cloud di messaggistica a coda di messaggi, con la particolarità di essere completamente gestito dal cloud provider (AWS), dunque l'utente non deve istanziare nessun elemento del sistema. 
Il sistema inoltre è regionale, dunque all'avvio del servizio SQS bisogna specificare la regione e code di messaggi in diverse regioni non possono interfacciarsi tra di loro. 
Il sistema è basato su un modello di polling, dunque per l'estrazione di un messaggio il consumer deve andare a chiamare la funzione di ricezione esplicitamente (non è previsto da SQS un servizio di notifica nel momento in cui c'è un messaggio disponibile).
#### Features
SQS è (secondo AWS) un sistema affidabile, altamente disponibile e scalabile. Questo fa intuire che il sistema sottostante è composto da un insieme di server, aspetto necessario per garantire l'affidabilità. Infatti, i singoli server del servizio SQS che gestiscono le code sono server replicati. Dalla figura si comprende che, la coda è unica, ma i messaggi contenuti sono replicati e distribuiti su un insieme di server dislocati nella stessa regione AWS.

![[SDCC/imgs/06-img10.png|center|600]]

In SQS un consumer che riceve un messaggio **deve** eliminare espressamente il messaggio dalla coda. Inoltre, le code sono delle locazioni in cui i messaggi sono mantenuti in modo temporaneo, quindi c'è un periodo di retention, cioè un periodo dopo il quale un messaggio viene eliminato dalla coda se non è inviato (e quindi cancellato) ad un consumer.
Il servizio fornisce un delivery basato su **timeout**, dunque i messaggio inviati vengono mantenuti in coda ma vengono nascosti per un certo periodo, nel quale il consumer processa il messaggio. Se lato consumer fallisce il processamento, allora il timeout scade e il messaggio viene reso nuovamente disponibile in coda. L'operazione di eliminazione del messaggio lanciato dal consumer è il segnale che il processamento è andato a buon fine.

![[SDCC/imgs/06-img11.png|center|600]]

Per ricevere i messaggi dalla coda, il consumer deve effettuare **polling** da essa. Amazon mette a disposizione due modalità di polling: lo *short* polling e il *long* polling.
Lo short polling prevede che nel momento in cui una componente dell'applicazione chiama la funzione di ricezione su una coda SQS, il sistema va ad interrogare un sottoinsieme di server, selezionato randomicamente, che possono avere messaggi della coda. In questo modo Amazon riduce il traffico, ma da punto di vista dello sviluppatore, Amazon potrebbe selezionare dei server che non contengono il messaggio a cui sono interessato: nella coda logica esiste il messaggio, ma il sottoinsieme di server scelto non contiene quel messaggio.
Nel long polling, il sistema va ad interrogare tutti i server che contengono i messaggi della coda. In questo modo otteniamo sicuramente il messaggio a cui si è interessati, dato che si va ad interrogare effettivamente tutta la coda logica, ma la richiesta genera più traffico, dunque la ricezione del messaggio è più lenta e più costosa.
Il servizio SQS permette di scegliere tra due tipi di code: standard o FIFO (First In First Out).
- Coda **Standard**: tipologia di default, garantisce che i messaggi sono memorizzati nella coda ma non è garantito l'ordinamento. Dunque l'ordinamento è di tipo best-effort, nel quale occasionalmente la consegna dei messaggi avviene non con lo stesso ordine con cui i messaggi sono stati inseriti nella stessa coda. Può accadere che vengano ricevuti messaggi duplicati.
- Coda **FIFO**: in questa tipologia, Amazon garantisce ordinamento dei messaggi: i messaggi verranno ricevuti e processato nello stesso ordine con cui i messaggi sono stati inseriti in coda. Non si ricevono messaggi duplicati. Il throughput è peggiore rispetto alla coda standard, dato che è necessario contattare tutti i server e far si che tutti siano d'accordo sull'ordine con cui mandano i messaggi: Amazon implementa un algoritmo di consenso distribuito fra le varie code, cosa che non è necessario per la coda Standard.
#### API di SQS
- `CreateQueue, ListQueues, DeleteQueues` per creare, mostrare e eliminare le code;
- `SendMessage` per inviare un messaggio nella coda, con dimensione limitata a 256KB;
- `Recievemessage` per estrarre un messaggio dalla coda specificata; non è possibile specificare quale messaggio ricevere, ma si può selezionare il numero massimo di messaggi da ricevere (per un massimo di 10 messaggi);
- `DeleteMessage` per eliminare un messaggio dalla coda, obbligatorio dopo aver ricevuto un messaggio.
- `ChangeMessageVisibility` per modificare il timeout di visibilità di un messaggio a runtime nella coda (dopo essere stato estratto); default di 30 secondi;
- `SetQueueAttributes, GetQueueAttributes` per controllare le impostazioni della coda e riceve informazioni su una coda.
#### Esempio di SQS
Si vede ora come usare una coda SQS per realizzare un applicazione di processamento di immagini.
L'applicazione è composta da due componenti: una di frontend, che riceve le richieste da parte degli utenti, e una di backend che effettua il processamento vero e proprio.
Usando un sistema RPC, si accoppiano le velocità del backend e frontend. Per disaccoppiarli, rendendo l'applicazione più flessibile, basiamo la comunicazione su una coda di messaggi, che ci permette anche di scalare frontend e backend in modo indipendente, dato che non ci sarà accoppiamento 1a1 tra nodo di frontend e nodo di backend.
Il nodo di frontend memorizza su un bucket S3 il file che deve essere processato, e nel messaggio che pone nella coda SQS va ad inserire l'URL al file. 
Nel backend, composto da istanze EC2, un nodo preleva il messaggio dalla coda, effettua il processamento, e ritorna il file processato allo stesso modo (bucket S3 e coda) al frontend. In questo esempio la coda effettua anche bilanciamento del carico, e inoltre si può configurare il servizio di autoscaling in modo da allocare/deallocare risorse in base al numero di messaggi in coda.
### RabbitMQ
**RabbitMQ** è un framework open source di messaggistica molto popolare, che utilizza un modello basato su **push**: è il sistema che "spinge"  il messaggio verso i consumer quando ci sono messaggi disponibili (e consumer collegati alla coda).

Il modello push è alternativo al modello **pull**, in cui è il consumer che richiede il messaggio. In quest'ultimo, si ottiene maggiore flessibilità e scalabilità, dato che il modello non è vincolato dalla velocità con cui i consumer consumano i messaggi. Invece in un sistema **push** la velocità dell'invio dei messaggi è vincolata dalla velocità con cui i consumer li elaborano.

Nella terminologia di rabbitMQ, chi manda i messaggi è detto *producer*, mentre chi riceve i messaggi è detto *consumer*. Il producer pubblica dei messaggi, mentre il consumer si sottoscrive a una coda di messaggi e li consuma.
Garantisce ordinamento di tipo First In First Out, dunque i messaggi vengono ricevuti nello stesso ordine con cui sono stati inseriti nella coda.
Supporta diversi protocolli di messaggistica, che sono dei protocolli di livello applicativo che definiscono il formato e il trasporto dei messaggio: tra questi protocolli abbiamo AMQP, STOMP e MQTT. 
#### Architettura
RabbitMQ è un broker (da non confondere con message broker), all'interno prevede un certo numero di componenti: delle code dei messaggi (un broker può gestisce più code) e dei nodi di exchange (di scambio dei messaggi).
Quando il messaggio viene inviato da un producer ad una coda RabbitMQ, esso viene inviato all'exchange, che ne fa il routing verso una delle code gestite dal broker a seconda: di chiavi di routing specificate dall'utente all'interno del messaggio, della coda che l'utente ha indicato e dei meccanismi di binding supportati dal protocollo di messaggistica sottostante (ad esempio esiste un binding per la replicazione in broadcast su tutte le code). Dunque un broker gestisce molteplici code e la componente di exchange fa routing sulle diverse code in base a requisiti indicati dal producer. Il binding è un collegamento tra l'exchange e una coda.

Un broker RabbitMQ può essere singolo oppure i broker possono essere distribuiti: il framework supporta più pattern di distribuzione: cluster di broker e federazione. Di solito si utilizza un cluster di RabbitMQ, recentemente è stato aggiunto supporto alla coda quorum: coda persistente e replicata su più server in grado di garantire ordinamento FIFO grazie all'algoritmo di consenso distribuito Raft.
#### Casi d'uso
In elenco, alcuni [casi d'uso](www.rabbitmq.com/getstarted.html) per RabbitMQ.
1. Producer manda messaggio a consumer: pattern a coda di messaggi di base. 
2. Consumer che competono tra loro: si ha un solo producer e molteplici consumer che prelevano dalla stessa coda. Questo può essere utile per distribuire, ad esempio, delle richieste di un task tra molteplici server replicati.
3. RabbitMQ come sistema pub/sub: in questo pattern sono necessarie tante code quanti sono i consumer che vogliono ricevere gli stessi messaggi. Inoltra il componente di exchange va impostato in modo tale che invii lo stesso messaggio su tutte le code.
4. Ricezione dei messaggi selettivamente: il producer manda messaggi all'exchange che instrada in modo selettivo i messaggi sulle diverse code. Ad esempio per indirizzare i messaggi di errore su una coda specifica per questi messaggi e i messaggi di warning su un'altra coda specifica.
5. Coda dei messaggi con un pattern request/reply, ad esempio per fare meccanismo RPC che abbia la caratteristica di persistenza del messaggio.

Si vedono nel dettaglio i primi due esempi, usando RabbitMQ, il linguaggio GO e il protocollo AMQP.
#### Installazione RabbitMQ e Go AMQP client
1. Installare [RabbitMQ](www.rabbitmq.com/download.html) e avviare il server RabbitMQ in localhost sulla porta di default.
```sh
$ rabbitmq-server
# RabbitMQ CLI tool: rabbitmqctl
$ rabbitmqctl status
$ rabbitmqctl shutdown
# Some useful commands for rabbitmqctl
list_channels
list_consumers
list_queues
stop_app
reset
# Also web UI for management and monitoring
```
2. Installare Go AMQP client library.
```sh
$ go get github.com/rabbitmq/amqp091-go
```
Vedere pkg.go.dev/github.com/rabbitmq/amqp091-go per i dettagli.

```ad-note
title: Nota bene
Ognuna delle componenti illustrate in questi esempi (producer, consumers e i broker RabbitMQ) possono essere eseguiti su macchine differenti tra loro. Ovviamente è necessario specificare opportunamento indirizzo IP e porta sulla quale è in esecuzione il broker.
```
#### Esempio 1
Classico pattern a coda di messaggi: producer e consumer singoli o multipli, i messaggi sono consegnati ad un solo consumer e la politica di ricezione è *push-based*

![[SDCC/imgs/06-img12.png|center|600]]
Codice da **STUDIARE BENE** in `sdcc/code/rabbitm-go/rabbitmq_1_hello`.


***-- finire appunti con lezione del 17 e del 20, poi rivedere questi esempi! --*** 
## Apache kafka
**Apache Kafka** è un sistema publish-subscribe general purpose e realizzato in modo distribuito. Sviluppato inizialmente da Linkedin e poi preso in carico da Apache, è un sistema largamente utilizzato, su scala anche molto ampia da tutti i tech giant.
Scritto in Scala (linguaggio di programmazione funzionale orientato agli oggetti), Kafka la maggior parte delle caratteristiche tipiche di un sistema distribuito: è un sistema altamente scalabile e gestisce un throughput molto elevato di messaggio (miliardi di messaggi), ed offre un'elevata tolleranza ai guasti.
Kafka è capace anche di fare data processing, ma nel corso ci si concentra al processamento dei messaggi
### Kafka at a glance
Kafka premette di memorizzare dei feed di messaggi all'interno di categorie, o *topics*. Per ciascun messaggio, il producer, cioè chi pubblica il messaggio, deve indicare il topic in cui vuole inserire quel messaggio. Dunque Kafka è un sistema publish-subscribe **topic based**.
I topic non sono esattamente delle code, e ciascun topic può avere 0,1 o molteplici consumer iscritti che consumano i messaggi del topic.
Chi pubblica producer o publisher, chi consuma e processa i messaggi consumer o subscriber. 
![[SDCC/imgs/06-img03.png|center|600]]
Kafka cluster: log di dati distribuiti su cluster di server anche detti broker.
Un producer, per pubblicare un messaggio, deve indicare un topic in cui inserire il messaggio: il topicsistema rappresenta la categoria in cui un messaggio viene pubblicato. Per ciascun topic, un Kafka cluster mantiene un **log partizionato**. Con **log** si intende una struttura dati che permette di memorizzare dei messaggi ed eventi inserendoli nel log soltanto in modalità append, ovvero sono aggiunti solamente in in modo incrementale alla fine del log, dunque la struttura dati mantiene gli elementi in modo totalmente ordinato rispetto al tempo.
Con **partizionato** si intende che ciascun topic non è memorizzato in un singolo log, ma viene suddiviso in un numero di partizioni: la partizione è l'unità minima di parallelismo di un topic. Questo garantisce che producer differenti e consumer differenti possano usare in modo parallelo lo stesso topic di Kafka, proprio perché accedono a partizioni differenti dello stesso log.
Dunque Kafka utilizza il partizionamento come tecnica di distribuzione dei dati.
La gestione delle partizioni di ciascun topic è affidata ai broker del cluster Kafka.
![[SDCC/imgs/06-img13.png|center|600]]
A ciascun messaggio è assegnato un numero, che indica l'*offset*, e viene utilizzato dai consumer.
Di default, i messaggi pubblicati in un topic vengono inseriti in modo round robin all'interno delle partizioni di quel topic. In alternativa, la scelta della partizione può avvenire sulla base di una chiave: nel messaggio inserito dal producer, è presente un elemento che rappresenta la chiave del messaggio, e quindi il partizionamento tra le partizioni dello stesso topic è basato su chiave.

Ad esempio, si suppone di usare kafka per memorizzare richieste fatte da diversi utenti. Usando come chiave di partizionamento l'id dell'utente, ogni partizione contiene tutte le richieste fatte da uno specifico utente.

In Kafka il producer pubblica i messaggi (anche detti record o eventi) nelle partizioni dei topic, e i consumer ottengono e consumano i record pubblicati nei topic.
Ogni partizione di un topic è una sequenza di record *ordinata, numerata* e *immutabile*, dove ogni record viene appeso come ultimo elemento della partizione. A ciascun record inserito nel log è associato un numero che ce cresce in modo monotono detto *offset*, che permette di identificare in modo univoco un messaggio e la sua posizione all'interno di una partizione.
![[SDCC/imgs/06-img14.png|center|600]]
Dalla figura, si notano due consumer che leggono contemporaneamente dalla stessa partizione, ma in due punti differenti. Per leggere indicano da quale record leggere, ossia indicano l'offset. E compito del consumer tenere traccia dell'offset

Per migliorare la scalabilità: le partizioni appartenenti allo stesso topic possono essere distribuiti su server differenti (su broker differenti). Grazie a ciò, il throughput I/O aumenta ed è inoltre possibile effettuare letture e scritture parallele su partizioni dello stesso topic (ma su macchine differenti).

Per migliorare la tolleranza ai guasti: ogni partizione può essere *replicata* su un certo numero, configurabile grazie ad un *fattore di replicazione*, di broker Kafka.
Poiché ciascuna partizione è gestita, in caso di replicazione, da più broker, ogni partizione ha un broker che fa da *leader* della partizione e più broker *follower* (se non ci sono broker follower, allora non è presente replicazione).
Quindi dato un cluster di broker Kafka, si hanno alcuni broker che fanno da leader di diverse partizioni, mentre sono follower per altre.

```ad-info
Dato che le partizioni leader sono quelle che svolgono maggior lavoro, Kafka bilancia il numero di partizioni leader su ogni broker: se ci sono $N$ broker e $N$ partizioni leader, Kafka assegna ad ogni broker una partizione leader.
```

L'uso di questo meccanismo semplifica la gestione della consistenza dei dati, dato che: quando viene scritto o consumato un messaggio in una partizione replicata, per mantenere la consistenza è necessario che il messaggio venga scritto/cancellato da tutte le repliche.
Quindi per semplificare le letture e le scritture, i producer scrivono sui broker leader e i consumer leggono dai broker leader, mentre i follower agiscono come repliche di backup, cioè replicano lo stato della partizione nel broker leader.
Questi follower possono essere perfettamente allineati (in-sync) con il broker leader, cioè sono delle repliche esatte del leader. Oppure possono non essere perfettamente allineate (out-of-sync): ad esempio, una scrittura non viene immediatamente replicata sui follower.
Il broker leader viene scelto tramite un algoritmo di elezione tra i follower.

La sincronizzazione tra follower e leader può avvenire tramite due modalità:
1. Tramite il sistema esterno  **Zookeper** per la sincronizzazione: mantiene i metadati del sistema e gestisce la coordinazione tra followers e leader, per mezzo di Zookeper un algoritmo di consenso.
2. Alternativa recente: uso di un protocollo di consenso (Raft) implementato all'interno di Kafka.
### Producer
I producer sono le sorgenti dei dati, e pubblicano i dati a topic di loro scelta, mandando i dati direttamente al broker leader della partizione. Dunque in Kafka non c'è un livello di routing tra producer e broker.
Come già detto, il producer è responsabile nella scelta della partizione del topic nella quale verrà scritto il messaggio.
Se il producer non specifica niente, allora il messaggio è inserito in modo round-robin ad una partizione, altrimenti usa una chiave di partizionamento per indirizzare il messaggio verso una specifica partizione. 
Infine, più producer possono scrivere sulla stessa partizione.
### Consumer
Abbiamo visto come RabbitMQ utilizza un modello *push*, cioè il broker RabbitMQ spinge i messaggi verso i consumer. Il vantaggio principale del modello push è la semplicità nella gestione e implementazione, dato che non si lascia nessun compito ai consumer; di contro il broker si deve adattare alle possibili (e probabili) velocità differenti dei vari consumer nel processamento dei dati. Un altra decisione che un broker di tipo push deve prendere è se mandare subito un messaggio o aspettare per poi mandare un batch di messaggi.
L'alternativa è liberare il broker da questo compito di spingere i messaggi verso i consumer, facendo si che i consumer *tirano* i messaggi dal broker.
Questo è il cosiddetto modello *pull* nel quale è compito dei consumer andare a recuperarsi i messaggi dal broker.
Nel caso di Kafka, il consumer deve mantenere l'offset, cioè il punto fin dove il consumer ha letto, per identificare il prossimo messaggio che deve processare. I consumer possono leggere dalla stessa partizioni a partire da offset differenti.
![[SDCC/imgs/06-img15.png|center|600]]
Delegando la ricezione dei messaggi ai consumer, si da meno carico di lavoro ai broker, da cui si ottiene una migliore scalabilità; inoltre il sistema è flessibile alle diverse velocità dei consumer. Di contro, se non ci sono dati, il consumer potrebbe rimanere in polling (busy-waiting) sulla partizione in attesa che arrivino dati.

Dato che il consumer deve mantenere l'offset, per migliorare la sua tolleranza ai guasti conserva il valore dell'offset in una zona sicura di memoria ogni volta che legge un messaggio, ad esempio tramite Zookeeper o all'interno di un topic speciale di Kafka chiamato `__consumer_offsets`.
Dunque un consumer agisce come producer inviando l'offset a cui è arrivato in questo topic per tenerlo memorizzato all'interno di Kafka; in questo modo, se il consumer subisce un crash, può recuperare l'offset che aveva prima del crash contattando questo topic speciale. Questo meccanismo di tolleranza ai guasti, detto `auto-commit`, è attivo di default in Kafka.
### Consumer Group
Kafka permette di definire un gruppo logico di consumer, cioè un'insieme di consumer che cooperano tra di loro per leggere dati da uno stesso topic e che condividono un identificatore del gruppo.
Dal punto di vista di Kafka, un gruppo di consumer appare come un unico consumer logico, quindi il messaggio letto da un gruppo sarò consegnato ad un solo consumer nel gruppo; inoltre ciascun gruppo mantiene il suo offset di gruppo per una determinata partizione.
![[SDCC/imgs/06-img18.png|center|600]]

![[SDCC/imgs/06-img19.png|center|600]]
I gruppi di consumer sono utili per la scalabilità nei sistemi a microservizi: supponiamo di avere due microservizi che comunicano con Kafka
![[SDCC/imgs/06-img16.png|center|600]]
allora posso replicare i due servizi e mettere le repliche in uno stesso gruppo di consumer: in questo modo, i messaggi non verranno processati da tutte le repliche, ma solo da una di queste. Dunque svolge anche il ruolo di load balancer.
![[SDCC/imgs/06-img17.png|center|600]]
### Ordinamento
I messaggi pubblicati da un producer su una partizione di un topic sono ordinati rispetto all'ordine con cui sono stati inviati alla partizione. I consumer vedono i record in ordine rispetto a come sono salvati nella partizione.
Dunque Kafka garantisce l'ordinamento dei messaggi *solo rispetto ad una partizione*: i messaggi sono totalmente ordinati all'interno di una partizione, ma non è preservato l'ordinamento dei messaggi tra partizioni differenti, dunque l'ordinamento non è garantito a livello di topic.
Nonostante ciò, l'ordinamento per partizione unito alla possibilità da parte dei producer di indirizzare i messaggi per chiave in una specifica partizione del topic è sufficiente per la maggior parte delle applicazioni.
Un ordinamento a livello di topic richiederebbe l'implementazione da parte di ogni partizione di un algoritmo di consenso distribuito.

parallelismo e replicazione introducono due problemi: garanzia dell'ordinamento dei messaggi e qual'è la semantica di consegna esposta da kafka x i suoi utilizzatori.

### Semantica di consegna
Kafka offre e supporta tre semantiche di consegna dei messaggi: **at-least-once**, **at-most-once** e **exactly-once**.

```ad-hint
I messaggi non vengono cancellati dai topic, rendendo agevole la gestione di molteplici consumer.
```
#### at-least-once semantic
At-least-once è la semantica di default. Garantisce che nessun messaggio venga perso, ma può accadere che i messaggi siano duplicati (consumer ricevono più volte stesso messaggio). Inoltre, i messaggi potrebbero essere consegnati in un ordine differente rispetto all'ordine di produzione da parte dei producer.
Per implementare questa semantica, il producer dopo aver mandato un messaggio, aspetta un messaggio di acknowledge da parte del broker leader (settando il parametri `acks=1`).
Se non viene ricevuto l'acknowledge entro certo intervallo di tempo, il producer rimanda il messaggio al broker.
Per implementare la semantica lato consumer, è sufficiente fare commit dell'offset **dopo** aver processato correttamente il messaggio.
![[SDCC/imgs/06-img20.png|center|600]]
#### exactly-once semantic
Nella semantica exactly-once si deve garantire che nessun messaggio venga perso, che nessun messaggio sia duplicato e che si mantenga l'ordinamento, a costo di una maggiore latenza e un throughput minore.
Per ottenere tale semantica, è necessario implementare il meccanismo di acknowledge lato producer: si aspetta di ricevere un acknowledge da parte del broker leader della partizione; se non riceve l'acknowledge entro un certo timeout, il producer ritrasmette il messaggio.
Rispetto alla semantica at-least-once, nella semantica exactly-once è richiesto che il broker leader vada a effettuare la trasmissione del messaggio ricevuto sugli altri broker replica per quella partizione, e risponda al producer con l'acknowledge solo quando la scrittura è stata completata sull'insieme di repliche che sono *in-sync* rispetto al leader della partizione.
Questo meccanismo si ottiene settando il parametro di configurazione `ack=all` nel producer.
Per evitare i duplicati, occorre identificare in modo univoco ogni messaggio e ogni producer: utilizzando un ID per ogni producer e assegnando ad ogni messaggio un numero di sequenza si possono identificare in modo univoco i messaggi; in questo modo, è possibile identificare ed eliminare messaggi replicati e dunque mantenere le partizioni ordinate rispetto all'ordine di invio del producer. In questo modo, un producer diventa *idempotente*.
Infine, lato consumer, sarà necessario fare commit dell'offset dopo il processamento del messaggio e avere delle repliche *in-sync* degli offset committati.
L'exactly-once offerto da Kafka **non** è completa: infatti questa semantica è garantita per il flusso di dati prodotto da un singolo producer in una sessione; infatti, se avviene crash di un producer, il suo ID cambia.
![[SDCC/imgs/06-img21.png|center|600]]
#### at-most-once
In questa semantica, i messaggi devono essere consegnati al massimo una volta. Dunque è possibile che un messaggio venga perso e mai più consegnato. Questa semantica si ottiene possibile disabilitando il sistema di acknowledge (settando `acks=0`) e facendo commit dell'offset prima del processamento del messaggio, dunque immediatamente dopo la consegna di esso.

```ad-tip
title: Take-away
La scelta della semantica di delivery dipende dal contesto dell'applicazione.
```
### Tolleranza ai guasti
Kafka replica le partizioni per garantire tolleranza ai guasti. Ogni partizione ha una sua replica che è leader della partizione, che si occupa di aggiornare le altre repliche quando arrivano i messaggi. In particolare, le partizioni che sono repliche in-sync del leader sono dette ISR (In Sync Replicas).
Mantenere tante repliche in-sync, cioè allineate con la partizione nel leader, genera molto overhead; dunque le ISR non possono essere molte.
Nel caso di crash da parte del leader di una partizione, si elegge una nuova replica come leader tramite un algoritmo di elezione; naturalmente, se è presente, una replica in-sync verrà nominata come leader.
Kafka rende un messaggio disponibile su una partizione solo dopo che tutte le repliche ISR contengono il suddetto messaggio. Questo può rende il messaggio non immediatamente disponibile per il consumo.
Inoltre i producer, settando il parametro `acks`, può aspettare o meno che il messaggio venga committato, permettendo un tradeoff tra latenza e durabilità.
Infine, Kafka memorizza i messaggi per un periodo di tempo configurabile. Dunque il retention time definisce per quanto tempo i messaggi rimangono memorizzati prima di essere eliminati.
### ZooKeeper
**ZooKeeper** è un sistema distribuito utilizzato per coordinare i componenti di un differente sistema distribuito. Esso è un sistema di datastore chiave, valore con una organizzazione gerarchica simile all'organizzazione di un file system.
Permette la sincronizzazione, l'elezione e viene spesso utilizzato come sistema di monitoraggio. Può essere utilizzato per fare leader election, dato che implementa un algoritmo di consenso.
In Zookeeper, Kafka memorizza la lista dei broker, la configurazione dei topic (nro di partizioni, repliche, nome) e vengono anche configurate le Access Control List per i topic, cioè configurate le regole di accesso. In partica, fa da repository per i metadati e le impostazioni e li rende accessibili da tutti i broker di Kafka.
Quindi Zookeeper permette a Kafka di sapere quando ci sono delle modifiche, come ad esempio delle nuove partizioni.
#### Kraft
Utilizzare Kafka con Zookeeper significa utilizzare un sistema distribuito differente da Kafka per gestire e mantenere i metadati di gestione e consenso; dunque Zookeeper può diventare un bottleneck per Kafka nel momento in cui la dimensione del cluster aumenta.
Per evitare di utilizzare Zookeeper, esiste da qualche tempo **KRaft** come suo sostituto. Grazie a KRaft, i metadati di gestione di Kafka sono memorizzati direttamente nel cluster Kafka che attuano un algoritmo di consenso per mantenere le copie dei metadati sincronizzate tra di loro.
Per tale scopo, KRaft utilizza il protocollo di consenso Raft, che viene utilizzato anche per eleggere un broker leader nel caso il broker leader corrente subisce un fallimento.
La replicazione dei metadati su tutti i broker ha il vantaggio di essere più semplice e veloci, dato che i broker hanno già i metadati e non devono far riferimento a zookeeper per ottenerli. Questo porta anche ad una maggiore scalabilità.
### Kafka API
- Producer API: per pubblicare dati sui topic Kafka;
- Consumer API: per leggere dati da un topic Kafka;
- Kafka Connect API: per creare ed eseguire un connettore riutilizzabile che connette i topic Kafka ad applicazioni esterne al sistema. Esistono connettori preconfezionati per sistemi tipo S3, RabbitMQ, MySQL, Postgres...
- Kafka Streams API: permette di trasformare stream di dati da input topics in output topics;
- Admin API: per gestire e ispezionare i topic, i brokers e altri oggetti di Kafka.
## Protocolli di messaggistica
Per i sistemi a code di messaggi sono stati definiti diversi protocolli di messaggistica, ossia protocolli di livello applicativo che definiscono il formato di messaggi e tutti i dettagli implementativi relativi alla comunicazione.
Esistono diversi protocolli che sono open e standard: questo è importante perché riducono il rischio di vendor lockin, migliorando la portabilità da un sistema all'altro e l'interoperabilità tra sistemi differenti.
### AMQP
Protocollo per sistemi a code di messaggi di tipo open e standardizzato, supportato pienamente a livello industriale.
Esso è un protocollo di livello applicativo, basato sullo scambio di dati binari tramite TCP e aggiunge meccanismi di affidabilità aggiuntivi per supportare le diverse semantiche di delivery.
Inoltre è un protocollo programmabile, perché le entità e gli schemi di instradamento dei messaggi possono essere definiti da codice ed è implementato in molti sistemi a code di messaggi.
Il modello di riferimento di AMQP include 3 attori principali: publisher, subcriber e dei broker che gestiscono le code.
All'interno dei broker, sono presenti altre entità: le code di messaggi, un punto di exchange e dei binding, cioè dei collegamenti tra l'exchange e le code.

Direct exchange: il routing dei messaggio viene fatta in base ad una chiave specificata all'interno dello stesso.
Fanout exchange: broadcast del messaggio verso tutte le code collegate all'exchange. Vengono mandate repliche del messaggio verso tutte le code.
Topic exchange: permette di consegnare un messaggio ad uno o piu code basate sul matching di topic (un po piu ampio del direct exc). Può essere usato per implementare pattern pub/sub e spesso usato per fare multicast (ma non broadcast).
Header exchange: routing del messaggio utilizzando attributi contenuti nell header, usato per fare routing sulla base di molteplici attributi che vengono espressi nell'header del messaggio. 

AMQP supporta due tipi di messaggi
bare message: contiene il messaggio, delle proprietà del mittente e dell'applicazione, è il messaggio inviato dai producer. Questo messaggio può essere ampliato generando il messaggio annotato, perché gli intermediari opossono aggiungere ulteriori header e annotazioni. Il protocollo prevede che siano specificati diversi parametri di delivery, ad esempio ttl, priorità e requisiti di durabilità.
## Comunicazione multicast
La comunicazione **multicast** è un pattern di comunicazione di gruppo in cui i dati sono inviati a molteplici destinatari, ma non a tutti i destinatari possibili. Il pattern di comunicazione nel multicast può essere *uno a molti* o *molti a molti*.
La comunicazione broadcast è un caso particolare di multicast, in cui si mandano i messaggi a tutti i destinatari.

La soluzione più semplice per implementare multicast è l'invio di più repliche del messaggio quanti sono il numero di destinatari per mezzo di connessioni unicast multiple. Questa soluzione è estremamente inefficiente e non adatto alla scalabilità.
Per implementare il multicast in modo efficiente, bisogna replicare il messaggio solo quando è necessario, cioè quando il messaggio deve seguire percorsi differenti.

Il multicast quindi può essere realizzato:
- A livello di rete (IP-level): Protocollo IP Multicast. Replicazione dei pacchetti e il routing gestito a livello dei router di rete. Uso limitato in reti locali.
- A livello applicativo: replicazione e routing gestiti dagli host.

L'idea di base è organizzare i nodi interessati al multicast in un overlay network, una rete logica che coinvolge i nodi che devono inviare e ricevere dati nel multicast. Dunque si utilizza questo overlay network per distribuire i dati che fanno parte della comunicazione.
- Multicast strutturato: percorsi di comunicazione espliciti e ben definiti. Le topologie con cui distribuire i dati in un overlay network per fare multicast sono l'albero (unico percorso tra ciascuna coppia di nodi) e la mesh (più percorsi tra coppie di nodi).
- Multicast non strutturato: il multicast è basato su flooding, random walk oppure **gossiping**.
### gossiping
I protocolli (o algoritmi) basati su gossiping sono di tipo **probabilistico**. Il nome gossip deriva dal fatto che essi richiamano le modalità con cui i pettegolezzi si diffondono. Spesso gli algoritmi di gossiping sono anche chiamate algoritmi *epidemici*, perché il modo con cui si diffondono le epidemie è molto simile al modo in cui si diffonde l'informazione.
Nei sistemi distribuiti vengono utilizzati questi algoritmi perché consentono di diffondere informazioni in modo efficace su reti a larga scala.
Il principio alla base di un algoritmo di gossiping è quello di scegliere in modo probabilistico chi sarà il destinatario dell'informazione. Dunque essa non viene diffusa a tutti i possibili contatti di un nodo che possiede l'informazione, ma solo ad un sottoinsieme di questi che viene scelto in modo aleatorio.

Questo genere di algoritmi viene introdotto a fine anni 80 da Demers et al. in un lavoro riguardante la diffusione di informazione in modo consistente in un database replicato composto da centinaia di server.

Perché gossiping largamente utilizzato in sistemi distribuiti su larga scala?
I protocolli di gossiping, come meccanismo per la diffusione dell'informazione, hanno tante caratteristiche interessanti dal punto di vista dei sistemi distribuiti.
- sono semplici da gestire e implementare;
- sono decentralizzati, non c'è un autorità centrale che controlla l'algoritmo, decentralizzato tra tutti i nodi del sistema. Non ci sono dunque colli di bottiglia o single point of failure, caratteristici di soluzioni centralizzate.
- sono scalabili: ogni nodo invia messaggi ad un numero limitato di nodi, e dunque un numero limitato di messaggi indipendentemente dalla larghezza della rete, dunque scalano bene anche su sistemi composti da milioni di nodi.
- poiché i messaggi diffusi nella rete sono replicati, questa replicazione introduce robustezza e dunque tolleranza ai guasti rispetto alla perdita di informazioni.

#### strategie per la diffusione
Le operazioni, o strategie, fondamentali che compongono i protocolli di gossiping sono due: **anti-entropia** e **rumor spreading**.
- **Anti-entropia**: nella diffusione di informazione, è importante fare si che tutti i nodi della rete sappiano la stessa cosa. Questo meccanismo prevede che periodicamente un nodo selezioni un altro nodi e si scambi con esso gli aggiornamenti di informazioni che conosce, con l'obiettivo di arrivare ad avere uno stato identico su tutti i nodi. 
- **Rumor spreading**: sinonimo di gossiping. Periodicamente un nodo che ha nuove informazioni o aggiornamenti, seleziona un sottoinsieme di nodi che conosce e manda questi aggiornamenti. Un nodo che viene raggiunto da queste informazioni, può decidere di non continuare a propagarla.
Usando entrambe le strategia, il protocollo di gossiping risulta più veloce.
#### anti-entropia
La strategia dell'anti-entropia si applica con lo scopo di fare in modo che lo stato dei nodi sia il più possibile simile tra loro, diminuendo il disordine tra essi.
Si supponga di avere un nodo P che sceglie a caso tra i suoi vicini il nodo Q: come può il nodo P aggiornare Q? tre strategie differenti
- **push**: Solamente il nodo P manda i suoi aggiornamenti al nodo Q;
- **pull**: Solamente il nodo P chiede di ricevere gli aggiornamenti presenti nel nodo Q;
- **push-pull**: I nodi P e Q si scambiano gli aggiornamenti l'uno con l'altro.
Come si può vedere da questi grafici, la strategia più performante è la **push-pull**; infatti risulta che in $O(\log_2 N)$ round gli aggiornamenti si propagano su $N$ nodi, dove per *round* (o *gossip cycle*) si intende l'intervallo di tempo in cui ogni nodo comincia uno scambio di informazioni. 
![[SDCC/imgs/06-img22.png|center|600]]
Con *oblivius node* si intende il numero di nodi che non sono ancora stati raggiunti dall'informazione.
![[06-img23.png|center|600]]
#### rumor spreading
Nella strategia **rumor spreding** (in italiano, *diffusione del pettegolezzo/informazione*), 
Si suppone di avere un nodo P con un aggiornamento, che viene inviato ad un nodo Q scelto a caso.
Può accadere che li nodo Q conosca già tale aggiornamento, e quindi il nodo P *può perdere interesse* nel continuare a diffondere questa informazione. Sia p_stop la probabilità con cui un nodo smette di diffondere informazioni. Si può dimostrare che la frazione dei nodi che non sonostati raggiunti dall'informazione è dipendente da p_stop ed è dato dalla seguente equazione trascendentale. s=percentuale dei nodi non raggiunti dall'informazione.

Al crescere di p_stop, aumenta s. 
Dunque  se p_stop è molto alto, è bene usare questa strategia in associazione all anti entropia per migliorare la velocità con cui l'informazione si diffonde.

### Schema generale
Si supponga di avere due nodi P e Q che si scambiano informazioni, e sia P il nodo attivo, cioè quello che seleziona il nodo Q e avvia lo scambio di informazioni. Inoltre si supponga che tale algoritmo viene eseguito periodicamente da ciascun nodo.

Innanzitutto il nodo P sceglie il nodo Q per mezzo di una strategia (ad esempio con probabilità uniforma). Dopodiché, P seleziona dalle informazioni in suo possesso che cosa inviare al nodo Q (tale selezione può dipendere da diversi fattori) e le invia. Il nodo Q riceve queste informazioni dal nodo P e a sua volta seleziona dalle informazioni che conosce un sottoinsieme di esse da inviare a Q (strategia push-pull). Ricevuti i dati da Q, i due nodi applicano una strategia per decidere quali delle informazioni ricevute rispettivamente mantenere. Infine, i dati mantenuti vengono processati dai nodi.

Questo schema è comune nella gran parte degli algoritmi di gossiping, ed è abbastanza semplice. La trattazione si complica nel momento in cui si entra nei dettagli di alcuni aspetti cruciali presenti nello schema generale:
- Come effettuare la selezione dei nodi? Ad esempio scegliendo i nodi in modo uniforme.
- Quali dati scambiare? Dipende fortemente dall'applicazione e dal contesto in cui l'algoritmo è utilizzato. Ad esempio, si supponga di usare il gossiping per implementare un meccanismo di failure detection: si vogliono diffondere informazioni riguardo ai nodi falliti nel sistema. I dati da inviare potrebbero essere: i nodi che sono stati contattati nell'ultimo round che non hanno risposto al ping.
- Come processare i dati scambiati? Anche in questo caso, dipende fortemente dall'applicazione 

#### gossipig vs flooding
Consideriamo il flooding nella figura, e confrontiamolo con il gossiping.

![[06-img24.png|center|600]]

Si considera il gossiping usando unicamente la strategia del rumor spreading, senza push-pull, in cui un nodo invia un messaggio ai suoi vicini con probabilità $p$, si ottiene il seguente risultato.

![[06-img25.png|center|600]]

Dunque il flooding, rispetto al gossiping, costa di più in termini di messaggi scambiati, ma è più veloce nel diffondere l'informazione, dato che l'informazione viene inoltrata a tutti i vicini di un nodo.

Le caratteristiche del gossiping sono:
- algoritmo di tipo probabilistico;
- Ciascun nodo prende una decisione nel diffondere l'informazione su base locale, che risulta in un effetto globale.
- Leggero, con meccanismi semplici da implementare;
- Tollerante ai guasti, dato che si ha replicazione dei messaggi.
Da parte sua il flooding raggiunge un'ampia copertura della rete e ogni nodo ha bisogno di informazioni di stato minime; di contro, inonda la rete con molti messaggi ridondanti.

Il gossiping ha l'obiettivo di ridurre le ritrasmissioni ridondanti che accadono nel flooding, cercando di mantenerne i vantaggi. Per la sua natura probabilistica, nel gossiping non si può garantire che tutti i nodi vengano raggiunti e il tempo per completare la diffusione è maggiore rispetto al flooding.

- peer sampling: strategia che permette in modo decentralizzato di rendere disponibile ad ogni nodo della rete una lista dei nodi con cui scambiare le informazioni
- gestione delle risorse: meccanismo di supporto, ad esempio scambiare dati di monitoraggio o informazioni sui nodi che hanno subito fallimenti
- computazioni distribuite: 
#### Blind counter rumor mongering
Il nome deriva dal fatto che in questo algoritmo un nodo con informazioni aggiornate le diffonderà periodicamente verso gli altri nodi, facendo si che un nodo smetta di inviare il messaggio a prescindere dal ricevente. Inoltre in questo algoritmo un nodo perde interesse nel diffondere informazione dopo aver avuto un certo numero di contatti.

Questo algoritmo ha due parametri per controllare il gossiping:
- B: numero massimo di vicini a cui un nodo va ad inoltrare il messaggio;
- F: numero di volte che un nodo inoltra lo stesso messaggio ai suoi vicini.

```
BCRM:
un nodo n inizia il broadcast mandando un messaggio m a B dei suoi vicini, scelti casualmente

Quando un nodo p riceve un messaggio m dal nodo q
	se p ha ricevuto m non più di F volte:
		p invia m a B vicini scelti u.a.r. tali che p sa
		che non hanno ancora visto
	
```

```ad-note
Un nodo $p$ sa se il suo vicino $r$ ha già ricevuto il messaggio $m$ solo se $p$ ha inviato $m$ ad $r$ precedentemente, o se $p$ ha ricevuto $m$ da $r$
```

Settando i parametri $B=F=2$, si è osservato che, sperimentalmente, l'algoritmo BCRM rispetto al flooding:
- invia la metà dei messaggi;
- copre il 90% dei nodi;
- è due volte più lento.

#### bimodal multicast (probabilistic broadcast)
Il bimodal multicast è un algoritmo che utilizza il gossiping, con l'obiettivo di effettuare il multicast in modo affidabile, in cui con alta probabilità un messaggio raggiunge tutti i destinatari.
L'algoritmo è composto da due fasi:
1. message distribuition: in cui un nodo invia un messaggio in multicast senza particolari garanzie di affidabilità;
2. gossip repair: dopo che un nodo riceve un messaggio, comincia a contattare un insieme di nodi per scambiare informazioni riguardo i messaggi ricevuti. Dunque tramite questa fase di gossiping, si verifica che il messaggio sia arrivato correttamente a tutti i destinatari. Se un nodo riscontra dei messaggi mancanti, può chiederli ai nodi che li hanno ricevuti

##### message distribution
fase 1. meccanismo di multicast standard senza garanzie di affidabilità (ip o applicativo): alcuni messaggi potrebbero perdersi (in rosso).
##### gossip repair
fase 2. periodicamente, ciascun nodo invia una descrizione del proprio stato a un sottoinsieme di nodi che sceglie in modo casuale. Non invia i messaggi, ma un summary dei messaggi (ad esempio digiest dato da una hashfunc). A questo punto, chi riceve digest confronta cio che ha ricevuto con il suo stato, cioè con i messagi che ha ricevuto: in questo modo si accorge se non ha ricevuto qualche messaggio, e può andare a chiedere a quel nodo che cosa non ha ricevuto.

si chiama multicast bimodale non perché ha due fasi, ma perché dall'algoritmo risulta un comportamento duale. In particolare, accade che l'algoritmo è in grado con alta probabilità di inviare il messaggio a tutti quanti i possibili destinatari; può anche accadere che il messaggio non arriva a nessuno, nel caso in cui il mittente subisce un fallimento. Invece la probabilità che il messaggio venga recapitato solo da alcuni nodi (ad esempio la metà) è prossima allo zero. Quindi il messaggio o viene recapitato a tutti o a nessuno.
Una sorta di comportamento atomico: o quasi tutti o quasi nessuno.

Un secondo comportamento bimodale dell'algoritmo risiede nella latenza di delivery. Se la rete è affidabile e i nodi non subiscono guasti, allora i messaggi arrivano a tutti i  destinatari molto rapidamente. Se invece un messaggio è perso per molti dei destinatari, allora ci vogliono molte iterazioni del meccanismo di gossip repair per far ottenere il messaggio a tutti i destinatari.
### pub/sub subscription
subscriber si sottoscrive a eventi interessati, publisher pubblica eventi: middleware deve fare matching tra messaggi pubblicati e sottoscrizioni, in modo da mandare notifiche ai subscriber.
soluzione 1: centralizzata. Server centralizzato che si occupa di gestire sottoscrizioni e notifiche. Facile implementare matching tra eventi e consumer, buona soluzione per deployment su piccola scala. Problemi: scalabilità e single point of failure.

soluzione 2: distribuita. Infrastruttura distribuita utilizzando piu server:
1. partizionamento: suddividere la responsabilità di gestire alcuni topic su molteplici server e quindi andiamo ad utilizzare ad esempio un pattern master/worker: arch gerarchica in cui il master distribuisce il meccanismo di matching a più worker e ogni worker quindi mantiene e gestisce un sottoinsieme di sottoscrizioni e consumer. Partizionamento facendo hash sul nome del topic per mappare sottoscrizioni con subscribers. Problema: punto di centralizzazione rappresentato dal master. Altrimenti evitiamo singolo master e 
2. overlay network: server decentralizzato organizzati su overlay network strutturata o non strutturata, usando i meccanismi già visti come flooding e gossiping