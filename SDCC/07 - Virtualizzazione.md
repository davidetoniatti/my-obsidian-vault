La virtualizzazione ha lo scopo di astrarre i dettagli di una implementazione sottostante per avere una vista logica differente dalla vista fisica. Astrarre le risorse computazionali a disposizione disaccoppiando l'architettura percepita dall'utente con la realizzazione fisica.

componenti ambiente virtualizzato: 3 principali attori: guest,host,layer di virtualizzazione
Guest: livello piu alto, che sfrutta la virtualizzazione, e dunque non interagisce col sistema fisico ma vede una vista astratta di essa tramite il layer di virtualizzazione, con il quale interagisce e che espone le risorse virtualizzate.
Host: ambiente fisico originale, in cui abbiamo le risorse fisica, è il sistema che indirettamente, tramite il layer, il nostro guest andrà a utilizzare.
Layer di virtualizzazione: layer di indirezione

Una macchina virtuale consente di rappresentare le risorse hw/sw a disposizione in modo differente rispetto alla rappresentazione fisica. 

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