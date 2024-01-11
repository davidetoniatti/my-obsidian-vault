La comunicazione nei sistemi distribuiti è tipicamente basata su **scambio di messaggi**: messaggi di invio e di ricezione come nella classica architettura client/server di base.
Per consentire lo scambio di messaggi, le parti coinvolte devono essere in accordo su una serie di dettagli di basso livello, ed è nota la soluzione per affrontare questi problemi: *dividere la comunicazione di rete in livelli*. Sono argomenti per un corso di reti, dunque non vengono trattati questi aspetti nel corso.
Si considererà una architettura a livelli che ha al di sopra del livello di sistema operativo il **middleware**.
### Livello di middleware
Il **middleware** è un livello intermedio tra il sistema operativo e le applicazioni che ha il compito principale di astrarre la complessità sottostante mascherando l'eterogeneità. L'obiettivo di un middleware è essere utilizzato da applicazioni differenti e essere indipendente dalla specifica applicazione.
![[05-img01.png|center|500]]
## Tipi di comunicazione
La comunicazione verrà trattata rispetto a tre caratteristiche:
- **Persistenza**: cioè la possibilità di memorizzare i messaggi per un certo periodo di tempo (persistente) o meno (transiente); 
- **Sincronizzazione**: cioè la possibilità di avere comunicazione sincrona (bloccante, attende la ricezione della risposta) o asincrona (non bloccante, handler che permette al mittente di fare altro e poi recuperare il messaggio di risposta);
- **Dipendenza temporale**: cioè la possibilità di avere comunicazione discreta o in streaming, in cui i pacchetti informativi sono collegati temporalmente tra loro.
### Persistente vs transiente
#### Comunicazione persistente
Nella comunicazione persistente, i messaggi sono salvati dal middleware fintantoché il messaggio non è consegnato al destinatario. Dato che il messaggio inviato è mantenuto e gestito dal middleware di comunicazione, né il mittente né il destinatario devono rimanere necessariamente attivi durante la comunicazione.
#### Comunicazione transiente
Nella comunicazione transiente, i messaggi sono salvati dal middleware soltanto fino a che mittente e destinatario sono in esecuzione: entrambi devono rimanere attivi durante la comunicazione. Più precisamente, se il middleware non può consegnare un messaggio a causa di un'interruzione della trasmissione o perché il destinatario non è attivo, il messaggio viene semplicemente scartato. Per esempio i router si comportano in questo modo: salvano e inoltrano i messaggi, ma li scartano se non è possibile inoltrarli.
### Sincrona vs asincrona
#### Comunicazione sincrona
Nella comunicazione sincrona, il mittente è bloccato fino a quando la sua richiesta non viene accettata. Dunque le operazioni di invio e ricezione sono **bloccanti**.
I punti in cui può avvenire la sincronizzazione sono tre.
- Il mittente può rimanere bloccato finché il middleware non comunica che si occuperà della trasmissione della richiesta;
- Il mittente può sincronizzarsi quando la sua richiesta è stata consegnata al destinatario;
- La sincronizzazione può avvenire lasciando che il mittente attenda fino a quando la sua richiesta sia stata completamente elaborata, cioè fino al momento in cui il destinatario restituisce una risposta.
![[05-img02.png|center|500]]
#### Comunicazione asincrona
Nella comunicazione asincrona, un mittente continua la sua esecuzione immediatamente dopo aver inviato il suo messaggio per la trasmissione. Ciò significa che il messaggio viene memorizzato (temporaneamente) dal middleware immediatamente dopo l'invio. In questo caso l'operazione di invio è non bloccante, mentre l'operazione di ricezione può essere bloccante o non bloccante.
### Discreta vs streaming
#### Comunicazione discreta
Nella comunicazione discreta, ogni messaggio forma un'unità completa di informazione; messaggi diversi non sono temporalmente legati.
#### Comunicazione streaming
Nella comunicazione in streaming, l'invio di informazione implica l'invio di molteplici messaggi in **relazione temporale** o comunque **relazionati da un ordinamento**, il quale deve essere mantenuto per poter ricostruire correttamente l'informazione.
### Combinazione dei tipi di comunicazione
Vediamo le caratteristiche dei tipi di comunicazione ottenuti combinando diverse caratteristiche di *persistenza* e *sincronizzazione*.
#### Persistente e asincrona
In questo caso i due estremi non si bloccano mai e non devono essere necessariamente operativi durante la comunicazione; i messaggi sono salvati temporaneamente in un middleware di comunicazione. Questa tipologia di comunicazione si ha nei sistemi di messaggistica quali Teams o le email, e anche nei middleware di comunicazione orientati ai messaggi.
#### Persistente e sincrona
In questo caso, il mittente rimane bloccato in attesa che il messaggio venga memorizzato in qualche locazione accessibile al destinatario (ad esempio un middleware di comunicazione lato destinatario). Dunque il destinatario non deve essere necessariamente attivo nel momento dell'invio del messaggio.
![[05-img03.png|center|500]]
#### Transiente e asincrona
Un esempio di questo tipo di comunicazione si ha nel protocollo UDP, in cui il mittente invia il messaggio quando il destinatario è in esecuzione ma non attende che il messaggio sia ricevuto correttamente dal destinatario, il quale non viene memorizzato da un middleware. Dunque se il destinatario non è raggiungibile, il messaggio potrebbe perdersi completamente.
![[05-img04.png|center|400]]
#### Transiente e sincrona
Sono possibili diverse tipologie di comunicazione transiente e sincrona, in base a fin quando il mittente rimane bloccato.
##### Basata su ricezione
In questo caso il mittente rimane bloccato fino a che il messaggio si trova nello spazio di memoria del destinatario. Dunque non attende che il destinatario veda il messaggio, ma attende che il messaggio si trovi in uno spazio che sarà accessibile in futuro dal destinatario.
![[05-img05.png|center|500]]
##### Basata su consegna
In questo caso il mittente rimane bloccato fino a che il messaggio risulta consegnato al destinatario. Dunque non attende che il destinatario processi il messaggio.
![[05-img06.png|center|500]]
##### Basata su risposta
In questo caso il mittente rimane bloccato fino a che il messaggio risulta processato dal destinatario, cioè continua la sua esecuzione quando riceve un messaggio di risposta dal destinatario.
![[05-img07.png|center|500]]
## Semantiche di fallimento
Nella comunicazione di rete, tra mittente e destinatario possono capitare diverse tipologie di fallimenti. Ad esempio, i messaggi si possono perdere o arrivare in ritardo. Oppure fallimenti che riguardano il lato client o il lato server della comunicazione. In particolare, il server potrebbe subire un fallimento:
1. Prima di aver ricevuto una richiesta;
2. Dopo aver ricevuto una richiesta, ma prima di aver processato il messaggio;
3. Dopo aver processato il messaggio, dunque prima di inviare la risposta.
In generale, il client non capisce se il fallimento avviene prima o dopo aver processato la richiesta: vede solo che non arriva un messaggio di risposta.
Infine, il client potrebbe subire un crash dopo aver inviato un messaggio di richiesta.

In presenza di fallimenti che possono avvenire lato client o lato server si distinguono quattro tipi di **semantiche di comunicazione** (semantica della comunicazione: sapere che cosa è successo alla comunicazione):
- Semantica **May-be**: forse il messaggio è stato ricevuto o processato;
- Semantica **At-least once** (almeno una volta): il client sa che il messaggio è stato consegnato/processato dal server almeno una volta;
- Semantica **At-most once** (al più una volta): il client sa che il messaggio è stato consegnato/processato al più una volta;
- Semantica **Exactly once** (esattamente una volta): il client sa che il messaggio è stato consegnato/processato precisamente una volta.
Le semantiche di comunicazioni sono riportate in ordine crescente di garanzie, e quindi in ordine crescente di complessità implementativa.
Infatti, maggiori garanzie richiedono maggiori meccanismi da implementare per supportarle.
Spesso i middleware ci forniscono semantica at least e at most. Garanzie complete costano troppo al sistema.
### Meccanismi per le semantiche di fallimento
Si implementano uno o più di questi tre meccanismi di base per garantire una (o più) semantica di fallimento:
1. **Request Retry (RR1)**: è un meccanismo lato client in cui se non vede arrivare il messaggio di risposta ritrasmette la richiesta, fintantoché riceve risposta o dopo un certo numero di ritrasmissioni fallite.
2. **Duplicate Filtering (DF)**: è un meccanismo lato server implementato per filtrare richieste duplicate inviate dallo stesso client;
3. **Result Retransmit (RR2)**: è un meccanismo lato server per cui è in grado di ritrasmettere una risposta già calcolata in precedenza per una stessa richiesta inviata dal client. Questo meccanismo è necessario nel caso in cui il servizio offerto dal server non è **idempotente**.
Si osservi che questi meccanismi sono implementati nel lato client e server **del middleware**, non direttamente su client e server. Quindi nel resto della trattazione, si parlerà di client e server in riferimento al middleware lato client e middleware lato server.
### Semantica May-be
La semantica May-be è quella che offre le minime garanzie. In particolare, non c'è alcuna garanzia che il messaggio di richiesta venga processato o meno dal server. Dunque non si prendono accorgimenti per garantire una comunicazione affidabile e non viene implementato nessuno dei meccanismi visti (RR1, DF, RR2).
![[05-img08.png|center|600]]
### Semantica At-least once
Nella semantica **at-least once** si garantisce che la richiesta di processamento viene eseguita *almeno* una volta, cioè il processamento del messaggio (aka il servizio) viene eseguito *almeno* una volta.
Per garantire questa semantica è necessario che il client deve implementare li meccanismo RR1. Dato che in questa semantica le richieste possono essere servite più di una volta, non è necessario che il server implementi il meccanismo DF o RR2.
Dato che il server non è in grado di trovare richieste duplicate, questa semantica è adatta per servizi che sono *idempotenti*.
Dunque il client non sa quante volte ha computato la richiesta inviata al server: infatti non è a conoscenza completa dello stato del server. Il client sa solo che la richiesta è stata sicuramente processata una volta quando riceve il messaggio di risposta.
La richiesta potrebbe essere stata computata più di una volta se, ad esempio, il server fallisce dopo la computazione ma prima dell'invio della risposta; oppure, se il messaggio di risposta si perde.
Senza meccanismo RR2, il server computa nuovamente la richiesta già processata.
Il meccanismo di ritrasmissione viene implementato con un timeout: se allo scadere del timeout il client non riceve risposta, rimanda la richiesta.
Se non è implementato nessun meccanismo dal server, allora un eventuale riconoscimento delle richieste duplicate deve essere implementato a livello applicativo nel server (vero e proprio, non middleware lato server).
![[05-img09.png|center|500]]
### Semantica At-most once
Nella semantica **At-most once** si garantisce che il servizio ha elaborato la richiesta *al più* una volta. Dunque quando il client riceve il messaggio di risposta, sa che la computazione è avvenuta solamente una volta. Se non riceve una risposta, allora il client non sa se computazione è stata eseguita o meno:  la computazione lato server potrebbe non essere stata eseguita.
Per ottenere questa semantica di fallimento, vengono implementati tutti i meccanismi:
1. Se il client non riceve risposta, deve essere in grado di ritrasmette la richiesta al client.
2. Il server deve essere in grado di filtrare i duplicati e ritrasmettere il risultato. Dunque deve essere in grado di identificare le richieste duplicate e deve essere in grado di evitare di riprocessare la stessa richiesta.
Il meccanismo di filtraggio di duplicati rende questa semantica adatta anche per servizi *non idempotenti*.
Anche in questa semantica però non si hanno garanzie complete, perché non c'è stretta una collaborazione tra client e server. In particolare, se accade un fallimento, il client non sa se il servizio è stato eseguito o meno, e inoltre il server non sa se il client sa che il servizio è stato eseguito.
![[05-img10.png|center|500]]

Il server deve essere in grado di identificare le richieste duplicate e di restituire un risultato già calcolato in precedenza. Le richieste duplicate vengono identificate usando un **id** univoco per le richieste: il client deve utilizzare lo stesso **id** nel caso di ritrasmissione di un messaggio.
In particolare
1. Per garantire che l'id sia univoco, si può utilizzare una funzione hash crittografica per ogni messaggio. Oppure ad ogni client viene associato un identificativo e il client mantiene un contatore che viene incluso nella richiesta e viene incrementato ad ogni nuova richiesta da inviare; in questo modo un messaggio ritrasmesso avrà lo stesso indice e stesso client id, e il server sarà in grado di intercettarlo.
2. Il server non può tenere traccia all'infinito di risposte calcolate in precedenza, dunque, a un certo punto, deve cancellare dei risultati già calcolati. Ad esempio, si possono mantenere le informazioni per un tempo massimo oppure usare un approccio del tipo sliding window.
3. Il server deve essere in grado di gestire il caso in cui arriva una richiesta ritrasmessa che però sta ancora processando sul server. Questo fatto può accadere quando il timeout di ritrasmissione è troppo basso. 
### Semantica Exactly once
Nella semantica **Exactly once** si garantisce che il server esegue il processamento di una richiesta **esattamente** una volta. Oppure, si garantisce che il middleware di comunicazione consegna esattamente una volta il messaggio al destinatario.
Questa è la semantica più complessa e onerosa da implementare, perché i tre meccanismi (RR1, DF, RR2) non sono più sufficienti: se il client non riceve risposta, non sa cosa è successo.
Dato che i meccanismi da implementare per ottenere questa semantica sono molteplici, risulta spesso oneroso ottenere questo tipo di garanzie.
Sostanzialmente, in questa semantica è necessaria una conoscenza completa, cioè se il servizio è stato eseguito esattamente una volta oppure non è stato seguito: *all-or-nothing*.
Se va tutto liscio, allora il servizio è stato eseguito solo una volta, dato che le richieste duplicate vengono filtrate. Se invece qualcosa va storto, il client o server sanno se il servizio è stato eseguito o meno.
Dunque in questa semantica c'è conoscenza concordante di ogni stato dell'altro. Inoltre non ci devono essere vincoli sulla massima durata massima del protocollo di interazione tra client e server. Quest'ultimo è il motivo per cui questa semantica non viene mai realizzata pienamente in pratica.
Infine, per ottenere questa semantica sono necessari dei meccanismi aggiuntivi per garantire la tolleranza ai guasti lato server: replicazione trasparente lato server, write-ahead logging e meccanismi di recovery che permettano al server di riprendere l'esecuzione da uno stato coerente nel caso di fallimento.
Un meccanismo di recovery può essere complicato da implementare nei sistemi distribuiti. Infatti è necessario salvare lo stato del server, uno *snapshots*, in modo da identificare lo snapshot più recente e ripartire da esso nel caso di fallimento. Però quando nel sistema è presente la replicazione, non è semplice qual'è lo snapshot di sistema da cui ripartire, dato che ogni server avrà il suo snapshot, quindi bisogna identificare lo snapshot corretto dal quale riprendere.
### write-ahead log
Il pattern [**Write-ahead logging (WAL)**](https://martinfowler.com/articles/patterns-of-distributed-systems/write-ahead-log.html), anche detto *commit log*, ha l'obiettivo di fornire garanzie di durabilità, rendendo persistente ciascun cambiamento di stato. Questo scopo viene raggiunto inserendo ogni cambiamento di stato  in un log di tipo append-only, cioè un log che viene scritto aggiungendo in modo sequenziale ogni entrata alla fine.
Quindi ogni volta che c'è cambiamento di stato, questo viene memorizzato come nuova riga del file di log alla fine del file.
In caso di fallimento, al riavvio il sistema può leggere il file di log e recuperare lo stato precedente al fallimenti rieseguendo i cambiamenti descritti nel log.
Questo file viene memorizzato su disco, allora in un sistema distribuito si pone il problema del mantenere l'ordinamento corretto dei cambiamenti.
Scrivendo il file di log in maniera sincrona, cioè bloccando tutto il sistema distribuito fino a che tutti i file di log sono sincronizzati, si potrebbe creare un overhead non accettabile.
Dunque nella pratica, il log viene scritto su un buffer in memoria, e in modo asincrono viene fatto il flushing (trasferimento) dei log nel buffer su disco o supporto esterno.
![[05-img11.png]]
### Summing up
Nei sistemi distribuiti si tende ad usare le semantiche *at-least once* e *at-most once* perché sono un compromesso in termini di garanzie e overhead sulle prestazioni. Si tende a preferire l'*at-least once* perché genera poco overhead ed è più semplice da scalare.
> **Distributed systems are all about trade-offs!**
# Distributed application programming
Nei corsi di reti di calcolatori si è già stati introdotti alla programmazione di rete *esplicita*, ossia la programmazione attraverso le *socket*. In questo contesto, le applicazioni vengono sviluppate utilizzando i costrutti di base dei sistemi operativi, per cui è necessario *implementare* una gestione esplicita dello *scambio di messaggi*. Sviluppando le applicazioni distribuite in questo modo, **non si ottiene** alcuna trasparenza alla distribuzione e la programmazione delle componenti per la comunicazione deve essere fatta in modo **esplicito** dal programmatore.
Allora, si può aumentare il livello di astrazione nella programmazione distribuita tramite un **middleware di comunicazione**, che permette di nascondere la complessità dei layer hardware e software sottostanti, di liberare il programmatore da tasks automatizzabili e dall'implementazione dei meccanismi di comunicazione. Inoltre, genera un miglioramento della qualità del software tramite il riutilizzo di soluzioni che sono efficienti, correte e ben testate.

Nasce quindi la programmazione di rete **implicita** con l'uso del meccanismo di **chiamata a procedura remota**.
In particolare, esistono due tecnologie per chiamata a procedura remota:
- **Remote Procedure Call (RPC)**: l'applicazione distribuita è realizzata per mezzo di chiamate a procedure dove però il soggetto chiamante (client) della procedura e il soggetto chiamato (server) che esegue la procedura chiamata sono localizzati in macchine remote e i dettagli di comunicazione tra queste due entità sono nascosti al programmatore.
- **Remote Method Invocation (Java RMI)**: l'applicazione distribuita in Java è realizzate per mezzo di chiamate a metodi di oggetti remoti, cioè questi metodi vengono eseguiti su oggetti che si trovano in macchine remote.
L'idea del meccanismo di chiamata a procedura remota risale agli anni 80: consiste nell'utilizzare il modello client/server per chiamare una procedura che viene eseguita su un'altra macchina.
Il meccanismo generale è il seguente:
- Il processo client sulla macchina A invoca una procedura sulla macchina B per mezzo di un messaggio.
- Il processo chiamante in A è sospeso, in attesa del valore di ritorno da parte del server B: la comunicazione è sincrona e bloccante;
- La procedura chiamata è eseguita localmente sul server B;
- Parametri di input e di output sono trasportati per mezzo di messaggi;
- Lo scambio di questi messaggi è **trasparente** al programmatore;
- **Comunicazione di tipo transiente**: A e B sono entrambi presenti all'atto della comunicazione;
- Questo processo è reso **trasparente allo sviluppatore** e quanto più possibile simile ad una chiamata di procedura locale.
![[05-img12.png]]
Il meccanismo RPC viene usato in molte applicazioni distribuite, tra cui applicazioni cloud: molti linguaggi la supportano ed esistono diversi framework a tale scopo.
### Chiamata a procedura locale
Si rivede con un esempio come funziona il meccanismo di chiamata a procedura locale, si prenda in considerazione la chiamata `newlist = append(data, dbList)`.
![[05-img16.png|center|500]]
Il processo chiamate inserisce nello stack i parametri di input e l'indirizzo del valore di ritorno. Quando il procedura chiamata ritorna, il controllo del flusso di esecuzione ritorna al processo chiamante.

**Come si può rendere RPC simile ad una chiamata a procedura locale?**
## Architettura RPC
Per la realizzazione del meccanismo RPC in modo da essere trasparente allo sviluppatore si aggiunge un livello di **indirezione**. Si aggiungono due componenti intermediarie: un proxy  lato client detto **client stub** e proxy lato server detto **sever stub**.
- **Client stub**: espone l'interfaccia del servizio che verrà eseguito dal server remoto.
- **Server stub**: riceve le richieste e si occupa di gestire la chiamata locale e restituire il messaggio al client stub.
![[05-img13.png|center|500]]
Ricordando che l'obiettivo è la trasparenza alla distribuzione, è conveniente che questi intermediari siano generati automaticamente cosicché lo sviluppatore si possa concentrare sullo sviluppo della logica dell'applicazione.
### Basic steps
Gli step basici dell'architettura RPC sono i seguenti:
1. Lato client, il client manda il nome e i parametri di input della procedura da chiamare al *client stub*;
2. Il client stub si occupa di creare un *messaggio di richiesta* con le informazioni ricevute dal client, e manda il messaggio al sistema operativo locale;
	- In questo stadio avviene il **marshaling** dei parametri: gli argomenti della funzione sono convertiti in un formato comune e vengono inseriti nel messaggio.
3. Il sistema operativo del client utilizza i meccanismi di comunicazione di rete per inviare il messaggio al server;
4. Il sistema operativo nel server riceve il messaggio di richiesta e lo inoltra al *server stub*;
5. Il server stub *spacchetta il messaggio di richiesta* recuperando tutte le info necessarie per chiamare la procedura richiesta nel server come una semplice procedura locale;
	- In questo stadio avviene l'**unmarshaling** dei parametri: gli argomenti della funzione vengono estratti dal messaggio e convertiti dal formato comune ad un formato interpretabile localmente.
6. Il server esegue la procedura locale e comunica il valore di ritorno al server stub;
7. Il server stub crea un *messaggio di risposta* contenente il valore di ritorno (con operazione di marshaling sul valore di ritorno);
8. Il sistema operativo del server utilizza i meccanismi di comunicazione di rete per inviare il messaggio al client;
9. Il client riceve il messaggio e lo inoltra al *client stub*;
10. Il client stub *spacchetta il messaggio di risposta* e ritorna il risultato contenuto nel messaggio al client (con operazione di unmarshaling sul valore di ritorno);
![[05-img14.png|center|600]]
### Ingredienti principali
Gli ingredienti (che il middleware di comunicazione dovrà gestire) necessari nell'architettura RPC sono:
- **Scambio di messaggi**: lo sviluppatore non deve vedere lo scambio di messaggi, cioè deve avere l'illusione che la sua chiamata sia di tipo locale. I principali fattori da gestire sono l'identificazione dei messaggi di richiesta e risposta e il passaggio dei parametri.
- **Gestione eterogeneità dei dati**: i dati coinvolti in RPC, ossia i parametri e i valori di ritorno, sono di natura eterogenea, dunque è importante gestirli tramite le tecniche di *marshaling* e *serializzazione*:
	- **marshaling**: impacchettare i parametri in modo che possono essere ricostruiti (unmarshaled) da un altro processo;
	- **serializzazione**: conversione degli oggetti in una sequenza di bytes che può essere trasferita in rete: la serializzazione viene utilizzata nel marshaling.
- **Gestione dei fallimenti** dovuti alla distribuzione: fallimenti che possono avvenire sia per errori utente, ma sopratutto per errori durante le fasi di comunicazione.
### I problemi da gestire in RPC
Si vede ora come vengono affrontati in generale nei middleware di comunicazione per RPC i seguenti problemi:
1. Gestire **eterogeneità** nella **rappresentazione dei dati**
	- Altro dettaglio: i proxy lato client e server devono anche decidere quale protocollo di trasporto utilizzare: TCP, UDP, o entrambi?
2. Gestire il passaggio di **parametri per riferimento**: i processi client e server sono in esecuzione su macchine differenti, ognuna con il proprio spazio di indirizzamento. Necessario un meccanismo che consenta di simulare il passaggio dei parametri per riferimento.
3. Definire la **semantica dei fallimenti**: nel caso della procedura locale, si è nel caso *exactly once* (nel momento in cui viene chiamata, sicuramente la procedura viene eseguita esattamente una volta). Nel caso di RPC è necessario scegliere quali delle quattro semantiche esistenti adottare: solitamente in RPC vengono adottate le semantiche *at-least once* o *at-most once*.
4. Gestire il collegamento (**binding**) tra client e server: come si localizza l'endpoint del server che verrà chiamato.
Si vedono ora, in generale, quali sono le soluzioni a tali problemi elencati.
#### Eterogeneità dei dati
Il client e il server, essendo macchine differenti, potrebbero usare rappresentazioni dei dati che sono diverse tra di loro: ad esempio, una macchina adotta l'ordinamento dei byte little endian, mentre l'altra adotta big endian.
Le soluzioni generali (non solo per RPC) per gestire l'eterogeneità nella rappresentazione dei dati sono:
1. Invio del messaggio specificandone l'encoding;
2. Conoscendo il formato che il destinatario si aspetta di ricevere, il mittente converte la sua rappresentazione dei dati in quella che si aspetta il destinatario.
3. Conversione in formato *comune* per la rappresentazione dei dati
	- Mittente: converte dal suo formato locale al formato comune;
	- Destinatario: converte dal formato comune al suo formato locale
4. Si lascia ad un intermediario (middleware di comunicazione) il compito di convertire la rappresentazione dei dati in diversi encoding.
Comparando le soluzioni 2 e 3, assumendo $N$ componenti distribuite:
- Nella soluzione 2, ogni componente conosce **tutte** le funzioni di conversione: le conversioni sono molto veloci e ognuno riceve con la rappresentazione attesa, ma ogni componente deve conoscere $N-1$ funzioni di conversione, dunque in totale nel sistema si hanno $N(N-1)$ funzioni di conversione, ***na fracca!***
- Nella soluzione 3, tutte le componenti *concordano su un encoding comune* per la rappresentazione dei dati e dunque ogni componente è in grado di convertire il formato comune nel suo formato locale e viceversa: la conversione risulterà più *lenta*, dato che ci sono *due step* di conversione (locale mittente -> comune -> locale destinatario); d'altra parte ogni componente deve conoscere *solo due funzioni* di conversione (da locale a comune e da comune a locale).

> La soluzione 3 è quella più comune nei sistemi RPC.

Si comparano ora le soluzioni 3 e 4, che nei sistemi distribuiti rappresentano due patter del middleware di comunicazione distinti: la soluzione 3 caratterizza il pattern *proxy*, la soluzione 4 caratterizza il pattern *broker*.
- **Proxy**: l'obiettivo del pattern proxy è quello di supportare la trasparenza all'accesso e alla locazione (per esempio in sunrpc non è necessario far conoscere la porta). Dunque il proxy permette di gestire la chiamata procedura remota tramite un'altro proxy localizzato sull'altro endpoint. Il proxy lato client rappresenta il server remoto lato client; nel caso di Java RMI, il proxy lato client espone la stessa interfaccia dell'oggetto remoto, in modo da poter invocare questi metodi. In concluse, il proxy lato client espone una copia locale del server che viene invocato.
- **Broker**: in questo caso si tratta di un'entità logicamente centralizza con lo scopo di separare e incapsulare i dettagli della comunicazione.
Nel caso di RPC, la soluzione adottata è la 3, cioè il **pattern proxy**.
#### Passaggio dei parametri
Il passaggio di parametri in una funzione può essere di due tipi: per **valore** e per **riferimento**. Nel passaggio di un parametro per valore, il valore viene copiato nello stack e quindi la procedura chiamata agisce su questa copia e i cambiamenti non hanno impatto sul chiamante.
Nel passaggio di un parametro per riferimento, viene copiato nello stack il riferimento al dato, dunque la procedura chiamata agisce direttamente sui dati che si trovano in quello spazio di memoria.
Esiste però anche una terza tipologia, chiamata **copy-restore**, nella quale i dati a cui si fa riferimento vengono copiati all'interno dello stack del chiamante; quando la procedura ritorna, i dati vengono copiati nella locazione di memoria originale. 
Dato che un riferimento rappresenta un indirizzo di memoria, esso è valido solo nel contesto della macchina locale: in una chiamata RPC se viene passato un riferimento, non ha senso nella macchina remota, che utilizza uno spazio di indirizzamento differente.
Dunque in RPC si simula il passaggio di un parametro usando la chiama **copy-restore**. In particolare, nei framework RPC accade che
1. Il client stub copia i dati puntati nel messaggio di richiesta e lo manda al server stub
2. Il server stub agisce sulla copia, usando lo spazio di indirizzamento della macchina remota.
3. Se la copia viene modificata nel processamento, il server stub invia la copia modificata al client stub che sovrascriverà i dati originale.
#### Semantica di comunicazione in RPC
Per uno sviluppatore è fondamentale conoscere la semantica che il middleware di comunicazione RPC offre.
La semantica exactly once difficilmente viene implementata da un sistema RPC, data la sua complessità. Le due semantiche solitamente offerte sono l'**at-least once** e l'**at-most once**.
- **Semantica at-least once**: se il client riceve una risposta dal server, allora la chiamata a procedura remota è stata eseguita *almeno una volta* dal server;
- **Semantica at-most once**: se il client riceve una risposta dal server, allora la chiamata a procedura remota è stata eseguita *al più una volta* dal server;
#### Server binding
Per garantire la trasparenza alla locazione, il sistema RPC deve implementare un meccanismo che permetta di identificare l'endpoint del server. Ii **binding**, cioè di come avviene il collegamento tra client e server,  può essere di tipo *statico* o *dinamico*:
- Nel **binding statico** il collegamento è definito a tempo di sviluppo dell'applicazione, ovvero le informazioni del server, tra cui l'indirizzo, sono definite staticamente nel codice. Questa è la soluzione più semplice  e con minore overhead, ma manca di trasparenza e flessibilità;
- Nel **binding dinamico** il collegamento al server avviene a runtime; in questo caso aumenta l'overhead, ma si garantisce trasparenza e flessibilità. Ad esempio, con il binding dinamico si possono inviare le richieste a server diversi in caso di replicazione
##### Server binding dinamico
Nel binding dinamico si distinguo due fasi durante il collegamento tra client e server:
- **Fase del namig**: è la fase che tipicamente precede l'esecuzione, con cui il client specifica a quale server. 
- **Fase dell'addressing**: è la fase dinamica in cui avviene il collegamento tra il client e il server specificato. Durante questa fase, a seconda di come è implementato il  middleware di comunicazione, si potrebbe gestire la replicazione di più server. Inoltre l'indirizzamento può essere *implicito* o *esplicito*:
	- *Indirizzamento esplicito*: il client manda la richiesta usando la comunicazione broadcast o multicast e aspetta per la prima risposta;
	- *Indirizzamento implicito*: è il tipo di indirizzamento usato nel caso di RPC. Un'entità terza (in SunRPC, è il portmapper), solitamente implementata sulla stessa macchina che espone il servizio remoto, detta *name server* (aka *binder, directory service, registry service*) che svolge un ruolo di server nei confronti del server stub, consentendogli di registrare i servizi remoti che il server espone; dunque svolge un ruolo di server nei confronti del client stub, permettendogli di cercare servizi remoti e ottenere il collegamento a chi li espone; nuovamente, meccanismo di indirezione. 
 ![[05-img20.png|500|center]]
Per ridurre overhead nel binding dinamico, i binding più recenti vengono mantenuti in una cache per essere riutilizzati.
#### RPC sincrono vs asincrono
Il meccanismo RPC nasce come una chiamata sincrona, dunque ha un comportamento estremamente bloccante per il client.
Di recente, alcuni middleware (nel caso di python e GO) supportano anche le chiamate a procedura remota asincrone, per cui client invia la richiesta e continua l'esecuzione; nel momento in cui arriva la risposta, un handler permetterà di ottenerla.
![[05-img19.png|center|300]]
#### Trasparenza
Il meccanismo RPC **non** è completamente trasparente. Infatti è molto più lento rispetto ad una procedura locale, per la latenza di rete, e può generare tanti tipi di fallimento: il client non riesce a localizzare il server, i messaggi vengono persi oppure crash di vario tipo sia lato client che lato server. Dunque non è in tutto e per tutto simile e "nascosta" in un comportamento da procedura locale.
#### Sicurezza
Inoltre nascono molti aspetti di sicurezza da gestire: ad esempio l'autenticazione di client e server, il grado di visibilità dei messaggi nella rete, tipi di attacco replay.
## Programmare con RPC
La programmazione con RPC dipende fortemente dal linguaggio di programmazione utilizzato. Ad esempio, alcuni linguaggi come il C o il C++ non supportano nativamente il concetto di chiamata a procedura remota, per cui i compilatori non sono in grado di generare automaticamente gli stub. Linguaggi più recenti o aggiornati, come Java, Python e GO supportano direttamente RPC. Però i meccanismi nativi di un linguaggio si possono utilizzare solo nel momento in cui l'implementazione lato server è fatta nello stesso linguaggio del client: usando sistemi esterni, è possibile mettere in comunicazione servizi scritti in linguaggi differenti.
### Interface Definition Language
Per generare client e server stub, comunemente si utilizza un **Interface Definition Language (IDL)**, ovvero un linguaggi ad-hoc per descrivere le procedure remote e dal quale, tramite compilatori separati, è possibile generare gli stub.
Un IDL consente di specificare l'interfaccia della procedura remota, dunque nome della procedura, parametri e valori di ritorno. Il compilatore IDL si occupa, a partire da un file IDL, di generare client e server stub, ossia due intermediari necessari per garantire la trasparenza, fornendo le funzionalità di marshaling e unmarshaling dei parametri e andando a gestire la comunicazione su rete.
![[05-img17.png|center|500]]
## SunRPC
**Sun RPC** è una prima generazione di RPC, introdotta da Sun ed è specifico per il linguaggio C. Le interfacce delle procedure sono definite in un IDL specifico chiamato **XDR**.
### XDR
Sun RPC utilizza **XDR (eXternal Data Representation)**, un linguaggio standardizzato che consente di descrivere e codificare i dati in modo indipendente dall'hardware sottostante, dunque è utile per risolvere il problema dell'eterogeneità dei dati. Il compilatore per questo IDL è `rpcgen`.
XDR fornisce delle funzioni di conversione built-in per i dati di tipo primitivo e alcune strutture semplici (int, string, bool, array). Per strutture di dati più complesse, lo sviluppatore deve fornire le funzioni di conversione. 
XDR utilizza un formato binario che usa *typing implicito*, ossia vengono trasmessi solamente i valori; non vengono trasmessi tipi di dato o informazioni sui parametri.
#### Usare XDR
Per utilizzare RPC nel linguaggio C, lo sviluppatore deve prima di tutto scrivere l'interfaccia delle procedure in un file XDR con estensione `.x`. Questo file comprende due parti:
1. **Definizione**: in cui si scrivono le definizioni delle procedure e le tipologie di dato dei parametri di input e output;
2. **XDR defintions**: contiene, eventualmente, le funzioni di conversioni se non sono già definite in XDR.
#### Esempio: definizione procedura remota
Si vede un'esempio di chiamata a procedura remota che calcola il quadrato di un intero.
```c
struct square_in { /* input (argument) */
	long arg1;
};
struct square_out { /* output (result) */
	long res1;
};

program SQUARE_PROG {
	version SQUARE_VERS {
		square_out SQUAREPROC(square_in) = 1; /* procedure number = 1 */
	} = 1; /* version number */
} = 0x31230000; /* program number */
```
Viene in primis definito un programma RPC `SQUARE_PROG`, composto da più procedure RPC (in questo caso una sola); ciascun programma è identificato da un nome ed è associato ad un *program number* in esadecimale.
Nella definizione del programma, vengono definite le procedure RPC. In questo caso è definita la procedura remota `SQUAREPROC`, alla quale è associato un identificativo. Inoltre XDR supporta il *versioning* delle procedure.
Una procedura è definita da un nome (scritto in uppercase), un solo parametro di input e un solo parametro di output.
### Implementare un programma RPC
Una volta definita l'interfaccia RPC in XDR, lo sviluppatore deve sviluppare il programma lato client e lato server. Il client implementa il `main()` e la logica necessaria per trovare la procedura remota e collegarsi ad essa. Il server implementa le procedure remote che vengono esposte da RPC. In particolare, nella parte server non c'è la funzione `main()`, dato che sarà il server stub a chiamare la procedura localmente.
#### Esempio: procedura locale
Vediamo in primo luogo come il calcolo del quadrato di un intero può essere fatto localmente tramite una procedura locale standard, cosi da confrontarlo con il calcolo fatto da procedura remota.
```C
#include <stdio.h>
#include <stdlib.h>

struct square_in { /* input (argument) */
	long arg;
};
struct square_out { /* output (result) */
	long res;
};

typedef struct square_in square_in;
typedef struct square_out square_out;

/*
 * La procedura ritorna un puntatore
 * ad una variabile static
 */
square_out *squareproc(square_in *inp) {
	static square_out out;

	out.res = inp->arg * inp->arg;
	return(&out);
}

int main(int argc, char **argv) {
	square_in in;
	square_out *outp;

	if (argc != 2) {
		printf("usage: %s <integer-value>\n", argv[0]);
		exit(1);
	}
	in.arg = atol(argv[1]);

	outp = squareproc(&in);
	printf("result: %ld\n", outp->res);
	exit(0);
}
```
#### Esempio: procedura remota
Vediamo come viene implementata la procedura remota sul server, evidenziando le differenze con la procedura locale.
```c
#include <rpc/rpc.h>
#include <stdio.h>
#include "square.h" /* generato da rpcgen */

square_out *squareproc_1_svc(square_in *inp, struct svc_req *rqstp) {
	static square_out out;

	out.res = inp->arg * inp->arg;
	return(&out);
}
```
Differenze con procedura locale:
- Bisogna includere due header: la libreria per RPC `rpc.h` e l'header file `square.h` generato a partire dall'interfaccia scritta in XDR da `rpcgen`;
- Nel nome della procedura bisogna aggiunge il numero di versione e la dicitura `_svc`;
- I parametri di input e output devono usare puntatori e il parametro di output deve puntare ad una variabile statica in modo che la sua locazione sia in memoria globale.
#### Esempio: lato client
Lato client lo sviluppatore deve scrivere il `main()` e gestire al suo interno la chiamata a procedura remota.
```c
#include <stdio.h>
#include <rpc/rpc.h>
#include "square.h" /* generated by rpcgen */

int main(int argc, char **argv) {
	CLIENT *clnt;
	char *host;
	square_in in;
	square_out *result;
	client.c
	if (argc != 3) {
		printf("usage: client <hostname> <integer-value>\n");
		exit(1);
	}
	host = argv[1];
	clnt = clnt_create(host, SQUARE_PROG, SQUARE_VERS, "tcp");
if (clnt == NULL) {
	clnt_pcreateerror(host);
	exit(1);
}
in.arg1 = atol(argv[2]);
if ((result = squareproc_1(&in, clnt)) == NULL) {
	printf("%s", clnt_sperror(clnt, argv[1]));
	exit(1);
}
printf("result: %ld\n", result->res1);
exit(0);
}
```
Per poter chiamare la procedura remota deve fare un certo numero di passi:
1. Includere la libreria `rpc.h` e l'header file del client stub generato da `rpcgen`;
2. Aggiungere la variabile `CLIENT *clnt` che mantiene il gestore del protocollo di trasporto lato client;
3. Usando la funzione `clnt_create` definita nella libreria RPC il gestore di trasporto `clnt`. Questa funzione ha come parametri di input: il nome dell'host, il nome del programma RPC (SQUARE_PROG), il numero della versione e il protocollo di trasporto da utilizzare (tcp o udp). Il server stub esporrà sia l'implementazione con tcp che quella con udp.
```c
CLIENT *clnt_create(char *host, unsigned long prog, 
					unsigned number vers, char *proto)
```
4. Se il gestore è stato creato correttamente, si può chiamare la procedura specificando il nome della procedura seguito dal numero di versione `squareproc_1`, passandogli in input anche il puntatore al gestore di trasporto del client. La gestione dell'errore viene fatta attraverso le funzioni `clnt_pcreateerror()` e `clnt_perror()`.
### Feature di SunRPC
Un programma scritto utilizzando SunRPC permette di definire più procedure remote, ha il supporto del *versioning*, ciascuna procedura ha un solo parametro di input e un solo parametro di output e utilizza la *copy-restore* per simulare il passaggio dei parametri per riferimento.
Inoltre RPC è *indipendente dal trasporto*, dato che rpc espone sia la chiamata con tcp che la chiamata con udp, dunque il client può scegliere a runtime se usare tcp o udp. 
La mutua esclusione è garantita dal server; di default il server è sequenziale e non corrente, più richieste vengono servite una per volta.
Il client è sincrono e bloccante, dunque rimane bloccato fino a quando riceve risposta.
Infine, viene garantita la semantica *at-least once*, infatti nel client stub è definito un timeout di ritrasmissione.
#### Server binding
Per effettuare il binding, il client stub deve conoscere la porta sulla quale contattare la procedura. Ma nel codice lato client, viene specificato solamente l'hostname. Infatti, la porta viene fornita dal *name server* (o service registry) di SunRPC detto *port mapper*. 
Il server stub registra le procedure remote che il server espone sul port mapper, che mantiene una tabella dinamica che associa ad ogni procedura registrata una porta.
Dunque il client stub contatta il port mapper (operativo sulla porta 111 dell'host che espone le procedure) per sapere qual'è la porta corrispondente ad un certo numero di programma e numero di versione specificato dall'utente nella chiamata a procedura.
Tramite il comando `rpcinfo -p`, è possibile visualizzare questa tabella di binding tra porta e procedura esposta.
![[05-img18.png|center|500]]
### Processo di sviluppo
Riassumendo, il processo di sviluppo di un programma con Sun RPC è il seguente:
1. Definizione delle specifiche del servizio (aka elenco procedure esposte con informazioni) tramite un file .x utilizzando il linguaggio XDL;
2. Usare il compilatore IDL `rpcgen` per generare: l'header file da includere in client e server; il programma C contenente le routine di conversione dei dati e i due stub client e server;
3. Scrivere l'implementazioni lato client e le implementazioni di tutte le procedure esposte dal server;
4. Compilare e linkare i file opportunamente per ottenere gli eseguibili lato client e lato server.
![[05-img21.png|500|center]]

![[05-img22.png|500|center]]
### Server stub
Si analizza ora il codice generato per il server stub.
Nello stub viene effettuato il dispatching verso l'effettiva procedura chiamata dal client, dato che un programma prevede più procedure; è presente, oltre le procedure esposte, una NULLPROC utilizzata per fare, ad esempio, testing della latenza.
La chiamata pmap_unset fa pulizie di vecchie registrazioni delle procedure nel programma.
Dopodiché, vengono registrate le procedure al portmapper sia con protocollo tcp che udp.
Dopo tutta questa inizializzazione, viene lanciato il server che si mette in attesa di ricevere richieste di connessione.
### Client stub
Si analizza ora il codice generato per il client stub.
Lo stub va ad invocare la funzione `clnt_call()` che istanzia il gestore di trasporto lato client. Questa riceve in input il gestore di trasporto creato e definito nel client (con la funzione `clnt_create()`) dal quale ottiene il nome della procedura, il parametro di input e di ritorno. Inoltre la funzione gestire il meccanismo di ritrasmissione e riceve in input il parametro che setta il **timeout** (25 secondi di timeout).
Sempre tramire questa funzione il client stub va a contattare il portmapper dal quale ottiene il numero di porta; dunque si occupa di aprire la connessione verso il server, rendendo trasparente allo sviluppatore tutta la parte della comunicazione (aka programmazione con le socket).

## Java RMI
**JAVA RMI (Remote Method Invocation)** è una tecnologia RPC di seconda generazione che fornisce il meccanismo RPC nativamente per il linguaggio Java. In particolare, estende il meccanismo RPC per il mondo ad oggetti, che sono distribuiti: con Java RMI è possibile scrivere un'applicazione in Java in cui un oggetto di una jvm può andare ad invocare i metodi esposti da un oggetto di un altra jvm.
![[Pasted image 20231201155653.png]]
L'obiettivo principale di Java RMI è quello di fornire allo sviluppatore la trasparenza all'accesso, mentre la trasparenza alla replicazione non è completa.
Java RMI sfrutta la separazione tra interfaccia e implementazione, caratteristica tipica di Java, rendendo questa separazione logica anche una separazione fisica. 
In particolare, l'**interfaccia remota** rappresenta l'insieme di metodi che possono essere invocati da un altro host, mentre l'**oggetto remoto** è l'istanza della classe che implementa l'interfaccia remota.
Dunque, lo sviluppatore può separare fisicamente interfaccia ed oggetto facendo si che lo **stub** gestisca l'interfaccia remota lato client, permettendo l'invocazione dei metodi forniti dall'oggetto remoto.
**Lo stato dell'oggetto remoto non è distribuito**, rimane locale sulla jvm del server.
![[Pasted image 20231201161006.png]]
### Invocazione di un metodo remoto
L'invocazione di un metodo remoto (**remote method invocation**) è l'invocazione di metodi di un'interfaccia remote su un oggetto remoto, cioè in esecuzione su un'altra jvm.
L'obiettivo è quello di garantire trasparenza all'accesso, cioè rendere l'invocazione remota del tutto simile ad una invocazione locale
Questo obiettivo si ottiene aggiungendo un livello di indirezione usando, come per Sun RPC, un proxy pattern con un client stub, detto semplicemente **stub**, e un server stub detto **skeleton**. Con queste due componenti, Java RMI cerca di nascondere la natura distribuita dell'applicazione.
In particolare, l'invocazione avviene a partire dal client che invocherà effettivamente un metodo che viene esposto dallo stub, che è la componente che *espone localmente* l'interfaccia remota. Dunque lo stub gestisce tutti i dettagli sottostanti: prendere nome del metodo e parametri, impacchettarli in un messaggio e spedirlo alla macchina host contenente l'implementazione dei metodi esposti.
Lato server, lo skeleton riceve il messaggio, lo spacchetta ed invoca effettivamente il metodo nel server.
![[05-img15.png]]

### Serializzazione/deserializzazione
Per il trasporto degli oggetti passati come parametri di un metodo remoto invocato, stub e skeleton utilizzano il meccanismo di serializzazione e deserializzazione supportato direttamente da Java grazie al bytecode: lo stub invia il bytecode allo skeleton che effettua la deserializzazione di esso usando le funzionalità offerte da Java stesso.
- **Serializzazione**: conversione di un oggetto passato come parametro in un byte stream (`writeObject` nello stream di output);
- **Deserializzazione**: decodifica del byte stream e costruisce una copia dell'oggetto originale (`readObject` nello stream di input).
#### Marshaling vs serialization 
### Interazione tra skeleton e stub
Si descrivono ora le fasi che attraversano lo skeleton e lo stub nel momento di una invocazione a metodo remoto.
- Il client ottiene lo stub tramite un service registry detto RMI registry. Questo è un servizio di naming che permette di mappare il nome di un servizio nel corrispondente oggetto remoto, dunque permette di far ottenere al client l'interfaccia remota, che egli gestirà;
- Il client invoca un metodo localmente sullo stub: la sintassi è la stessa rispetto ad una invocazione locale;
- Lo stub serializza i dati utili all'invocazione del metodo, dunque l'id del metodo e i parametri di input, e li invia allo skeleton per mezzo di un messaggio;
- Lo skeleton (proxy lato server) riceve il messaggio, lo deserializza (dato che conosce il codebase), invoca il metodo identificato nel messaggio e riceve il valore di ritorno che serializza e invia allo stub per mezzo di un messaggio;
- Lo stub riceve il messaggio, lo deserializza e ritorna il valore di ritorno al client.
### Esempio
Si vede ora un banale esempio di un metodo remoto che fa l'echo di una stringa di testo.
#### Definizione interfaccia
Il primo passo è definire l'interfaccia remota. In primis, per indicare che l'interfaccia definita è un interfaccia remota e per gestire le eccezioni derivanti dalle comunicazione, si importano le librerie Remote e RemoteException.
A questo punto, si dichiara l'interfaccia estendendo l'interfaccia Remote (in modo da indicare che i metodi dichiarati possono essere eseguiti in una jvm non locale) e si scrivono le firme dei metodi da esporre.
Si osserva che la trasparenza non è completa, dato che lo sviluppatore deve specificare che l'interfaccia è remota e che i metodi di tale interfaccia devono lanciare un eccezione specifica per fallimenti derivanti dalla comunicazione.
Infine, i parametri vengono passati:
- per valore in caso di tipi primitivi o oggetti che implementano l'interfaccia java.io.Serializable, cioè in caso di oggetti che sono serializzabili; la serializzazione/deserializzazione è gestita da stub e skeleton;
- per riferimento nel caso di oggetti remoti.
#### Server
Lato server è presente la classe che implementa l'interfaccia remota, questa deve andare ad estendere la classe UnicastRemoteObject (dunque la comunicazione è unicast). Il costruttore deve invocare il metodo super() che chiama il costruttore della classe UnicastRemoteObject che si occupa di eseguire tutto ciò che è necessario per far si che il server attendi le richieste remote e le serva quando arrivano. Dopodiché si implementano i metodi definiti nell'interfaccia remota, i quali lanceranno in caso di fallimento le eccezioni remote.
Nel main si crea l'istanza di oggetto remoto, dunque va reso noto al mondo esterno. Per fare questo, è necessario registrare l'oggetto sull'RMI registry in modo che esso possa essere contattato. Per effettuare la registrazione, si utilizza il metodo bind (o rebind) supportato dalla classe Naming, che andrà a registrare l'interfaccia remota sul registry, assegnandogli un nome. 
A differenza di SunRPC, la registrazione sul registry la effettua lo sviluppatore e non il server stub.
Si osserva inoltre che RMI registry e sever che offre i metodi remoti devono trovarsi nello stesso host.
#### Client
Lato client si deve innanzitutto ottenere l'interfaccia remota: si va a contattare l'RMI registry specificando l'hostname e il nome con cui il server ha registrato il servizio sul registry, ottenendo l'interfaccia remota. A questo punto, il client può invocare i metodi remoti.
Si osserva che quando l'invocazione del metodo remoto per il client è sincrona e bloccante.

## RPC in Python
Per python esistono diverse implementazioni del meccanismo RPC.
Si andrà ad esaminare RPyC, perché ha delle caratteristiche più avanzate che vanno nella direzione di gRPC (ad esempio comunicazione simmetrica).
Come sempre, l'obiettivo è fornire trasparenza all'accesso: Python raggiunge questo scopo andando ad ispezionare gli oggetti a runtime tramite il modulo `inspect`; questo modulo consente a Python di andare ad esaminare il contenuto di una classe, di ottenere il codice sorgente di un metodo, e di estrarre la lista degli argomenti per una funzione.
Questo meccanismo viene sfruttato da tutte le implementazioni RPC disponibili in Python.
### Caratteristiche principali
Si vedono le caratteristiche principali di RPyC.
- L'uso del meccanismo RPC è più **trasparente** rispetto a SunRPC e Java RMI: **non** è necessario scrivere file di definizione o utilizzare compilatori IDL e **non** è necessario il service registry;
- Le operazioni sono *simmetriche*: entrambi i lati (client e server) possono invocare chiamate RPC su di loro, cioè client può invocare su server, e server può invocare su client. Questo rende possibile le chiamate RPC asincrone, dato che il server può fare una callback al client con il valore di ritorno.
- Il server può supportare la concorrenza: si può scegliere server one-shot, server threaded, server thread pool e server che fa forking del processo. Non usando nameserver, il binding della porta viene fatto su una porta specifica, standard o specificata dall'utente.
- Il client si collega al server e può eseguire le operazioni remote attraverso la proprietà `modules` che permette di esporre il namespace del modulo del server.
### Serializzazione
Il passaggio dei dati in RPyC avviene per valori per i tipi semplici, ossia gli oggetti immutabili come stringe, interi e tuple: i valori vengono inviati direttamente al server.
Per gli oggetti, il passaggio avviene per riferimento: viene passato il nome dell'oggetto al server, che andrà a contattare il client per ottenere gli attributi relativi all'oggetto; i cambiamenti che il server effettua sull'oggetto verranno riflessi nell'oggetto sul client.
Questo meccanismo permette il passaggio di oggetti che sono dipendenti alla locazione, dunque è possibile passare file e altre risorse del sistema operativo: ad esempio, il processo remoto può scrivere sullo stdoutput del processo locale.
Questo meccanismo viene implementato attraverso il meccanismo dei proxy, in particolare, in RPyC il proxy è detto netrefs ed è il proxy di un oggetto.

Il client crea degli oggetti proxy locali in corrispondenza dei moduli remoti, consentendo un accesso trasparente e permette di far si che ogni operazione eseguita sul proxy lato client si rifletta sul server in modo trasparente al client.
RPyC supporta sia chiamate sincrone che chiamate asincrone. La chiamata sincrona è implementata come caso particolare di una chiamata asincrona.

la libreria RPyC è basata sui servizi: una chiamata a procedura remota corrisponde ad un servizio esposto dal server.
I servizi sono delle classi che derivano dalla classe e espongono metodi che possono essere esposti antecedendo il loro nome con `exposed_` o usando il decorator ``.

oppure avviene per riferimento, viene passato il nome di un oggetto ad endpoint e il server contatta il client per ottenere attributi di tale oggetto. le modifiche vengono poi inviate al client.
Questo meccanismo permette il passaggio di oggetti dipendenti dalla locazione, tipo file o altre risorse dell'os.
Implementazione tramite i proxy -> netrefs 
client crea oggetti proxy locali per i moduli remoti

rpyc basato sui servizi, cioè delle classi che definiscono ed espongono dei metodi

con piu chiamate async, nessuna garanzia sull'ordine di esecuzione di esse.
implementare eventi come callback async. client diventa consumer di un prodotto del server

## RPC in GO
Nel linguaggio Go, a differenza degli altri linguaggi visti, il supporto alle chiamate RPC è nativo e fa parte della libreria standard. In particolare, il supporto è dato da package `net/rpc` che fornisce l'accesso ai metodi esportati di un oggetto ed eseguiti su un server. Inoltre, Go supporta come protocolli di trasporto per la comunicazione RPC sia TCP che HTTP.
Un metodo per essere accessibile da remoto, deve seguire certi vincoli:
- Il tipo e il nome del metodo devono essere esportati: bisogna porre l'iniziale con lettera maiuscola;
- Deve avere solo due argomenti, entrambi esportati (iniziale maiuscola) ;
- Il secondo argomento deve essere un puntatore ad una struct che conterrà la risposta fornita dal server;
- Deve sempre restituire un errore;
```go
func (t *T) MethodName(argType T1, replyType *T2) error
```
### Lato server
Il primo passo è creare il server TCP (o HTTP) che permette al server di riceve l'invocazione del metodo e dunque eseguirlo.
A questo punto, si utilizza il metodo `Register` (o il metodo `RegisterName`) della libreria `rpc` per registrare l'interfaccia e quindi renderla visibile come un servizio.
```go
func (server *Server) Register(rcvr any) error
func RegisterName(name string, rcvr any) error
```
Il metodo prende in input un parametro, che è l'interfaccia (`any` è un alias per `interface{}`) \[ nel caso di `RegisterName`, prende in input anche un nome da assegnare al servizio\], e pubblica i metodi che sono parte dell'interfaccia data in input sul server RPC, che ora possono essere invocati dai client che si connettono al servizio.
Una volta fatta la registrazione, il server deve usare il metodo `Listen` per ottenere un listener che li espone su una porta ben definita. Nella chiamata di Listen si specifica se usare il protocollo TCP o HTTP.
```go
func Listen(network, address string) (Listener, error)
```
Il listener viene utilizzato dunque dal metodo `Accept` che permette di accettare richieste di connessione sul listener e quindi di servire le richieste per ogni connessione entrante.
```go
func (server *Server) Accept(lis net.Listener)
```
Il metodo `Accept` è bloccante, dunque se il server vuole fare altro lavoro, bisogna chiamare il metodo in una goroutine.
### Lato client
Il client deve per prima cosa connettersi al server RPC usando il metodo `Dial` e indicando l'indirizzo e la porta a cui connettersi. Se la connessione del server RPC usa HTTP, si usa il metodo `DialHTTP`.
```go
func Dial(network, address string) (*Client, error)
```
A questo punto il client usa il metodo `Call` per fare una chiamata RPC sincrona: il metodo blocca l'esecuzione in attesa di risposta da parte del client.
```go
func (client *Client) Call(serviceMethod string,
						   args any, reply any) error
```
Se invece il client vuole fare una chiamata asincrona, utilizza il metodo `Go` che invoca la chiamata RPC in modo asincrono e riceve una notifica quando arriva la risposta utilizzando un canale di comunicazione creato automaticamente quando viene invocato il metodo `Go`.
```go
func (client *Client) Go(serviceMethod string,
						 args any, reply any,
						 done chan *Call) *Call
```
Il puntatore a Call restituito è un puntatore ad una struct che rappresenta una chiamata RPC attiva e ha la seguente struttura
```go
type Call struct {
	ServiceMethod string // The name of the service
						 // and method to call.
	Args any // The argument to the function (*struct)
	Reply any // The reply from the function (*struct)
	Error error // After completion, the error status.
	Done chan *Call // Receives *Call when Go is complete.
}
```
Dunque il client mettendosi in attesa sul canale `Done` quando ottiene la risposta sa che la chiamata è terminata.
