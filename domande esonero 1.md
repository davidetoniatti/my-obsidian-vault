### Pattern di disaccoppiamento supportato da coda di messaggi
- temporale: perché la coda memorizza i messaggi in modo persistente.
- asincronia: client e server non devono essere entrambi presenti nel momento della comunicazione;
- spaziale: destinatario e mittente non devono conoscere esplicitamente l'uno la posizione dell'altro, entrambi devono solo essere in grado di contattare la coda.

Pattern 1to1 coda di messaggi: un messaggio viene prodotto da un solo producer e letto da un solo consumer. Questo non esclude il fatto che ci siano più consumer che leggono messaggi dalla coda e più producer che inseriscono messaggi: il collegamento rispetto ad un singolo messaggio è però 1 mittente 1 destinatario.

Pattern 1toMany pub/sub: un messaggio viene consegnato a molteplici consumer: più consumer iscritti ad un topic (nel caso di sistema topic-based) possono leggere lo stesso messaggio.

Gruppo di consumer kafka: solo un consumer del gruppo riceve il messaggio -> come se fosse pattern 1to1 rispetto ai consumer nel gruppo.

## Prova itinere 22/23
#### punto (d)
Il multicast bimodale è un algoritmo con l'obiettivo di inviare in modo affidabile un messaggio in multicast. Per fare questo l'algoritmo è composto da due fasi:
1) usando meccanismo di multicast tradizionale, si invia il messaggio a tutti i destinatari.
2) può accadere che non venga ricevuto da tutti i destinatari; la seconda fase usa il gossiping con lo scopo di riparare quei messaggi che non sono arrivati. In questa fase detta di gossip repair, ogni nodi coinvolto nel gruppo di multicast periodicamente invia il digest dei messaggi che ha ricevuto ad un sottoinsieme casuale dei nodi coinvolti nel multicast. Ciascun nodo che riceve un digest lo confronta con il proprio digest dei messaggi ricevuti, se non corrispondono il nodo chiede al nodo da cui ha ricevuto il digest di trasmettergli i messaggi mancanti.
Con la fase di gossiping, l'algoritmo risulta affidabile.
Vantaggi e svantaggi legati alla scelta di usare gossiping invece di flooding:
- numero di messaggi ridotto;
- è più lento dato che sono necessarie più iterazione: essendo scelto casualmente il numero di nodi, un nodo a cui manca il messaggio potrebbe ricevere digest da nodo a cui manca lo stesso messaggio.
### Domanda 3
#### Punto (a)
Client stub: proxy lato client, esso rappresenta il server per il client. Riceve la chiamata della procedura remota, la impacchetta in un messaggio effettuando il marshaling dei parametri e lo inoltra la server stub (nel caso di sunRPC, si occupa anche di recuperare la porta di comunicazione). Quando riceve il messaggio di risposta dal server stub, lo spacchetta effettuando unmarshaling dei parametri e lo inoltra al client.

server stub: proxy lato server, esso rappresenta il client lato server e ha il compito di ricevere la chiamata della procedura dal client stub, spacchettare il messaggio con unmarshaling dei parametri, fare l'invocazione locale di essa e veicolare la risposta ottenuta attraverso un messaggio di risposta, con opportuno marshaling dei parametri, al client stub.

IDL: Interface definition language. Ha lo scopo di definire l'interfaccia in modo uniforme e non ambiguo della procedura remota esposta insieme ai suoi parametri di input e ritorno, inoltre fornisce il supporto per il marshaling e l'unmarshaling dei messaggi di richiesta e risposta.
Un esempio di IDL è XDR e protocolbuffer di gRPC.

gRPC contraddistinto dal fatto che è poliglotta: tramite compiler del protocolbuffer si generano stub lato client e lato server che gestiscono dettagli di comunicazione per linguaggi differenti.
#### punto (b)
Il service registry è un intermediario che ha lo scopo di comunicare alcuni dettagli relativi alla comunicazione.
- SunRPC: serve a far conoscere le porte al client, il quale deve conoscere il nome del servizio/procedura, il numero di versione e l'indirizzo dell'host. La porta gli viene data dal port mapper. Dunque in sunrpc il client stub contatta per prima cosa il port mapper (che si trova sullo stesso host in cui si trova la procedura) e gli chiede la porta per il programma di interesse. Il server stub deve registrare il servizio esposto sul portmapper, dopodiché farà il dispatching della procedura richiesta dal client stub.
- JavaRMI: il service registry e detto RMI registry. Il client fa lookup su RMI registry, eseguito su stesso host del server, da cui ottiene l'interfaccia remota (che è lo stub), da cui è possibile invocare i metodi come invocazione locale.
- In python e go non è imperativo avere un server registry: il client per chiamare la procedura si collega al server specificando anche il numero di porta.
#### punto (c)
- creazione account: at-most-once, dato che si vogliono evitare creazioni multiple di un account;
- lettura saldo: at-least-once, dato che l'operazione di lettura è idempotente, non cambia a seconda dello stato del server.
#### punto (d)
gRPC supporta quattro tipi di rpc
1. streaming unario
2. streaming client/server
3. streaming server/client
4. bidirezionale
L'esempio rappresenta la definizione di un'interfaccia utilizzando protocolBuffer; viene definito un servizio PaymentService che contiene una sola procedura, ProcessPayment, che è un streaming unario dato che non è specificata la keyword stream.
Dopodiché vengono specificati cosa sono parametro di input e parametro di output, cioè vengono definiti i due messaggi corrispondenti.
Output message - payment status: contiene due campi uno booleano che indica successo o fallimento e un opzionale che contiene messaggio di errore; il numero accanto indica l'ordine dei campi all'interno del messaggio.
Input message - payment request: contiene a sua volta un messaggio, come se fosse dato strutturato: card che contiene informazioni relative alla carta di credito; al di fuori abbiamo campo enum che indica valuta; dopodichè viene specificato l'ordine dei campi definiti nel messaggio con aggiunto un campo amount. 1 carta, 2 importo, 3 tipo di valuta.
A runtime se client specifica euro, ci sarà un errore.
### Domanda 4
#### punto (a)
Per la gestione di una applicazione web multilayer, dove si vuole replicazione delle componenti di ciascun layer, le differenze sono
1. a livello IaaS, bisogna istanziare manualmente tutte le componenti strutturali di ogni layer dell'applicazione, e gestire la replicazione attraverso il meccanismo di autoscaling offerto.
2. usando PaaS, il provider cloud mette a disposizione un servizio che automatizza deployment e gestione di applicazioni web. In questo modo utente non deve occuparsi di istanziare e configurare le macchine virtuali, l'autoscaling e il loadbalancer. con iaas ho maggior controllo sul sistema e sull'infrastruttura.
#### punto (b)
1. Per poter usare politiche differenti su privacy e sicurezza rispetto ad affidarsi solo ad un provider: dati riservati gestiti nel cloud aziendale mentre dati non sensibili possono risiedere nel provider.
2. Load balancing: capacità di far fronte a carico di lavoro superiore rispetto alla capacità del cloud privato, facendo offloading su cloud pubblico.
3. Maggiore distribuzione geografica
4. Maggiore tolleranza ai guasti
5. riduzione del rischio di vendor lockin
#### punto (c)
Lo SLA è il contratto firmato col cloud provider in cui vengono garantite certi livelli di prestazioni del servizio