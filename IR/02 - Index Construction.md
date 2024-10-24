In questo capitolo, si studiano tecniche e metodi per la costruzione di un indice inverso. Questo processo è detto *costruzione dell'indice* o *indexing*; il processo o la macchina che lo esegue è detto *indexer*.
# Hardware basics
Il dispositivo di memorizzazione utilizzato per salvare in modo persistente le strutture dati è l'**hard disk**. L'**SSD** invece, nonostante la maggiore velocità, è caratterizzato una obsolescenza programmata data dal numero di scritture medio, dunque non soddisfa i requisiti di persistenza.
Dato che l'hard disk è un dispositivo diviso in blocchi, e dato che per leggere un blocco l'hard disk deve spostare una testina fisica nel particolare settore in cui il blocco è situato, l'accesso ai dati sul disco è significativamente più lento dell'accesso dei dati in memoria. In particolare una delle misure più importanti per capire le performance degli hard disk è il **disk seek**, ovvero il tempo necessario per spostare la testina nel settore del disco in cui dobbiamo scrivere e/o leggere. L'idea è quella di minimizzare il numero di letture e scritture effettuate su disco, utilizzando la memoria principale il più possibile.
# Dataset Reuters RCV1
Per mostrare degli esempio concreti, si considera la collezione di articoli di Reuters. Le statistiche più importanti del dataset sono le seguenti.

- **Totale documenti**: $N = 800.000$
- **Numero medio di token per documento**: $L = 200$
- **Numero termini distinti**: $M = 400.000$
- **Numero medio byte per token con spazi/punteggiatura**: $6$
- **Numero medio byte per token senza spazi/punteggiatura**: $4.5$
- **Numero medio byte per termine**: $7.5$
- **Non positional postings**: $100.000.000$
# Basic inverted index construction
I passi base per la costruzione di un [indice inverso](01%20-%20Inverted-Index.md#Inverted%20Index) non posizionale sono:
- Elaborazione dei termini nella collezione di documenti per la costruzione delle coppie $\text{(term, docID)}$;
- Ordinamento delle coppie, prima rispetto ai termini e poi rispetto ai $\text{docID}$;
- Organizzazione dei $\text{docID}$ per ogni termine in posting list e calcolo di statistiche come la frequenza dei termini e la document frequency.
Per piccole collezioni, tutte queste operazioni possono essere effettuate in memoria. In generale, potrebbe presentarsi l'impossibilità di memorizzare tutte le coppie necessarie, specialmente nella costruzione di un indice posizionale. In particolare, se la memoria principale non è sufficiente, è necessario utilizzare un algoritmo di ordinamento che utilizza il disco. Per ottenere una velocità accettabile, l'algoritmo deve minimizzare il numero di ricerche casuali su disco durante l'ordinamento (letture sequenziali sono più veloci rispetto alle operazioni di seek).
In questa sezione, si studiano delle strategie utili alla costruzione di un indice in grado di supportare una collezione di documenti talmente grande da non poter essere memorizzata completamente in RAM: l'idea è quella di salvare i risultati intermedi sul disco.
## Blocked sort-based indexing (BSBI)
Il **Blocked Sort-Based Indexing** (BSBI) è un metodo per costruire un indice inverso non posizionale, basato sull'idea di spezzare il dataset in blocchi.

```ad-note
title: Nota
Per rendere la costruzione più efficiente i termini del dizionario verranno rappresentati da interi chiamati $\text{termID}$ anziché da stringhe. Il mapping può essere costruito *on the fly* durante l'elaborazione della collezione.
```

I passi del metodo BSBI sono i seguenti.
1. Si dividono le coppie $\text{(termID, docID)}$ in vari blocchi, ciascuno con la stessa grandezza;
2. Si effettua l'ordinamento delle coppie in ogni blocco direttamente in memoria, creando degli indici inversi parziali;
3. Si memorizzano sul disc tali indici inversi parziali come risultati intermedi;
4. Una volta che tutti i documenti sono stati processati, si esegue il merge degli indici parziali per ottenere l'indice inverso finale.
L'algoritmo è descritto dal seguente pseudocodice.
![center|600](C074DC0DF118728F63CBA907A57C2AA0.png)
L'algoritmo trasforma i documenti nelle coppie $\text{(termID, docID)}$ e accumula le coppie in memoria fintantoché il blocco non risulti pieno. Si osserva che la grandezza del blocco deve essere tale da permettere una memorizzazione in RAM del blocco. A questo punto il blocco viene *invertito*, cioè:
- le coppie vengono ordinate;
- tutte le coppie con $\text{termID}$ uguale vengono aggregate in una posting list, dove con posting si intende un $\text{docID}$.
Il risultato dell'inversione, cioè un indice inverso per il blocco caricato in memoria, viene scritto su disco.

Lo step finale consiste nel fondere i gli indici parziali in un unico indice inverso. Un esempio con due blocchi è mostrato in figura.
![center|600](83AC23B7B2EF5237A8BEADA74E4224C8.png)
Per effettuare il merge, si aprono tutti i block files simultaneamente e si mantiene un buffer in lettura per ognuno. Inoltre si mantiene un buffer di scrittura per il file di output. Ad ogni iterazione, si considera il $\text{termID}$ più piccolo che non è stato ancora processato usando una coda con priorità e si fondono tutte le posting list per quel $\text{termID}$ e si scrivono sul buffer di output.
La complessità di questo metodo è $\Theta(T\log T)$, dove $T$ è un upper bound al numero di oggetti da ordinare, ovvero al numero di coppie $\text{(termID, docID)}$ presenti nel dataset. Il tempo di costruzione utilizzando BSBI è tipicamente dominato dal tempo richiesto per parsare i documenti tramite la funzione $\texttt{ParseNextBlock()}$ e per eseguire la fusione finale tramite la funzione $\texttt{MergeBlocks()}$.
Anche se il BSBI è altamente scalabile, questo metodo necessita di memorizza sulla memoria fisica la struttura dati utilizzata per memorizzare il mapping term con termID. Per collezioni molto grandi, questa struttura dati, simile ad un dizionario, potrebbe non entrare in memoria. Sono necessarie dunque di soluzioni ancora più scalabili.
## Single-pass in-memory indexing (SPIMI)
Un metodo per la costruzione di un indice inverso più scalabile rispetto al BSBI è il  **Single-pass in-memory indexing (SPIMI)**. In questo metodo viene processata la collezione termine dopo termine fino a che non viene saturata la memoria fisica, ottenendo un blocco. Dunque si scrive il dizionario corrispondente al blocco appena processato su disco e si procede alla creazione di un nuovo dizionario per il blocco successivo. Quindi SPIMI può indicizzare collezioni di qualsiasi dimensioni a patto che ci sia sufficiente spazio disponibile su disco.
L'algoritmo SPIMI è descritto dallo pseudocodice in figura. La parte dell'algoritmo che genera lo stream di coppie $(\text{term,docID})$ a partire dalla collezione viene omessa. Le coppie prendono il nome di *token*, e l'algoritmo $\texttt{SPIMI-Invert}$ viene invocato iterativamente sullo stream di token fino a quando non è stata processata tutta la collezione.
![center|600](D9C166E80233190E54D65F16DEAD8E5F.png)
I token vengono elaborati uno alla volta. Quando un termine occorre per la prima volta, viene aggiunto al dizionario (implementato come una hash table) e viene creata la posting list da associare. Si osserva che, a differenza del BSBI, SPIMI aggiunge direttamente un posting (o $\text{docID}$) alla sua posting list: invece di raccogliere prima tutte le coppie e poi ordinarle, come in BSBI, ogni posting list è dinamica (cioè, la sua dimensione viene aumentata man mano) ed è immediatamente disponibile a memorizzare posting. Questo porta due vantaggi:
- l'algoritmo è più veloce, in quanto non viene effettuato nessun ordinamento;
- l'algoritmo usa meno memoria, in quanto per ogni posting lists è noto il termine a cui fa riferimento, dunque non si deve memorizzare più volte lo stesso $\text{termID}$.
Di conseguenza, i blocchi che le singole chiamate di $\texttt{SPIMI-Invert}$ possono elaborare sono molto più grandi e il processo di costruzione dell'indice nel suo complesso è più efficiente.
Quando la memoria viene esaurita, si scrive l'indice del blocco (che è formato da un dizionario e dalle postings lists) su disco. Si osserva che prima di fare questo step si devono ordinare i termini, in modo da facilitare il passo finale di merge. La fusione finale viene fatto in modo analogo al merge effettuato nel BSBI.
Un ulteriore vantaggio di SPIMI è la possibilità di utilizzare metodi di compressione sia per le posting list che per il dizionario. La compressione aumenta ulteriormente l'efficienza dell'algoritmo, perché si possono elaborare blocchi ancora più grandi e perché i singoli blocchi richiedono meno spazio su disco.
La complessità del SPIMI è $\Theta(T)$, in quanto non si effettua il sorting dei tokens, e tutte le operazioni sono al più lineari rispetto alla dimensione della collezione.
In conclusione, i vantaggi di utilizzare SPIMI rispetto a BSBI sono:
- Maggiore velocità, in quanto non si effettuano ordinamenti;
- Minor uso di memoria, in quanto è noto il termine a cui fa riferimento ogni posting list;
- Compatibile con metodi di compressione per il dizionario e per le posting list.
# Distributed indexing
Oggigiorno le collezioni di documenti sono così massive che non è possibile costruire indici per tali collezioni su una singola macchina. Questo è particolarmente vero se si deve costruire un indice per le pagine web. Di fatto i motori di ricerca usano algoritmi di indicizzazione distribuita per la costruzione di indici. Il risultato di questo processo distribuito è un indice partizionato in diverse macchine, solitamente rispetto ai documenti o rispetto ai termini. In questa sezione si descrive un metodo di indicizzazione distribuita che partiziona rispetto ai termini e che si basa sul paradigma **MapReduce**.
## Architettura master/worker
Il framework MapReduce si basa su una architettura distribuita del tipo master/worker, composta da computer standard (no supercomputer o hardware specializzato), per risolvere problemi che necessitano l'elaborazione di grandi quantità di dati:
- Le macchine **master** hanno il compito di coordinare il lavoro che devono svolgere le macchine worker. In particolare, i nodi master assegnano segmenti del lavoro globale alle macchine worker. In caso di fallimento di un nodo worker, il nodo master si occupa di riassegnare il lavoro in carico al nodo fallito verso un altro nodo worker attivo. In generale le macchine master occupano una frazione molto ridotta del cluster di macchine a disposizione, dunque è importante che queste siano meno soggette a guasti;
- Le macchine **Worker** invece sono semplici macchine che eseguono i compiti assegnati dai nodi master. Sono macchine che possono smettere di funzionare per un tempo limitato senza causare disagio al processo globale.
La divisione del lavoro effettuata dai masters deve avvenire sia a livello di macchina, ovvero più macchine devono lavorare contemporaneamente, sia a livello di task, ovvero il compito da eseguire deve essere spezzato in parti atomiche da assegnare ai vari worker.
## Tipi di task
Dunque il processo di costruzione dell'indice non è più una singola attività assegnata, con dati diversi, a diverse macchine; viene invece spezzato in task eseguiti in modo parallelo. In pratica, è una semplice applicazione del paradigma MapReduce.
Per prima cosa, la collezione di documenti viene partizionata in $n$ sottoinsiemi detti *split* di dimensione fissata e tale da poter distribuire il lavoro in modo equo ed efficiente. Allora nel processo sono presenti i due seguenti task:
- **Parsing**: è il processo che permette, partendo da uno split in input, di ottenere le coppie $(\text{term, docID})$. Le macchine worker che eseguono questo processo sono chiamate **parsers**. Un nodo master assegna ad un parser in idle uno split, e il parser scrive le coppie che ha generato in $j$ *segmenti*. Ogni segmento contiene un range di termini ordinati rispetto alla prima lettera.
- **Inverting**: consiste nel collezionare le coppie $(\text{term, docID})$ per un dato segmento al fine di effettuare un ordinamento delle coppie e generare le posting lists. Le coppie $(\text{term, docID})$ relative ad un dato segmento possono essere generate da più parsers. L'inverter, mano a mano che i vari parsers gli inviano le loro coppie, costruisce l'indice parziale. Gli indici parziali costruiti dagli inverters vengono poi uniti tra loro.
Un esempio del processo distribuito appena descritto con $j=3$ è mostrato nel seguente schema.
![center|600](4D66C6DD1C000621E890B1F3DCAF4BCE.png)
In tale processo, la fase di **Map** è data in mano ai parser, i quali trasformano gli splits in segmenti. La fase di **Reduce** è invece incaricata agli inverter, i quali fondono i segmenti in indici parziali.
# Dynamic Indexing
Fin'ora si sono trattate le collezioni di documenti con l'ipotesi che esse siano statiche. La maggior parte delle collezioni però sono frequentemente aggiornate con inserimenti, cancellazioni e aggiornamenti. Questo implica che i nuovi termini devono essere aggiunti al dizionario e le posting list devono essere aggiornate per i termini già presenti. L'approccio naive per implementare questo **indice dinamico** è quello di ricostruire periodicamente l'indice da zero: una buona soluzione solamente nel caso in cui la collezione varia poco frequentemente; inoltre il sistema deve avere memoria sufficiente per costruire un nuovo indice mentre mantiene quello vecchio.
## Indice ausiliario
Se i nuovi documento devono poter essere inclusi velocemente, una soluzione è quella di mantenere due indici: un'indice principale e un piccolo indice ausiliario che indicizza i documenti più recenti. L'indice ausiliario è mantenuto in memoria principale, e le richieste vengono soddisfatte guardando sia l'indice principale che quello ausiliario. La delete di un documento avviene per mezzo di una bitmask, che permette di filtrare i documenti ritornati, non includendo quelli eliminati nel risultato di una richiesta.
Quando l'indice ausiliario diventa troppo grande, si fonde con l'indice principale. Il costo di tale operazione dipende da come è salvato l'indice nel file system. Se ogni posting list è memorizzata in un file separato, allora il merge consiste nell'estendere ogni posting list dell'indice principale con la corrispondente posting list dell'indice ausiliario. Sfortunatamente i file system non riescono a gestire efficientemente un gran numero di file. L'alternativa più semplice consiste nel memorizzare l'indice come la concatenazione di tutte le posting list in un unico grande file.
Sia $T$ il numero di postings e sia $n$ la dimensione dell'indice ausiliario
## Logarithmic merge
Nello schema detto logarithmic merging, si mantengono $\log_{2}\left( \frac{T}{n} \right)$ indici $I_{0},I_{1},\dots$ di dimensione $2^0\times n, 2^1\times n\dots$ Fino ad $n$ posting vengono mantenuti in un indice ausiliario in memoria $Z_0$. Quando viene raggiungo il limite $n$, gli $2^0\times n$ posting in $Z_0$ vengono memorizzati nell'indice $I_0$ su disco. Quando $Z_0$ è nuovamente pieno viene fuso con $I_0$ e viene creato un indice $Z_1$ di dimensione $2^1\times n$. Dopodiché $Z_1$ o viene memorizzato come $I_1$ su disco (se ancora non presente) oppure viene fuso con l'indice $I_1$ già presente e viene creato un indice $Z_{2}$ di dimensione $2^2 \times n$; e cosi via. Le richieste vengono soddisfate guardando $Z_0$ e tutti gli indici $I_i$ su disco e fondendo i risultati.
![center|600](B075B20D5FAD497C2B60B1F706D2DF78.png)

![](2DF46993891DDB4FC3B5363565A09205.png)
Il logarithmic merge permette di diminuire il numero di merge, limitando il numero di indici ausiliari utilizzati. In particolare il numero di merge cresce in modo logaritmico al crescere del size totale dell'indice.
Sia $T$ il numero totale dei postings e sia $n$ la dimensione dell'indice ausiliario, allora la costruzione dell'indice ha costo $O\left( T\log\left( \frac{T}{n} \right) \right)$ in quanto si effettuano al più $O\left( \log\left( \frac{T}{n} \right) \right)$ operazioni di merge tra gli indici ausiliari. Invece l'elaborazione di una richiesta richiede la fusione di $O\left( \log\left( \frac{T}{n} \right) \right)$ indici.