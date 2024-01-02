**Docker** è un *container engine* leggero e open source che offre la virtualizzazione basato su container. Rilasciato nel 2013 con licenza open, è il container engine più diffuso e utilizzato.
I container vengono istanziati sopra il motore di Docker, o *Docker engine*, che viene eseguito al di sopra del sistema operativo.
Ciascun container include un'applicazione assieme a tutte le dipendenze di cui ha bisogno per l'esecuzione. Questo permette di distribuire il codice di una applicazione in modo agevole, evitando tutti i problemi di installazione legati alle dipende del codice stesso.
I container di Docker sono di *tipo applicativo*, dunque non hanno un sistema operativo completo e condividono lo stesso kernel del sistema operativo host: ogni container è eseguito come un processo isolato nello spazio utente.
Inoltre, i container Docker non sono vincolati a infrastrutture specifiche, ossia
sono indipendenti dall'ambiente di esecuzione: si può eseguire la stessa applicazione containerizzata sia su linux che windows, basta che il docker engine sia installato.
![[08-img01.png|center|500]]
Alcune feature di Docker sono:
- Deployment portabile tra diverse macchine;
- Versioning dei container;
- Riuso di componenti;
- Librerie condivise tramite registri;
- Supporto all'Open Container Initiative, che definisce un insieme di standard per i container, in modo da poter eseguire gli stessi container a prescindere da layer di containerizzazione utilizzato.
## Aspetti di sistema
Docker è scritto in linguaggio Go e sfrutta i meccanismi offerti dal kernel linux per creare questi ambienti di esecuzione indipendenti e isolati tra loro che sono i container. In particolare, utilizza i *cgroups* e i *namesapces*:
- namespace: utilizzati per isolare le informazioni che ciascun container vede (insieme dei processi, porzione di filesystem);
- cgroups: utilizzati per isolare le risorse (hardware) che ciascun container utilizza (quanta cpu, memoria, ecc);
Inizialmente, Docker era basato sull'engine LXC; a partire dal 2015, utilizza il runtime *libcontainer* che fornisce un interfaccia nativa in Go per la creazione dei container con i namespaces, cgroups e controlli di accesso al filesystem e consente di gestire il ciclo di vita di un container.
Libcontainer è incluso in *runc*: un tool CLI che permette di creare e eseguire container secondo le specifiche OCI.

Il cuore del sistema è dato dal **Docker Engine**, una applicazione strutturata in modo client/server (dove i due lati sono installati sulla stessa macchina).
Il server è detto *docker demon*, si mette in ascolto di richieste effettuate dal client e si occupa di gestire le diverse entità di docker come le immagini, container, reti e volumi. Il client può inviare comandi al docker demon tramite una CLI. Docker mette a disposizione una API Rest che specifica l'interfaccia che i programmi possono utilizzare per interagire con il server di Docker.
![[Pasted image 20231210120744.png]]
Come già detto, docker utilizza un architettura client-server: il client Docker invia le richieste al demone Docker, il quale fa il build, esegue e distribuisce i container. Client e demone comunicano via socket o API REST. 
![[Pasted image 20231210121003.png]]

Immagine di un container: è un template read-only, utilizzato per creare un container in docker (la componente build di docker). Andando a distribuire l'immagine, si va a distribuire ciò che server per istanziare un container. L'immagine incorpora al suo interno l'applicazione da eseguire e tutte le dipendenze necessarie. La macchina su cui viene costruito il container deve avere docker engine installato.
Un immagine pu essere costruita automaticamente a partire da un dockerfile.
Un dockerfile è una sequenza di istruzioni che definiscono il contenuto del container. è un file di testo con sintassi molto semplice.
Le immagini docker possono essere caricate o scaricate da un registry pubblico o privato: necessario meccanismo di naming per identificare nome di un immagine: `[registro/][user/]name[:tag]`. di default il tag è `latest`.
Le immagini sono create a partire dal docker file, il codice dell'applicazione e dati. Tipicamente una immagine è basata su un'immagine padre

from: specificare immagine genitore da cui deriva immagine corrente, vedremo che docker utilizza union filesystem: container strutturati a livelli, più container possono condividere tra di loro dei livelli
root del container = mountpoint del container
copy: copia il contenuto della directory in cui sto eseguendo il dockerfile per costruire l'immagine nella workdir
run: istruzione eseguita nel momento in cui il container viene creato
cmd: istruzione che sarà eseguita nel momento in cui il container è stato istanziato.

layer docker image
Ciascun container è composto da layer; ciascun layer è identificato in modo univico da un hash e ha una sua dimensione. Questa struttura serve per ottimizzare lo spazio occupato dalle immagini dei container: union filesystem permette di combinare diversi layer in modo da dare una vista unificata. Layer di container differenti possono essere lo stesso layer, riducendo  lo spazio occupato.
i layer sono readonly, l'ultimo layer è di lettura e scrittura detto anche layer del container: i dati verranno scritti all'interno di questo layer.
Il layer di scrittura è utile solo per memorizzare dati di tipo effimero generati a runtime e memorizzati solo per la durata del container. Una volta che il container è deallocato, si perdono i dati nel layer writable. Inoltre, le operazioni di scrittura su questo layer sono poco efficienti. L'alternativa da usare per effettuare operazioni di lettura o scrittura è il montaggio di un volume persistente.
Quasi tutti i comandi nel dockerfile corrispondono ad un layer.
Tipicamente un container è stateless: non ha uno stato associato. Un container stateless è più semplice da scalare

possibile limitare quantita di mem o cpu che un container può usare a runtime tramite flag di configurazione. docker engine implementa queste configurazioni andando ad utilizzare i meccanismi offerti dai cgroups.