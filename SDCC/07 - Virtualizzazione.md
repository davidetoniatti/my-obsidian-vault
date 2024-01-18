La **virtualizzazione** ha lo scopo di astrarre i dettagli di una implementazione sottostante per avere una vista logica differente dalla vista fisica. L'idea è quella di astrarre le risorse computazionali a disposizione disaccoppiando l'architettura percepita dalle applicazioni a livello utente con la realizzazione fisica.
I vantaggi della virtualizzazione sono numerosi: realizzare soluzioni più agili e flessibili, migliorare le prestazioni, migliorare la scalabilità orizzontale, migliorare la sicurezza creando ambienti maggiormente isolati tra loro, e molti altri.
## Componenti in un'ambiente virtualizzato
Sono tre gli attori principali presenti in un'ambiente virtualizzato:
- **Guest.** È il livello più alto nell'architettura ed è colui che utilizza l'ambiente virtualizzato: non interagisce col sistema fisico ma si interfaccia con una vista astratta del sistema fisico per mezzo del *layer di virtualizzazione*, con il quale interagisce per mezzo delle risorse virtualizzate che sono esposte.
- **Host**. È l'ambiente fisico originale, in cui sono presenti le risorse fisiche: queste risorse vengono utilizzate dal *guest* in modo indiretto, grazie al *layer di virtualizzazione*.
- **Layer di virtualizzazione.**  È il layer di indirezione, responsabile della creazione dell'ambiente virtualizzato in cui l'host andrà ad operare.
# Le macchine virtuali
Una **macchina virtuale** è un'ambiente di calcolo completo con le proprie capacità computazionali, di memoria e di comunicazione. Consente di rappresentare le risorse hardware e software a disposizione in modo differente rispetto alla rappresentazione fisica: risorse hardware differenti da risorse hardware reali e risorse software, ad esempio sistema operativo, diverse dalle risorse software reali.
Il primo aspetto fondamentale della virtualizzazione basata su macchine virtuali è la possibilità di poter eseguire **molteplici** macchine virtuali su una **singola** macchina fisica.
![](Pasted%20image%2020240102230607.png)
## Storia delle macchine virtuale
#TODO
## Vantaggi delle macchine virtuali
Si elencano una serie di vantaggi che porta la virtualizzazione per mezzo di macchine virtuali.
- Facilita la **compatibiltà, portabilità, interoperabillità e migrazione** di applicazioni e ambienti. Ad esempio, è possibile eseguire un programma che funziona interfacciandosi con un sistema software A  su un sistema software B utilizzando un layer di virtualizzazione grazie al quale viene mimata l'interfaccia del sistema A sul sistema B. Dunque la virtualizzazione permette di avere **hardware independence**, cioè di poter eseguire una applicazione su ogni host indipendentemente. Inoltre, creando delle macchine virtuale con sistema operativo legacy, è possibile far girare applicazioni legacy su sistemi più moderni;
![](Pasted%20image%2020240102233058.png)
- Permette il **consolidamento dei server** in un data center. I server possono essere sfruttati al massimo, eseguendo sullo stesso server fisico diverse macchine virtuali. Ad esempio, un applicazione web composto da tre layer ognuno dei quali viene eseguito su un sistema operativo differente, non dovrà occupare tre server fisici differenti: verranno create tre macchine virtuali distinte che potranno essere eseguite su un unico server fisico. L'obiettivo è chiaramente quello di ridurre il numero di server fisici ed usarli in modo efficiente, con lo scopo di ridurre i costi di risorse come il consumo energetico, lo spazio occupato e la manutenzione. Inoltre, la migrazione live delle macchine virtuali riduce i tempi di disservizio.
![](Pasted%20image%2020240102233840.png)
- Permette di **isolare le componenti delle applicazioni**. Questo porta numerosi vantaggi nell'ambito della sicurezza e della gestione dei guasti: una componente mal funzionante o sotto attacco, non può danneggiare le altre componenti in esecuzione su macchine virtuali differenti dato che non è in grado di accedere alle loro risorse.
- Permette di **isolare le performance** sulle differenti macchine virtuali. Il layer di virtualizzazione offre un ulteriore livello di *scheduling* che permette di schedulare le risorse alle macchine virtuali in esecuzione.
- Permette di **bilanciare il carico** sulle macchine fisiche. Questo si ottiene grazie alla possibilità di **migrare** le macchine virtuale da una macchina fisica all'altra.
# Livelli implementativi della virtualizzazione
La virtualizzazione può essere implementata a diversi livelli operazionali: nella trattazione si vedrà l'implementazione a livello hardware (sistemi di macchine virtuali) e l'implementazione a livello di sistema operativo (container).
## Interfacce in un calcolatore e virtualizzazione
Si ricordano i tre livelli di interfaccia presenti nei calcolatori moderni, elencati dal livello più basso al livello più alto:
- Livello **ISA (instruction set architecture)**: è un interfaccia posizionata tra hardware e software che si distingue in ISA di sistema e ISA utente:
	- *ISA di sistema*: utilizzata primariamente per la gestione delle risorse, dunque contiene quelle istruzioni *privileggiate* eseguibili solamente dal sistema operativo;
	- *ISA utente*: utilizzato primariamente per operazioni computazionali, contiene le istruzioni *non privileggiate* eseguibili da ogni programma;
- Livello dato dal **sistema operativo** che offre due interfacce:  le chiamate di sistema o *system call* e la application binary interface;
- Livello dato dalle **librerie** utente che offrono le *API (Application Programming Interface)*.
![](Pasted%20image%2020240114163203.png)
Dunque in generale una applicazione usa le chiamate API offerte dalla libreria (A1), che andranno a fare delle chiamate di sistema (A2), le quali andranno a far eseguire delle istruzioni macchina a livello ISA (A3).

La virtualizzazione può essere implementata in questi livelli di interfaccia; in particolare, l'obiettivo della virtualizzazione per un certo livello è quello di **imitare** il comportamento dell'interfaccia in tale livello. 
### Implementazione a livello ISA
L'obiettivo dell'implementazione della virtualizzazione a livello ISA è quello di **emulare** una certa interfaccia ISA al di sopra dell'interfaccia ISA della macchina host: dare l'illusione di lavorare su un ISA diverso da quello originale della macchina. Ad esempio, eseguire codice MIPS su una macchina x86 con il supporto dell'emulazione ISA.
L'emulazione ISA si può ottenere per mezzo di due tecniche:
- **code interpretation**: letteralmente interpretazione del codice, in generale una operazione lenta, dato che ogni istruzione sorgente viene interpretata dall'emulatore per poter eseguire le corrispettive istruzioni ISA native;
- **Dynamic binary translation**: è una tecnica che genera meno overhead, dato che vengono convertiti blocchi di istruzioni e non singole istruzioni.
### Implementazione a livello di sistema
L'obiettivo dell'implementazione a livello di sistema (o *macchine virtuali di sistema*) è quello di virtualizzare le risorse hardware e software della macchina host, come i processori, la memoria e i dispositivi di I/O. In questa implementazione il layer di virtualizzazione è rappresentato da un **Virtual Machine Monitor (VMM)**, detto anche *hypervisor*: si interfaccerà con l'hardware o il sistema operativo sottostanti (a seconda del livello di esecuzione dell hypervisor) per poter eseguire delle macchine virtuali.
In particolare, l'hypervisor fornisce un ambiente in cui possono coesistere **molteplice** macchine virtuali: il VMM gestisce le risorse hardware condividendole tra le molteplici VM e garantisce isolamento e protezione alle varie istanze.
Il problema principale che si deve affrontare in questa implementazione è la gestione delle **istruzioni privileggiate**: si utilizzeranno meccanismi che permettono al VMM di intercettare le istruzioni privileggiate per andarla ad eseguire, rendendosi un tramite rispetto alla VM.
### Implementazione a livello di SO
L'obiettivo della virtualizzazione a livello di sistema operativo è quello di creare molteplici ambienti di esecuzione isolati al di sopra del sistema operativo. Questi ambienti di esecuzione sono conosciuti col nome di *container*, che sono resi isolati tra di loro grazie a delle funzionalità del sistema operativo Linux.
### Implementazione a livello di libreria
L'obiettivo della virtualizzazione a livello di libreria è quello di creare un ambiente di esecuzione sull'host per premettere l'esecuzione di applicazioni non supportate nativamente senza dover creare una VM o installare un'altro sistema operativo. Un esempio di tale virtualizzazione è il layer di compatibilità [Wine](https://www.winehq.org/), che permette di eseguire applicazioni Windows in sistemi POSIX-compliant traducendo le chiamate API di Windows in chiamate POSIX durante la loro esecuzione.
### Implementazione a livello utente
L'obiettivo è quello di avere una piattaforma virtuale che esegue un singolo processo. L'applicazione viene compilata in un linguaggio intermediario e poi eseguito in ambienti di esecuzione offerti dai questi processi virtuali.
# Virtualizzazione a livello di sistema
Ci si focalizza ora sulla **virtualizzazione a livello di sistema o hardware**, la quale viene ottenuta per mezzo di un *VVM* o *hypervisor*.

Si chiarisce in primis la differenza tra **host** e **guest**:
- Con **host** si intende la piattaforma di base sulla quale verranno eseguite le macchine virtuali, in particolare un host è un macchina fisica, con eventualmente un sistema operativo detto *host OS*, e un *hypervisor*;
- Con **guest** si intende tutto ciò che è contenuto in una macchina virtuale: ad esempio il sistema operativo detto *Guest OS* e tutte le applicazioni che sono eseguite all'interno della VM.

Dunque, si classificano le diverse soluzioni di virtualizzazione a livello di sistema in base a due caratteristiche:
1. **Dove**, cioè a quale livello dell'host, viene eseguito l'hypervisor. Esistono due soluzioni: **VMM di sistema** o bare-metal hypervisor o hypervisor di tipo 1 e **VMM ospitati**, cioè che sfrutta il sistema operativo dell'host, detto anche hypervisor di tipo 2;
2. **Come** viene virtualizzata l'esecuzione delle istruzioni. Anche in questo caso ci sono due soluzioni: **Virtualizzazione completa** che può essere *assistita dall'hardware* o *assistita dal software* e la **paravirtualizzazione**.
![](Pasted%20image%2020240112221849.png)
## Hypervisor di sistema vs ospitato
Un **hypervisor di sistema** ha la caratteristica di essere direttamente posizionato ed eseguito al di sopra dell'hardware, mentre un **hypervisor ospitato** è collocato ed eseguito al di sopra del sistema operativo host.
![](Pasted%20image%2020240112222040.png)

Un **hypervisor di sistema** o di tipo 1 viene eseguito direttamente sopra l'hardware, dunque risulta essere un sistema operativo semplice (cioè non complesso come un classico sistema operativo host) che offre le funzionalità di virtualizzazione. In particolare, può avere un microkernel o un classico kernel monolitico e risulterà avere un minor overhead rispetto ad un hypervisor di tipo 2, dato che non c'è il livello del sistema operativo host tra VMm e hardware.
Un **hypervisor ospitato** o di tipo 2 viene eseguito al di sopra del sistema operativo host, quindi accede alle risorse hardware tramite le chiamate di sistema del sistema operativo. Dunque può i servizi di basso livello che sono offerti dall'host OS, come la gestione dell' I/O o lo scheduling delle risorse. Inoltre, non è necessario modificare il sistema operativo guest (tranne che nel caso della paravirtualizzazione).
## Virtualizzazione completa vs paravirtualizzazione
La **virtualizzazione completa** e la **paravirtualizzazione** sono le due modalità di dialogo tra macchine virtuali e hypervisor  che permettono l'esecuzione di istruzioni privilegiate. Le due modalità si differenziano in base come vengono gestite tali esecuzioni.

Nella virtualizzazione completa l'hypervisor espone ad ogni macchina virtuale un interfaccia hardware simulata *funzionalmente identica* a quella della sottostante macchina fisica. Dunque il VMM intercetta le richieste di accesso privileggiato all'hardware fatte dalle macchine virtuali e ne andrà ad emulare il comportamento.
Nella paravirtualizzazione l'hypervisor espone ad ogni macchina virtuale un interfaccia hardware simulata *funzionalmente simile*, ma non identica, a quella della sottostante macchina fisica. Dunque l'hypervisor espone alle macchine virtuali una *Virtual Hardware API*, cioè una interfaccia applicativa che virtualizza l'hardware che permetterà la gestione e l'isolamento delle macchine virtuali. In questo caso i sistemi operativi guest dovranno essere opportunamente modificati, in quanto dovranno interfacciarci con la Virtual Hardware API esposta dall'hypervisor.
## Problemi di realizzazione della virtualizzazione completa.
In generale, una architettura non virtualizzata opera secondo almeno *due livelli di protezione* o *rings*: **user** e **supervisor**. Il ring 0 è il livello con i privilegi massimi, mentre il ring 3 è quello con i privilegi minimi. Le applicazioni utente vengono eseguite in user mode nel ring 3, mentre il sistema operativo è eseguito in kernel mode nel ring 0.
![](Pasted%20image%2020240113113553.png)
Con la virtualizzazione, l'hypervisor deve operare nel ring 0, quello con i privilegi massimi, dato che deve poter controllare le risorse fisiche. Le applicazioni e il sistema operativo guest devono operare in user mode (ring 1 o 3). Quindi si verifica il problema del **ring deprivileging**: il SO guest, che dovrebbe operare nel ring 0, opera in un ring non privilegiato che non consente di eseguire istruzioni privilegiate, che sono fondamentale per il corretto funzionamento del sistema operativo.
Inoltre se il SO guest si trova ad operare nello stesso ring delle applicazioni, si presenta il problema del **ring compression**: poiché applicazioni e SO guest lavorano nello stesso livello, occorre proteggere lo spazio del SO guest.
### Soluzione al ring deprivilegin: trap-and-emulate
Per risolvere il problema del ring deprivileging, le istruzioni eseguite dal SO guest che dovrebbero essere eseguite in kernel mode, devono essere prese in carico dall'hypervisor. Dunque è necessario un meccanismo che permette di notificare un'eccezione all'hypervisor nel momento in cui il SO guest vuole eseguire un'istruzione privilegiata; in questo modo l'hypervisor prende il controllo dell'istruzioni privilegiata, ne controlla la correttezza, ne emula l'esecuzione e restituisce il risultato al sistema operativo guest.
Invece, le istruzioni non privilegiate possono essere eseguite direttamente dal SO guest.
![](Pasted%20image%2020240113114855.png)
Esistono però delle istruzioni non privilegiate che richiedono accesso alle risorse fisiche.
Nel 74 vengono definite un'insieme di condizioni sufficienti affinché una architettura di un calcolatore possa supportare in modo efficiente la virtualizzazione di sistema. In particolare, vengono definite tre classi di istruzioni ISA:
1. **Istruzioni privileggiate**: sono istruzioni che se vengono eseguite in user mode, causano direttamente un'eccezione;
2. **Istruzioni sensitive**: sono istruzioni che vanno a modificare le risorse sottostanti oppure osservano delle informazioni che indicano il livello corrente di privilegio;
	- **Control sensitive**: cambiano lo stato di privilegio;
	- **Behavior sensitive**: espongono uno stato di privilegio;
3. **Istruzioni innocue**: sono istruzioni non sensitive.

**Condizione necessaria per virtualizzazione**: Un monitor di macchina virtuale può essere costruito se il set di istruzioni sensitive della macchina host è un sottoinsieme delle istruzioni privilegiate. Questa è una condizione che non sempre viene soddisfatta, cioè possono essere presenti istruzioni che sono sensitive ma non privilegiate. Ad esempio, in x86 ci sono molte istruzioni sensitive ma non privilegiate, ad esempio `pushf` non è privilegiata ma sensitive. In generale, secondo la condizione necessaria, le architetture più comuni **non** sono virtualizzabili.
![](Pasted%20image%2020240113115920.png)
Le istruzioni sensitive per definizione non sono privilegiate, dunque non generano eccezioni. Dunque per virtualizzare le istruzioni sensitive e non privilegiate si propongono due soluzioni:
- Fare in modo che tutte le istruzioni sensitive (privilegiate e non) sfruttino il meccanismo di **trap-and-emulate** già introdotto: nel momento in cui queste istruzioni non privilegiate ma sensitive vengono eseguite, il controllo dell'esecuzione di tali istruzioni va in carico all'hypervisor che le esegue. Questa soluzione può essere realizzato sia in hardware che in software;
- Usare la **paravirtualizzazione**, dunque aggiungere un layer di chiamate invocabili dal sistema operativo guest e modificare tale sistema operativo per invocare tali chiamate.
## Meccanismi di trap
Il **meccanismo di trap** può essere realizzato a livello **hardware** quando il processore supporta la virtualizzazione e a livello **software** quando li processore non supporta la virtualizzazione.
### Livello hardware
Il meccanismo di trap a livello hardware è un supporto hardware detto **hardware-assisted cpu virtualization** e fornisce due nuovi modi operativi della CPU: **root mode**  e **non-root mode**, dove entrambi supportano i 4 rings classici delle architetture x86. In particolare, l'hypervisor viene eseguito in root mode, mentre il sistema operativo guest viene eseguito in non-root mode ma all'interno del livello di privilegio 0, cioè il livello sul quale si aspetta di essere eseguito. Nel momento in cui il so guest prova ad eseguire una istruzione privilegiata o non privilegiata ma sensitive, viene inviata una trap tramite il processore all'hypervisor, il quale si farà carico dell'istruzione emulandone il comportamento.
![](Pasted%20image%2020240113130514.png)
### Livello software
Il supporto hardware richiede un processore che sia in grado di offrirlo, e questi processori sono generalmente tutti quelli usciti dal 2007 in poi. In alternativa al supporto hardware, si utilizza una soluzione realizzata a livello software denominata **fast binary transaltion**, traduzione binaria veloce.
In questa soluzione il meccanismo di trap che intercetta le istruzioni privilegiate e le istruzioni non privilegiate ma sensitive e dunque passa il controllo all'hypervisor è implementato a livello software. In particolare, l'hypervisor scansiona il codice che deve essere eseguito e sostituisce i blocchi che contengono le istruzioni privilegiate (o sensitive) con blocchi funzionalmente equivalenti che contengono la trap all'hypervisor.
Questo meccanismo di scansionamento e traduzione del codice deve essere fatto in modo efficiente per avere una virtualizzazione veloce, dunque si implementa un meccanismo di caching che conserva le traduzioni già fatte per riutilizzarle successivamente.
In generale, questa soluzione rispetto a quella a livello hardware ha una maggiore complessità e peggiora le performance del meccanismo complessivo di virtualizzazione.
![](Pasted%20image%2020240113143546.png)
## Paravirtualizzazione
La **paravirtualizzazione** è una soluzione di virtualizzazione nella quale l'hypervisor espone una API virtuale, grazie alla quale vengono sostituite le istruzioni non virtualizzabili (cioè le chiamate privilegiate e sensitive) con delle *hypercall* che comunicano direttamente con l'hypervisor.
Le **hypercall** sono delle software trap fatte dal sistema operativo guest all'hypervisor, così come una systemcall è una trap fatta dall'applicazione a kernel del sistema operativo.
Per invocare queste hypercall, il kernel del sistema operativo guest deve essere modificato, in modo da poter invocare questa API virtuale esposta dall'hypervisor. Dunque a differenza della virtualizzazione completa, la paravirtualizzazione non è *completamente trasparente*, dato che il SO guest deve essere opportunamente modificato. 
![](Pasted%20image%2020240113150738.png)
Vantaggi della paravirtualizzazione rispetto alla virtualizzazione completa:
- Relativamente più semplice e pratico da realizzare, sopratutto rispetto alla fast binary translation, e non richiede un supporto hardware specifico;
- Crea un minor overhead rispetto alla fast binary translation, dato che non è richiesta la traduzione di blocchi di codice da parte dell'hypervisor;
Svantaggio principali invece è quello di richiedere un sistema operativo guest paravirtualizzato, cioè il codice sorgente del sistema operativo deve essere disponibile in modo tale da poter essere modificato. Inoltre questi sistemi operativi speciali devono essere mantenuti e aggiornati opportunamente, ricordando che tali sistemi non si possono anche usare su bare metal.
La seguente immagine riassume gli approcci visti fin'ora.
![](Pasted%20image%2020240113200307.png)
## Architettura di un hypervisor
A livello architetturale, un hypervisor è molto simile ad un sistema operativo, dato che è una componente che si fa carico dell'esecuzione di istruzioni. Un hypervisor contiene tre componenti principali:
- **Dispatcher**: è l'entry point dell'hypervisor che fa il routing delle istruzioni privilegiate richieste dalla macchina virtuale in uno dei due moduli seguenti;
- **Allocator o scheduler**: si occupa di decidere come allocare le risorse della macchina host alle molteplici macchine virtuali in esecuzione;
- **Interpreter**: si occupa di eseguire una routine specifica nel momento in cui la macchina virtuale esegue una istruzione privilegiata.
### Scheduler
La componente di **scheduling** si occupa di allocare le risorse hardware e fisiche della macchina host tra le diverse macchine virtuali in esecuzione, in modo che queste vedano delle CPU virtuali. Lo scheduler deve decidere quale macchina virtuale viene eseguita di volta in volta andandogli ad allocare la CPU fisica a disposizione dell'host. Dunque, lo scheduler definisce come vengono allocate le CPU virtuali delle macchine virtuali sulla CPU fisica.
### Virtualizzazione della memoria
Nelle architetture non virtualizzate è già presente una memoria virtuale, infatti a livello applicativo si ha una vista della memoria virtuale che poi deve essere mappata sulla memoria fisica della macchina. Questo meccanismo è offerto attraverso le tabelle delle pagine ed è estremamente efficiente grazie alle componenti hardware MMU e TLB che si occupano di questa traduzione da memoria virtuale a memoria fisica.
In un architettura virtualizzata, accade che più macchine virtuali condividono la stessa memoria fisica, cioè la memoria fisica della macchina host. Dunque l'hypervisor deve partizionare questa memoria tra le diverse macchine virtuali, in modo che ogni VM abbia la sua memoria isolata e non condivisa con le altre VM. In particolare, è necessario un nuovo livello di mapping degli indirizzi: la memoria virtuale guest viene mappata alla memoria fisica guest dal sistema operativo guest, quindi la memoria fisica guest viene mappa alla memoria fisica dell'host tramite l'hypervisor.
![](Pasted%20image%2020240113203129.png)
Dunque mappare la memoria virtuale guest alla memoria fisica dell'host richiede due salti passando per la memoria fisica guest, e questi passaggi devono essere efficienti per ottenere una buona performance di virtualizzazione.
In particolare, un indirizzo virtuale della memoria guest viene mappato ad un indirizzo fisico di memoria guest, il quale viene mappato ad un indirizzo di memoria fisico dell'host.
Inoltre, le macchine virtuali devono avere l'illusione di possedere una memoria fisica contigua che comincia all'indirizzo zero. Naturalmente, dato che la macchina host può avere molteplici VM, un indirizzo di memoria fisica del guest non corrisponde ad un indirizzo di memoria fisica dell'host, dunque la memoria fisica dell'host riservata ad una macchina virtuale non è necessariamente contigua.
Infine, gli indirizzi fisici assegnati alle diverse macchine virtuali devono essere ben isolati tra loro
#### Shadow page table
Per evitare il degrado di performance dato dal memory mapping aggiuntivo, l'hypervisor mantiene una *shadow page table*, cioè una tabella che mappa *direttamente* indirizzi di memoria virtuale del guest in indirizzi di memoria fisica dell'host. In particolare, questa shadow page table viene creata dall'hypervisor e caricata nell'MMU, e deve essere mantenuta aggiornata dall'hypervisor rispetto a cambiamenti fatti da ogni sistema operativo guest alle proprie page table, che vengono create e gestite dal SO guest in modo classico (naturalmente queste page table non sono usate dalla MMU dell'host).
![](Pasted%20image%2020240113214016.png)
Dunque l'hypervisor usa il TLB per mappare memoria virtuale delle V; in memoria fisica della macchina per evitare una doppia traduzione ad ogni accesso alla memoria da parte delle VM.
Inoltre l'hypervisor deve mantenere le shadow page table consistenti con le tabelle delle pagine dei sistemi operativi guest. Questo richiede che ogni cambiamento fatto ad una page table di un sistema operativo guest venga propagato nella shadow page table. In particolare, per effettuare tali aggiornamenti viene implementato un meccanismo di trap che richiama l'hypervisor ogni volta che il sistema operativo guest scrive su una sua page table; dunque l'hypervisor scrive il cambiamento sia nella shadow page table che nella page table del SO guest e restituisce il controllo al SO guest. Tale meccanismo aggiunge dell'overhead.
![](Pasted%20image%2020240113231044.png)
Quella delle shadow page table è una soluzione software, che presenta diversi aspetti critici:
- L'hypervisor deve preservare l'illusione per il SO guest di avere una memoria fisica contigua che comincia da zero;
- L'implementazione della shadow page table è complessa e l'hypervisor deve intercettare le operazioni di paging e costruire delle copie della page table guest;
- In generale una SPT consuma una quantità significativa di memoria nell'host, inoltre il mantenimento della consistenza e l'invocazione dell'hypervisor ad ogni operazione di paging (con conseguente uscita dalla macchina virtuale) generano un overhead non indifferente.

Per evitare gli overhead causati dalle shadow page table, i processori di nuova generazione supportano la traduzione da indirizzi logici guest ad indirizzi fisici host in hardware attraverso una soluzione detta **Second Level Address Translation**. L'utilizzo di tale soluzione comporta un significativo miglioramento delle prestazioni rispetto al meccanismo con le shadow page table.
![](Pasted%20image%2020240113231037.png)
# Caso studio: Xen
[**Xen**](www.xenproject.org) è un hypervisor open source di tipo 1, dunque viene eseguito direttamente sull'hardware, con un design a *microkernel* che fa uso della paravirtualizzazione. Dunque fornisce al sistema operativo guest una interfaccia virtuale, o *hypercall API*, alla quale delle fare riferimento nel momento in cui vuole accedere alle risorse fisiche della macchina. Oltre alla paravirtualizzazione, supporta la virtualizzazione assistita dall'hardware, dunque è possibile utilizzare anche sistemi operativi che non sono modificati per supportare l'API virtuale.

Xen è un hypervisor *leggero*, costituito da poche linee di codice e genera un footprint sulla memoria molto piccola. Inoltre è estremamente scalabile e risulta più robusto e sicuro rispetto ad altri hypervisor. Xen viene continuamente aggiornato e migliorato e risulta molto flessibile nella gestione, infatti è altamente configurabile. Infine, introduce un overhead estremamente contenuto rispetto all'ambiente senza virtualizzazione e supporta la migrazione live delle macchine virtuali.
Il punto debole di Xen sono le performance nelle operazioni di I/O.
## Architettura di Xen
L'obiettivo principale del gruppo di ricerca che ha progettato Xen era quello di creare un hypervisor capace di scalare fino a ~100 VMs in esecuzione su una singola macchina fisica senza andare a modificare l'Application Binary Interface.
Xen ha un design a microkernel e le componenti che vengono paravirtualizzate sono:
- Le istruzioni privilegiate, per cui quando tali istruzioni vengono chiamate dal sistema operativo guest vengono sostituite con delle hypercall;
- Le tabelle delle pagine, dunque gli accessi alla memoria;
- L'accesso al disco e ai dispositivi di I/O;
- Gli interrupt e i timer;
![](Pasted%20image%2020240114120543.png)
La figura mostra l'architettura generale di Xen. In basso è presente l'host inteso come macchina bare metal, con tutte le sue componenti hardware quali memoria, cpu e dispositivi di I/O. Al di sopra dell'hardware viene eseguito Xen. Dopodiché, al di sopra dell'hypervisor, ci sono una serie di macchine virtuali VM$_0$ (o Dom0), VM$_1$ (o DomU$_1$) fino a VM$_n$ (o DomU$_n$), dove VM$_0$ è una macchina virtuale speciale utilizzata per il controllo, che offre delle funzionalità di supporto alle altre macchine virtuali, che sono classiche macchine virtuali utente e non privilegiate.
La macchina VM$_0$ mette a disposizione dei driver nativi che permettono l'utilizzo di risorse I/O; questi driver verranno utilizzati dalle macchine virtuali utente.
### hypervisor
L'hypervisor è di tipo 1, dunque è in esecuzione direttamente sulla macchina fisica. È composto da uno scheduler che schedula l'esecuzione delle differenti macchine virtuali sulle CPU a disposizione, da una MMU che si occupa della gestione della memoria e da componenti che permettono di gestire interrupt e timer. Infine, mantiene delle informazione di gestione per ciascuna macchina virtuale (o **dominio**) e le informazioni di gestione delle CPU virtuali assegnate alle varie macchine virtuali utente.
### Domini
Al di sopra dell'hypervisor sono eseguite una serie di macchine virtuali, che in Xen sono dette **domini**. In particolare ci sono due tipi di dominio:
- **Dominio di controllo (Dom0)**: è una macchina virtuale specializzata con privilegi speciali che non hanno le altre macchine virtuali. Questi privilegi riguardano l'accesso diretto all'hardware, l'accesso alle funzionalità I/O del sistema. Infine, interagisce con le altre macchine virtuali. Questa macchina speciale deve essere sempre presente ed è la prima macchina avviata al boot di Xen. Inoltre contiene i driver per tutti i dispositivi e tre servizi di sistema: device emulation, xen store e toolstack.
- **Domini guest non privileggiati (DomU)**: sono le macchine virtuali non privilegiate che eseguono il sistema operativo e le applicazioni scelte dall'utente. Ad ognuna di queste sono assegnate delle CPU virtuali e sono totalmente isolate dall'hardware, dunque non hanno nessun privilegio per accedere a funzionalità hardware e di I/O.
#### XenStore e Toolstack
##### XenStore
Lo **XenStore** è un servizio di memorizzazione delle informazioni relativo alla gestione delle altre macchine virtuali e di Xen, condiviso tra la macchina virtuale di controllo e le altre macchine virtuali per mezzo di un demone chiamato `xenstored`. In questo spazio di memorizzazione sono salvate le configurazioni e tutte le informazioni di stato.
La memorizzazione è implementata come un sistema gerarchico di tipo chiave-valore. Inoltre, le componenti come i driver possono sottoscriversi a delle chiavi per essere notificati nel momento in cui i valori di tali chiavi vengono modificato.
##### Toolstack
Il **Toolstack** consente di andare a gestire il ciclo di vita (creazione, shutdown, pausa e migrazione) e la configurazione di una macchina virtuale.
In Xen per creare una nuova macchina virtuale, l'utente deve fornire un file di configurazione che specifica l'allocazione di memoria e di CPU e la configurazione dei dispositivi.
Il Toolstack si occupa del parsing del file e scrive le informazioni di configurazione della nuova macchina virtuale nello XenStore.
A questo punto quando la nuova macchina virtuale viene creata, si utilizzano i privilegi della Dom0, la quale effettua: il mapping della memoria guest, il caricamento del kernel e del BIOS e il setup dei canali di comunicazione con lo XenStore.
### Scheduler in Xen
Lo scheduling in Xen, e in generale in un hypervisor, ha il compito di decidere tra tutte le CPU virtuali, delle varie macchine virtuali, quali assegnare alle CPU fisiche a disposizione.
Xen permette di scegliere la politica di scheduling da utilizzare; lo scheduler di default è detto **Credit**.
In generale gli obiettivi di un algoritmo di scheduling per vCPUs sono:
- Fare in modo che tutte le macchine virtuali abbiano un accesso *equo* alle CPU fisiche. L'algoritmo usato da Xen è *proportional share*, cioè la quantità di CPU fisica è allocata in proporzione al numero di share, o pesi, che vengono assegnati alle diverse CPU virtuali;
- Fare in modo che la CPU fisica sia sempre occupata: l'algoritmo di Xen è *work-conserving*, cioè non permette alle CPU fisiche di andare in idle quando ci sono richieste di utilizzo non soddisfatte;
- Fare in modo che introduce un overhead minimale.
#### Credit scheduler
Il **credit** è lo scheduler di default di Xen di tipo *Proportional fair* e *work-conserving*.
In questo algoritmo ad ogni dominio (anche il Domain0) è assegnato un peso e, opzionalmente, un parametro detto *cap*. Entrambi i parametri sono configurabili. In particolare:
- Il **peso** rappresenta la quantità di CPU fisica allocata al dominio;
- Il **cap** rappresenta la quantità massima di CPU fisica che un dominio può utilizzare: un cap=0 indica che la vCPU può ricevere una quantità extra di CPU; un cap != 0 limita la quantità di CPU fisica extra che una CPU virtuale può ricevere, espresso in percentuale rispetto alle CPU fisiche;
Dopodiché, lo scheduler trasforma il peso in una allocazione di **credito** a ciascuna CPU virtuale. Quando una CPU virtuale viene allocata ad una CPU fisica, consuma il suo credito a disposizione.
In particolare, quando una vCPU ha credito negativo, si dice che il dominio di quella vCPU è in *OVER priority*; viceversa, il dominio è in *UNDER priority*.
Dunque per ogni CPU fisica, lo scheduler mantiene una *coda di vCPU* con priorità data dalla quantità di credito delle vCPU: le vCPU in UNDER priority hanno maggiore priorità rispetto a quelle in OVER priority. Lo scheduler seleziona la prima CPU virtuale in questa coda e la assegna alla CPU fisica per 30ms, dopo i quali la CPU fisica può essere assegnata ad un'altra vCPU. Le vCPU in OVER priority possono essere assegnate ad una CPU fisica solamente se non ci sono vCPU in UNDER priority nella coda.
Lo scheduler si occupa anche di fare bilanciamento del carico tra le diverse CPU fisiche nei sistemi multiprocessore. In particolare, prima che una CPU fisica vada in idle, lo scheduler controlla la presenza di vCPU in UNDER priority nelle code delle altre CPU fisiche: se ne trova una, viene assegnata a quella CPU fisica. In questo modo, nessuna CPU fisica va in idle se c'è del lavoro da fare.
# Confronto tra le performance degli hypervisor
Nel corso degli anni, lo sviluppo continuo sulle tecniche di virtualizzazione a livello di sistema ha fatto si che il degrado di prestazione percepito nell'esecuzione di un applicazione in ambiente virtualizzato rispetto la sua esecuzione in ambiente non virtualizzato si riducesse in modo deciso. Però è ancora presente dell'overhead, che si presenta in particolar modo quando molteplici macchine virtuali in esecuzione competono per le risorse hardware.
Si considerano due studi, dai quali si estrapola il seguente esito: non esiste una soluzione di virtualizzazione che è decisamente migliore delle altre; infatti differenti tipologie di hypervisor mostrano differenti prestazioni al variare del contesto e al tipo di applicazioni eseguite.
In un primo studio, confrontando quattro hypervisor differenti con virtualizzazione assistita dall'hardware, è risultato che non c'è un hypervisor nettamente migliore rispetto agli altri. In particolare, il maggior overhead si è notato nelle operazioni di I/O e networking, sopratutto per Xen nelle operazioni su piccole quantità di dati.
Anche in un secondo studio, fatto su applicazioni per i big data, è risultato che per operazioni CPU-intensive le differenze in prestazione tra i diversi hypervisor è minima, mentre per operazioni I/O-intensive le differenze in prestazioni sono più significative.
# Portabilità delle macchine virtuali
Lo strumento delle macchine virtuali a livello di sistema, va nella direzione della **portabilità**. Infatti per una macchina virtuale è definita una **immagine** associata a tale macchina virtuale.
Una **immagine di macchina virtuale** è un singolo file che contiene il sistema operativo, i dati, le applicazioni e le librerie. In generale, una immagine di macchina virtuale pesa svariati GB e può avere diversi formati. Per evitare vendor lock-in rispetto ad un prodotto di virtualizzazione e per avere maggiore portabilità, esiste un formato standard e open per le immagini di macchine virtuali: **Open Virtualization Forma (OVF)**.
Il formato OVF è uno standard open utilizzato per impacchettare e distribuire macchine virtuali, supportato da diversi prodotti di virtualizzazione. L'immagine della macchina virtuale è contenuta in un file `.ova`, mentre la configurazione di essa è specificata in XML all'interno di un file di supporto con estensione `.ovx`.
# Ridimensionamento e migrazione di macchine virtuali
Tramite lo strumento della virtualizzazione, è possibile implementare delle tecniche utili al deploy e alla gestione di ambienti virtualizzati a larga scala. In particolare, è possibile fare agilmente lo **scaling verticale** di macchine virtuali, cioè fare un *ridimensionamento dinamico* delle risorse a disposizione di una macchina virtuale; inoltre, è possibile effettuare la **migrazione live** di macchine virtuali, ossia è possibile spostarle su macchine fisiche differenti senza mai fermarne l'esecuzione. 
## Ridimensionamento dinamico
Il **ridimensionamento dinamico** di macchine virtuali è un meccanismo a *grana fine* che consente di allocare o deallocare delle risorse fisiche ad una macchina virtuale mentre è in esecuzione, senza necessità di riavviarla. In generale questa tecnica permette di effettuare lo scaling verticale in modo più rapido, ma non è supportato da tutti i prodotti di virtualizzazione e sistemi operativi guest.
In particolare, è possibile ridimensionare senza stoppare e riavviare la macchina virtuale: il **numero di vCPU** e la quantità di **memoria**.
### Ridimensionamento CPU
Nei sistemi operativi Linux è possibile ridimensionare il numero di CPU virtuali senza spegnere la macchina virtuale per mezzo dell'API **CPU hot-plug/hot-unplug**.
In Linux le informazioni riguardo le CPU allocate si trovano nel file system `sysfs`, in particolare nella directory `/sys/devices/system/cpu`. Da qui è possibile accendere e spegnere le diverse CPU, in particolare
```bash
# Turn on CPU
echo 1 > /sys/devices/system/cpu/cpu5/online 
# Turn off CPU
echo 0 > /sys/devices/system/cpu/cpu5/online 
```

Il CPU resizing delle macchine virtuali in esecuzione è gestibile tramite il tool `virsh`, che permette di controllare le macchine virtuali per alcuni hypervisor più comuni (KVM, Xen).
Ad esempio è possibili settare il numero di vCPU mentre la macchina virtuale è in esecuzione con il comando
```bash
virsh setvcpus <VM_name> <vcpu_count> --current
```
questo comando replica i comandi visti sopra all'interno del sistema operativo guest.
### Ridimensionamento memoria
Il ridimensionamento live della memoria di una macchina virtuale avviene attraverso la tecnica del *memory balooning*. L'idea è quella di avere una sorta di pallone all'interno della memoria:
- Sgonfiando il pallone, aumenta la quantità di memoria disponibile alla macchina virtuale, che comunque non può eccedere la dimensione massima di memoria;
- Gonfiando il pallone, diminuisce la quantità di memoria disponibile. Se una parte dell memoria tolta alla VM conteneva delle pagine, il SO guest deve effettuare lo swap di tali pagine su disco.
![|center|400](Pasted%20image%2020240114162717.png)
## Migrazione delle macchine virtuali
La migrazione delle macchine virtuali è utile nella gestione dei sistemi cluster e dei data center:
- semplifica il *consolidamento dell'infrastruttura*, ad esempio spostando delle macchine virtuali su un altro server in modo da poter spegnere un server fisico e risparmiare sul consumo energetico;
- permette di avere maggiore flessibilità, e abilita la cosiddetta *business continuity* cioè la possibilità di mantenere attivi i servizi nel momento in cui bisogna fare manutenzione ai server.
- è uno strumento di **bilanciamento del carico**, migrando le macchine virtuali che si trovano in macchine fisiche sovraccariche su macchine con meno carico di lavoro.
La migrazione deve essere supportata dall'hypervisor; inoltre l'overhead di migrazione non è trascurabile e la migrazione in rete WAN è scarsamente supportata.

In generale, esistono due approcci per migrare istanze di macchine virtuali tra differenti macchine virtuali:
- **Stop and copy**

In generale, esistono due approcci per migrare istanze di macchine virtuali tra differenti macchine virtuali:
- **Stop and copy**: approccio banale nel quale si spegne la macchina virtuale da migrare, si copia la sua immagine all'interno del nuovo host e si avvia tale VM. Questo approccio porta a tempi di disservizio molto lunghi, dato che le immagini delle VM sono potenzialmente molto pesanti e la banda di rete per il trasferimento può essere limitata;
- **Live migration**: è l'approccio principalmente utilizzato, nel quale l'istanza della macchina virtuale rimane in esecuzione durante la sua migrazione.
![](Pasted%20image%2020240115231407.png)
### Migrazione live
La migrazione live di una macchina virtuale comincia con una serie di passi preliminari, che avvengono prima della migrazione vera e propria. In particolare, questa fase è detta di **setup** e comprende operazioni tipo: determinare l'host sorgente, l'host di destinazione e la macchina virtuale da migrare; questa fase viene fatta tenendo conto dei motivi della migrazione (bilanciamento del carico, consolidamento dei server, risparmio energetico).  Inoltre, si determina **cosa** migrare della macchina virtuale: in particolare si tratterà la migrazione di **memoria, storage e connessioni di rete**. Infine, si determina **come** la migrazione deve avvenire; generalmente la migrazione deve apparire *trasparente* alle applicazioni che sono in esecuzione nella macchina virtuale. In generale però ottenere trasparenza nella migrazione è difficile e la migrazione live può comunque causare degli istanti di **downtime** dell'applicazione.
![](Pasted%20image%2020240115232454.png)
#### Migrazione dello storage
Per effettuare la migrazione dello storage, sono possibili diverse soluzioni.
- Se lo storage tra le macchine fisiche è *condiviso*, ad esempio il file system è distribuito, allora la migrazione è banale: lo storage della VM è visibile sia sulla macchina sorgente che sulla macchina di destinazione;
- Se lo storage non è condiviso, allora  l'hypervisor deve *congelare* lo storage della macchina virtuale presente nella macchina sorgente, creando un file di immagine (contente solo lo storage) che verrà trasferito sulla macchina fisica di destinazione.
#### Migrazione delle connessioni di rete
In generale, una macchina virtuale sorgente possiede un indirizzo IP virtuale che risulta differente dall'indirizzo IP della macchina host sorgente; può anche avere un suo indirizzo MAC virtuale distinto da quello dell'host. Il mapping tra indirizzi IP virtuali/MAC virtuali e macchine virtuali è mantenuto dall'hypervisor.
Se la macchina sorgente e la macchina di destinazione sono nella stessa sottorete IP non è necessario fare forwarding, perché viene fornita una risposta ARP non richiesta dalla sorgente, pubblicizzando che l'indirizzo IP si è spostato in una nuova posizione.
#### Migrazione della memoria
Con **migrazione della memoria** si intende il contenuto della memoria RAM, dei registri della CPU e dello stato dei dispositivi connessi.
##### Pre-copy
L'approccio classico implementato nella maggioranza degli hypervisor è detto **pre-copy**, nel quale la memoria viene copiata *prima* che la macchina virtuale riprenda l'esecuzione sull'host di destinazione. Questo approccio è composto da 3 fasi:
1. Fase di **pre-copy**: l'hypervisor copia in modo *iterativo* le pagine di memoria della macchina virtuale sorgente verso la macchina virtuale di destinazione *mentre* la macchina virtuale sorgente è in esecuzione. Viene fatto in modo iterativo perché la macchina sorgente continua ad essere in uso: la memoria verrà modificata rispetto a quella copiata; grazie a questo ciclo iterativo di copiatura, durante l'iterazione $n$ le pagine modificate durante l'iterazione $n-1$ di copiatura vengono aggiornate;
2. Fase di **stop-and-copy**: viene fermata l'esecuzione della macchina virtuale e vengono copiate sulla macchina di destinazione: le ultime pagine che risultano modificate, lo stato della CPU e lo stato dei driver dei dispositivi; in questa è presente un tempo di **downtime**, dato che la macchina sorgente è spenta e la macchina destinazione ancora non è attiva, dunque le applicazioni in esecuzione sulla VM non sono in esecuzione durante questo tempo.
3. Fase di **commitment e riattivazione**: la macchina virtuale di destinazione viene attivata e viene recuperata l'esecuzione delle applicazioni; la macchina virtuale sorgente viene rimossa.
L'immagine descrive le fasi di questo approccio.
![](Pasted%20image%2020240116095514.png)
L'approccio pre-copy non funziona bene, nel senso che la migrazione non avviene in modo trasparente, quando la macchina virtuale esegue delle applicazioni *memory intensive*: la memoria viene modificata continuamente, e ad ogni iterazione si devono copiare tanti dati, dunque il tempo di downtime sarà *significativo*.

Due approcci alternativi sono il **post-copy** e **ibrido**.
##### Post-copy
In questo approccio, lo stato della CPU e dei device driver viene copiato immediatamente sull'host di destinazione; in questo modo, la macchina host può attivare immediatamente la macchina virtuale. A questo punto, le pagine della memoria vengono copiate sulla macchina di destinazione su richiesta: la macchina di destinazione fa *pull* dalla macchina sorgente delle pagine di cui la macchina virtuale ha bisogno. Questo approccio risulta in un tempo di migrazione e downtime estremamente ridotto, ma può portare ad un degradamento delle prestazioni visto che le pagine vengono trasferite per mezzo della rete.
##### Ibrido
L'idea dell'approccio ibrido e quella di unire i due approcci pre e post copy. In particolare, si effettua un pre-copy parziale delle pagine di memoria  e si procede con un trasferimento post-copy. L'idea è quella di trasferire un sottoinsieme delle pagine accedute più frequentemente dalle applicazioni prima che l'esecuzione della macchina virtuale sia trasferita, in modo da ridurre il degrado di performance dovuto al trasferimento della memoria da host sorgente a host destinazione mentre la VM è in esecuzione sul nuovo host.
Dunque la fase di pre-copy riduce il numero di futuri page fault legati alla rete, poiché una vasta porzione della memoria della macchina virtuale è già stata pre copiata.

Negli hypervisor attuali, non esiste un implementazione standard per gli approcci post-copy e ibrido.

L'immagine seguente riassume i diversi approcci.
![](Pasted%20image%2020240116101439.png)

Infine, la migrazione live viene supportata da diversi hypervisor commerciali e open-source. Ad esempio, è possibile usare il tool `virsh` per effettuare la migrazione live.
```sh
$ virsh migrate --live [--undefinesource] \
[--copy-storage-all] [--copy-storage-inc] domain desturi
$ virsh migrate-setmaxdowntime domain downtime
$ virsh migrate-setspeed domain bandwidth
$ virsh migrate-getspeed domain
```
### Migrazione VM in WAN
La migrazione live di macchine virtuali può essere ottenuta anche in contesto di distribuzione geografica, cioè nel caso in cui il trasferimento delle VM avviene tra più data center distribuiti sul globo. I problemi principali in questo tipo di migrazioni riguarda le connessioni di rete: come mantenere la connessione di rete e preservare le connessioni aperte durante e dopo la migrazione.
#### Migrazione storage in WAN
In questo contesto, usare uno storage condiviso per la migrazione dello storage non è ideale, visto che i tempi di accesso possono risultare molto lenti, data la distribuzione geografica dei server.
Un'altro approccio è quello del fetching dello storage su richiesta, cioè vengono trasferiti solo alcuni blocchi di storage al destinatario e i blocchi rimanenti vengono trasferiti dalla sorgente solo quando vengono richiesti. Un crash della macchina sorgente comporta la perdita di tutta la memoria non trasferita.
Un ultimo approccio è quello del **pre-copy** insieme al **write throttling**. In questo caso, si effettua una pre-copia dello storage dalla sorgente alla destinazione mentre la VM sulla macchina sorgente continua l'esecuzione. Durante la copiatura, si tiene traccia delle operazioni di scrittura effettuate sulla macchina sorgente, in modo da replicarle sulla macchina di destinazione.
Nel caso in cui il tasso di scrittura è troppo rapido sulla macchina sorgente, esso viene rallentato in modo da renderlo simile al tasso di trasferimento verso la macchina di destinazione.
#### Migrazione rete in WAN
Si devono migrare tutte le connessioni di rete aperte verso la macchina virtuale sorgente effettuando una ridirezione di tali connessioni, in modo trasparente rispetto all'utente che utilizza la VM, verso la macchia virtuale di destinazione.
La soluzione principale è quella dell'**IP tunneling**. Consiste nel creare un tunnel tra il vecchio indirizzo IP sulla macchina virtuale sorgente e il nuovo indirizzo IP sulla macchina virtuale di destinazione e usarlo per inoltrare tutti i pacchetti che arrivano alla VM sorgente verso la VM destinazione. Il tunnel deve rimanere attivo fintantoché la migrazione non è stata completata. Quando la migrazione è completata, si aggiornano le entry DNS facendo puntare l'hostname del dominio al nuovo indirizzo IP della VM destinazione. Per avere questo tunnel, è necessario mantenere attiva la macchina virtuale sorgente.
Altre soluzioni possibili sono l'uso di una **VPN** o di un **software-defined networking**.
# Virtualizzazione a livello di sistema operativo
Si considera ora la virtualizzazione **a livello di sistema operativo**, anche nota come **virtualizzazione basata su container**.
L'idea in questo approccio di virtualizzazione è quella di sfruttare le funzionalità kernel del sistema operativo per creare dei **container**, cioè delle istanze di ambienti di esecuzione utente tra loro isolati. Ogni container viene eseguito al di sopra di un *singolo sistema operativo* e possiede un'insieme di processi, un file system, degli utenti, interfacce di rete e indirizzi IP, tabelle di routing, firewall e molto altro. Tutti i container in esecuzione sullo stesso host *condividono* solamente il **kernel** del sistema operativo host.
![](Pasted%20image%2020240116145552.png)
Dunque non si possono istanziare container con sistemi operativi differenti (inteso come kernel, ad esempio container differenti possono avere distribuzioni linux differenti).
## Meccanismi per OS-level virtualization
Si studiano ora i meccanismi offerti dal kernel Linux che permettono di realizzare la virtualizzazione basata su container, cioè che permettono di creare e gestire questi ambienti isolati. In particolare, è necessario poter isolare i processi tra di loro in termini di risorse hardware e software.
I meccanismi offerti dal kernel Linux e utilizzati a tale scopo sono:
- **chroot (change root directory)**: è un comando che permette di cambiare la directory root per il processo correntemente in esecuzione e per i suoi processi figli;
- **cgroups**: permettono di gestire le risorse hardware e software per gruppi di processi;
- **namespaces**: permettono di gestire l'isolamento dei singoli processi;
I **namespaces** implementano l'*isolamento delle informazioni*, cioè limitano cosa un container può vedere ed accedere.
I **cgroups** implementano l'*isolamento delle risorse*, cioè limitano il numero di risorse che un container può utilizzare.
### Namespaces
Il meccanismo dei **namespaces** permette di *isolare* ciò che un insieme di processi può visualizzare del sistema operativo. Dunque permette di creare ambienti isolati che hanno processi differenti, che operano su file differenti, su porte differenti ecc...
Tramite questo meccanismo le risorse del kernel vengono partizionate tra i diversi insiemi di processi in modo tale che un'insieme di processi veda un certo insieme di risorse, mentre un altro insieme di processi veda un insieme di risorse differente.
![](Pasted%20image%2020240116151019.png)
Esistono diverse tipologie di namespace:
- **mnt**: permette di isolare i punti di mount visti da un container. Dunque il file system viene virtualmente partizionato, in modo che processi in esecuzione su diversi mount namespaces non posso accedere a file al di fuori del loro punto di mount;
- **pid**: permette di isolare lo spazio dei PID (ProcessID), in modo che ogni processo veda solamente lui stesso e i suoi processi figli;
- **network**: permette di isolare i container rispetto allo stack di rete, cioè ogni container ha il suo stack di rete comprensivo di tabella di routing privata, un insieme di indirizzi IP e altre risorse relative alla rete;
- **utente**: permette di isolare gli identificativi di utenti e gruppi: ad esempio, permette di mappare un utente non root ad un utente che è root all'interno di un container;
- **uts (Unix timesharing)**: permette di effett
- **ipc**: permette di assegnare ad un container una memoria condivisa dedicata per la comunicazione IPC: ad esempio, delle code di messaggi Posix dedicate al container e differenti da quelle host.
### Cgroups
Il meccanismo dei **cgroups (control groups)** permette di limitare, misurare e isolare l'uso delle risorse hardware di un insieme di processi. Questo meccanismo permette, ad esempio, di allocare una certa quota di CPU e memoria a ciascun container.
I cgroups espongono un'interfaccia a basso livello, che viene generalmente usata dal container engine, simile al `sysfs`e `procfs`. Di default è montata nella directory
```bash
/sys/fs/cgroup/
```
## Vantaggi e svantaggi della OS-level virtualization
Si vedono ora vantaggi e svantaggi della virtualizzazione basata su container rispetto alla virtualizzazione di sistema basata su hypervisor. In breve: un container è molto più leggero rispetto ad una macchina virtuale, dato che contiene solamente applicazioni e librerie e non contiene un intero sistema operativo. Questo fatto porta sia dei vantaggi che degli svantaggi.
![](Pasted%20image%2020240116153056.png)
#### Vantaggi della virtualizzazione basata su container
Vantaggi rispetto alla virtualizzazione di sistema di tipo-1, cioè quella in cui l'hypervisor viene eseguito sull'hardware.
- **Degrado minimo delle prestazioni**. Non c'è il livello di indirezione dell'hypervisor: nessun meccanismo di emulazione per le istruzioni privilegiate o sensitive, le applicazioni in un container possono invocare direttamente le chiamate di sistema;
- **Minor tempo di startup e shutdown**. I container si spengono e accendono in pochi secondi o millisecondi; accendere una macchina virtuale richiede dei minuti;
- **Maggiore densità**. Su un singolo host si possono lanciare centinaia di container;
- **Minore footprint**. La footprint sulla memoria di un container è minima, dato che non include un intero kernel;
- Possibilità di **condividere pagine di memoria** tra molteplici container in esecuzione sulla stessa macchina fisica;
- Migliore **portabilità** e **interoperabilità**, i container sono ormai lo strumento di distribuzione del software più utilizzato;
- Rendono l'esecuzione di applicazioni **indipendente** dal contesto operativo sottostante.
#### Svantaggi della virtualizzazione basata su container
Svantaggi rispetto alla virtualizzazione di sistema di tipo-1.
- **Minore flessibilità**. All'interno dei container su una stessa macchina fisica non è possibile eseguire kernel differenti;
- Nei container si possono eseguire solo applicazioni **supportate nativamente** dal kernel del sistema operativo sottostante, dunque usando Linux, i container possono eseguire solamente applicazioni native per Linux;
- **Minor isolamento** e **maggiore interferenza** più elevata sulle risorse di sistema condivise, dato che l'isolamento nei container è a livello di processo;
- **Maggiori rischi di vulnerabilità** dati proprio dal minor isolamento dei container: le vulnerabilità del kernel compromettono l'intero sistema, e dunque compromettono tutto l'insieme dei container. In particolare, la compromissione di un container potrebbe compromettere altri container.
## Prodotti per virtualizzazione basata su container
I prodotti software più famosi che forniscono virtualizzazione basata su container sono
- **Docker**. Il *container engine* più popolare. Supporta lo standard OCI (Open Container Initiative), che definisce una standardizzazione per i container. Permette di creare *container di applicazioni*, dunque ogni container esegue un applicazione;
- **LXC (LinuX Container)**. È un container engine supportato nativamente dal kernel linux, e permette di creare *container di sistema*, cioè dei container che eseguono un intera immagine di sistema operativo;
- **Podman**. Alternativa simile a docker, che supporta lo standard OCI e permette di eseguire i container in modo *rootless*, cioè eseguirli da utente non root;
- FreeBSD Jail;
- OpenVZ;
- Virtuozzo Container.
### Solo per linux?
La virtualizzazione è supportata nativamente solo da Linux. Per eseguire container su sistemi windows o mac, si può utilizzare Docker Desktop, che comunque crea un'ambiente virtualizzato che esegue il docker engine sul quale vengono eseguiti i container. In alternativa, si può creare una macchina virtuale con Linux e installare il prodotto di virtualizzazione basato su container  al suo interno; questo porta ad un degrado delle prestazioni visto che si ha una *virtualizzazione annidata*.
## Containers, DevOps e CI/CD
I container sono la tecnologia abilitante nell'ambito del **DevOps** e del **CI/CD** (Continuous Integration and Continuous Deployment). In particolare, i container sono diventati uno strumento standard per effettuare il **build**, il **packaging**, la **condivisione** e il **deploy** di applicazioni e delle loro dipendenze. Questi consentono agli sviluppatori di scrivere codice in modo collaborativo condividendo immagini di container, semplificando contemporaneamente il rilascio in diversi ambienti senza ulteriori configurazioni.
![](Pasted%20image%2020240116165334.png)
### DevOps
Il **Development and Operation** è una metodologia di sviluppo software in cui l'obiettivo è quello di colmare la netta separazione tra la fase di sviluppo e la fase operativa. Il DevOps va nella direzione di enfatizzare la comunicazione e la collaborazione tra i diversi team che lavorano per lo sviluppo di un applicazione complessa, con l'obiettivo di migliorare la qualità del prodotto.
![](Pasted%20image%2020240116164228.png)
Tramite la containerizzazione, il continuo tra fase di sviluppo e fase operazionale delle applicazioni risulta più semplice.
### CI/CD
- **Continuous integration**: pratica di sviluppo software che unisce il lavoro di tutti gli sviluppatori che lavorano al progetto software;
- **Continuous delivery**: pratica che assicura rilasci continui e affidabili dell'applicazioni
### Microservizi e serverless
In generale usando i container le applicazioni e tutte le dipendenze vengono impacchettate in una singola immagine che può essere eseguita in un qualsiasi ambiente, andando ad utilizzare meno risorse rispetto ad una tradizionale macchina virtuale.
Allora i container sono la tecnologia abilitante per le **applicazioni a microservizi** e per il **serveless computing**: i microservizi e le funzioni vengono incapsulate in dei container.
## Ridimensionamento e migrazione dei container
Come per le macchine virtuali, si possono **ridimensionare** e **migrare** i container in docker.
### Ridimensionamento
Il ridimensionamento o *resizing* di un container consiste nel modificare dinamicamente i limiti alle risorse (CPU, memoria, I/O) impostati per tale container.
```bash
$ docker update [OPTIONS] CONTAINER [CONTAINER...]

$ docker update --cpu-shares 512 <containerID>
$ docker update --cpu-shares 512 -m 300M <containerID>
```
### Migrazione
Come nella migrazione di macchine virtuali, per migrare un container è necessario: salvare lo stato del container, trasferire tale stato nella macchina di destinazione e  ripristinare il container su tale macchina. Il salvataggio, il trasferimento e il ripristino dello stato di un container avviene ad applicazione congelata se non si utilizzano meccanismi di pre-copy o post-copy della memoria: la migrazione genera un downtime.
In generale, i container engine non supportano nativamente la migrazione, dunque si utilizzano dei tool aggiuntivi.
## Sicurezza, orchestrazione e container nel cloud
### Sicurezza dei container
### Orchestrazione di container
### Container nel cloud
# Nuovi approcci di virtualizzazione