# Sistemi distribuiti self-adaptive (auto adattativi)
Un **sistema software auto-adattativo** è un sistema in grado di adattare il suo comportamento a tempo di esecuzione, durante il funzionamento stesso del sistema, rispetto ad eventi che avvengono nel sistema stesso (es. guasto di una componente di sistema) o che avvengono nell'ambiente (es. incremento di traffico improvviso al sistema).
Dunque un sistema auto-adattativo, o autonomico, è in grado di adattarsi senza richiedere intervento umano o comunque solo un intervento minimale.
Le applicazioni di sistemi auto-adattativi spaziano dal cloud computing e edge/fog computing fino all'High-perfomance computing.

Un sistema auto-adattativo viene anche detto autonomico perché questo aggettivo è ispirato al sistema nervoso autonomico, cioè quella parte del sistema nervoso che si occupa di funzioni vitali del corpo umano come digestione o battito cardiaco, che sono funzioni che avvengono "senza" l'intervento umano.

L'idea generale dell'autonomic computing è quello di avere un paradigma di computazione in cui i sistemi informatici sono in grado di adattarsi rispetto alla loro complessità, a quella dell'ambiente in cui operano e rispetto all'eterogeneità in cui operano.

Un sistema autonomico gestisce in modo indipendente le sue funzionalità, senza richiedere intervento umano (o comunque minimo), ed è in grado di rispondere ai cambiamenti e alle incertezza che caratterizzano l'ambiente in cui il sistema opera e il sistema stesso.

Diversi sistemi rientrano in questo dominio: kubernetes, EC2 autoscaling, juniper, autonomus database.
## Obiettivi dei sistemi auto-adattativi
Un sistema software auto-adattativo deve avere la capacità di autogestirsi, cercando di raggiungere i seguenti obiettivi:
- **self-optimize**: capacità del sistema di ottimizzare l'utilizzo delle sue risorse, dunque ottimizzare le sue prestazioni, in modo da raggiungere i suoi obiettivi di qualità del servizio; ad esempio, se il sistema deve spendere un massimo $x$ e allo stesso tempo soddisfare una garanzia sul tempo di risposta $y$, il sistema usando l'elasticità orizzontale controlla il numero di istanze allocate all'applicazione evitando di allocare troppo per non sforare il massimo $x$ ma allo stesso tempo allocare sufficienti istanze per soddisfare il vincolo sul tempo $y$.
- **self-heal (auto-guarirsi)**: capacità del sistema di scoprire, diagnosticare e recuperare un guasto in modo da soddisfare i requisiti di qualità, o eventualmente degradare in modo soft le prestazioni, diminuendo la qualità del servizio ma mantenendole comunque l'operatività. Esempio. individuare dei nodi che hanno crashato e dunque non indirizzare richieste su questi nodi;
- **self-configure**: capacità del sistema di integrare automaticamente nuove componenti senza interrompere la normale operatività del sistema o servizio, oppure la capacità di fare autoconfigurazione dei parametri;
- **self-protect**: capacità del sistema di identificare anomalie di sicurezza (es. intrusion detection) e di reagire all'intrusione e alle azioni di attacco e le conseguenze di tali azioni (es. intrusion response) per proteggersi da minacce di sicurezza.
### Realizzazione degli obiettivi
Il primo passo per ottenere un sistema auto-adattativo funzionale è quello di realizzare un  sistema in grado di essere a conoscenza ed essere consapevole di quello che accade al suo interno (self-awareness). Inoltre deve essere in grado di conoscere l'ambiente circostante (self-situation).
Avendo queste conoscenze, il sistema deve essere in grado di identificare cambiamenti che avvengono all'interno del sistema o nell'ambiente che lo circonda (self-monitoring) e deve adattarsi di conseguenza (self-adjustment).
Questi attributi, diventano quelli che sono i meccanismi di implementazione. Cerchiamo di capire da un punto di vista architetturale, qual'è il modello di riferimento per i sistemi auto-adattativi.
## Modello concettuale di un sistema auto-adattativo
![[04-img01.png|center|600]]

Un sistema software auto-adattativo opera in un certo ambiente, composto da hardware, rete, componenti software non controllabili dal sistema e il comportamento degli utenti.
Il sistema opera in questo ambiente ricevendo degli input e producendo degli output che possono andare a modificare in qualche modo l'ambiente in cui il sistema opera; a questi input si aggiungono però dei disturbi, che cambiano l'input atteso rispetto al funzionamento del sistema, indicati in figura come **incertezze**.
In tutto ciò il sistema deve essere in grado di soddisfare degli obiettivi di alto livello, come un service level agreement che stabilisce dei service level objectives.
![[04-img02.png|center|600]]
Allo strato base, ossia il software che deve essere gestito, si aggiunge un layer di strumentazione che permette di monitorare il funzionamento del sistema, dell'ambiente e permette di adattare il sistema stesso; dopodiché si aggiunge la parte del sistema che permette di gestire e controllare il sistema stesso, ossia il controllore o **sistema di gestione**. 
Dunque il sistema di gestione riceve dei dati di monitoraggio provenienti dal sistema e dall'esterno; a questo punto determina, se necessarie, delle azioni di adattamento e indica come queste azioni devono essere eseguite in modo tale da mantenere gli obiettivi di funzionamento.
## MAPE (Monitor, Analyze, Plan, Execute)
**M.A.P.E.** è il modello architetturale di riferimento per sistemi auto-adattativi; i componenti fondamentali che caratterizzano questo pattern architetturale sono quelli dati dal nome stesso, che è un acronimo per **Monitoraggio, Analisi, Pianificazione, Esecuzione**.
- **Monitoraggio**: è la componente che riceve in input dati provenienti da sensori che possono essere sia interni al sistema stesso che relativi all'ambiente in cui il sistema opera, dunque esterni; può aggregare questi dati facendo un analisi di tipo statistico e passa questi dati alla componente di analisi.
- **Analisi**: è la componente con l'obiettivo di analizzare i dati di monitoraggio e identificare se è necessario un'adattamento, ossia una modifica o cambiamento nel sistema stesso. A volte, le componenti di monitoraggio e analisi vengono implementate in un unica componente. Verranno comunque tenute distinte a livello logico.
- **Pianificazione**: è la componente che riceve l'input della fase di analisi e identifica quali sono le azioni di adattamento da compiere. Quindi la componente produce un piano di adattamento e lo notifica alla componente di esecuzione che è in grado di mettere in atto l'adattamento che è stato programmato.
- **Esecuzione**: è la componente che si occupa di attuare il piano di adattamento generato dalla componente di pianificazione, tramite degli effettori in modo da adattare il sistema gestito.
Queste componenti sono organizzati in un loop, per questo si parla spesso di **ciclo MAPE**.

Tutte queste componenti, e quindi durante tutte le fasi del ciclo M.A.P.E., si fa riferimento ad una base di conoscenza comune detta **knowledge**, che mantiene conoscenza condivisa riguardo aspetti rilevanti del sistema gestito, dell'ambiente e riguardo i goal dell'amministratore di sistema.

Si entra ora in dettaglio nelle singole macrocomponenti del ciclo.
### Monitoraggio
Nella componente di monitoraggio, le tipiche variabili che la caratterizzano sono:
- *Quando viene effettuato il monitoraggio*: ad esempio monitoraggio continuo, on demand oppure periodico. Si predilige il monitoraggio periodico, dunque ad intervalli regolari la componente monitora il sistema.
- *Che cosa viene monitorato*: dipende da cosa fa il sistema. Si monitora generalmente: il consumo di risorse (utilizzazione della CPU, throughput di disco, memoria e rete); il carico di lavoro, quindi il numero di richieste nell'unità di tempo (richieste/secondo); e le performance dell'applicazione, come il tempo di risposta, il tempo di latenza o il numero di richieste che è in grado di fornire in una unità di tempo.
- *Come effettuare il monitoraggio*: cioè l'architettura (centralizzata o decentralizzata) e la metodologia (attiva o passiva) del sistema di monitoraggio.
	- *Monitoraggio passivo*: la componente osserva ciò che accade al sistema;
	- *Monitoraggio attivo*: la componente genera del traffico verso il sistema per effettuare le misurazioni.
- *Dove memorizzare i dati di monitoraggio*: ad esempio su un database basato su serie temporali o un file di log, e come memorizzarli, ad esempio fare del pre-processing prima di salvare questi dati.
### Analisi
Nella componente di analisi, le tipiche variabili che la caratterizzano sono:
- *Quando effettuare l'attività di analisi*:
	- **event-triggered**: viene effettuata ogni volta che la componente riceve nuovi dati; 
	- **time-triggered**: viene effettuata in modo periodico;
- Se effettuare un'analisi sui dati di tipo **reattiva** o **proattiva**:
	- **reattiva**: analisi che si basa sull'identificare situazioni problematiche presenti nel sistema grazie ai dati disponibili, dunque su eventi che sono già avvenuti nel sistema. Quindi il sistema reagisce sulla base di eventi che sono già capitati: ad esempio, aumentare il numero di risorse in reazione ad un aumento del workload.
	- **proattiva**: analisi che si basa sul predire possibili situazioni problematiche future utilizzando i dati disponibili, in modo da impostare un piano di adattamento in anticipo rispetto al momento in cui l'evento potrebbe accadere. Ad esempio, incremento il numero di risorse avendo predetto un possibile aumento futuro delle risorse utilizzate.
### Pianificazione
La componente (fase) di pianificazione è la più studiata tra tutte le fasi del ciclo MAPE. Una vasta gamma di metodologie e tecniche può essere usata per creare un piano di adattamento, tra le quali:
- **Teoria dell'ottimizzazione**, quindi formulazione di problemi interi o lineari. Il vantaggio di questa tecnica è la capacità di individuare esattamente le azioni di ottimizzazioni ottimali (ad esempio, il numero di macchine virtuali ideali per mantenere il sistema funzionante). Lo svantaggio è la possibilità di avere una formulazione che risulta NP-Hard.
- **Euristiche**: facili da definire, veloci da applicare, ma non garantiscono azioni di adattamento ottime.
- **Machine learning**: utilizzare i dati disponibili per addestrare una rete neurale, con lo svantaggio di poter non essere in grado di gestire bene eventi che non sono stati visti durante l'addestramento. Si usa reinforcement learning, sistema che impara osservando il feedback del sistema.
- **Teoria del controllo**;
- **Teoria delle code**.
#### Esempio: problema del bin-packing
Il placement delle macchine virtuali e/o container in un sistema distribuito può essere modellato con il problema di ottimizzazione del *bin packing*.
Nel problema del bin-packing si hanno degli oggetti di dimensione differente da collocare nel minor numero di contenitori, ognuno avente una certa capacità, in modo che per ogni contenitore la somma delle dimensioni degli oggetti contenuti non superi la capacità.
Nel contesto di un sistema distribuito, i server sono i contenitori, le risorse di tali server sono le capacità e gli oggetti da inserire nei contenitori sono le macchine virtuali o i containers.
La formulazione matematica come problema lineare del bin-packing è a variabili intere, dunque il problema è NP-Hard.
Dunque per risolvere il problema è necessario adottare delle euristiche efficienti che risolvo il problema in modo sub-ottimale.
- *Round-robin*: si organizzano i contenitori (server) in modo circolare e si mantiene l'ultimo contenitore in cui viene inserito un oggetto; si inserisce un oggetto nel prossimo contenitore con capacità sufficiente, a partire dalla posizione corrente. Questa euristica distribuisce bene il carico sui contenitori (server), ma non minimizza il numero di contenitori.
- *First Fit*: si organizzano i contenitori in una lista e si colloca l'oggetto da posizionare nel primo contenitore con capacità sufficiente, cominciando ogni volta dall'inizio della lista. Questa euristica minimizza il numero di contenitori usati. Un'alternativa include l'ordinamento in ordine decrescente degli oggetti da inserire.
- *Best Fit*: l'oggetto viene collocato nel contenitore con il minimo spazio residuo sufficiente. Più inefficiente, dato che bisogna guardare tutti i contenitori.
- *Worst Fit*: simile a best fit, ma l'oggetto viene collocato nel contenitore con il massimo spazio residuo.
Kubernetes utilizza una variante di queste politiche.
### Soluzioni architetturali per MAPE
Si vede ora come istanziare a livello architetturale le componenti logiche che caratterizzano il ciclo MAPE.
In particolare:
- *MAPE centralizzato:* tutte le componenti del ciclo MAPE su un unico server centralizzato, è la soluzione più semplice ma manca di scalabilità.
- **MAPE decentralizzato**: in cui le componenti di MAPE sono distribuite; esistono molti pattern architetturali, ognuno con i suoi pro e contro. Il pattern giusto da utilizzare dipende dal sistema, dai requisiti e dalle funzionalità dell'applicazione.
![[04-img03.png|center|500]]
#### MAPE decentralizzato
Si dividono i pattern che verranno analizzati in due famiglie: pattern **gerarchici** e pattern **flat** (orizzontali). Si analizzano quindi due pattern gerarchici e due pattern flat.

Un pattern gerarchico è più semplice da controllare, ma introduce più colli di bottiglia o single point of failure. Inoltre, tipicamente rende più semplice realizzare le metodologie di controllo, cioè la parte del plan.
Un pattern flat è migliore in termini di scalabilità, dato che non ci sono elementi di centralizzazione, ma ha lo svantaggio di rendere più difficile la coordinazione tra più componenti di controllo.
##### Pattern MAPE gerarchici
I pattern mape gerarchici sono caratterizzati da cicli MAPE multipli organizzati in una gerarchia, dove i loop di controllo ad alto livello gestiscono i loop di controllo subordinati.
###### Master-worker pattern
In questo patten il sistema è composto da un master e da un certo numero di worker. Sui nodi worker sono presenti le componenti di monitoraggio e di esecuzione, che sono dunque decentralizzate. Le componenti di analisi e pianificazione invece si trovano in un'unica istanza posizionate sul nodo master.
Dunque le azioni di adattamento sono decise centralmente dal nodo master, portando il vantaggio di avere un'unica vista globale del sistema e dell'ambiente in cui il master conosce tutto ciò che è stato monitorato; quindi prende le decisioni rispetto ad uno stato globale del sistema e dell'ambiente.
La componente di analisi riceve tutti i dati dalle componenti di monitoraggio distribuite sui nodi worker, le passa alla componenete di pianificazione centralizzata nel nodo master e il piano di adattamento viene inviato ai nodi worker in modo che ogni worker possa eseguire il piano in modo decentralizzato.
Gli svantaggi di questa architettura includono: single point of failure del nodo master, colli di bottiglia nelle performance e overhead nella comunicazione dovuti dalla quantità di informazioni inviate da worker a master e viceversa. 
![[04-img04.png|center|500]]
###### MAPE gerarchico
In questo pattern si mantiene la visione gerarchica ma a livello di istanze di cicli MAPE che agiscono a livelli differenti del sistema, eventualmente anche su scale temporali differenti, e che si occupano di prendere decisioni differenti di adattamento sul sistema.
In questo esempio si ha una gerarchia a due livelli: ogni nodo ha il proprio ciclo MAPE, in cui le azioni le azioni di monitoraggio del livello più basso vengono coordinate con il monitor del livello superiore, il quale decide cosa fare e comunica cosa fare alle componenti sottostanti. Le componenti sottostanti però prendono anch'essi decisioni locali sulle azioni da compiere.
Dunque le componenti MAPE di livello inferiore prendono decisioni a livello locale, mentre le componenti MAPE di livello superiori prendono decisioni globali.
![[04-img04.png|center|500]]
##### Pattern MAPE flat
Nei pattern MAPE flat si hanno cicli MAPE multipli che si comportano e cooperano come dei peer.
###### Coordinated control pattern
Cicli MAPE multipli, ognuno incaricato di controllare una parte del sistema, che comunicano tra di loro e si coordinano per decidere al meglio.
Ad esempio i dati di monitoraggio di ognuno vengono comunicati alle altre componenti, cosi anche per i risultati dell'analisi ecc.
Il vantaggio di questa soluzione è una migliore scalabilità. Il problema principale invece risiede nella difficoltà di prendere decisioni di adattamento comune. Inoltre si genera tanto overhead di comunicazione.
![[04-img04.png|center|500]]
###### Information sharing pattern
Caso speciale del pattern coordinato, in cui i diversi cicli MAPE si scambiano le informazioni del monitoraggio.
In questo modo, ogni componente ha una vista globale del sistema e prende le decisioni per conto proprio.
Oltre a migliorare la scalabilità, in questo pattern si riduce l'overhead di comunicazione e non c'è necessità di prendere una decisione comune.
La mancanza di decisione comune è però anche uno svantaggio: si possono prendere a livello di pianificazione decisioni conflittuali o sub-ottimali.
![[04-img04.png|center|500]]
### Esempi di sistemi auto adattativi
Si considerano tre esempi di sistemi auto adattativi nel contesto della gestione delle risorse: EC2 autoscaling; applicazioni composte da tanti servizi; auto-scaling di applicazioni basate su microservizi.
Gli aspetti in comune che hanno questi esempi sono.
- Eventi inattesi durante l'esecuzione del servizio;
- L'obiettivo di adattamento è soddisfare dei Sistem Level Objectives definiti tra utenti e sistema stesso (ad esempio tempo di risposta o disponibilità);
Questi esempi differiscono nella metodologia di planning e nelle architetture di controllo decentralizzato.
#### Esempio 1: EC2 autoscaling
**Amazon EC2 autoscaling** è il servizio AWS che consente di aggiungere e rimuovere istanze EC2 automaticamente a seconda di condizioni definite dall'utente e a seconda dei risultati del monitoraggio sullo stato di funzionamento delle risorse in quel momento attive.
![[04-img08.png|center|500]]
##### Monitoraggio: CloudWatch
Il servizio di autoscaling prevede l'utilizzo del servizio di monitoraggio CloudWatch, che monitora lo stato delle singole istanze (uso GPU, throughput memoria, I/O, rete) e offre monitoraggio a livello applicativo (relativo alle risorse di amazon, non a livello di rete). L'utente può scegliere le metriche in base alle quali verrà effettuato lo scaling delle istanze.
##### Pianificazione
Si studiano due politiche implementate nel servizio di Auto Scaling per determinare le decisioni di scale-out e scale-in: la *dynamic simple scaling* e il *predictive scaling*.
In aggiunta all'auto-scaling, il servizio Auto Scaling è anche un sistema self-healing (auto-guarisce) perché, tramite CloudWatch, può individuare quando un'istanza EC2 non funziona correttamente e dunque può terminarla e lanciare una nuova istanza per rimpiazzarla.
###### Dynamic simple scaling
Dynamic simple scaling è una politica di elasticità orizzontale di tipo reattivo, molto semplice e basata su un euristica basata su soglie.
L'utente definisce una soglia inferiore e una soglia superiore rispetto ad una o più metriche di scaling: la soglia superiore definisce il livello oltre il quale bisogna aumentare le risorse, la soglia inferiore definisce il livello al di sotto del quale bisogna diminuire le risorse.
Se l'utilizzazione media di tutte le istanze del sistema supera una certa soglia in un certo intervallo di tempo, il sistema aggiunge delle istanze. Similmente avviene per la soglia bassa.
- Esempio di regola di *scale-out*: se l'utilizzo medio della CPU di tutte le istanze è maggiore del 70% nell'ultimo minuto, allora aggiungi una nuova istanza;
- Esempio di regola di *scale-in*: se l'utilizzo medio della CPU di tutte le istanze è minore del 35% negli ultimi 5 minuti, allora rimuovi una istanza;
Il servizio CloudWatch monitora rispetto alle metriche scelte per lo scaling le istanze attive e invia alla componente di pianificazione un allarme di scale-out o scale-in, la quale in base alle regole definite dall'utente aggiunge o rimuove istanze.
Infine, è previsto un periodo di *cooldown* (breve) tra ogni attività di scaling, per evitare che ci siano troppe operazioni di scaling.

I vantaggi di questa politica sono la semplicità e la facilità di utilizzo: l'utente deve selezionare le metriche e i valori delle soglie. Ma la scelta di questi valori da parte dell'utente è tutt'altro che semplice:
- Chi imposta il servizio deve conoscere bene come utilizza le risorse l'applicazione, dato che le metriche su cui basarsi per autoscaling dipendono fortemente dall'applicazione e dalle risorse che utilizza maggiormente; spesso non è facile capire su quale risorse basare l'autoscaling dell'applicazione;
- I valori di soglia potrebbero essere troppo aggressivi o troppo conservativi;
- Non è possibile utilizzare metriche specifiche dell'applicazione (es. tempo di risposta);
Infine, essendo un sistema reattivo, non è robusto a pattern di carico variabili.
![[04-img07.png]]
###### Predictive scaling
Il *predictive scaling* è una politica di scaling *proattiva* basata su Machine Learning. Viene usato un modello di ML addestrato per prevedere il traffico e l'utilizzo delle risorse EC2 futuro, su base giornaliera e settimanale.
Per utilizzare questa politica, è necessario dare accesso al modello i dati storici collezionati da CloudWatch: in particolare, il modello deve avere almeno una giornata di dati storici per fare previsioni e viene rivalutato ogni 24 ore per fare previsioni delle successive 48 ore.
![[04-img09.png|center|500]]
Il vantaggio di avere un sistema proattivo come questo richiede l'allenamento della rete e dunque la precisione delle previsioni dipende dalla quantità di dati storici.
#### Esempio 2: service selection
#### Hierarchical scaling of microservices