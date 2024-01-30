# Similar items
Molti problemi possono essere espressi come "trovare insiemi simili".
Ad esempio:
1. Pagine con una grande frazione di parole simili in esse
	- Per l'individuazione di duplicati, classificazione in base all'argomento
2. Clienti che comprano una grande frazione di prodotti simili
	- Prodotti con insiemi dei clienti simili 
#3. Immagini con caratteristiche simili
## Scene completion Problem
Dato un insieme di immagini $\mathbf{I}$ di dimensione $N$ molto grande, ed un'immagine target $i_t$, si vuole trovare all'interno di tale insieme l'immagine $i\in \mathbf{I}$ più simile rispetto al target $i_t$.

Le immagini in input possono venir rappresentate mediante una successione di $N$ punti in alta dimensione $x_1,x_2,\ldots,x_N$, dove l'immagine $x$ è rappresentata mediante un lungo vettore di colori di pixel, tale per cui a ciascun colore è associato univocamente un intero da $1$ a $c$, ad esempio:
$$

x = \begin{bmatrix} 
\ 1 & 4 & 1 \ \\
\ 0 & 2 & 1 \ \\
\ 0 & 1 & 0 \ 
\end{bmatrix} 
\longrightarrow
\begin{bmatrix}
\ 1 & 4 & 1 & 0 & 2 & 1 &0 & 1 & 0
\end{bmatrix}\ =\ 
\in \{0,1,\ldots,c\}^h
$$
dove $h$ è la dimensione dei dati.

Dati due data points $x_1$ ed $x_2$ si vuole quantificare quanto essi siano "distanti", ossia simili tra loro. Per fare ciò si introduce una funzione distanza $d(x_1,x_2)$.

Si vogliono quindi trovare tutte le coppie di punti $(x_i,x_j)$ che si trovano entro una data soglia di distanza $s$, ossia tali per cui $d(x_i,x_j) \leq s$.
La soluzione banale, ossia quella di controllare tutte le possibili coppie, richiederebbe tempo $\mathcal{O}(N^2h)$, essendo $N$ il numero di data points ed $h$ la loro dimensione.
Tale approccio, nonostante rispetti le delimitazioni polinomiali della trattabilità, risulta nella pratica proibitivo nel caso in cui il data set in input risulti essere di elevate dimensioni. Basti pensare ad un data set di un milione di oggetti, dove l'approccio banale richiederebbe l'analisi di mezzo trilione di coppie.

Ciò può essere fatto in tempo $\mathcal{O}(Nh^{'})$ , con $h^{'} << h$ con alta confidenza.

## Trovare documenti simili
Si studia il problema di trovare documenti simili all'interno di un data set di grandi dimensioni.
Dato quindi un numero $N$ di documenti, nell'ordine di milioni o anche miliardi, si vogliono individuare le coppie di documenti simili.
Per fare ciò, bisogna inizialmente chiarire il concetto di *similarità* tra documenti:
l'aspetto di similarità tra documenti ricercato in questo problema è a livello di caratteri, e non di significato. La similarità testuale ha diversi utilizzi, molti dei quali permettono di individuare documenti duplicati o "quasi duplicati". Si osserva che, dati due documenti, controllare se questi siano esattamente duplicati risulta possibile controllando semplicemente i due documenti carattere per carattere, e se questi differiscono allora non sono uguali. Bisogna tener presente che, in molte applicazioni, i documenti non risultano essere identici, ma nonostante ciò, condividono una grande porzione del loro testo. Alcuni esempi più significativi sono
- Plagio: Un'applicazione per testare la similarità testuale è quella dell'individuazione di documenti plagiati. Da un documento originale possono essere estratte solo singole parti, le quali possono venir utilizzate in un altro documento frutto di plagio. Potrebbero venir modificate in questo alcune parole, o l'ordine delle frasi nelle quali queste appaiono nel testo originale. Nonostante ciò, il documento frutto di questa operazione risulta contenere molto dell'originale. Nessun processo semplice di confronto tra documento carattere per carattere risulta essere in grado di identificare un plagio sofisticato.
- Mirror pages: Le pagine web possono venir duplicati in un maggior numero di host, affinché il loro carico venga distribuito. Le pagine di questi siti *mirror* saranno simili, ma raramente identiche. Ad esempio, ognuna di esse può contenere informazioni specifiche associate con il particolare host che le fornisce in rete, e ciascuna di esse può contenere riferimenti ad altri siti mirror. E' importante individuare pagine simili di questo tipo, affinché i motori di ricerca producano miglior risultati evitando di mostrare due pagine che sono quasi identiche.

Sono diverse le difficoltà tecniche che si possono incontrare al fine di risolvere questo problema, tra cui:
- Come nel plagio, molte porzioni brevi di un documento possono apparire in ordine differente in un altro.
- Come detto in precedenza, un data set di grandi dimensioni non permette il confronto tra tutte le possibili coppie di documenti.
- I documento possono essere di un numero e di una dimensione così elevata che non risulta possibile rappresentarli nella memoria principale.

Per individuare quindi le similarità tra documenti, bisogna definire il concetto di distanza tra essi. Per fare ciò si fa riferimento ad una particolare nozione di "similarità" insiemistica, che, dati due insiemi guarda la dimensione relativa della loro intersezione. 
Questa particolare similarità prende il nome di *Jaccard similarity*.

```ad-Definizione
title: Definizione (Jaccard Similarity)
Dati due insiemi $S$ e $T$, la loro *Jaccard similarity* è data dal rapporto tra la dimensione della loro intersezione e la dimensione della loro unione:
$$
\texttt{J.sim}(S,T) = \frac{|S \cap T|}{|S \cup T|}
$$
```

A partire da questo concetto di similarità, si definisce una particolare distanza tra due documenti.
```ad-Definizione
title: Definizione (Jaccard Distance)
Dati due insiemi $S$ e $T$, la loro *Jaccard distance* è definita come 
$$
d(S,T) = 1 - \texttt{J.sim}(S,T)
$$
```

Essendo tale misura applicabile ad insiemi di elementi, risulta necessario rappresentare i documenti, ossia stringhe di lunghezza finita, a livello insiemistico.
Sia $U$ un'alfabeto. Un documento $Doc$ è una stringa di caratteri, ossia $Doc \in U^{*}$. Il metodo più efficace per rappresentare documenti come insiemi, con l'obiettivo di identificare documenti lessicalmente simili, consiste nel costruire dal documento un insieme di stringhe di lunghezza limitata che appaiono all'interno di esso. Così facendo, i documenti che condividono delle parti nella forma di frasi, anche se queste appaiono in ordine differente, avranno molti elementi in comune. Tale approccio prende il nome di *shingling*.

Schematicamente, il processo di individuazione di documenti simili può essere descritto come segue:
1. **Input**: Si ha un universo di documenti $Doc \in U^*$ di grandi dimensioni, dove $U$ è l'alfabeto.
2. **Shingling**: Processo al fine di convertire documenti in insiemi di grandi dimensioni.
3. **Min-Hashing**: Processo al fine di convertire insiemi di grandi dimensioni in *signatures* di dimensioni ridotte, permettendo di stimare le *J. similarity* dei relativi insiemi.
4. **Locality-Sensitive Hashing**: Processo al fine di restringere il numero di confronti da effettuare per le coppie di documenti. Permette di concentrarsi su coppie di *signatures* provenienti probabilmente da documenti simili, evitando il costo quadratico dei confronti nel numero dei documenti.
5. **Output**: Coppie candidate di documenti.

Si studiano quindi i vari passi al fine della risoluzione del problema.
### Shingling
Si vogliono rappresentare i documenti mediante dati ad alta dimensione.
Un documento è una stringa di caratteri. Si definisce un $k-$shingle per un documento come una sottostringa di lunghezza $k$ trovata al suo interno. Individuando tali $k-$shingle, si può associare a ciascun documento l'insieme di $k-$shingles che appaiono una o più volte dentro di esso.
Analogamente, un $k-$shingle può essere definito come segue:
```ad-Definizione
title: Definizione ($k-$shingle)
Un $k-$shingle per un documento $D$ è una sequenza di $k$ tokens che appare all'interno di $D$.

I *tokens* possono essere caratteri, parole o altro, a seconda dell'applicazione. 
```

Sia $U$ l'universo di tutti i possibili tokens.
Sia $S$ la funzione che prende in input un documento e restituisce l'insieme dei suoi $k-$shingles per un dato $k$.

Si fa un esempio: Sia $D$ il documento costituito dalla stringa $\texttt{abcdabd}$, sia $k=2$ e siano i tokens nel documento dei caratteri. Allora, per $D$, il suo insieme di $2-$shingles è $S(D) = \{\texttt{ab,bc,cd,da,bd}\}$.
Si osserva che la sottostringa $\texttt{ab}$ appare due volte nel documento, ma solo una volta come shingle. E' possibile definire una variante dello shingling che produce dei multiset, che prendono il nome di *bag*. In questo caso, ogni shingle appare nel risultato tante volte quante appare all'interno del documento. Per l'esempio precedente, si ha che $S(D)=\{\texttt{ab,bc,cd,da,ab,bd}\}$.

Ogni documento $D \in U^*$, dove $U$ è l'alfabeto di caratteri che costituiscono i documenti, viene rappresentato quindi come l'insieme dei $k-$shingles $C=S(D)$ *presenti al suo interno*.
Equivalentemente, può essere visto come un lungo vettore binario $S(D) = C \in \{0,1\}^{U^k}$ avente una componente per ogni elemento dell'insieme $U^k$ di *tutti* i possibili $k$-shingle.
Ogni singolo shingle definisce una dimensione (ossia una componente) del vettore $C$,
(si ha quindi una dimensione per ciascun elemento (shingle) nell'universo $U^k$).
Se l'$i-$esimo $k-$shingle dell'insieme ordinato $U^k$ appartiene al documento $D$, allora l'$i-$esima entrata di $C$ ha valore $1$, altrimenti ha valore $0$. Si fa presente come questi vettori risultino essere molto sparsi.

Data quindi la rappresentazione di documenti mediante questi insiemi, si fa riferimento al concetto di *J. similarity* per individuarne le similarità testuali.
E' possibile, dato un data set di documenti, dare un'interpretazione probabilistica della *J. sim.* Siano $D_1$ e $D_2$ due documenti, si ha che
$$
\texttt{J.sim}(D_1,D_2) = \frac{|C_1 \cap C_2|}{|C_1 \cup C_2|} = \mathbf{Pr} \left[ \frac{C_1 \cap C_2}{C_1 \cup C_2} \right]
$$
Dove $C_i = S(D_i)$ per $i=1,2$.

I documenti aventi parecchi $k-$shingles in comune hanno testo simile, anche se questi appaiono all'interno di essi in ordine differente.

Bisogna prestare particolare attenzione alla scelta di $k$. Nonostante questa possa essere una qualsiasi costante, con la scelta di un $k$ troppo piccolo ci si aspetta di trovare la maggior parte delle sequenze di $k$ caratteri in molteplici documenti. Ciò implica che, per la metrica di valutazione della similarità utilizzata, si possono avere documenti per i quali i relativi insiemi di shingles possiedono alta *Jaccard similarity* nonostante questi non abbiano nessuna frase in comune.
Ad esempio, per $k=1$, la maggior parte di pagine presenti sul Web risulterebbero simili tra loro, avendo queste la maggior parte dei caratteri che le costituiscono in comune.
La scelta di $k$ dovrebbe dipendere a seconda della tipica lunghezza dei documenti e di quanto grande è il tipico insieme di caratteri che le costituisce. In particolar modo,
- $k$ deve essere scelto sufficientemente grande affinché la probabilità che un qualsiasi shingle appaia in un qualsiasi documento è bassa.
In generale, si ha che:
- $k=5$ risulta sufficiente per documenti brevi, come emails.
- $k=10$ va bene per documenti di grande dimensione.
### Min-Hashing
Gli insiemi di shingles sono di grande dimensione. Se si hanno milioni di documenti, potrebbe non risultare possibile memorizzare tutti gli shingle-sets nella memoria principale.
Si vogliono convertire quindi insiemi di grandi dimensioni in rappresentazioni molto più piccole, che prendono il nome di *firme* (signatures), preservandone la similarità.
Risulta fondamentale perciò che, dal confronto delle firme di due insiemi, si possa stimare la Jaccard similarity di tali insiemi facendo riferimento solo ad esse. Non è possibile che le firme diano l'esatta similarità degli insiemi che rappresentano, ma le stime che forniscono sono vicine ad essa, e maggiore è la dimensione delle firme, più accurata è la stima.

Per fare ciò, si usa la tecnica del **Min-Hashing**. 
Si supponga di dover trovare documenti simili in un data set di dimensione $N=10^6$, e quindi di disporre di $10^6$ insiemi di $k-$shingles. Il Min-Hashing non permette di evitare i $\Theta(N^2)$ confronti tra le possibile coppie, ma rende ciascun confronto (dove ciascun confronto consiste nel calcolo della J. similarity), molto più efficiente. 
Si fa presente come, siano $C_1$ e $C_2$ le rappresentazioni vettoriali ad alta dimensione di due documenti $D_1$ e $D_2$ per l'insieme di tutti i possibili $k-$shingles definiti su un'alfabeto $U$.
Allora $C_1,C_2 \in \{0,1\}^k$ , e il confronto banale tra essi comporta un costo di $\Theta(|U|^k)$, che per un alfabeto di dimensione $|U|=27$ e $k=10$ risulta eccessivo.

Prima di spiegare come sia possibile costruire piccole firme da grandi dimensioni, si fa presente che risulta utile visualizzare una collezione di insiemi mediante la loro *matrice caratteristica*. Le colonne della matrice corrispondono agli insiemi, e le righe corrispondono agli elementi dell'insieme universo dal quale vengono presi gli elementi costituenti gli insiemi stessi.
Per essere più chiari, nel nostro scenario, sia $U$ l'alfabeto dei caratteri costituenti i documenti da prendere in analisi e sia $k$ il valore per il quale vengono definiti i $k-$shingles. Allora, la matrice caratteristica possederà una colonna per ogni insieme di $k-$shingle associato al relativo documento $S(D_1)=S_1,S(D_2)=S_2,\ldots,S(D_N)=S_N$, dove si fa presente che $S_i \subseteq U^k$ per ogni $i \in \{1,2,\ldots,N\}$, ed una riga per ogni elemento dell'insieme universo $U^k$. Nell'entrata $(r,C)$ di tale matrice è presente $1$ se l'elemento dell'universo nella riga $r$ appartiene all'insieme associato alla colonna $C$, $0$ altrimenti.  Ad esempio:

![[charMatrix.png]]

```ad-Osservazione
Data la rappresentazione di un documento mediante un vettore ad alta dimensione, tale matrice caratteristica è formata dalle rappresentazioni vettoriali di tutti i documenti presi in analisi.
```

Mediante questa rappresentazione, l'intersezione tra due insiemi è ottenuta eseguendo l'AND bitwise tra i bit appartenenti alle rispettive colonne, e l'unione mediante l'operazione OR bitwise. La dimensione di un insieme corrisponde al numero di entrate della rispettiva colonna che posseggono valore $1$. La similarità tra due colonne è data dalla J. similarity dei corrispettivi insiemi, calcolabile eseguendo le operazioni appena descritte.

Prendendo di nuovo come esempio la matrice in figura, si vuole calcolare la similarità di $S_1$ ed $S_3$.  Sia $C(S_i)=C_i$ la sequenza di bit contenuta nella colonna associata ad $S_i$. Si ha che
$C_1 = 00110$, $C_3=11010$. Allora 
$$(C_1 \textnormal{ AND } C_3) = 00010 = C_{AND}\ \ \textnormal{ e } \ \ (C_1 \textnormal{ OR } C_3) = 11110 = C_{OR}$$

$$
\texttt{J.sim}(C_1,C_3) = \frac{|C_{AND}|}{|C_{OR}|}= \frac{1}{5}
$$
dove $|C_i| = \# \textnormal{ di entrate in } C_i \textnormal{ aventi valore } 1$.

E' importante tenere a mente che la matrice caratteristica è una maniera utile per visualizzare i dati, ma non risulta un buon metodo per memorizzarli. Uno dei motivi principali è che queste matrici sono quasi sempre sparse (hanno molti più entrate a $0$ che $1$). Una rappresentazione di tale matrice per risparmiare spazio ottenibile memorizzando le posizioni nelle quali appaiono le entrate poste ad $1$.

Si è quindi data una rappresentazione dei documenti mediante sottoinsiemi di shingles, e tali sottoinsiemi sono stati rappresentati mediante colonne binarie di una matrice.
Il prossimo obiettivo è quello di trovare colonne simili calcolando delle signatures per le colonne di dimensione ridotta.
In particolar modo, si vuole definire un buon algoritmo di firme tale per cui la J. similarity delle colonne risulta non essere troppo distante dalla "similarità" delle firme.

Si segue il seguente approccio:
1. Si definiscono le firme delle colonne.
2. Si esaminano coppie di firme per trovare colonne simili. E' essenziale che le similarità delle firme e delle relative colonne siano correlate
3. Opzionalmente, controllare che le colonne con firme simili siano realmente simili
Si fa presente che con questo approccio:
- Controllare tutte le coppie può richiedere troppo tempo. Si gestirà tale problematica con la tecnica del locality sensitive hashing
- Nel caso in cui non venisse eseguito il passo $3$, questo metodo può produrre falsi positivi (colonne non simili risultano essere simili mediante il confronto delle relative firme). L'approccio adottato può produrre falsi negativi (colonne simili risultano non essere simili mediante il confronto delle relative firme), i quali non possono venir individuati, essendo che i confronti vengono effettuati solamente per documenti aventi firme simili. Se si volessero effettuare confronti anche per le coppie di documenti aventi firme non simili, si effettuerebbero allora i confronti tra tutte le possibili coppie, e tale operazione richiederebbe tempo $\Theta(N^2)$.

Sia ora $|U^k|=m$ la dimensione dell'universo di $k-$shingles definiti su alfabeto $U$.
L'idea chiave è quindi quella di effettuare l'hash mediante una funzione $h$ di ciascuna colonna $C$ per ottenere una firma $h(C)$ tale per cui:
1. $h(C)$ è sufficientemente piccola affinché questa sia memorizzabile nella RAM, in particolar modo $|h(C)| << |C| = m$
2. Siano $C_1$ e $C_2$ due colonne. Allora $\texttt{J.sim}(C_1,C_2)$ è vicina all'equivalenza delle firme $h(C_1)$ ed $h(C_2)$.
Si deve trovare quindi una funzione $h(\cdot)$ tale per cui:
- Se $\texttt{J.sim}(C_1,C_2)$ è alta, allora, con alta probabilità, $h(C_1) = h(C_2)$.
- Se $\texttt{J.sim}(C_1,C_2)$ è bassa, allora, con alta probabilità, $h(C_1) \neq h(C_2)$.

La scelta della funzione hash dipende dalla metrica di similarità adottata.
Per la Jaccard Similarity, la funzione hash adatta prende il nome di MinHash.

Le firme che si vogliono costruire per gli insiemi sono composte dal risultato di un gran numero di computazioni, ciascuna delle quali è un "minhash" della matrice caratteristica.
Per eseguire il minhash di un insieme, rappresentato da una colonna della matrice caratteristica, bisogna scegliere una permutazione delle righe. Il valore di minhash di una colonna è il numero della prima riga, in ordine permutato, nella quale la colonna ha come entrata $1$.

La Minhash hash function può essere definita come segue
```ad-Definizione
title: Definizione (MinHash hash function)
Sia $M$ la matrice caratteristica del data set preso in analisi. Sia $\pi$ una permutazione scelta casualmente delle righe di $M$. Sia $m$ il numero di righe di $M$.
Per ciascuna colonna $C$ di $M$, si definisce la funzione hash MinHash $h_{\pi}(C)$ come 
$$
h_{\pi}(C) = \textnormal{min} \{i \in [m]: C_{\pi}[i] = 1\} = \textnormal{min}(\pi(C))
$$
Ossia l'indice della prima riga, secondo l'ordinamento indotto dalla permutazione $\pi$, in cui $C$ ha valore $1$, dove $C_{\pi}$ è la colonna $C$ ordinata secondo $\pi$.

```

Esiste un'importante connessione tra il minhashing e la J. Similarity degli insiemi sottoposti a tale procedura:
- La probabilità che la funzione MinHash per una permutazione casuale delle righe produca lo stesso valore per due insiemi è uguale alla Jaccard similarity di tali insiemi.

Ciò può essere formalizzato mediante il seguente teorema:
```ad-Teorema
title: Teorema (J.Similarity preserving)
Siano $C_1$ e $C_2$ due colonne. Sia $\pi$ una permutazione scelta uniformly at random. Allora
$$
\mathbf{Pr}_{\pi}\left[ h_{\pi}(C_1) = h_{\pi}(C_2)\right] = \texttt{J.sim}(C_1,C_2)
$$
```

Prima di dimostrare il teorema, si da un'argomentazione informale relativa ad esso.
Si considerino le colonne $C_1$ e $C_2$ per i rispettivi insiemi $S_1$ ed $S_2$. Le righe di queste due colonne possono essere divise in tre classi:
1. Righe di tipo $X$, aventi $1$ in entrambe le colonne.
2. Righe di tipo $Y$, aventi $1$ in una delle due colonne e $0$ nell'altra.
3. Righe di tipo $Z$, aventi $0$ in entrambe le colonne.
Essendo la matrice sparsa, la maggior parte delle righe sono di tipo $Z$. Nonostante ciò, è il rapporto del numero di righe di tipo $X$ e di tipo $Y$ che determina sia $\texttt{J.sim}(C_1,C_2)$, sia la probabilità che $h_{\pi}(C_1)=h_{\pi}(C_2)$. Sia $x$ il numero di righe di tipo $X$ ed $y$ il numero di righe di tipo $Y$. Allora si ha che 
$$
\texttt{J.sim}(C_1,C_2) = \frac{x}{x+y}
$$
Questo è vero perché $x$ è la dimensione di $S_1 \cap S_2$, ossia il numero di righe nelle due colonne tali per cui i valori contenuti in esse sono uguali, ed $x + y$ è la dimensione di $S_1 \cup S_2$.
Le righe di tipo $Z$ non vanno prese in considerazione per questo tipo di conteggio, perché i rispettivi shingles associati ad esse non sono contenuti in nessuno dei due insiemi presi in considerazione.
Si consideri ora la probabilità che $h_{\pi}(C_1)=h_{\pi}(C_2)$. Se si considerano le righe permutate casualmente, partendo dal primo elemento permutato e seguendo l'ordinamento indotto dalla permutazione, la probabilità di incontrare una riga di tipo $X$ prima di incontrare una riga di tipo $Y$ è proprio $x/(x+y)$, perchè su un numero totale di $x+y$ righe prese in questione, quelle di tipo $X$ sono proprio $x$. Ma se la prima riga incontrata, escluse quelle di tipo $Z$, è una riga di tipo $X$, allora sicuramente $h_{\pi}(C_1)=h_{\pi}(C_2)$. Se invece la prima riga incontrata, escluse quello di tipo $Z$, è di tipo $Y$, allora la colonna con il valore $1$ assume tale riga come indice di minhash. Ma la colonna con lo $0$ in quella riga sicuramente avrà, successivamente ad essa, una riga contenente $1$ nella lista permutata. Quindi $h_{\pi}(C_1) \neq h_{\pi}(C_2)$ se si incontra una riga di tipo $Y$ prima di una riga di tipo $X$. Si conclude quindi che la probabilità che $h_{\pi}(C_1)=h_{\pi}(C_2)$ è $x/(x+y)$, che risulta essere anche la Jaccard similarity delle due colonne (e quindi dei due rispettivi insiemi).

Per dimostrare formalmente tale teorema, si fa riferimento alla seguente variabile aleatoria
```ad-Definizione
title: Definzione (MinHash Estimator)
Siano $S_1$ e $S_2$ due insiemi di shingles e siano $C_1$ e $C_2$ le rispettive rappresentazioni mediante vettore binario ad alta dimensione. Sia $\pi$ una permutazione. Si definisce MinHash estimator la seguente variabile aleatoria
$$


\begin{equation*}
J_{\pi}(S_1,S_2) = 
  \left\{ 
    \begin{aligned}
      &\ 1 \ \ \ \textnormal{se }\ h_{\pi}(C_1)=h_{\pi}(C_2) \\
      &\ 0 \ \ \  \textnormal{altrimenti }\   
    \end{aligned}
  \right.
\end{equation*}
$$
```

E si dimostra il seguente lemma, la quale implica la dimostrazione del teorema.
```ad-Lemma
Se $\pi$ è una permutazione uniforme, allora $\mathbf{E}\left[J_{\pi}(S_1,S_2)\right]=\texttt{J.sim}(S_1,S_2)$
```
```ad-Dimostrazione
Siano $S_1$ ed $S_2$ due insiemi di shingles, e siano $C_1$ e $C_2$ le rispettive rappresentazioni mediante vettore binario ad alta dimensione.
Sia $|S_1 \cup S_2|=n$. Per un $i\in S_1\cup S_2$ si consideri l'evento
$$
s(i)=(\forall j \in (S_1 \cup S_2)\backslash \{i\})(\pi(i)<\pi(j))
$$
che afferma che $i$ è l'elemento di $S_1 \cup S_2$ mappato nel più piccolo indice $\pi(i)$ tra tutti gli elementi di $S_1 \cup S_2$. Essendo $\pi$ una permutazione, esattamente un elemento di $S_1 \cup S_2$ verrà mappato nel più piccolo indice (ossia, $s(i)$ è vero per esattamente un $i \in S_1 \cup S_2$), allora $\{s(i)\}_{i \in S_1 \cup S_2}$ è una partizione di cardinalità $n = |S_1 \cup S_2|$ dello spazio degli eventi. Inoltre, essendo che $\pi$ è completamente uniforme implica che 
$$
\mathbf{Pr}\left[s(i)\right] = \mathbf{Pr}\left[s(j)\right]
$$
per ogni $i,j \in S_1 \cup S_2$, ossia, ogni elemento di $S_1 \cup S_2$ ha la stessa probabilità di venir mappato nel più piccolo indice. Questo implica che $\mathbf{Pr}\left[s(i)\right] = 1 /n$ per ogni $i \in S_1 \cup S_2$.
Si osserva che, se $s(i)$ fosse vero ed $i \in S_1 \cap S_2$, allora $J_{\pi}(S_1,S_2)=1$, perché $i$ appartiene ad entrambi gli insiemi e $\pi$ raggiunge il suo minimo $\texttt{min}$ per $i$, quindi $h_{\pi}(C_1)=h_{\pi}(C_2)=\texttt{min}$.
In alternativa, se $s(i)$ fosse vero e $i \in (S_1 \cup S_2)\backslash (S_1 \cap S_2)$ allora $J_{\pi}(S_1,S_2)=0$, perché $i$ apparterrebbe solamente ad uno tra $S_1$ e $S_2$, e non ad entrambi, e $\pi$ raggiunge il suo minimo $\texttt{min}$ per $i$, quindi si ha avrebbe che $h_{\pi}(C_1) \neq h_{\pi}(C_2) = \texttt{min}$ o, alternativamente, $\texttt{min}=h_{\pi}(C_1) \neq h_{\pi}(C_2)$.

Usando questa osservazione ed applicando la legge delle aspettative iterate alla partizione $\{s(i)\}_{i \in S_1 \cup S_2}$ dello spazio degli eventi, si ottiene
$$
\begin{align*}
\mathbf{E}\left[J_{\pi}(S_1,S_2)\right] &= \sum_{i\in S_1 \cup S_2} \mathbf{Pr}\left[s(i)\right] \cdot \mathbf{E}\left[J_{\pi}(S_1,S_2) | s(i)\right] \\
\\
&= \sum_{i \in S_1 \cup S_2} \frac{1}{n} \cdot \mathbf{E}\left[J_{\pi}(S_1,S_2) | s(i)\right] \\
\\
&= \sum_{i \in S_1 \cap S_2} \frac{1}{n} \cdot \mathbf{E}\left[J_{\pi}(S_1,S_2) | s(i)\right] + \sum_{i \in (S_1 \cup S_2)\backslash (S_1 \cap S_2)} \frac{1}{n} \cdot \mathbf{E}\left[J_{\pi}(S_1,S_2) | s(i)\right] \\
\\
&= \sum_{i \in S_1 \cap S_2} \frac{1}{n} \cdot 1 + \sum_{i \in (S_1 \cup S_2)\backslash (S_1 \cap S_2)} \frac{1}{n} \cdot 0 \\
\\
&= \frac{1}{n} \cdot \sum_{i \in S_1 \cap S_2} 1 \\
\\
&= \frac{1}{n} |S_1 \cap S_2| \\
\\
&= \frac{|S_1 \cap S_2|}{|S_1 \cup S_2 |} = \texttt{J.sim}(S_1,S_2)
\end{align*}
$$
```

Il teorema dimostra quindi che $\texttt{J.sim}(S_1,S_2)= \mathbf{E}\left[J_{\pi}(S_1,S_2)\right]$. Essendo $J_{\pi}(S_1,S_2)$ una variabile aleatoria binomiale, si ha che 
$$
\begin{align*}
\mathbf{E}\left[J_{\pi}(S_1,S_2)\right] \ \ &= \ \ \mathbf{Pr}[h_\pi(C_1) =
h_\pi(C_2)] \cdot 1 +  \mathbf{Pr}[h_\pi(C_1) \neq h_\pi(C_2)] \cdot 0 \\
\\
&= \ \  \mathbf{Pr}[h_\pi(C_1) = h_\pi(C_2)] 
\end{align*}
$$
e quindi
$$
\mathbf{Pr}[h_\pi(C_1) = h_\pi(C_2)] = \texttt{J.sim}(S_1,S_2) = \texttt{J.sim}(C_1,C_2)
$$

Si pensi ancora ad una collezione di insiemi rappresentata dalla loro matrice caratteristica $M.$
Nella pratica, si combinano parecchie (ad esempio $100$) funzioni hash scelte uniformly at random (ossia, permutazioni scelte casualmente), e quindi mutualmente indipendenti tra loro, per creare una firma per una colonna C della matrice. Questo approccio permette di ottenere maggior confidenza.
Si supponga quindi di scegliere un numero $t>>1$ di permutazioni delle righe di $M$. Siano le funzioni di minhash determinate da queste permutazioni $h_{\pi 1},h_{\pi 2},\ldots, h_{\pi t}$. Dalla colonna $C$, rappresentante l'insieme $S$, si definisce la *minhash signature* di $S$ come il vettore $$SIG(C)=[h_{\pi 1}(C),h_{\pi 2}(C),\ldots, h_{\pi t}(C)]$$
Si rappresenta questo vettore come una colonna. Quindi, è possibile formare, a partire dalla matrice $M$, una *matrice di firme*, nella quale la $i-$esima colonna di $M$ è sostituita dalla minhash signature per l'insieme dell'$i-$esima colonna.
La matrice di firme ha lo stesso numero di colonne di $M$ ma solamente $t$ righe. Anche se $M$ non è rappresentata esplicitamente, ma in una forma compressa adatta ad una matrice sparsa (ad esempio, tenendo traccia della posizione delle entrate poste ad $1$), è normale che la matrice delle firme risulti essere molto più piccola di $M$.
L'aspetto fondamentale delle matrici di firme è dato dal fatto che risulta possibile utilizzare le loro colonne per stimare la Jaccard similarity degli insiemi ad esse corrispondenti. Dal teorema enunciato in precedenza, si ha che la probabilità che due colonne abbiano lo stesso valore in una data riga della matrice delle firme è uguale alla J. similarity degli insiemi corrispondenti a queste colonne. Inoltre, essendo le permutazioni sulle quali questi valori di minhashing si basano state scelte indipendentemente, si può pensare a ciascuna di queste righe della matrice di firme come un esperimento indipendente. Quindi, il valore atteso del numero di righe nelle quali due colonne assumono lo stesso valore è uguale alla Jaccard similarity dei corrispettivi insiemi. In particolar modo, si definisce il seguente concetto di similarità tra firme:
```ad-Definizione
title: Definizione (Signatures similarity)
Siano $C_1$ e $C_2$ due colonne, si definisce la similarità di due vettori di firme $SIG(C_1)$ e $SIG(C_2)$ come
$$
SignSim(C_1,C_2) = \frac{|\{j: h_{\pi j}(C_1) = h_{\pi j}(C_2)\}|}{t}
$$
Ossia la frazione delle min-hash signatures nelle quali $C_1$ e $C_2$ hanno lo stesso elemento
```

Si fa presente che $\texttt{J.sim}(C_1,C_2) \neq SignSim(C_1,C_2)$, ma per la proprietà del Min Hash descritto nel teorema, si ha che $\texttt{J.sim}(C_1,C_2) \equiv \mathbf{E}_{\pi}\left[SignSim(C_1,C_2)\right]$, quindi, maggiore è $t$, ossia più minhashings si usano, maggiori saranno le righe nella matrice di firme, e minore sarà l'errore atteso nella stima della Jaccard similarity.
Alla luce di quanto appena detto, si da il seguente corollario

```ad-Corollario
La $SignSim(C_1,C_2)$ di due colonne tende alla $\texttt{J.sim}(C_1,C_2)$ al crescere di $t$.
```

Si da ora uno pseudocodice che descrive la procedura di MinHashing:
![[MinhashAlg1.png]]

```ad-Osservazione
Essendo che le $t$ funzioni hash sono mutualmente indipendenti, si hanno risultati di concentrazione su $|SignSim(C_1,C_2) - \texttt{J.sim}(C_1,C_2)|$.
```

Si analizza ora la complessità spaziale del MinHash.
Data la matrice di firme, $SIG(C)$ è un vettore colonna di tale matrice associato alla colonna $C$ della matrice caratteristica.
Sia $SIG(i,C)$ l'$i-$esima entrata di $SIG(C)$, ossia l'indice della prima riga che ha valore $1$ nella colonna $C$ secondo la $i-$esima permutazione casuale. Per rappresentare quindi univocamente un singolo intero che assume valori tra $1$ e $|C|$, tale $SIG(i,C)$, sono necessari $\Theta(\log |C|)= \Theta(\log m)$ bits (dove $m=|C|=|U^k|$ essendovi in $C$ un'entrata per ogni possibile $k-$shingle). 
Essendo che, per una colonna $C$, la matrice di firme contiene $t$ elementi, ossia le $t$ funzioni della forma $h_{\pi i}(C)$ per $i=1,\ldots,t$, si ha che il costo in memoria dello *sketch*, ossia la signature, $SIG(C)$ è ordine di $\Theta(t \log m)$. Si è quindi raggiunto l'obiettivo di "comprimere" lungi vettori di bit avente complessità spaziale $\Theta(m)$ in firme di dimensione $\Theta( t \log m)$.

Si apre una digressione sul termine *sketch*.
Sia $x$ una variabile che rappresenta dei dati, come ad esempio un insieme, una stringa o un intero. Un *data sketch* è l'output di una funzione casuale $f$ (in generale è data dalla combinazione di un certo numero di funzioni hash) che mappa $x$ in una sequenza di bit $f(x)$ che rispetta le proprietà $1$-$3$ elencate di seguito, ed eventualmente, a seconda dell'applicazione, anche la $4$.
1. La dimensione in bit di $f(x)$ è molto più piccola della dimensione in bit di $x$ (di solito sublineare o polilogaritmica).
2. $f(x)$ può essere usata per calcolare, in maniera efficiente, delle proprietà di $x$. Ad esempio, se $x$ è un multi set, allora $f(x)$ può essere usata per calcolare un'approssimazione del numero di elementi distinti contenuti in $x$, o l'elemento che appare con più occorrenze in $x$.
3. $f(x)$ può essere aggiornato efficientemente se $x$ viene aggiornato. E' fondamentale che risulti possibile aggiornare $f(x)$ senza conoscere $x$. Ad esempio:
	- Se si aggiunge un elemento $y$ ad un insieme $x$, deve essere possibile calcolare $f(x \cup \{y\})$ conoscendo solamente $f(x)$ ed $y$ (e non $x$).
	- In generale, dati due sketches $f(x_1)$ ed $f(x_2)$, deve risultar possibile calcolare lo sketch della composizione di $x_1$ ed $x_2$ mediante un qualche operatore. Ad esempio, se $x_1$ e $x_2$ sono insiemi, si può essere interessati ad ottenere lo sketch di $f(x_1 \cup x_2)$ senza conoscere $x_1$ e $x_2$.
4. Se $x$ ed $y$ sono simili rispetto ad una misura di similarità (Jaccard distance, distanza euclidea ...), allora $f(x)$ e $f(y)$ devono essere simili con alta probabilità.

```ad-Osservazione
Essendo $f$ una funzione casuale, $f(x)$ è una variabile aleatoria.
```

Dalla definizione di data sketch, si osserva esplicitamente che l'output delle operazioni eseguite dalla procedura di min hashing rientra in questa categoria.

Nella pratica, le permutazioni sono ottenute mediante utilizzo di funzioni hash.
Questo perché, per la descrizione della procedura di minhashing, bisogna generare diverse permutazioni $\pi$ casuali ed indipendenti tra loro dell'insieme $U^k$. Ma eseguire anche una singola permutazione delle $U^k$ righe della matrice caratteristica risulta troppo costoso in termini di tempo. Ordinare mediante una funzione hash $h$ restituisce una permutazione $\pi$ (quasi) casuale.  
Di conseguenza, è possibile simulare l'effetto di una permutazione casuale utilizzando una funzione hash casuale che mappa ogni indice di riga in tanti buckets quanti sono le righe.
Si fa presente che una funzione hash $f:[m] \rightarrow [m]$ può mappare interi distinti nello stesso bucket, e lasciarne altri vuoti. E' possibile non tener conto di ciò finché $m$ è di grande dimensione e il numero di collisioni non è eccessivo. 
La scelta della funzione casuale, per rendere bassa la probabilità di collisioni, può essere fatta facendo riferimento all'Universal hashing, ossia si scelgono $t$ funzioni della forma
$$
h_{a,b}(x) = ((a \cdot x +b) \mod p) \mod m_1
$$
dove $a,b$ sono interi casuali e $p$ è un numero primo tale che $p > m_1$.
(**NOTA**: verificare se $m_1$ deve essere proprio $m$, sulle è slides è scritto $m$ ma verificare se non è un errore di notazione)

Si fornisce quindi il seguente pseudocodice
![[MinhashAlg2.png]]

Si fa ora un'analisi dell'algoritmo
Il ciclo principale (righe $3-8$) impiega $\Theta(mN)$ iterazioni.
Ciascuna di queste iterazioni richiede $t$ calcoli di funzioni hash $f_i(j)$ se e solo se $M(j,C)=1$, ma essendo $M$ sparsa, ciò è improbabile che sia vero.
Il problema principale di questo approccio è dato dal fatto che l'hash può causare collisioni, non portando una permutazione, ma come detto in precedenza si può non tenerne conto.

```ad-Osservazione
title: Notazione
$M$: matrice identità
$SIG$: matrice delle firme costruita da $M$ mediante minhashing
$SIG(C)$: Colonna della matrice delle firme associata al documento $C$.
$SIG(i,C)$: $i-$esima entrata della colonna colonna nella matrice delle firme associata al documento $C$.
```
### Locality-sensitive Hashing
Il Minhash permette quindi di comprimere documenti di grandi dimensioni in firme di piccole dimensioni, e di preservare la similarità attesa di ogni coppia di documenti. Nonostante ciò, nel caso in cui il data set preso in analisi abbia un grande numero di elementi al suo interno, può risultare impossibile trovare le coppie di documenti con la maggior similarità in maniera efficiente.
Si supponga di voler trovare le coppie di documenti simili in un data set avente dimensione $N.$ L'approccio banale consiste nel calcolare le J. similarities per ogni coppia di documenti e, nonostante il minhashing permetta di calcolare tale misura di similarità in maniera molto più efficiente, non evita i $\Theta(N^2)$ confronti tra coppie, che per data sets di grandi dimensioni risulta essere un numero di operazioni eccessivamente grande.

In particolar modo, se l'obiettivo è quello di calcolare le similarità di *ogni coppia* di documenti, non si può fare a meno di effettuare tale numero di confronti. 
Nella maggior parte dei casi, invece, si vogliono trovare le coppie che risultano essere simili entro una certa soglia di similarità $s$. In tal caso, è possibile concentrarsi solamente su coppie di documenti i quali risultano essere probabilmente simili, senza dover effettuare il confronto per ogni possibile coppia. Per fare ciò si fa riferimento alla tecnica del *locality-sensitive hashing*, che permette di individuare coppie di firme che, probabilmente, derivano da documenti tra loro simili.

L'idea generale del LSH è quella di utilizzare una funzione $f(x,y)$ che afferma se $x$ ed $y$, dove $x$ ed $y$ sono due documenti, è una coppia di candidati, ossia una coppia di documenti la quale similarità deve venir valutata in maniera più approfondita. 

Si studia una forma specifica di LSH adatta al problema studiato. Si fa presente che nella sua trattazione, si assume che i documenti siano stati già rappresentati mediante firme.

Un approccio generale al LSH è quello di eseguire più volte l'hash degli oggetti in analisi (le firme di ciascun documento nel nostro caso), in maniera tale che quelli simili vengano mappati nello stesso bucket con maggior probabilità rispetto a quelli tra loro dissimili. Si considerano poi tutte le coppie di oggetti mappate nello stesso bucket da almeno una procedura di hashing come *coppie candidate*, le quali saranno le uniche ad essere soggette al calcolo della similarità degli insiemi che costituiscono ciascuna di esse. 
In particolar modo, sia $0<s<1$ una soglia di similarità. Si definisce formalmente il concetto di coppia candidata.
```ad-Definizione
title: Definizione 
Siano $x$ ed $y$ due documenti appartenenti al data set preso in analisi, e siano $SIG(x)$ e $SIG(y)$ le colonne nella matrice di firme associate rispettivamente ai due documenti, ottenute mediante la procedura di minhashing.
I documenti $x$ ed $y$ sono coppie di candidati se le colonne $SIG(x)$ e $SIG(y)$ assumono lo stesso valore per almeno una frazione $s$ delle loro righe, ossia
$$
\frac{|\{i \in [t]: SIG(i,x) = SIG(i,y)\}|}{t} \geq s
$$
dove $SIG(i,C)$ è l'$i-$esima entrata della colonna $C$ nella matrice di firme.
```


Si spera che la maggior parte di coppie di documenti tra loro dissimili non vengano mai mappate, da nessuna delle funzioni hash usate dalla procedura, nello stesso bucket, e che quindi non vengano controllate.  Le coppie dissimili che vengono mappate nello stesso bucket prendono il nome di *falsi positivi*, e si vuole che queste siano una piccola frazione tra tutte le possibili coppie. 
Si vuole inoltre che la maggior parte di coppie veramente simili vengano mappate nello stesso bucket per almeno una delle funzioni hash utilizzate. Le coppie per cui ciò non accade vengono chiamate *falsi negativi*, e anche per questi si vuole che siano una piccola frazione delle coppie veramente simili.

Il problema di questo approccio è dato dal fatto che, eseguire l'hash dell'intera firma di un documento, ossia dell'intera colonna della matrice delle firme associata al documento in questione, in un bucket può generare diversi falsi positivi e falsi negativi.

Per ovviare a questo problema, si divide la matrice di firme in $b$ bands  (un sottoinsieme di righe adiacenti) composte ciascuna da $r$ righe.
Per ogni band si definisce una funzione hash, che prende il vettore di $r$ interi di una colonna della matrice delle firme (ossia la porzione di una colonna all'interno di tale band), e lo mappa in una hash table avente $c$ buckets, dove $c$ è abbastanza grande rispetto ad $r$ per evitare, con alta probabilità, la collisione di firme differenti. Si può usare la stessa funzione hash per tutte le bands, ma si usa un array di bucket separato per ciascuna di esse, in maniera tale che le colonne con vettori uguali in bands differenti non vengano mappate nello stesso bucket.

Se i sottovettori di due colonne contenuti in una data bands finiscono nello stesso bucket, allora i due documenti associati a tali colonne sono probabilmente J. simili, e rientrano quindi nella categoria di coppie candidate.
Le coppie di colonne candidate sono quelle che vengono mappate nello stesso bucket per almeno una band.

Si fa ora un'assunzione che semplifica l'analisi dell'algoritmo:
- Ci sono abbastaza buckets ($c>>r$) in maniera tale che sia improbabile che le colonne vengano mappate nello stesso bucket a meno che queste siano identiche in una particolare band.
Si assume quindi che due vettori vengano mappati nello stesso bucket se e solo se questi sono identici.

Si supponga quindi di dividere la matrice delle firme in $b$ bands, ciascuna avente $r$ righe, e si supponga che una particolare coppia di documenti abbia Jaccard similarity $s$. Si ricorda che la probabilità che le firme ottenute mediante minhashing per questi documenti assumono lo stesso valore in una data riga della matrice delle firme è proprio $s$.
Si può calcolare la probabilità che questi due documenti, o meglio, le loro firme, diventino una coppia di candidati come segue:
1. La probabilità che le firme assumano lo stesso valore in tutte le righe di una particolare band è $s^r$.
2. La probabilità che le firme siano diverse in almeno una riga di una particolare band è $1-s^r$.
3. La probabilità che le firme siano diverse in almeno una riga di ogni band è $(1-s^r)^b$.
4. La probabilità che le firme assumano lo stesso valore in tutte le righe di almeno una band, e quindi diventino una coppia candidata, è $1-(1-s^r)^b$. 

Indipendentemente dalle costanti $b$ e $r$ scelte, questa funzione assume la forma di una *curva ad S*.![[SCurve.png]]
La soglia, ossia il valore della similarità $s$ per il quale la probabilità che una coppia diventi un candidato è $1/2$, è in funzione di $b$ ed $r$. La soglia è circa dove la funzione è "più ripida", e per $b$ ed $r$ di grandi dimensioni si ha che per le coppie aventi similarità sopra alla soglia è molto probabile che questi diventano candidati, mentre per quelli sotto alla soglia è improbabile che questi lo diventino. Un'approssimazione buona per la soglia è $(1/b)^{1/r}$.

Si ha quindi che, per una soglia fissata $s$, il quale è un parametro in input del problema, bisogna impostare i seguenti parametri dell'algoritmo:
- Il parametro $t$ del numero di Minhashes da eseguire (che saranno quindi il numero di righe della matrice delle firme)
- I parametri $b$ ed $r$, in maniera tale che $br=t$, e in maniera tale che si ottengano quasi tutte coppie con firme simili, ma che vengano anche eliminate le coppie per cui le firme non lo sono. La scelta più bilanciata per $b$ ed $r$ si ha quando $(1/b)^{1/r} \approx s$.
Aumentare $b$ diminuisce il numero di falsi negativi, mentre aumentare $r$ diminuisce il numero di falsi positivi.

Si fa un riassunto generale della tecnica studiata:
- **Obiettivo:** Dati in input le $t$ colonne per ciascun documento (ossia la matrice di firme), e la soglia di similarità $s$ (con $0<s\leq 1$), si vogliono trovare tutte le coppie di documenti $(x,y)$ tali per cui $SignSim(x,y) \geq s$. 
- **Soluzione mediante LSH:** Applicare la tecnica della divisione in bande dell'LSH alla matrice di firme, impostando i parametri $b$ ed $r$ affinché $b \cdot r = t$ e $(1/b)^{1/r} \approx s$.
- **Output:** tutte le coppie di documenti candidate aventi la stessa firma in *tutte* le $r$ righe di *almeno* una band.
### Combinazione delle tecniche
Si può ora dare un approccio per trovare l'insieme di coppie candidate di documenti simili,  per poi individuare i documenti veramente simili tra esse. Si enfatizza che questo approccio può produrre falsi negativi, ossia coppie di documenti simili che non vengono identificate come tali per via del fatto che non rientrano nei possibili candidati, e falsi positivi, coppie di candidati che al termine della procedura vengono analizzati, ma per i quali si ha che non risultano sufficientemente simili.

1. Scegli un valore di $k$ e costruisci da ciascun documento l'insieme di $k-$shingles.
2. Scegli una lunghezza $t$ per le firme da ottenere mediante la procedura di minhash, e fornisci all'algoritmo di minhashing gli insiemi per calcolare le firme di minhash per tutti i documenti.
3. Scegli una soglia $s$ che definisce quanto simili debbano essere i documenti affinché possano essere considerate "coppie simili". Scegli un numero di bands $b$ ed un numero $r$ di righe tali per cui $br=t$ e $s \approx (1/b)^{1/r}$. Se risulta più importante evitare i falsi negativi, si possono impostare i parametri $b$ ed $r$ per produrre una soglia minore di $t$. Se risulta di maggior importanza la velocità e si vogliono limitare i falsi positivi, scegli $b$ ed $r$ per produrre una soglia più grande.
4. Costruisci le coppie di candidati applicando la tecnica del locality sensitive hashing.
5. Esamina le firme di ogni coppia di candidati e determina se la frazione dei componenti per i quali i valori assunti sono uguali è almeno $t$.
6. Opzionalmente, se le firme sono sufficientemente simili, analizza i documenti in questione per verificare che questi siano veramente simili.
**NOTA**: 
- Inserire "Miglioramenti dell'implementazione del Min-Hashing"
- Argomentare meglio il ruolo dei parametri $b$ ed $r$ per LSH, inserendo i grafici sulle slides