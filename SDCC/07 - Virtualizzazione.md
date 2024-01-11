La virtualizzazione ha lo scopo di astrarre i dettagli di una implementazione sottostante per avere una vista logica differente dalla vista fisica. L'idea è quella di astrarre le risorse computazionali a disposizione disaccoppiando l'architettura percepita dalle applicazioni a livello utente con la realizzazione fisica.
I vantaggi della virtualizzazione sono numerosi: realizzare soluzioni più agili e flessibili, migliorare le prestazioni, migliorare la scalabilità orizzontale, migliorare la sicurezza creando ambienti maggiormente isolati tra loro, e molti altri.
## componenti dell'ambiente virtualizzato
Sono tre gli attori principali presenti in un'ambiente virtualizzato:
- **Guest.** È il livello più alto nell'architettura ed è colui che utilizza l'ambiente virtualizzato: non interagisce col sistema fisico ma si interfaccia con una vista astratta del sistema fisico per mezzo del *layer di virtualizzazione*, con il quale interagisce per mezzo delle risorse virtualizzate che sono esposte.
- **Host**. È l'ambiente fisico originale, in cui sono presenti le risorse fisiche: queste risorse vengono utilizzate dal *guest* in modo indiretto, grazie al *layer di virtualizzazione*.
- **Layer di virtualizzazione.**  È il layer di indirezione, responsabile della creazione dell'ambiente virtualizzato in cui l'host andrà ad operare.
# Macchina virtuale
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
## Livelli implementativi della virtualizzazione
La virtualizzazione può essere implementata a diversi livelli operazionali: nella trattazione si vedrà l'implementazione a livello hardware (sistemi di macchine virtuali) e l'implementazione a livello di sistema operativo (container).
### Interfacce in un calcolatore e virtualizzazione
Si ricordano i tre livelli di interfaccia presenti nei calcolatori moderni, elencati dal livello più basso al livello più alto:
- livello **ISA (instruction set architecture)**: è un interfaccia posizionata tra hardware e software che si distingue in ISA di sistema e ISA utente:
	- ISA di sistema: utilizzata primariamente per la gestione delle risorse, dunque contiene quelle istruzioni *privileggiate* eseguibili solamente dal sistema operativo;
	- ISA utente: utilizzato primariamente per operazioni computazionali, contiene le istruzioni *non privileggiate* eseguibili da ogni programma;
- livello dato dal sistema operativo che offre due interfacce:  le chiamate di sistema o *system call* e la application binary interface;
- livello dato dalle librerie utente che offrono le API (Application Programming Interface).
Dunque in generale una applicazione usa le chiamate API offerte dalla libreria, che andranno a fare delle chiamate di sistema, le quali andranno a far eseguire delle istruzioni macchina a livello ISA.

La virtualizzazione può essere implementata in questi livelli di interfaccia; in particolare, l'obiettivo della virtualizzazione per un certo livello è quello di **imitare** il comportamento dell'interfaccia in tale livello. 
#### Implementazione a livello ISA
L'obiettivo dell'implementazione della virtualizzazione a livello ISA è quello di **emulare** una certa interfaccia ISA al di sopra dell'interfaccia ISA della macchina host: dare l'illusione di lavorare su un ISA diverso da quello originale della macchina. Ad esempio, eseguire codice MIPS su una macchina x86 con il supporto dell'emulazione ISA.
L'emulazione ISA si può ottenere per mezzo di due tecniche:
- **code interpretation**: letteralmente interpretazione del codice, in generale una operazione lenta, dato che ogni istruzione sorgente viene interpretata dall'emulatore per poter eseguire le corrispettive istruzioni ISA native;
- **Dynamic binary translation**: è una tecnica che genera meno overhead, dato che vengono convertiti blocchi di istruzioni e non singole istruzioni.

quali modalità di dialogo tra vm ed il vmm per l'accesso alle risorse fisiche, ovvero come gesitre l'esecuzione di istruzioni privilegiate.

Virtualizzazione completa: hypervisor espone a ciascuna macchina virtuale un interfaccia hw simulata funzionalmente identica a quelle della macchina sottostante.
Paravirtualizzazione: hypervisor espone interfaccia hw simulate funzionalmente simli VH API per assicurare la gestione delle vm ed il loro isolamento.

Problemi di realizzazione di virtualizzazione completa.
architettura non virtualizzata lavora secondo almeno due livelli di protezione: user e supervisor.
con la virtualizzazione, hypervisor deve operare nel ring 0 superuser, con i privilegi massimi. Le applicazioni e il SO guest operano in user mode (ring 1 o 3). Si verifica il problema del ring deprivileging: il SO guest opera in un ring che non gli è proprio -> non può eseguire istruzioni privilegiati.
problema del ring compression: poiché applicazioni e so lavorano nello stesso livello, occorre proteggere lo spazio del SO.

Soluzione ring deprivileging: trap-and-emulate -> far si che le istruzioni eseguite dal so guest che andrebbero eseguite in kernel mode, vengano prese in carico dall'hypervisor. Meccanismo che permette di notificare eccezione all hypervisor, e dunque prende il controllo della gestione dell'istruzioni privilegiata, emulandone il comportamento. Le istruzioni non privilegiate possono essere eseguite direttamente dal SO guest.
esistono però delle istruzioni non privilegiate che richiedono risorse fisiche. nel 74definite una serie di condizioni sufficienti per far si che una architettura supporti virtualizzazione efficientemente.
istruzioni isa in 3 gruppi
1. privileggiate: sono quelle istruzioni che se vengono eseguite in user mode, chiamano un eccezione
2. sensitive: sono istruzioni che vanno a modificare le risorse sottostanti oppure osservano delle informazioni che indicano il livello corrente di privilegio.
3. innocue

Condizione necessaria: vm efficienti se il set di istruzioni sensitive è un sottoinsieme delle istruzioni privileggiate. condizione non sempre soddisfatta, cioè possono essere presenti istruzioni sensitive ma non privilegiate.
in x86 molte istruzioni sensitive ma non privilegiate, ad esempio pushf non è privilegiata ma sensitive.

1a soluzione: trap-and-emulate per istruzioni sensitive, come si implementa? 
2a soluzione: paravirtualizzazione, aggiungo un layer di chiamate invocabili da so guest.

realizzazione meccanismo di trap: a livello hardware se il processore fornisce supporto alla virtualizzazione -> hardware-assisted cpu virtualization; a livello software se il processore non fornisce supporto alla virtualizzazione -> fast binary translation.

hardware: vengono aggiunti due modi operativi per la cpu: modo non root e modo root: ciascuno di questi supporta i 4 ring di protezione. Non si pone più il problema della ring deprivilegin e compression. SO guest viene eseguito in ring 0 con livello di privilegio non root. istruzione che non può eseguire intercettata da hardware e passata all'hypervisor nella modalità root.

fast binary translation: 

Paravirtualizzazione: hypervisor espone api virtuali, chiamate a queste api tramite hypercall fatte dal so guest all hypervisor. sono equivalenti a syscall ma nell'ambito della virtualizzazione.

Architettura di un hypervisor: è una sorta di so, visto che si fa carico dell'esecuzione di istruzioni.
Tre componenti princiapli:
- dispatcher: fa il routing delle istruzioni privilegiate;
- allocator: decide come andare ad allocare le risorse dell'host alle macchine virtuali;
- interprete: si occupa di eseguire routine specifica quando guest esegue istruzione privilegiata.
Scheduler: va a schedulare le risorse hw fisiche tra le diverse macchine virtuali in modo che esse vedano delle cpu virtuali. Lo scheduler deve decidere quale mv viene eseguita di volta in volta ndandogli ad allocare la cpu fisica. lo scheduling di queste cpu virtuali su cpu fisica.

virtualizzazione memoria
in arch non virtualizzata, a livello applicativo vediamo la mem virtuale che viene mappata su memoria fisica della macchina.
in ambiente virtualizzato, necessario partizionamento della memoria fisica dato che vm condividono la stessa memoria fisica. ulteriore livello di mapping degli indirizzi: da memoria virtuale guest -> memoria fisica guest -> memoria fisica host.