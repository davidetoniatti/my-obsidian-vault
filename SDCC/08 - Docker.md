**Docker** è un *container engine* leggero e open source che offre la virtualizzazione basata su container. Rilasciato nel 2013 con licenza open, è il container engine più diffuso e utilizzato.
I container vengono istanziati sopra il motore di Docker, o *Docker engine*, che viene eseguito al di sopra del sistema operativo.
Ciascun container include un'applicazione e tutte le dipendenze necessarie per eseguirla. Questo permette di distribuire il codice di una applicazione in modo agevole, evitando tutti i problemi di installazione legati alle dipende del codice stesso.
I container di Docker sono di *tipo applicativo*, dunque non hanno un sistema operativo completo e condividono lo stesso kernel del sistema operativo host: ogni container è eseguito come un processo isolato nello spazio utente.
Inoltre, i container Docker *non* sono vincolati a infrastrutture specifiche, ossia sono indipendenti dall'ambiente di esecuzione: si può eseguire la stessa applicazione containerizzata sia su linux che windows, è sufficiente che il Docker engine sia installato.
![[08-img01.png|center|500]]
Alcune caratteristiche di Docker sono:
- Deployment portabile tra macchine differenti;
- Versioning dei container;
- Possibilità di riusare delle componenti, in Docker si possono riutilizzare i *layer*;
- Librerie condivise tramite registri;
- Supporto all'Open Container Initiative, che definisce un insieme di standard per i container, in modo da poter eseguire gli stessi container a prescindere da layer di containerizzazione utilizzato.
# Aspetti principali
Docker è scritto in linguaggio Go e sfrutta i meccanismi offerti dal kernel *Linux* per creare gli ambienti di esecuzione indipendenti e isolati tra loro, cioè i container. In particolare, utilizza i **cgroups** e i **namespaces**:
- **namespace**: sono utilizzati per isolare le informazioni che ciascun container può visualizzare (insieme dei processi, porzione di filesystem, ecc);
- **cgroups**: sono utilizzati per isolare le risorse (hardware) che ciascun container utilizza, ovvero stabiliscono la quantità di risorse che un container può utilizzare (CPU, memoria, ecc);

Inizialmente, Docker era basato sul container engine LXC (LinuX Container). A partire dal 2015, Docker sviluppa un proprio runtime runtime *libcontainer* che fornisce un interfaccia nativa in Go per la creazione dei container con i namespaces, cgroups e per il controllo dell'accesso al filesystem e consente di gestire il ciclo di vita di un container.
Libcontainer è incluso in *runc*: un tool CLI che permette di creare e eseguire container secondo le specifiche OCI.
## Docker engine
Il cuore del sistema è dato dal **Docker Engine**, una applicazione di tipo client/server, con entrambi i lati installati sulla stessa macchina.
Il server è detto *Docker daemon*: si mette in ascolto delle richieste effettuate dal client e si occupa di gestire le diverse entità di Docker come le immagini, i container, le reti e i volumi. Il *client* può inviare comandi al Docker demon tramite una CLI.
Inoltre, Docker mette a disposizione una *API Rest* che specifica un'interfaccia che i programmi possono utilizzare per interagire con il demone di Docker.
![[08-sdcc_img02.png]]
# Architettura
Docker usa una architettura di tipo client/server. Dunque il client Docker invia le richieste al demone, il quale fa il build, esegue e distribuisce i container. Client e demone comunicano tramite *socket* o *API Rest*. 
![[08-sdcc_img03.png]]
# Immagine di un container
L'**immagine** di un container è un template di sola lettura, utilizzato per creare un container in Docker, ossia è la componente build di Docker).
Queste immagini, abilitano la possibilità di distribuire le applicazioni assieme al loro ambiente di esecuzioni. Infatti, queste immagini incorporano al loro interno l'applicazione da eseguire e tutte le dipendenze necessarie. Dunque grazie all'immagine si distribuisce tutto ciò che serve per istanziare il container contenente l'applicazione. Naturalmente, la macchina su cui viene costruito il container deve avere Docker engine installato.
Le immagini sono create a partire da un *Dockerfile*, il codice dell'applicazione e i dati. Inoltre, un'immagine è sempre basata su un'immagine padre.
Le immagini Docker possono essere caricate o scaricate da dei *registry* pubblico o privato. Questo sistema di registry implica la necessità di un meccanismo di naming per identificare nome di un immagine. Il nome di un'immagine è in docker ha il seguente formato:
```docker
[registro/][user/]name[:tag]
# di default il tag è `latest`
```
## Dockerfile
Un **Dockerfile** è un file contenente una sequenza di istruzioni che definiscono come l'immagine del container viene costruita. È un file di testo con una sintassi molto semplice. 
Con *contesto* di un immagine si intende un'insieme di file che viene incluso nell'immagine; ad esempio l'applicazione da eseguire nel container e le sue librerie.
Tramite il Dockerfile e i file di contesto è possibile creare un *immagine* docker, che contiene tutti gli oggetti per la creazione del container.

Alcune istruzioni del Dockerfile sono:
- **FROM**: per specificare l'immagine genitore da cui deriva immagine corrente; Docker utilizza *union filesystem*: i container sono strutturati a livelli, e più container possono condividere tra di loro dei livelli;
- **RUN**: per eseguire un comando in un nuovo layer al di sopra dell'immagine attuale e confermare i risultati; l'istruzione è eseguita *mentre* il container è in costruzione.
- **ENV:** consente di impostare delle variabili d'ambiente;
- **EXPOSE**: consente di esporre il container su una determinata porta a runtime, collegando la porta interna al container ad una porta dell'host;
- **CMD**: per specificare dei comandi di default da eseguire nel momento in cui il container è in esecuzione;
Un esempio di Dockerfile è il seguente.
```Dockerfile
FROM node:18-apline
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node","src/index.js"]
EXPOSE 3000
```
Consente di costruire l'immagine di un container che esegue una semplice applicazione scritta in *node.js*. Alcune osservazioni:
- L'immagine padre definita nella `FROM` viene scaricata dal registry pubblico di docker;
- Il comando `WORKDIR` permette di impostare la directory di lavoro: il file system del container verrà visualizzato a partire dalla cartella specificata;
- Il comando `COPY` copia il contenuto della cartella in cui si esegue il Dockerfile, identificata dal `.`,  nella directory di lavoro;
- Expose mappa la porta 3000 del container sulla porta 3000 dell'host.
## Build di un'immagine
Per costruire un'immagine a partire dal Dockerfile e dal contesto si utilizza il comando
```bash
$ docker build [OPTIONS] PATH | URL | -
```
dove il contesto è dato dall'insieme dei file specificato in `PATH` o in `URL`.
```bash
# Costruire l'immagine del Dockerfile precedente
$ docker build -t getting-started .
```
## Livelli di un'immagine
Ciascun container è composto da un'insieme di **layer**.
Ciascun layer viene identificato in modo univoco da un *hash* e ha una certa dimensione.
Questa struttura a livelli serve per ottimizzare lo spazio occupato dalle immagini: Docker utilizza uno *union filesystem* che permette di combinare diversi layer in una singola vista unificata.
Quando due container differenti utilizzano uno stesso layer, Docker scarica una sola copia di tale layer.
![](Pasted%20image%2020240116192524.png)

I layer sono *read-only* e solo l'ultimo layer è accessibile sia in lettura che in scrittura; questo è detto anche *layer del container*: i dati verranno scritti all'interno di questo layer.
In generale, il layer del container è utile solo per memorizzare dati di *effimeri* generati a runtime, che possono essere memorizzati solo per la durata della vita del container. Quindi una volta che il container viene spento e rimosso, si perdono i dati che sono stati scritti nel suo layer del container. Inoltre, le operazioni di scrittura su questo layer sono poco efficienti. L'alternativa da utilizzare per effettuare operazioni di lettura o scrittura è il montaggio di un *volume persistente*.

Quasi tutti i comandi nel Dockerfile corrispondono ad un layer. Ad esempio, eseguendo il comando `RUN`, si andrà a costruire un layer che conterrà tutte le modifiche avvenute per il comando (ad esempio, delle dipendenze scaricate).
Per ispezionare un immagine, inclusi i suoi layer, si utilizza il comando
```bash
docker inspect imageid
```

Tipicamente un container è **stateless**: non ha uno stato associato. Un container stateless infatti risulta più semplice da scalare, dato che non è necessario copiare uno stato sulle repliche da creare. Inoltre, se subisce un crash, non è necessario avere un meccanismo per recuperare lo stato: è sufficiente riavviare il container. Infine, risulta più semplice fare la migrazione.
Dunque tipicamente vengono scritti pochi dati nel layer di scrittura del container; i dati sono tipicamente scritti e mantenuti in un **Docker volume**, cioè uno spazio di memorizzazione persistente.

Lo **storage driver** è la componente di docker che si occupa della memorizzazione e gestione di immagini e container sull'host. Esistono molteplici storage driver: **Overlay2** è lo storage driver di default. La scelta dello storage driver influisce sulle performance dei container.
## Avvio di un'immagine
Una volta definita e costruita un'immagine Docker, è possibile eseguirla usando il comando `docker run`. A questo punto viene creato effettivamente un **container Docker**, cioè l'istanza eseguibile di una Docker image. Tramite i comandi di Docker è possibile gestire i container, andandoli ad avviare, stopparli, muoverli o eliminarli.
In Docker è possibile anche fare il `run` di un container la cui immagine non è presente sulla macchina host. In questo caso, l'immagine può essere ottenuta da un **Docker registry**, cioè un registro dal quale è possibile scaricare e distribuire delle immagini Docker. Questi registri possono essere pubblici o privati; Docker mantiene un registro pubblico di nome *Docker Hub*, che è il registro di default dal quale vengono scaricate le immagini.

La figura seguente illustra cosa accade quando si esegue il comando `run` di un container.
![](Pasted%20image%2020240116195401.png)
Quest'altra figura invece illustra i cambiamenti di stato che possono avvenire in un container di Docker.
![](Pasted%20image%2020240116195424.png)
# Comandi 
## Docker info
## Gestione delle immagini
## Docker run
## Gestione dei container
# Networking
# Volumi
Un **volume** in Docker è uno spazio sul disco dell'host **persistente** che può essere usato in lettura e in scrittura dai container. Un volume corrisponde ad una directory del file system gestita da Docker, nel percorso `/var/lib/docker/volumes/` . I dati scritti dai container nel volume vengono scritti e mantenuti da Docker all'interno di questa cartella. Un volume non deve necessariamente essere presente nell'host, infatti se non è presente viene creato su richiesta nel momento dell'avvio di un container che richiede tale volume. Per montare un volume su un container, si utilizza l'opzione `-v` al run del container.
Alcuni vantaggi dei volumi:
- completamente gestiti da Docker;
- possono essere usati per effettuare facilmente il backup e la migrazione di container;
- possono essere gestiti sia tramite CLI che API;
- possono essere condivisi tra molteplici container;
- possono essere criptati;
- possono essere pre-popolati, cioè si possono copiare dei file al di fuori dell'esecuzione dei container;
- risulta la scelta migliora per la memorizzazione di dati persistenti rispetto al layer di scrittura del container. Infatti un volume non incrementa la dimensione del container e il contenuto rimane disponibile anche al di fuori del ciclo di vita del container;
È consigliabile usare un volume per le applicazioni write-intensive, dato che il throughput della scrittura corrisponde al throughput del disco: quando si scrive su un volume si sta scrivendo effettivamente sul file system dell'host.
# Ridurre la dimensione delle immagini
Ottimizzare lo spazio occupato dalle immagini permette di: ridurre lo spazio occupato su disco, ridurre il tempo di download e ridurre il tempo di deployment. Inoltre un'immagine più piccola ha una minor superficie d'attacco, dunque garantisce maggiore sicurezza. 
Tecniche per ridurre la dimensione delle immagini:
1. Usare immagini padri che siano minimali, ossia delle distribuzioni di linux minimali tipo alpine. In alternativa, usare una immagine *distroless*: è un immagine che contiene soltanto l'applicazione e le dipendenze a runtime; non contiene la shell, package manager o qualsiasi altro programma disponibile in Linux;
2. Minimizzare il numero di layer dell'immagine
3. Fare build multistage. Tecnica utile in fase di sviluppo di una applicazione: si costruiscono immagini
4. Eseguire i comandi RUN per installare dipendenze e pacchetti prima dei comandi COPY: in questo modo si sfrutta il caching dei layer dell'immagine.
5. Usare il file dockerignore nel quale si possono specificare quali file e directory escludere nella creazione dell'immagine;
6. Mantenere i dati applicativi su un volume e non all'interno del container.
# Configurare memoria e CPU di un container
Di default, i container non hanno nessun limite sull'uso delle risorse: possono utilizzare le risorse che lo scheduler del sistema operativo mette a disposizione. Docker permette di limitare la quantità di memoria e di CPU che un container può utilizzare a runtime, attraverso dei flag nel comando `docker run`. Queste modifiche verranno applicate dal docker engine andando a cambiare le impostazioni dei cgroup del container.
## Memoria
Limitare la memoria di un container è utile per evitare che vada in out of memory. I limiti di quantità di memoria possono essere hard e soft:
- hard: container non può utilizzare più della quantità di memoria specificata, anche se a disposizione
- soft: container può usare tutta la memoria di cui a bisogno fintantoché non si verifichino certe condizioni, come ad esempio poca memoria a disposizione nel sistema.
## CPU
Diverse opzione per limitare l'uso di cicli di CPU da parte di un container.
- --cpus: hard limit sul numero di risorse di cpu;
- --cpuset-cpus: numero massimo di core che può usare;

# Docker compose
**Docker comopse** consente di effettuare il deployment di molteplici container, in comunicazione tra di loro, su uno stesso host. Permette di definire e coordinare una composizione di molteplici container da eseguire su un singolo host. Inoltre, questo strumento crea automaticamente una rete che collega tutti i container istanziati.
La composizione si definisce in un file `docker-compose.yaml`. Questo file contiene tutte le regole per la composizione: quali container istanziare e in quale ordine, i volumi da creare e montare e permette di stabilire delle dipendenze tra i diversi container.
In particolare, il file di compose è composto da **servizi**, in poche parole, un servizio corrisponde ad un container, al quale sono allegate una serie di parametri riguardo la scalabilità, le dipendenze, i limiti delle risorse, le porte esposte  e i volumi accessibili.