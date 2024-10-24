In questa sezione si introduce alla materia **Information Retrieval** e si presenta la principale struttura dati utilizzata per risolvere problemi di indicizzazione, l'**indice inverso (inverted-index)**.
# Information Retrieval
L'**Information Retrieval** studia una serie di tecniche e metodologie che permettono di *trovare informazioni* all'interno di una collezione di dati *non strutturata*, in modo da soddisfare l'*information need* di un certo utente. Tipicamente, per collezione dati non strutturati si intendono documenti testuali memorizzati su un computer.
Le tecniche sviluppate e studiate nel campo dell'IR sono utilizzate in diversi contesti, in particolare:
- Web search, con i **motori di ricerca** tipo Google;
- E-mail search;
- Computer filesystem search;
- Legal information retrieval.

Nella prima parte del corso si lavorerà con l'assunzione che le collezioni di documenti sono *statiche*, ovvero non cambiano nel tempo.
L'obiettivo principale dell'IR è dunque quello di: recuperare i documenti contenenti informazioni **rilevati** rispetto all'information need e dunque utili al completamento di un **task** da parte dell'utente.
# Classical search model
Il modello di ricerca di riferimento nella progettazione di sistemi di retrieval viene descritto nel seguente diagramma
![center|600](01-img01.png)

- L'utente deve risolvere un determinato problema (**user task**) e a tale scopo necessita di un qualche tipo di informazione (**information need**);
- Al fine di ottenere tale informazione l'utente effettua una richiesta (**query**) ad un sistema detto **search engine** (ad esempio, un motore di ricerca online);
- Il motore di ricerca utilizza delle strutture dati interne, costruite a partire da una collezione di documenti (**collection**) per ottenere una serie di risultati (**results**).
- Se l'utente non è soddisfatto dei risultati ottenuti, può effettuare una modifica alla richiesta (**query refinement**) al fine di esplicitare in modo più chiaro l'informazione cercata.
Al fine di individuare quali documenti sono maggiormente rilevanti, il sistema può utilizzare una funzione di *ranking* che ordina i documenti ritornati in base alla loro rilevanza per l'utente. 
## Precision e recall
Si definiscono due misure per stabilire quanto siano buoni i risultati restituiti da un sistema di IR.
- **Precision.** Viene definita come la frazione dei documenti ritornati che sono rilevanti per l'information need dell'utente rispetto a numero totale di documenti ritornati;
- **Recall.** Viene definita come la frazione dei documenti rilevanti ritornati rispetto al numero totale di documenti rilevanti presenti nella collezione.
Le due misure presentano tra loro il seguente trade-off:
- maggiore è la precisione del sistema, minore sarà la sua recall: pur di recuperare solamente i documenti utili, il sistema limiterà il numero di documenti ritornati, ovvero il sistema esclude dei falsi negativi;
- maggiore è la recall del sistema, minore sarà la sua precisione: pur di recuperare tutti i documenti utili, includerà nei risultati necessariamente alcuni falsi positivi.
Dato che un buon sistema di retrieval deve, generalmente,  essere tale da massimizzare entrambe le misure, spesso la performance del sistema è data dalla *media armonica* delle due misure.
# Boolean retrieval model
Si studia ora un primo modello di retrieval che permette di rispondere a semplici richieste.
## Boolean query problem
Si vuole progettare un sistema che sia in grado di recuperare i documenti che rispettano una certa richiesta di carattere booleano rispetto alle parole contenute in essi. Ad esempio deve poter soddisfare una richiesta del tipo
$$
\text{Brutus AND Caesar but NOT Calpurnia}
$$
e il sistema deve recuperare esattamente tutti i documenti che contengono Brutus e Caesar ma non Calpurnia, cioè deve soddisfare precisamente la richiesta booleana.
Senza un sistema di IR, il problema può essere risolto o per mezzo di una **query SQL**, che però risulta essere molto lenta, oppure per mezzo di un tool per la ricerca di stringhe in un documento come **grep**, che però risulta complesso da utilizzare per soddisfare certi task.
## Term-Doc Incidence Matrix
Per risolvere il problema si può utilizzare una matrice binaria $B$ che memorizza le occorrenze dei termini nei documenti. Le righe indicano i termini presenti nella collezione, mentre le colonne rappresentano i diversi documenti della collezione. Allora per ogni coppia termine $i$, documento $j$ vale che
$$
B[i,j]=\begin{cases}
1 \ \ \text{se il termine $i$-esimo è presente nel $j$-esimo documento} \\
0 \ \ \text{altrimenti}
\end{cases}
$$
Tale struttura dati prende il nome di **matrice di incidenza dei termini nei documenti** e permette di stabilire per ogni termine $i$, i documenti $j$ in cui compare $i$.

| **Antony and Cleopatra** | **Julius Caesar** | **The Tempest** | **Hamlet** | **Othello** | **Macbeth** |
| ------------------------ | ----------------- | --------------- | ---------- | ----------- | ----------- |
| **Antony**               | 1                 | 1               | 0          | 0           | 0           |
| **Brutus**               | 1                 | 1               | 0          | 1           | 0           |
| **Caesar**               | 1                 | 1               | 0          | 1           | 1           |
| **Calpurnia**            | 0                 | 1               | 0          | 0           | 0           |
| **Cleopatra**            | 1                 | 0               | 0          | 0           | 0           |
| **mercy**                | 1                 | 0               | 1          | 1           | 1           |
| **worser**               | 1                 | 0               | 1          | 1           | 1           |
| ...                      | ...               | ...             | ...        | ...         | ...         |
A questo punto per rispondere a richieste strutturate come la precedente è sufficiente considerare le righe di $B$ corrispondenti ai termini presenti nella richiesta e combinarle tra loro tramite le operazioni logiche bit a bit.
$$
\begin{split}
   \text{Caesar} &\longrightarrow \text{11011} \\
   \text{Brutus} &\longrightarrow \text{11010} \\
   \text{Calpurnia} &\longrightarrow \text{01000} \\
   \text{Brutus AND Caesar} &\longrightarrow \text{11010} \land \text{11011} = \text{11010} \\
   \text{NOT Calpurnia} &\longrightarrow \neg \text{01000} = \text{10111} \\
   \text{Brutus AND Caesar BUT NOT Calpurnia} &\longrightarrow \text{11010} \land \text{11011} \land \neg \text{01000}= \text{10010} \\
   \end{split}
$$
### Rappresentazione della matrice
Si osserva che la matrice $B$ risulta, in generale, una matrice **sparsa** in quanto tendenzialmente un testo contiene un numero di parole distinte limitato.
Allora la matrice $B$ può essere memorizzata in memoria seguendo due possibili rappresentazioni:
- Una rappresentazione **densa**, nella quale vengono memorizzate tutte le entrate di $B$, sia quelle nulle che quelle non nulle. Questo comporta un maggiore costo di memoria, ma potrebbe rendere l'esecuzione di alcune operazioni più efficienti;
- Una rappresentazione **sparsa**, nella quale si memorizzano solamente le entrate non nulle. Questo permette di risparmiare risorse di memoria, ma potrebbe rallentare l'esecuzione di alcuni operazioni.
Si supponga di trovarsi nel seguente contesto:
$$
\begin{cases}
   \text{documenti} &= 1000000 \,\, \text{(1 milione)} \\
   \text{token per documento} &= 1000 \\
   \text{bytes per token (in media)} &= 6 \\
   \text{termini distinti} &= 500000 \\
   \end{cases}
$$
segue che tale collezione occupa una quantità di memoria pari a
$$
\begin{split}
   \text{size collection} &= \text{documenti } \times \text{token per documento} \times \text{bytes per token} \\
   &= 1000000 \times 1000 \times 6 \\
   &= 6 \times 10^{9} \\
   &= 6 \text{GB} \\
   \end{split}
$$
mentre la matrice di incidenza ha memorizzati un numero di bit pari a
$$
\begin{split}
   \text{size matrice} &= \text{termini distinti} \times \text{documenti} \\
   &= 500000 \times 1000000 \\
   &= 500000000000 \\
   &= 5 \times 10^{11} \\
   \end{split}
$$
la quale è composta principalmente da $0$, con non più di un miliardo di bit ad $1$.
## Inverted Index
L'utilizzo della matrice $B$ nel sistema di retrieval comporta un estremo spreco di risorse. Si utilizza dunque una rappresentazione **sparsa** della matrice. La struttura dati risultante prende il nome di **indice inverso (Inverted Index)**: per ogni termine $i$, contiene una lista che mantiene tutti e soli i documenti $j$ che contengono $i$. All'interno dell'indice i documenti vengono identificati per mezzo di numeri seriali, detti $\text{docID}$.
L'indice inverso è dunque strutturato come segue: è composto da un dizionario in cui l'insieme delle chiavi è la lista dei termini, e ad ogni chiave $i$ viene associata una lista contenente tutti i $\text{docID}$ relativi a documenti in cui è presente il termine $i$. Tali liste sono dette **posting lists**.
$$
\begin{split}
   \text{Brutus} &\longrightarrow [1, 2, 4, 11, 31, 45] \\
   \text{Caesar} &\longrightarrow [1, 2, 4, 5, 6, 16] \\
   \text{Calpurnia} &\longrightarrow [2, 31, 54, 101] \\   
\end{split}
$$
Si osserva che i $\text{docID}$ nelle liste sono ordinati, con lo scopo di favorire alcune operazioni.
Si vede ora come costruire e utilizzare questa struttura dati, riassunto 
![center|600](01-img05.png)
### Initial stages of text processing
Prima della costruzione dell'indice inverso, i documenti devono subire diverse fasi di elaborazione. Queste fasi sono utili a trasformare la sequenza di simboli che appare in un documento in una sequenza di elementi atomici che possono poi essere usati all'interno dell'indice. Alcuni di queste fasi sono le seguenti:
- **Estrazione dei dati testuali dal documento**. Potenzialmente complicata, specialmente nel caso in cui i dati testuali sono contenuti in formati come word o pdf;
- **Tokenizzazione**. In questa fase si divide la sequenza di caratteri in elementi linguistici distinti, detti **token**, usando, ad esempio, gli spazi o la punteggiatura. Dunque un **token** è un'istanza di una sequenza di caratteri in un particolare documento che sono raggruppati come un'utile unità semantica per l'elaborazione. La classe di tutti i token che contengono la stessa sequenza di caratteri è detta **tipo**.
- **Normalizzazione**. I token differenti che rappresentano una stessa cosa vengono portati ad una forma comune, cioè ad uno stesso token. Ad esempio "U.S.A." viene portato nella forma "USA".
- **Stop words**. Si eliminano alcune parole molto comuni, dette *stop word*. Nella lingua inglese queste sono parole come the, a, to, of. Tale eliminazione viene fatta con lo scopo di risparmiare lo spazio utilizzato, in quanto le posting list associate alle stop words sono tendenzialmente molto grandi.
- **Stemming**. Per ogni termine all'interno dei documenti si separare la radice dalla desinenza, memorizzando solo la desinenza. In questo modo, parole con la stessa radice ma desinenza differente verranno associati allo stesso indice, con la conseguente riduzione del numero di termini (indici) totali.
- **Lemmatizzazione**. Per le lingue morfologicamente ricche come l'italiano lo stemming non garantisce buoni risultati. Dunque si applica la *lemmatizzazione*, che consiste nel riportare ogni parola alla sua forma base. Ad esempio, tutti i verbi vengono portati nella forma all'infinito.
In generale la fase di pre-processamento è critica e problematica, dunque si deve porre molta attenzione a come avviene tale fase.
### Costruzione dell'indice
La costruzione dell'indice inverso è effettuata in vari passi.
#### Sequenza di token
Si costruisce una sequenza di coppie $(\text{modifiedToken, docID})$; ogni coppia indica che il token $\text{modifiedToken}$ si trova nel documento associato a $\text{docID}$.
![center|600](01-img02.png)
#### Ordinamento
La sequenza di coppia viene ordinata, prima rispetto al termine e poi rispetto al $\text{docID}$.
![center|600](01-img03.png)
### Dizionario e postings
Si procede con la scansione della sequenza di coppie costruendo le posting list per ogni termine. Per ogni termine $i$, si memorizza anche il numero di documenti in cui $i$ appare.
Le coppie uguali vengono inserite una sola volta nel dizionario, memorizzando eventualmente informazioni sulla frequenza del termine all'interno del documento.
![](01-img04.png)
## Query processing
Una volta costruito l'indice, per processare una richiesta del tipo
$$
\text{Brutus AND Caesar}
$$
si procede nel modo seguente:
1. Si recupera la posting list associata a Brutus;
2. Si recupera la posting list associata a Caesar;
3. Si fa il merge delle due posting list, rispettando l'operatore utilizzato.
Un esempio di pseudo-codice scritto in python in grado di rispondere a delle query della forma $\text{term1 AND term2}$ è il seguente
```python
def intersect(term1, term2):
    answer = []
    
    # return posting lists associated with each terms
    p1 = InvIndex(term1)
    p2 = InvIndex(term2)

    while p1 is not None and p1 is not None:
        if p1.docID == p2.docID:
            answer.append(p1.docID)
            p1 = p1.nextDoc
            p2 = p2.nextDoc
        elif p1.docID < p2.docID:
            p1 = p1.nextDoc
        else:
            p2 = p2.nextDoc
```
Essendo le posting list entrambe ordinate, l'operazione di intersect richiede tempo $O(x+y)$ dove $x$ e $y$ sono le lunghezze delle due posting list.
### Operatore OR
Un esempio di pseudo-codice scritto in python in grado di rispondere a delle query della forma $\text{term1 OR term2}$ è il seguente
```python
def union(term1, term2):
    answer = []
    
    # return posting lists associated with each terms
    p1 = InvIndex(term1)
    p2 = InvIndex(term2)

    while p1 is not None and p1 is not None:
        if p1.docID == p2.docID:
            answer.append(p1.docID)
            p1 = p1.nextDoc
            p2 = p2.nextDoc
        elif p1.docID < p2.docID:
            answer.append(p1.docID)
            p1 = p1.nextDoc
        else:
            answer.append(p1.docID)
            p2 = p2.nextDoc
	if p1 is None:
		while p2 is not None:
			answer.append(p2.docID)
            p2 = p2.nextDoc
	elif p2 is None:
		while p1 is not None:
			answer.append(p1.docID)
            p1 = p1.nextDoc
```
Essendo le posting list entrambe ordinate, l'operazione di union richiede tempo $O(x+y)$ dove $x$ e $y$ sono le lunghezze delle due posting list.
### Operatore differenza (AND NOT)
Un esempio di pseudo-codice scritto in python in grado di rispondere a delle query della forma $\text{term1 AND NOT term2}$ (differenza) è il seguente
```python
def difference(term1, term2):
    answer = []
    
    # return posting lists associated with each terms
    p1 = InvIndex(term1)
    p2 = InvIndex(term2)

    while p1 is not None and p1 is not None:
        if p1.docID == p2.docID:
            p1 = p1.nextDoc
            p2 = p2.nextDoc
        elif p1.docID < p2.docID:
            answer.append(p1.docID)
            p1 = p1.nextDoc
        else:
            p2 = p2.nextDoc
	if p2 is None:
		while p1 is not None:
			answer.append(p1.docID)
            p1 = p1.nextDoc
```
Essendo le posting list entrambe ordinate, l'operazione di difference richiede tempo $O(x+y)$ dove $x$ e $y$ sono le lunghezze delle due posting list.
### Ottimizzazione delle query
Si consideri la query $\text{Brutus AND Calpurina AND Caesar}$. Tale richiesta può essere eseguita in tre modi differenti:
1. $\text{(Brutus AND Calpurnia) AND Caesar}$;
2. $\text{Brutus AND (Calpurnia AND Caesar)}$;
2. $\text{Calpurnia AND (Brutus AND Caesar)}$.
Siano $n,m,l$ le lunghezze delle posting list dei rispettivi termini $\text{Brutus, Calpurnia, Caesar}$ e sia senza perdita di generalità $n<m<l$.
Si consideri la prima implementazione. La complessità della prima intersezione è pari a $2\min\{ n,m \} = O(n)$ e la lunghezza della posting list risultate è al più $n$. Infine la complessità dell'intersezione tra il risultato precedente e la posting list di $\text{Caesar}$ è pari a $2\min{n,l} = O(n).$ Dunque la complessità della prima implementazione è $O(n)$.
Si consideri ora la seconda implementazione. In questo caso la complessità della prima intersezione (quella tra parentesi) è pari a $O(m)$ mentre la seconda è $O(n)$ per una complessità totale pari a $O(n+m) \in O(m)$, peggiore in confronto alla prima implementazione.
Dunque le query di questo tipo possono essere ottimizzate intersecando prima la posting list di lunghezza minima, per poi iterare.
Per questo motivo, nell'indice inverso viene memorizzata per ogni termine $i$ la **frequenza**, ovvero il numero di documenti in cui $i$ appare.
### Esercizi sulle query
#### Esercizio 1
> (**tangerine** OR **trees**) AND (**marmalade** OR **skies**) AND (**kaleidoscope** OR **eyes**)

le frequenze dei termini sono riportate in tabella

| **Term**     | **Freq** |
| ------------ | -------- |
| eyes         | 213312   |
| kaleidoscope | 87009    |
| marmalade    | 107913   |
| skies        | 271658   |
| tangerine    | 46653    |
| trees        | 316812   |
Quale intersezione va processata prima?
##### Soluzione
Le unioni risultanti avranno dimensione
- $316812 \leqslant | \text{tangerine OR trees}|\leqslant 363465$;
- $271658 \leqslant | \text{marmalade OR skies}|\leqslant 379571$;
- $213312 \leqslant | \text{kaleidoscope OR eyes}|\leqslant 300321$;
Dunque l'ordine corretto è
$$
\text{(tangerine OR trees) AND ((marmalade OR skies) AND (kaleidoscope OR eyes))}
$$
---

### Exercise 2
Adapt the merge for the queries:
- _Brutus_ AND NOT _Caesar_ 
- _Brutus_ OR NOT _Caesar_ 
Can we still run through the merge in time $O(L_{1} + L_{2})$? What can we achieve?
##### Soluzione
Per la prima query è sufficiente applicare la funzione differenza precedentemente implementata, mantenendo dunque la complessità $O(L_{1} + L_{2})$.
# Phrase queries and positional indexes
Per costruzione, l'indice inverso definito nella sezione precedente non è in grado di rispondere a richieste in cui sono presenti delle *forme terminologiche*, ovvero parole separate da spazi che sono collegate tra loro e che fanno riferimento ad una singola entità. Ad esempio, una richiesta del tipo "Stanford University" dovrebbe essere trattata come un unica frase, ma l'indice inverso definito recupererebbe anche documenti con frasi del tipo "The inventor *Stanford* Ovshinsky never went to *university*".
Per poter supportare tali richieste, non è più sufficiente che le posting list siano semplicemente elenchi di documenti che contengono singoli termini. In questa sezione, si considerano due approcci per supportare queste **phrase queries** e la loro combinazione.
## Biwords indexes
Un primo approccio per risolvere tale problema è quello di indicizzare tutti i *bigrammi*, ovvero tutte le coppie di termini consecutivi presenti nel testo. Ad esempio, un testo del tipo
$$
\text{Friends, Romans, Countrymen}
$$
porta la generazione dei seguenti bigrammi:
$$
\{ \text{Friends, Romans} \},\{ \text{Romans, Countrymen} \}
$$
Con questo approccio il processamento di richieste con due parole risulta immediato. Le richieste che contengono più di due parole possono essere spezzate in richieste più piccole, ciascuna di due termini, poste in AND tra di loro.
Tale approccio presenta un paio di problematiche:
- **Presenza di falsi positivi.** Se una query contiene più di due parole consecutive, allora potrebbe recuperare dei falsi positivi, in quanto non è detto che i bigrammi della query siano unicamente consecutivi nel testo. In altre parole, se si considerano i bigrammi d'esempio precedenti, non è detto che le tre parole appaiano solo in modo consecutivo nel testo.
- **Uso eccessivo di memoria.** L'indice può diventare molto grande, specialmente se si indicizzano trigrammi di parole o in generale $n$-grammi di parole.
## Indice posizionale
Una seconda soluzione al problema è quella di memorizzare la posizione di ogni termine all'interno di ogni documento, andando ad ampliare le informazioni memorizzate dalle posting list. In particolare, ad ogni $\text{docID}$ nella posting list associata al termine $t$, viene associata una lista che contiene le posizioni nel documento in cui $t$ appare.
![center|600](584DFEA06C70DF2314535F095DE69F85.png)

![center|600](B39533A6C972D1048BEFBC6E85F35436.png)
Per processare le richieste con questa struttura dati si devono recuperare le posting list di ogni termine e si deve effettuare il merge di esse, controllando non solo che i vari termini siano presenti in un documento, ma anche che le loro posizioni nel documento siano compatibili con la query che si sta valutando.
Si osserva che grazie a questa struttura aggiunta si è in grado di rispondere anche a query con *richieste di prossimità*, ovvero query del tipo $\text{Tor /3 Vergata}$ dove $\text{/3}$ sta per *within 3 words of* e indica il fatto che i documenti da recuperare sono quelli che contengono le parole $\text{Tor}$ e $\text{Vergata}$ che si trovano distanti tra loro di al massimo 3 parole.
### Positional index size
Memorizzando tutte le posizioni in cui i termini compaiono, la dimensione dell'indice risulta chiaramente maggiore. Dunque anche la complessità dell'operazione di intersezione tra posting list cambia, perché il numero di elementi da confrontare non è limitato dal numero di documenti ma dal numero totale di token presenti nella collezione $T$. Quindi la complessità risulta $O(T)$ rispetto a $O(N)$.
Ciononostante, gli indici posizionali sono largamente utilizzati per via della potenza e dell'utilità delle richieste con frasi e di prossimità.
Dato che nell'indice posizionale è presente una entry per ogni occorrenza di ogni termine, la dimensione dell'indice dipende dalla dimensione media del documento: più è pesante, più termini ci sono, e più occorrenze dello stesso termine ci sono.
Il supplemento di memoria aggiuntiva potrebbe risultare eccessivo, soprattutto in presenza di molti termini altamente frequenti. Per esempio, sia $D$ un documento di dimensione $T$ (in termine di numero di termini). Sia $t$ un termine generico all'interno di $D$ con frequenza all'incirca dell'$1\%$. Allora la memoria usata dal termine $t$ per il solo documento $D$ cresce secondo la tabella seguente.

| T      | Expected postings | Expected entries in positional list |
| ------ | ----------------- | ----------------------------------- |
| 1000   | 1                 | 1                                   |
| 10000  | 1                 | 10                                  |
| 100000 | 1                 | 100                                 |
| ...    | ...               | ...                                 |
Dato che le lingue hanno una **struttura robusta**, si è visto che, empiricamente, un **positional index** è circa 2-4 volte più grande di uno non posizionale (_non eccessivamente grande_), e generalmente occupa il $35\%$ del volume dei testi originali (perciò può essere visto come una sorta di metodo di compressione).
## Combination schemes
I due schemi descritti in precedenza, i _biword indexes_ e i _positional indexes_, possono essere combinati tra loro per definire degli schemi ibridi che utilizzano entrambi gli approcci. Ad esempio, per le espressioni terminologicamente significative, come $\text{San Diego,}$ è opportuno mantenere la loro forma composta.
Uno schema di indicizzazione ancora più complesso è quello di costruire un indice extra in modo da rispondere alle query più richieste dagli utenti velocemente. Questo approccio aumenta la memoria utilizzata, ma permette di dare delle risposte più velocemente.
Si osserva infine che anche se le stop-words hanno principalmente un ruolo grammaticale, non portano significato alla frase e occupano molto spazio. Quindi tendenzialmente vengono rimosse dall'indice. Per gestire i termini composti che contengono stop-words, come $\text{The Who,}$ si memorizza il termine come un singolo token.