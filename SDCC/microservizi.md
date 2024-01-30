Quello dei microservizi è uno stile architetturale per lo sviluppo di applicazioni distribuite, che permette di sviluppare l'applicazione come un insieme di servizi tra loro lascamente accoppiati.
Questo stile deriva dai SOA (service oriented architecture) e dai Web services che avevano sempre l'idea di sviluppare delle app distribuite come insieme di componenti tra loro disaccoppiate.
L'idea dei microservizi è quello di creare una applicazione composta da moduli piccoli e autocontenuti: nello sviluppo ci si concentra sulla modularizzazione dell'applicazione. Si decompone l'app in un insieme di servizi indipendenti ma cooperativi tra di loro, che comunica per mezzo di una delle tante tecnologie per la comunicazione (code di messaggi, rpc, grpc, etc).
Ciascun microservizio deve poter essere rapidamente scalato e il suo deployment, cioè la sua istanziazione, deve essere rapida.

Ai microservizi vengono aggiunti strumenti per mantenere lo stato, questo perché dei microservizi stateless sono più semplici da gestire in termini di scalabilità (non bisogna copiare lo stato su ogni replica). Se serve mantenere lo stato, è conveniente mantenerlo in un datastore esterno ai microservizi, come redis.

## Gli antenati: service oriented architecture
nascono inizio anno 2000, come paradigma architetturale per realizzare app distribuite con componenti lascamente accoppiate. 
Proprietà: vista logica formata da componenti che interagiscono tramite tecnologie di comunicazione; scambio di informazione avviene tramite messaggi; descrizione della sintassi dei singoli servizi; architettura indipendente dalle specifiche piattaforme sottostanti
 3 attori fondamentali: chi richiede il servizio, chi fornisce il servizio e un service registry che offre servizi di pubblicazione e discovery dei servizi pubblicati dai provider.
## soa vs microservizi
- soa tende a fare affidamento in modo importante ad un middleware pesante, mentre i microservizi si affidano a tecnologie leggere;
- i protocolli usati in soa sono molti e molto specifici, mentre i microservizi si basano su REST e HTTP;
- SOA è uno strumento usato principalmente per integrare differenti soluzioni software; mentre i microservizi vengono utilizzati per costruire singole applistatocazioni software.

I microservizi si sposano perfettamente con i container: si associa ad ogni microservizio un container. Ogni microservizio diventa un immagine di un container che può essere istanziato come un qualsiasi container, dunque si ottengono tutti i vantaggi di gestione che derivano dall'uso di container, tra cui semplicità nella scalabilità e migrazione.
Pro
- scale in /out di ogni microservizio aumentando o diminuendo il numero repliche;
-  scale up/down di ogni istanza di microservizio aumentando o diminuendo i limiti di uso di risorsa;
- isolamento dei microservizi
- applicare limiti di uso di risorse ai microservizi
- build e deploy estremamente rapido
L'unico svantaggio è quello di dover utilizzare uno strumento per la **orchestrazione dei container** per gestire queste applicazioni multi container.

Benefici dei microservizi:
- maggiore agilità nello sviluppo del software: team di sviluppo differenti si occupano di sottoinsiemi di microservizi.
- migliore scalabilità e isolamento dei guasti
- migliore riusabilità tra differenti aree di business;
- migliore sicurezza dei dati;
- sviluppo e rilasco delle applicazioni più veloce
- maggiore autonomia dei team di sviluppo
Aspetti negativi:
- I microservizi devono comunicare tra di loro, dunque si ha un incremento del traffico di rete;
- aumenta la complessità: più complicato effettuare deploy, testing e debbugging.
## Come decomporre un'applicazione
Pattern principali
1. approccio di decomposizione basato sulla business capability, andando ad identificare e disaccoppiare le differenti azioni che generano valore in differenti microservizi. ad es. microservizio di order managment si occupa di gestire ordini;
2. approccio di decomposizione domain-driven design subdomain, dunque assegnare un sottodominio ad ogni microservizio
3. approccio di decomposizione per use-case: ogni microservizio è responsabile di particolare azioni;
4. approccio di decomposizione basato sui nomi o sulle risorse, dunque ogni microservizio è responsabile per le operazioni su entità e risorse di un certo tipo.


Per ottenere scalabilità, è sufficiente utilizzare diverse istanze dello stesso microservizio e bilanciare il carico delle richieste tra le varie istanze. Per renderla semplice da gestire, è preferibile implementare microservizi stateless, in modo da evitare la complessità e la gestione dello stato nel momento in cui si scala il servizio.

**Servizio stateless**: lo stato è gestito esternamente al servizio per facilitarne lo scale out e rendere l'applicazione più tollerante ai guasti del servizio.
**Servizio stateful**: le varie istanze di un servizio scalato devono sincronizzare il proprio stato interno per garantire un comportamento unico. Come mantenere questo stato sincronizzato?

## Service discovery
In un applicazione basata su microservizi è necessaria la presenza di un servizio che permetta la ricerca di un microservizio: le diverse istanze di microservizi hanno generalmente indirizzo IP e porta assegnato dinamicamente che può cambiare nel momento in cui si effettuano operazioni di scaling, aggiornamento o altro.
Il service discovery mette a disposizione un meccanismo che permette di
1. registrare un istanza di microservizio;
2. cercare un servizio tra quelli registrati.
![](Pasted%20image%2020240119182514.png)
### Pattern for service discovery
#### Client side service discovery
In questo pattern il cliente del servizio si occupa di determinare la posizione in rete delle istanze del servizio disponibili e bilancia le proprie richieste tra di esse. Per trovare le istanze del servizio, il client interpella un service register, dunque utilizza un algoritmo di load balancing per scegliere l'istanza alla quale mandare la richiesta.
![](Pasted%20image%2020240119182759.png)
Svantaggio: non essendo load balancing globale, potrebbe accadere che ogni client fa richiesta ad una specifica istanza, oppure un client fa richiesta ad una istanza già molto carica di richieste.  
Vantaggio: nessun intermediario da cui dipendere, dunque più semplice, minor overhead e maggiore tolleranza ai guasti.
#### Server side service discovery
In questo pattern il client si affida ad un intermediario che agisce da load balancer delle richieste che ha una posizione ben definita nella rete (Ip e porta non cambiano). Dunque il client manda la richiesta ad un servizio passando per il load balancer; dunque il load balancer interpella il service registry per localizzare le istanze del servizio e inoltra la richiesta ad una delle istanze disponibili.
![](Pasted%20image%2020240119183010.png)
Vantaggio: vero load balancing;
Svantaggio: intermediario che genera overhead e diminuisce la tolleranza ai guasti -> se cade load balancer, nessun servizio può fare richieste (soluzione: replicazione load balancer).
## Sincrono vs asincrono
La comunicazione tra microservizi può essere **sincrona** o **asincrona**
- Sincrona: comunicazione di tipo request/response;
- Asincrona: comunicazione di tipo event-driven;
La comunicazione sincrona richiede dei semplici meccanismi di tipo request/response come REST (basato su HTTP) o gRPC.
La comunicazione asincrona richiede un meccanismo di comunicazione asincrono basato su messaggi, tipo un middleware di comunicazione di tipo pub-sub o basato su code di messaggi.
```ad-important
La comunicazione sincrona potrebbe ridurre la disponibilità del servizio, dato che un microservizio in attesa di risposta non può eseguire altre operazioni.
```
Esempio comunicazione sincrona.
![](Pasted%20image%2020240120152330.png)
Esempio comunicazione asincrona.
![](Pasted%20image%2020240120152353.png)
## Orchestrazione e coerografia
I microservizi possono interagire tra loro in accordo a 2 pattern: orchestrazione e coerogragia.
### Orchestrazione
Approccio centralizzato, in cui un singolo processo centralizzato detto orchestratore o message broker si occupa dell'interazione dei microservizi. Dunque l'orchestratore si occupa di invocare e combinare i diversi servizi; in particolare, ogni servizio non è necessariamente al corrente della composizione degli altri servizi che lo circonda.
Message broker <-> orchestratore
![](Pasted%20image%2020240120152621.png)
Vantaggi:
- approccio più semplice e popolare;
Svantaggi:
- Orchestratore è un Single point of Failure e un bottleneck per le prestazioni;
- Stretto accoppiamento tra le componenti;
- Maggiore traffico di rete e maggiore latenza
### Coerografia
La coerografia è un approccio decentralizzato, per cui esiste una descrizione globale dei servizi partecipanti all'interazione. Questa descrizione è definita per mezzo di scambio di messaggi, regole di interazioni e accordi tra due o più microservizi. In questo caso, i servizi sono al corrente della composizione che li circonda (almeno in parte), dunque possono scambiarsi i messaggi direttamente senza la necessità di un'unità centrale.
Vantaggi:
- Miglior disaccoppiamento, minore complessità e maggiore flessibilità e facilità di modifica;
Svantaggi:
- I servizi si devono conoscere tra loro;
- Lavoro in più per tenere aggiornato lo stato dei servizi da parte di ogni servizio;
- Implementare alcuni meccanismi, tipo garantire la consegna dei messaggi, può risultare più difficile

# Design patterns per applicazioni a microservizi
## Circuit breaker
Pattern utilizzato per evitare che il fallimento di un servizio si ripercuota a cascata su altri microservizi. La cascata può avvenire nel momento in cui un microservizio A effettua una richiesta ad altro microservizio B e questo non risponde (guasto o altro), quindi A rimane bloccato in attesa della risposta e dunque non può servire le richieste che gli arrivano da altri microservizi, i quali a loro volta rimarranno bloccati.
Dunque si introduce un elemento intermedio detto circuit braker che svolge un ruolo di proxy. A manda richiesta a B passando per il circuit breaker, che svolge il ruolo di interruttore del circuito: quando il numero di fallimenti consecutivi supera una soglia, il circuit braker scatta e, per la durata di un periodo di timeout, tutti i tentativi di invocazione verso B falliranno immediatamente. Quando scade il timeout, il circuit breaker consente a un numero limitato di richieste di prova verso B. Se tali richieste hanno successo, il circuit breaker chiude il circuito e permette tutte le richieste verso B. In caso contrario, il timeout ricomincia.
## Database per service
Pattern che riguarda l'architettura del database. In questo pattern, ogni microservizio mantiene i suoi dati persistenti **privati** su uno storage esterno e accessibile tramite le sue API. Dunque ogni microservizio ha il suo database privato, e le transazioni di tale servizio impattano solo su tale database.
Vantaggi:
- Migliora il disaccoppiamento dei servizi;
- Ogni servizio può usare il DB che più gli conviene;
Svantaggi:
- Risulta più complesso implementare delle transazioni che coinvolgono più microservizi (utilizzare pattern SAGA);
- Bisogna gestire molteplici database.
## SAGA
Supponendo di aver usate il pattern database per service, vengono effettuate delle transazioni che coinvolgono molteplici servizi: come si può mantenere consistenza dei dati tra i servizi senza usare protocolli di transazione distribuita come il 2 phase commit? Si può implementare ogni transazione che coinvolge molteplici servizi come una transizione SAGA.
Una SAGA è una sequenza di transazioni locali: ogni microservizio effettua la sua transizione locale che aggiorna il suo DB privato e pubblica un messaggio o triggera un evento per innescare la prossima transizione locale nella saga. Se la transazione locale fallisce, allora saga esegue una serie di transazioni di compensazione per fare in modo che tutti i cambiamenti fatti dalle precedenti transazioni locali vengano ripristinate allo stato precedente all'inizio della saga: si fa rollback di tutte le transazioni locali compiute.
![](Pasted%20image%2020240120162137.png)
Per coordinare una transizione saga, si possono usare due pattern:
- corerografia: in cui ogni transazione locale pubblica un evento che triggera le transazioni locali di altri servizi; ![](Pasted%20image%2020240120162308.png)
- orchestrazione: in cui un orchestratore si occupa di dire ai partecipanti della saga quale transazioni locali eseguire.![](Pasted%20image%2020240120162321.png)
Spesso in combinazione con il pattern saga si utilizza il pattern **event sourcing** per risolvere il problema che un servizio che partecipa in saga deve automaticamente aggiornare il Db e inviare un messaggio o evento per evitare inconsistenza dei dati. L'idea è quella di usare una sequenza di eventi di dominio che rappresentano i cambiamenti di stato; ciascuno di questi eventi viene memorizzato in un event-store, cioè un Db di eventi, di tipo append-only. Dunque event-store mantiene un log degli eventi avvenuti per ciascun microservizio, inseriti in modo append-only.
## CQRS (Command Query Responsability Segregation)
Questo pattern viene utilizzato per implementare delle query che prelevano dati da molteplici servizi. In particolare, si vogliono separare le operazioni di lettura e di scrittura in modo tale che le singole operazioni possano essere scalate in modo indipendente.
Il pattern consiste nel definire una vista del DB che permette di supportare la query. Si ricorda che una vista è in genere una replica readonly del database. Le applicazioni mantengono le repliche aggiornate sottoscrivendosi ad Domain events pubblicati dal servizio che mantiene i dati. In questo modo, si separano le operazioni di lettura e aggiornamento del data store.
## Pattern per il monitoraggio di microservizi
La distribuzione dei servizi di un'applicazione a microservizi su larga scala implica una certa difficoltà nel monitoring dei diversi microservizi; in particolare è necessario implementare un meccanismo che permetta di osservare i microservizi, cioè in grado di individuare le relazioni temporali e causali che ci sono tra eventi dei microservizi.
Il monitoring dei microservizi è fondamentale per:
- fare debug dell'applicazione;
- analizzare le performance e la latenza;
- analizzare le dipendenze tra i servizi;
- identificare la **radice** di anomalie tra i microservizi, cioè quale evento di quale microservizio scatena le anomalie riscontrate nell'applicazione.
In particolare per identificare la causa radice delle anomalie dell'applicazione è necessario costruire un grafo delle dipendenze tra i servizi che metta in evidenza la struttura e le relazioni che ci sono nella sequenza di invocazione dei microservizi, grazie al quale si può identificare la causa radice.

Latenza della coda: garanzie sulla latenza non sono date sui valori medi, ma sulla coda della distribuzione
### Log aggregation
Questo pattern consiste nel mantenere un servizio di logging centralizzato che aggrega i log di tutte le istanze di microservizi e che permetta di effettuare ricerche e analisi su tali log. 
Svantaggi:
- entità logicamente (o fisicamente) centralizzata;
- il servizio di logging potrebbe dover gestire una quantità non indifferente di log;
### Distributed tracing
Questo pattern consiste nell'integrare all'interno dei microservizi un meccanismo che permetta di tracciare il percorso che ogni richiesta fa attraverso i diversi microservizi. Questo è utile per comprendere il comportamento di un'applicazione a microservizi complessa, e permette di individuare possibili problemi.
Lo svantaggio principale sta nell'implementazione di questo meccanismo: richiede una infrastruttura di un certo livello.
Ad ogni richiesta viene assegnato un *trace id* unico che permette di tracciare la richiesta attraverso tutti i microservizi che attraversa. Questo trace id può essere utilizzato dai microservizi per fare logging.
### Service mesh
Una service mesh è un layer infrastrutturale dedicato aggiunto ad una applicazione a microservizi che facilita la comunicazione tra i diversi microservizi per mezzo di un proxy, con l'obiettivo di aggiungere in modo trasparente dei servizi aggiuntivi, tra cui l'osservabilità, la gestione del traffico e della rete (anche load balancing), e servizi di sicurezza. Tutti questi servizi sono aggiunti senza dover modificare il codice dell'applicazione (per questo trasparente).
# Serverless computing (o function as a service)
L'obiettivo è quello di semplificare ulteriormente la parte di deplyment e delivery di una applicazione.
Nel serverless computing la parte di gestione dell'infrastruttura è completamente invisibile allo sviluppatore.

Il serverless è un paradigma del cloud computing che astrae tutti gli aspetti di gestione dell'infrastruttura, della piattaforma e tutte le decisioni di "basso livello" riguardo l'infrastruttura.
Lo sviluppatore dunque si può concentrare sulla parte di business e sulla logica dell'applicazione, cioè sullo sviluppo delle **funzioni**, senza preoccuparsi del provisioning, della gestione e dello scaling delle risorse computazionali.
L'ambiente di esecuzione è completamente gestito dal cloud provider;
Serverless: le funzioni vengono eseguite su dei server da qualche parte, ma non è di interesse dell'utente.
Function as a service come sinonimo.

Features del serverless:
- Le risorse computazionali sono effimere: l'infrastruttura necessaria per eseguire una funzione viene tirata su al momento dell'invocazione, e può rimanere attiva per una singola invocazione. Si crea il problema del cold start: quando una richiesta di invocazione e l'infrastruttura per servirla non è pronta, l'esecuzione della funzione deve essere ritardata fino a che non viene istanziata tale infrastruttura (container o microVM);
- Elasticità automatica, cioè senza bisogno di configurazione: le risorse computazionali scalano automaticamente in base alle richieste e al workload;
- Vero pay-per-use: il pagamento dell'uso del servizio è a grana fine e basato sull'effettivo utilizzo delle risorse; dunque si paga l'effettivo tempo di utilizzo delle risorse e non per un intera unità di calcolo (ad es. una istanza EC2 per un ora).
- Event-driven: i provider cloud permettono di allocare dinamicamente l'infrastruttura per eseguire una funzione nel momento in cui viene triggerato un evento, come il caricamento di un immagine su un servizio di storage;
- NoOps: tutto il processo di deployment del codice in produzione è semplificato dato che lo scaling, il capacity planning e le operazioni di manutenzione sono nascoste agli sviluppatori;
- Supporto in diversi ambiti applicativi.
Diversi cloud provider forniscono servizi di serverless computing. In particolare, l'utente che usa questi servizi non ha controllo sulle prestazioni delle funzioni, può solo decidere quanta memoria allocare per la funzione: non può controllare in modo raffinato le risorse computazionali da utilizzare per la funzione. Infine, i cloud provider offrono altri servizi di supporto che sono spesso necessari nell'ambito di un sistema serverless.

Nel serverless è conveniente eseguire delle funzioni che sono stateless, dato che sono più semplici da gestire. Spesso però è necessario utilizzare funzioni che mantengono uno stato. In questi casi, lo stato può essere mantenuto esternamente. Questo stato esterno però deve essere ben integrato con il servizio di serverless computing per mantenerne i benefici.

Limitazioni:
- Nessun controllo sulle performance e problema del cold start, dunque si potrebbero aspettare diversi minuti prima che la funzione venga eseguita, sopratutto alla prima chiamata di essa: spesso i cloud provider mantengono le risorse allocate per un certo lasso di tempo, in modo da servire velocemente le esecuzioni successive. Inoltre è necessario implementare un sistema di autoscaling proattivo per gestire picchi nelle richieste.
- Limitati tipi di ambienti di esecuzione e linguaggi supportati;
- Limiti nell'uso delle risorse: non è possibile usare una quantità di risorse indefinita;
- Sicurezza;
- Non ci sono soluzioni standardizzate, rischio di vendor lock-in.

Una buona pratica quando si costruisce un app serverless è quella di definire tante funzioni  piccole, semplici e stateless e poi comporle per creare un workflow dell'applicazione. Funzioni pesanti e complesse sono difficili da mantenere, inoltre è bene separare il codice dalle strutture dati. Amazon offre un servizio che permette di orchestrare e coordinare molteplici funzioni serverless in modo da ottenere un workflow.




applicazione strutturata e realizzata con una collezione di componenti lascamente accoppiate.
Deriva dai webservices
L'idea è avere delle compoennti modulari piccole e autocontenute. L'aspetto prinvipale che andiamo a guardare è la modularizzazione: decomporre l'applicazione in un insieme di servizi indipendenti che solo lascamente accoppiati e cooperativi e che possono essere rapidamente deployate e scalate in modo indipendente. ai microservizi vengono aggiunti degli strumenti per mantenere lo stato. microservizi stateless sono facili da scalare.
### SOA
nascono inizio anni 00 come paradigma architetturale per realizzare app distribuite con componenti lascamente accopiate tra di loro.
chi richiede servizio, chi fornisce servizio e service regisrty che fa da intermediario 