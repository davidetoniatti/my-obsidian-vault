# Link analysis 
Si vogliono studiare problemi di Data Mining su reti sociali di enormi dimensioni. Un esempio di queste reti è il Web, il quale può essere modellato come un grafo diretto, dove le pagine corrispondo ai nodi, e vi è un arco diretto dalla pagina $p_1$ alla pagina $p_2$ se vi sono uno o più link all'interno di $p_1$ che indirizzano a $p_2$.
## Indicizzazione delle pagine
Informalmente, il **PageRank** è un algoritmo utilizzato dai motori di ricerca che permette di *valutare l'importanza di pagine Web* in maniera tale che tale parametro non venga manomesso facilmente.
Per entrare nei dettagli di tale funzionalità, ed individuare la motivazione del suo utilizzo, risulta necessario fornire un background su come operano i motori di ricerca.
In generale, il loro obiettivo è quello di, dato un insieme $S$ di parole, trovare le pagine Web più rilevanti per $S.$
Prima di Google, i motori di ricerca lavoravano navigando le pagine Web ed elencando i *termini* trovati in ogni pagina all'interno di un *indice invertito*, una struttura dati che dato un termine permette di individuare i puntatori a tutte le pagine web dove esso risulta essere presente.
Quando una *query di ricerca*, consistente in una lista di termini, veniva effettuata ad un motore di ricerca, le pagine contenenti quei termini venivano estratte dall'indice invertito e classificate in base all'utilizzo di tali termini al loro interno. Ad esempio:
- la presenza di un termine nell'header di una pagina la rendeva più rilevante rispetto ad un'altra contenente lo stesso termine all'interno di una sezione di testo ordinario;
- il numero di occorrenze di un dato termine influiva sulla classifica delle pagine estratte. Infatti, maggiore era la presenza di un termine all'interno di una pagina, maggiore risultava essere la sua rilevanza.
Tale operazione è gestita da due moduli Software che costituiscono un motore di ricerca:
- **Query Module.** Converte una query scritta in linguaggio naturale, in una query avente un linguaggio comprensibile dal motore di ricerca. Dunque il motore consulta l'indice invertito dal quale seleziona un insieme $P$ di pagine che contengono i termini $T$ della query. Successivamente, il Query Module passa $P$ al Ranking Module;
- **Ranking Module.** Ha il compito di assegnare un punteggio ad ogni pagina $p\in P$ e di restituire $P$ all'utente *ordinato* in base al punteggio. Inizialmente, il punteggio veniva determinato solamente in base al *Content Score,* ossia il punteggio del contenuto basato sui parametri descritti in precedenza. Si fa presente che tale componente è la più importante nel processo di ricerca di contenuti sul Web, perché evita all'utente di effettuare un'estensiva, e spesso impossibile, ricerca nelle migliaia di pagine rilevanti che restituisce il query module a seguito di una richiesta.

Questa modalità operativa dei motori di ricerca presentava delle **debolezze**. Infatti, risultava possibile *modificare impropriamente* le pagine Web in maniera tale da *ingannare* il sistema di classificazione, portando i motori a fornire risultati *non consoni alle richieste* degli utenti.
Ad esempio, si supponga di disporre di una pagina Web dedicata alla vendita di un prodotto. Per la modalità di funzionamento appena descritta, una maniera ingannevole per far si che tale pagina venga visualizzata da un maggior numero di utenti consiste nell'inserire al suo interno un elevato numero di occorrenze di termini non inerenti al servizio fornito, cosicché questa appaia tra i primi risultati in risposta alle query di ricerca effettuate da un utente contenente tali termini.  Queste tecniche per manipolare i risultati forniti dai motori di ricerca prendono il nome di *term sparm*.
Google, per ovviare a tale problema, ha fornito due innovazioni:
1. Il **PageRank**, un algoritmo utilizzato per **simulare** il comportamento di navigatori del Web all'interno della rete. In particolar modo, simulava dove questi, partendo da una pagina casuale, tendevano a congregarsi nel caso cui avessero selezionato casualmente dei link uscenti dalla pagina in cui questi si trovavano in un dato istante, reiterando più volte tale processo. Le pagine che riportavano un grande numero di visitatori venivano considerate più importanti rispetto a pagine raramente visitate. 
2. La valutazione del contenuto di una pagina non avveniva più semplicemente in base ai termini presenti al suo interno, ma anche mediante termini contenuti in altre pagine aventi un link diretto verso essa. In particolar modo, la valutazione prendeva in considerazione i termini vicini o contenuti all'interno di tali links. Questo perché, nonostante fosse facile per uno spammer aggiungere termini impropri in una pagina di sua proprietà, risultava difficile aggiungerli in pagine di proprietà altrui contenenti link diretti verso essa. Questo approccio di valutazione ha portato quindi alla definizione di un altro punteggio per il Ranking Module, ossia il *popularity score (o autorità)* di una pagina, il quale viene determinato da una link analysis della struttura del Web. Il Ranking Module quindi assegna un punteggio complessivo basato sul *Content Score* e sul *Popularity Score*. La rilevanza di una pagina è quindi calcolata tramite analisi dei link che la coinvolgono.

Queste due tecniche combinate fanno si che il motore di ricerca non possa venir ingannato facilmente da comportamenti scorretti da parte di un proprietario di una pagina Web. Intuitivamente, una semplice contromisura per contrastarle, può essere quella di creare diverse pagine Web, ciascuna delle quali contenenti link , aventi al loro interno i termini scelti per ingannare il sistema, diretti verso la pagina di interesse, ma tale approccio non risulterebbe funzionare essendo che tali pagine, a loro volta, non vengono referenziate da altre. 
Le motivazioni per cui la simulazione di navigatori Web casuali permetta di approssimare l'importanza di una pagina sono le seguenti:
- Gli utenti del Web tendono ad inserire link diretti verso pagine che reputano di interesse, piuttosto che pagine inutili.
- Il comportamento di un navigatore casuale indica quali pagine saranno probabilmente visitate dagli utenti. E' più probabile che questi visitino pagine utili o di interesse piuttosto che pagine non rilevanti.
## PageRank
Formalmente, il **PageRank** è una funzione che assegna un numero reale a ciascuna pagina nel Web nota. 
Maggiore è il PageRank di una pagina, maggiore sarà la sua **importanza**. Non esiste un algoritmo fissato per l'assegnazione del PageRank: diverse varianti dell'idea di base portano a valutazioni diverse per la stessa pagina.

L'idea alla base dell'algoritmo per il calcolo del **PageRank** è quella che, un link da una pagina $p_1$ diretto verso una pagina $p_2$ è una ***raccomandazione*** per la pagina $p_2$. In particolar modo, maggiore è il numero di raccomandazioni per una pagina, ossia maggiore è il numero di archi entranti nel nodo associato ad essa nella modellazione del Web mediante grafo, maggiore la pagina risulta essere importante. Un altro fattore che influenza il punteggio di popolarità per una pagina è dato dallo *stato* delle pagine che la puntano. In particolar modo:
- Maggiore è l'importanza di una pagina, maggiore sarà il suo peso sul fattore di popolarità delle pagine puntate da questa.
- Maggiore è il numero di pagine puntate da una data pagina, ossia maggiore è il numero di archi uscenti dal relativo nodo, minore sarà il peso di tali link sul fattore di popolarità delle pagine puntate.

Si danno due formulazioni equivalenti del PageRank:
- Modellazione algebrica
- Modellazione probabilistica
### Modellazione algebrica del PageRank
Il **PageRank** può essere considerato come un **metodo iterativo** basato sull'analisi dei link entranti in una pagina, ossia, i soli link entranti in essa concorrono a determinarne il rank.
L'idea è quella di considerare quindi i link come dei voti. Maggiore è il numero di link entranti in una pagina, maggiore è la sua importanza.
Informalmente, il **rank** di una pagina è un indice che esprime la sua rilevanza ai fini della ricerca.
Il **rank** di una pagina non dipende solamente dal numero di archi entranti in essa, ma anche dalla rilevanza che hanno tali pagine Web. Non tutti i link entranti in una pagina hanno quindi lo stesso peso sul come influenzano la sua popolarità, in particolar modo, i link provenienti da *pagine importanti* hanno un peso *maggiore*. 
Bisogna ricordare anche che, maggiore è il numero di link uscenti da una pagina, minore sarà la sua influenza sulle pagine puntate.
Si da quindi una formulazione ricorsiva del modello descritto.
- Ogni voto di un link ha un peso *proporzionale* al rank della sua pagina sorgente.
- Se la pagina $j$, avente importanza $r_j,$ ha $d_{j}$ links uscenti, ogni link ha un peso pari ad $r_j/d_{j}$. Tale peso corrisponde ai *voti* che andranno a influenzare la popolarità della pagina verso la quale l'arco è diretto.
- L'importanza $r_j$ della pagina $j$ è data dalla somma dei voti presenti nei suoi archi entranti.

Quindi, il voto proveniente da una pagina importante ha un peso maggiore, ed, intuitivamente, una pagina è importante se è puntata da altre pagine importanti. Si definisce quindi il rank $r_j$ di una pagina $j$ come 
$$
r_j = \sum_{i \rightarrow j} \frac{r_i}{d_i}
$$
dove $d_i$ è il grado uscente del nodo $i$, ossia il numero di archi uscenti da esso. Quindi l'importanza di una pagina è data dalla somma dei rapporti tra l'importanza delle pagine che la puntano e il loro grado uscente.
![](Pasted%20image%2020240211195103.png)
Si supponga ora di disporre di una rete costituita di pagine Web, per le quali si vuole calcolare l'importanza di ciascuna di esse. Allora, è possibile formulare tale scenario mediante un sistema lineare, dove le incognite sono date proprio dagli $r_j$ per ogni pagina $j.$ Tale modello prende il nome di *flow model.* Si fa un esempio
![center|400][flowModel.png]

Si ha un sistema lineare costituito dalle equazioni che descrivono il flusso entrante in ogni nodo 
$$
\begin{cases} 
r_y = \frac{r_y}{2} + \frac{r_a}{2} \\
\\
r_a = \frac{r_y}{2} + r_m \\
\\
r_m = \frac{r_a}{2}
\end{cases}
$$
Si hanno $3$ equazioni, $3$ incognite e nessuna costante. Non si ha quindi un'unica soluzione. 
Si pone quindi un ulteriore vincolo, che forza l'unicità della soluzione:
$$
r_y + r_a + r_m = 1
$$
Ossia, somma dell'autorità di tutte le pagine deve essere pari ad $1.$
E si ha quindi
$$
r_y = \frac{2}{5}, \ \ r_a = \frac{2}{5}, \ \ r_m=\frac{1}{5}
$$
Il metodo dell'eliminazione Gaussiana per la risoluzione di sistemi lineari funziona per piccoli esempi, ma risulta necessario un metodo migliore per grafi di grande dimensione. Questo perché il metodo dell'eliminazione Gaussiana impiega tempo cubico nel numero di equazione.
#### Metodo più figo
Si definisce una matrice di adiacenza $M$ di dimensione $n \times n$, dove $n$ è il numero di pagine Web, come segue. Si indicano con $j$ le righe e con $i$ le colonne di $M$.
Sia $i$ una pagina avente $d_i$ link uscenti. Allora
$$
M_{ji} = \begin{cases} 
\frac{1}{d_i} \textnormal{ se } i \rightarrow j \\
\\
0 \textnormal{ altrimenti}

\end{cases}
$$
Dove $i \rightarrow j$ indica che esiste un arco diretto da $i$ a $j.$ Tale matrice risulta essere stocastica rispetto alle colonne:
```ad-Definizione
title: Definizione (Matrice stocastica per colonne)
Una matrice $M \in \mathbb{R}^{n\times n}$ si definisce stocastica per colonne se:
- $0 \leq M_{ji} \leq 1$ per ogni $i,j$.
- Per ogni $i$ vale $\sum^{n}_{j=1}M_{ji}=1$, ossia, per ciascuna colonna di $M$, la somma degli elementi è pari ad $1$.
```
Si definisce un vettore $r,$ avente un'entrata per ogni pagina. Per tale vettore, si ha che
- $r_j = \sum_{i \rightarrow j} \frac{r_i}{d_i}$ è il punteggio di importanza della pagina $j.$
- $\sum_j r_j = 1$, ossia $r$ è un vettore di probabilità. 

Si ricorda ora la definizione di autovettore.
```ad-Definizione
title: Definizione (Autovettore)
Sia $M$ una matrice quadrata, $\mathbf{v}$ un vettore diverso dal vettore nullo, e $\lambda$ un numero tale per cui 
$$
M \mathbf{v} = \lambda \mathbf{v}
$$
Allora $\mathbf{v}$ è detto un **autovettore** di $M$ con **autovalore** $\lambda.$

Se $\mathbf{v}$ è un autovettore di $M$ con autovalore $\lambda$, allora lo sono anche tutti i multipli di $\mathbf{v}$ diversi da $0.$
```
Si danno ora i seguenti teoremi:
```ad-Teorema
title: Teorema (Teorema di Perron semplificato)
Se $A$ è una matrice reale $n \times n$ a elementi positivi, allora:
- $A$ ha un autovalore $c \in \mathbb{R}^+$ tale che, $c>|c'|$ per ogni altro autovalore $c'$ di $A.$
- L'autovettore $\mathbf{x}$ di $A$ corrispondente a $c$ è unico ed è a elementi reali e positivi, la cui somma è pari ad $1.$
```
Essendo $M$ una matrice stocastica, questa rispetta le ipotesi del teorema. Inoltre
```ad-Teorema
Se $A$ è una matrice stocastica, allora $A$ ha un autovalore $\lambda$ tale che $|\lambda| = 1$ e $\lambda$ è l'autovalore di modulo massimo di $A$
```
Tale autovalore prende il nome di **autovalore dominante**.
```ad-Definizione
title: Definizione (Autovalore dominante)
Siano $\lambda_1,\lambda_2,\ldots,\lambda_n$ gli autovalori di una matrice $A$ $n \times n$. $\lambda_1$ è detto l'**autovalore dominante** di $A$ se
$$
|\lambda_1| > |\lambda_i|, \ \forall i=2,\ldots,n.
$$
Gli autovettori associati a $\lambda_1$ sono chiamati autovettori dominanti di $A.$
```
Allora, il più grande autovalore di $M$ è proprio $\lambda = 1,$ e per il teorema di Perron, vi è un unico autovettore corrispondente a $\lambda,$ la cui somma degli elementi è pari ad $1.$ Si vuole mostrare che $r$ è proprio tale autovettore.
Si ha che
$$
M r = \begin{bmatrix} 
\ M_{11} & M_{12} & \cdots & M_{1n}\ \\
\ M_{21} & M_{22} & \cdots & M_{2n}\ \\
\vdots & \vdots & \ddots & \vdots \\
\ M_{n1} & M_{n2} & \cdots & M_{nn}
\end{bmatrix} 
\begin{bmatrix}
\ r_{1}\ \\
\ r_{2}\ \\
\ \vdots\ \\
\ r_{n}\ \\
\end{bmatrix}\ =\ 
\begin{bmatrix}
\ \sum_{i} M_{1i}r_i\ \\
\ \sum_{i} M_{2i}r_i\ \\
\ \vdots\ \\
\ \sum_{i} M_{ni}r_i\ \\
\end{bmatrix} \ 
$$
Sia 
$$
\sum_{i} M_{ji}r_i 
$$
il $j-$esimo elemento di $Mr$. Essendo che 
$$
M_{ji} = \begin{cases} 
\frac{1}{d_i} \textnormal{ se } i \rightarrow j \\
\\
0 \textnormal{ altrimenti}

\end{cases}
$$
allora
$$
\sum_{i} M_{ji}r_i = \sum_{i \rightarrow j} \frac{r_i}{d_i} = r_j
$$
Le equazioni di flusso possono essere quindi scritte come 
$$
r = Mr
$$
#### Schema iterativo: Power Iteration
Si definisce quindi uno schema iterativo detto **Power Iteration** per individuare $r$, ossia l'autovettore dominante.
Si considera un Web graph con $n$ nodi, dove i nodi sono le pagine e gli archi gli hyperlinks. Inoltre, si impone che tale grafo sia fortemente connesso. L'algoritmo opera come segue:
___
- Inizializza $\mathbf{r}^{(0)} = \left[1/n,\ldots,1/n \right].$
- Itera: $\mathbf{r}^{(t+1)} =M \cdot \mathbf{r^{(t)}}$ 
- Termina quando $\lVert \mathbf{r}^{(t+1)}- \mathbf{r}^{(t)} \rVert _1< \varepsilon$
___
Dove $\varepsilon$ è una costante maggiore di $0$ arbitraria molto piccola e
$$
\lVert x \rVert _1 = \sum_{1 \leq i \leq n} |x_i|
$$
è la norma $L_1$ di $x$.
Si osserva che
$$
r_j^{(t+1)} = \sum_{i \rightarrow j} \frac{r_i^{(t)}}{d_i}
$$
Bisogna ora mostrare il motivo per cui il metodo risulti essere funzionante, ossia, che la sequenza $Mr^{(0)}, M^2r^{(0)}, \dots, M^kr^{(0)}, \dots$ tende all'autovettore dominante di $M,$ e che quindi l'algoritmo fornito dia una buona approssimazione.
```ad-Teorema
Se $M$ è una matrice $n \times n$ diagonalizzabile con un autovalore dominante, allora esiste un vettore non negativo $r^{(0)}$ tale che la sequenza di vettori 
$$
Mr^{(0)}, M^2 r^{(0)}, \dots, M^k r^{(0)}, \dots
$$
tende all'autovettore dominante di $M$.
```
```ad-Dimostrazione
Essendo $M$ diagonalizzabile, si ha che questa ha $n$ autovettori $\mathbf{x}_1,\mathbf{x}_2,\ldots,\mathbf{x}_n$ linearmente indipendenti, con corrispondenti autovalori $\lambda_1,\lambda_2,\ldots,\lambda_n.$
Si assume che questi autovalori siano ordinati in maniera tale che $\lambda_1$ sia l'autovalore dominante (con corrispondente autovettore $\mathbf{x}_1$). Essendo che gli $n$ autovettori $\mathbf{x}_1,\mathbf{x}_2,\ldots,\mathbf{x}_n$ sono linearmente indipendenti, allora questi devono formare una base per $\mathbb{R}^n.$
Per l'approssimazione iniziale $r^{(0)}$, si sceglie un qualsiasi vettore diverso da zero tale per cui la combinazione lineare
$$
r^{(0)} = c_1\mathbf{x}_1 + c_2\mathbf{x}_2 + \cdots + c_n\mathbf{x}_n
$$
non abbia coefficiente $c_1$ uguale a zero. Questo perché, per $c_1 = 0,$ il power method potrebbe non convergere, e risulterebbe necessario utilizzare un $r_0$ differente. Ora, moltiplicare per $M$ entrambi i lati dell'equazione porta a 
$$
\begin{align}
Mr^{(0)} &= M(c_1\mathbf{x}_1 + c_2\mathbf{x}_2 + \cdots + c_n\mathbf{x}_n) \\
\\
&= c_1(M\mathbf{x}_1) + c_2(M\mathbf{x}_2) + \cdots + c_n(M\mathbf{x}_n) \\
\\
&= c_1(\lambda_1\mathbf{x}_1) + c_2(\lambda_2\mathbf{x}_2) + \cdots + c_n(\lambda_n\mathbf{x}_n)
\end{align}
$$
Moltiplicando per $M$ entrambi i lati $k$ volte si ottiene che
$$
M^kr^{(0)} = c_1(\lambda^k_1\mathbf{x}_1) + c_2(\lambda^k_2\mathbf{x}_2) + \cdots + c_n(\lambda^k_n\mathbf{x}_n)
$$
che implica
$$
M^kr^{(0)} = \lambda^k_1\left[(c_1\mathbf{x}_1) + c_2\left(\frac{\lambda^k_2}{\lambda^k_1}\right)\mathbf{x}_2 + \cdots + c_n\left(\frac{\lambda^k_n}{\lambda^k_1}\right)\mathbf{x}_n\right]
$$
Per ipotesi, essendo $\lambda_1$ il più grande autovalore in valore assoluto si ha che 
$$
\frac{\lambda_i}{\lambda_1} < 1 \ \ \forall i=2,\ldots,n
$$
in valore assoluto. Allora, per ogni $i$ che va da $2$ ad $n$ si ha che
$$
\left(\frac{\lambda_i}{\lambda_1}\right)^k
$$
tende a $0$ al tendere di $k$ ad infinito. Questo implica che l'approssimazione 
$$
M^k r^0 \approx \lambda_1^kc_1\mathbf{x}_1, \ \ c_1 \neq 0
$$
migliore all'aumentare di $k.$ 
```
#### $M$ è diagonalizzabile
Bisogna ora mostrare che $M$ sia **diagonalizzabile**.
Per fare ciò, si osserva che se una matrice stocastica è **irriducibile** ed ammette un unico autovalore dominante uguale ad $1$, allora la matrice è **diagonalizzabile**.
Una matrice si dice **irriducibile** se il suo grafo associato è **fortemente connesso**. Il grafo associato ad una matrice $A$ $n \times n$ è un grafo orientato con $n$ nodi, dove esiste un arco $i \rightarrow j$ se e solo se $A_{ij} \neq 0.$ 
Ma allora il grafo associato ad $M$ è proprio il grafo del Web con direzioni degli archi invertiti, infatti
$$
M_{ji} = \begin{cases} 
\frac{1}{d_i} \textnormal{ se } i \rightarrow j \\
\\
0 \textnormal{ altrimenti}

\end{cases}
$$
di conseguenza il grafo associato ha un arco $j \rightarrow i$ se e solo se $i \rightarrow j$.
Essendo il web Graph fortemente connesso, lo è anche il grafo associato alla matrice. Per vedere meglio questa cosa, si ricorda che un grafo fortemente connesso ha un ciclo che attraversa tutti i suoi nodi. Se si invertono le direzioni degli archi, allora si ha tale ciclo attraversabile inversamente. Di conseguenza, $M$ è diagonalizzabile.

Si ricorda inoltre che una matrice $n \times n$ è diagonalizzabile se e solo se possiede $n$ autovettori linearmente indipendenti.
### Modellazione probabilistica del PageRank
Si studia ora il PageRank facendo riferimento alla teoria delle **catene di Markov**.
Si supponga che un navigatore aleatorio si trovi, in un certo istante, su una pagina associata ad un nodo avente $k$ archi uscenti, corrispondenti a dei link verso altre pagine. Allora, all'istante successivo, tale navigatore si troverà su ciascuna di queste pagine raggiungibili attraversando i link con probabilità $1/k,$ ossia, segue un link uscente dal nodo in cui si trova scelto u.a.r.

Questo processo può quindi essere modellato mediante un particolare *processo stocastico*, che prende il nome di **catena di Markov**.
```ad-Definizione
title: Definizione (Processo stocastico)
Un **processo stocastico** $\mathbf{X}=\{X(t):t\in T\}$ è una collezione di variabili aleatorie. L'indice $t$ rappresenta un istante di tempo, e in tal caso il processo $\mathbf{X}$ modella il valore di una variabile aleatoria $X$ che cambia nel tempo.
Si definisce $X(t)=X_t$ come lo stato del processo all'istante $t$. 
Se $T$ è un insieme infinito numerabile, si dice che $\mathbf{X}$ è un processo discreto.
```
```ad-Definizione
title: Definizione (Catena di Markov)
Un processo stocastico $X_0,X_1,X_2,\dots$ è una **catena di Markov** se
$$
\mathbf{Pr}\left[X_t = a_t|X_{t-1}=a_{t-1},X_{t-2}=a_{t-2},\dots, X_0 = a_0\right] = \mathbf{Pr}\left[X_t=a_t | X_{t-1} = a_{t-1}\right]
$$
```
Questa definizione esprime come lo stato di $X_t$ dipenda dallo stato precedente $X_{t-1}$, ma come sia indipendente dal modo in cui il processo abbia raggiunto $X_{t-1}.$ Questa proprietà prende il nome di *mancanza di memoria.* Bisogna far presente che questa proprietà non implichi che $X_t$ sia indipendente dalle variabili aleatorie $X_0,X_1,\ldots,X_{t-2}$, ma implica solo che ogni dipendenza di $X_t$ nel passato è catturata dal valore $X_{t-1}.$
Senza perdita di generalità, si assuma che lo spazi di stati discreti della catena di Markov sia $\{0,1,2,\ldots,n\}$ (o $\{0,1,2,\ldots \}$ se è infinito numerabile). La probabilità di transizione
$$
P_{ij} = \mathbf{Pr}\left[X_t = j | X_{t-1} = i\right]
$$
è la probabilità che il processo si sposti da $i$ a $j$ in un passo. Per via della proprietà di mancanza di memoria, la catena di Markov è definibile da una matrice di transizione dove l'entrata nella riga $i$ e colonna $j$ è la probabilità $P_{ij}.$ Da ciò segue che, per ogni $i$, $\sum_{j\geq 0}P_{ij} = 1$. 
Si definisce una particolare catena di Markov.
```ad-Definizione
title: Definizione (Processo di vita-e-morte)
Un **processo (o catena) di vita-e-morte** è una Catena di Markov avente insieme di stati $\mathcal{X} = \{0,1,...,n\}$. In un passo, la catena può trovarsi allo stato successivo, allo stato precedente o rimanere nello stato in cui si trova. Le probabilità di transizione possono essere specificate dalle triple $\{(p_k,r_k,q_k)\}_{k=0}^n$ dove, per $k=0,...,n$ e $p_k+r_k+q_k=1$ si ha che
- $p_k$ è la probabilità di passare dallo stato $k$ allo stato $k+1$ quando $0 \leq k <n$.
- $q_k$ è la probabilità di passare dallo stato $k$ allo stato $k-1$ quando  $0<k\leq n$.
- $r_k$ è la probabilità di rimanere a $k$ per $0\leq k \leq n$.
- $q_0=p_n=0$
```
In generale, è possibile definire la *matrice di transizione del Web* per descrivere cosa accade ad un navigatore aleatorio dopo un passo. Questa matrice $M$ ha $n$ righe ed $n$ colonne, se vi sono $n$ pagine. L'elemento $m_{ij}$ nella riga $i$ e colonna $j$ ha valore $1/d_{j}$ se la pagina $j$ ha $d_{j}$ archi uscenti, ed uno di questi è diretto verso la pagina $i.$ Altrimenti, $m_{ij} = 0$. Dunque $m_{ij}$ è la probabilità che un navigatore al nodo $j$ si sposti verso il nodo $i$ al passo successivo

Il comportamento di un navigatore aleatorio, in particolar modo, può essere descritto mediante una passeggiata aleatoria.
```ad-Definizione
title: Definizione (Passeggiata aleatoria)
Sia $G=(V,E)$ un grafo non diretto connesso.
Una passeggiata aleatoria su $G$ è una catena di Markov definita dalla sequenza di mosse di una particella che si muove tra i vertici di $G$. In questo processo, la posizione della particella in un dato istante di tempo definisce lo stato del sistema. Se la particella si trova sul vertice $j$ ed $j$ ha $d_j$ archi incidenti, allora la probabilità che la particella attraversi l'arco $(j,i)$ e si sposti su un vicino $i$ è $1/d_j$
```
$X_t$ è la pagina visitata da un navigatore aleatorio all'istante $t,$ e rappresenta quindi lo stato del processo in tale istante. Ad ogni istante $t,$ l'utente può trovarsi in una tra le $n$ pagine, corrispondenti agli $n$ stati della catena di Markov.
Si assume che quando un utente si trovi sulla pagina $j$ al tempo $t,$ allora la probabilità di trovarsi su una pagina $i$ all'istante $t+1$ dipende solo dal fatto che l'utente si trovi sulla pagina $j$, e non dalle pagine visitate precedentemente.
La distribuzione di probabilità per la posizione di un navigatore aleatorio può essere descritta da un vettore colonna $\mathbf{p}(t)=\mathbf{p}_t$ il cui $j-$esimo componente è la probabilità che esso si trovi alla pagina $j$ all'istante $t.$ $\mathbf{p}_t$ è una quindi distribuzione di probabilità sulle pagine, essendo $\sum_{i}(\mathbf{p}_t)_i = 1.$ 
Si supponga quindi di avere $n$ pagine nel Web, e che un navigatore aleatorio si trovi inizialmente su una di esse con probabilità $1/n.$ Allora, il vettore iniziale $\mathbf{p}_0$ avrà $1/n$ per ogni componente. Se $M$ è la matrice di transizione del Web, allora dopo uno step, la distribuzione del navigatore sarà $\mathbf{p}_1=M\mathbf{p}_0,$ dopo due step sarà $\mathbf{p}_2=M\mathbf{p}_1= M(M\mathbf{p}_0)=M^2\mathbf{p}_0$ e, proseguendo in questa maniera, si ha che $\mathbf{p}_t = M\mathbf{p}_{t-1} = M^t\mathbf{p}_0$ è la distribuzione di probabilità per la posizione del navigatore dopo $t$ passi.

Si osserva ora perché il prodotto tra un vettore di distribuzione $\mathbf{p}$ ed una matrice di transizione $M$ è uguale alla distribuzione $\mathbf{x}=M\mathbf{p}$ del passo successivo. La probabilità $\mathbf{x}_i$ che un navigatore aleatorio si trovi al nodo $i$ al passo successivo è
$$
 \sum_{j} \mathbf{Pr}[X_{t+1} = i | X_{t} = j]\cdot\mathbf{Pr}[X_{t} = j]=\sum_{j} m_{ij}\mathbf{p}_j
$$
In questa sommatoria, $m_{ij}$ è la probabilità che un navigatore al nodo $j$ si sposti verso il nodo $i$ al passo successivo, e $\mathbf{p}_j$ è la probabilità che esso si sia trovato sul nodo $j$ al passo precedente.
Si supponga ora che la passeggiata aleatoria raggiunga uno stato
$$
\mathbf{p}_{t+1} = M \mathbf{p}_{t} = \mathbf{p}_{t}
$$
allora $p_{t}$ è la **distribuzione stazionaria** del cammino aleatorio.
```ad-Definizione
title: Definizione (Distribuzione Stazionaria)
Sia $\mathbf{P}$ una matrice $n \times n$ di transizione che descrive una catena di Markov. Una distribuzione stazionaria $\pi$ è un vettore tale per cui:
- $\sum_{i} \pi_i = 1$, $\pi_i \in [0,1]$
- $\mathbf{P}\pi = \pi$
Dove $\pi$ è un vettore colonna. 
```
Questo significa che partendo da una situazione iniziale aleatoria con distribuzione di probabilità $\pi$, dopo un passo, e quindi dopo un numero arbitrario di passi, essendo
$$
\mathbf{P}^n \pi =  \mathbf{P}\mathbf{P}^{n-1} \pi=  \mathbf{P}^{n-1} \pi= \ldots = \pi
$$
la distribuzione di probabilità è sempre $\pi.$

Una distribuzione stazionaria esiste sempre per una catena di Markov, ma **non** è garantito che sia unica. Se esiste **una sola** distribuzione di probabilità stazionaria allora si ha che
$$
\pi = \lim_{k}\mathbf{P}^k x
$$
dove $x$ è una distribuzione generica sugli $n$ stati, ossia è un vettore $n-$dimensionale le quali entrate sono minori o uguali ad $1,$ e la loro somma è proprio pari ad $1$.
Si osserva che tale condizione è proprio quella che rende valido il Power Method.
Lo stato $\mathbf{p}_{t+1}=\mathbf{p}_{t}$ si raggiunge se si verificano due condizioni:
1. Il grafo è fortemente connesso, ossia esiste un cammino per ogni coppia di nodi del grafo (spider traps).
2. Non ci sono nodi pozzo, ossia nodi privi di archi uscenti (dead ends).

La distribuzione stazionaria viene raggiunta quando il prodotto della distribuzione $\mathbf{p}$ con la matrice $M$ non cambia la distribuzione, ossia, $\mathbf{p}$ è un **autovettore** di $M.$
Anche in questo caso, essendo $M$ *stocastica*, $\mathbf{p}$ è l'autovettore dominante, ossia il suo autovalore associato è il più grande tra tutti gli autovalori. Inoltre, sempre per via del fatto che tale matrice è stocastica, l'autovalore associato all'autovettore principale è uguale ad $1$. Il vettore $r$ soddisfa $r= Mr,$ quindi $r$ è una distribuzione stazionaria per la passeggiata aleatoria.
#### Esistenza e unicità distribuzione stazionaria
Si è detto che se esiste una sola distribuzione di probabilità stazionaria, si ha che
$$
\pi = \lim_{k \rightarrow \infty}\mathbf{P}^k x
$$
e che quindi l'applicazione del Power Method porti al vettore desiderato. Si vedono quindi delle condizioni sufficienti per l'esistenza e l'unicità della distribuzione stazionaria per il processo del random surfer su un grafo.
Si ha che per grafi che soddisfano determinate condizioni, la distribuzione stazionaria è unica, e questa verrà raggiunta mediante il metodo iterativo indipendentemente dalla distribuzione di probabilità iniziale all'istante $t=0.$
Una tipologia di grafi che soddisfano queste condizioni sono le **catene di Markov ergodiche**. Informalmente, una catena di Markov è **ergodica** se esiste un cammino tra ogni coppia di stati (grafo fortemente connesso), e che gli stati non siano partizionati in insiemi tali per cui tutte le transizioni di stato avvengano **ciclicamente** da un insieme all'altro. Si descrive ora questo concetto formalmente. Per fare ciò, bisogna individuare delle proprietà di una catena di Markov ergodica.
```ad-Definizione
title: Definizione (Catena di Markov irriducibile)
Sia $S$ l'insieme degli stati di una catena di Markov.
Una catena di Markov si dice irriducibile se tutti gli stati sono raggiungibili dagli altri stati, ossia, per ogni $i,j \in S$, esiste un $t \in N$ tale per cui 
$$
p^{(t)}_{ij} > 0
$$
dove $p^{(t)}_{ij}$ indica la probabilità che in $t$ passi venga raggiunto lo stato $j$ a partire da $i.$
```

Si introduce ora il concetto di **periodo** di una catena di Markov.
Informalmente, il *periodo* di una catena di Markov è una caratteristica che riflette la regolarità con cui un sistema può ritornare a uno stato particolare nel corso del tempo. Il periodo è definito come il massimo comun divisore di tutte le lunghezze dei cicli indipendenti presenti nella catena di Markov.
Un *ciclo indipendente* in una catena di Markov si riferisce a un insieme di stati attraverso i quali è possibile raggiungere uno stato qualsiasi all'interno del ciclo e ritornare al punto di partenza. Questo ciclo può essere attraversato più volte e in modi diversi, ma è indipendente da altri cicli presenti nella catena di Markov.
Una catena di Markov può essere classificata come *aperiodica* quando il suo periodo è 1. In altre parole, se il massimo comun divisore delle lunghezze dei cicli indipendenti è 1, allora la catena di Markov è *aperiodica*. In una catena di Markov *aperiodica*, non esiste un ciclo fisso regolare di ritorno ad uno stato, e quindi il processo non segue un modello di comportamento ciclico.

Per capire meglio:
- **Periodo**: Se la catena di Markov ha un periodo maggiore di 1, significa che esiste una certa regolarità nella frequenza con cui il sistema ritorna a uno stato specifico. Ad esempio, se il periodo è 2, la catena di Markov può tornare a uno stato solo in passi di lunghezza pari.

- **Aperiodica**: Se il periodo è 1, la catena di Markov è aperiodica e non segue un ciclo regolare. Questo implica che il sistema può ritornare a uno stato in passi di lunghezza qualsiasi, senza seguire un modello ciclico prevedibile.
In sintesi, il periodo e l'aperiodicità in una catena di Markov descrivono la regolarità o l'assenza di essa nel ritorno a determinati stati nel corso del tempo.
```ad-Definizione
title: Definizione (Catena di Markov Aperiodica)
Il periodo di uno stato $j \in S$ è il più grande $\xi \in \mathbb{N}$ tale per cui 
$$
\{n\in \mathbb{N}\ |\ p^{(n)}_{jj}>0\} \subseteq \{i \cdot \xi\ |\ i \in \mathbb{N}\}
$$
Uno stato con periodo $\xi = 1$ è detto **aperiodico**, e la catena di Markov si dice **aperiodica** se lo sono tutti i suoi stati.
```

Si da ora la definizione di catena di Markov ergodica
```ad-Definizione
title: Definizione (Catena di Markov ergodica)
Se una catena di Markov è **irriducibile** e **aperiodica**, allora è detta **ergodica**.
```
Si da quindi il seguente teorema
```ad-Teorema
Se una catena di Markov è ergodica, allora si ha che 

$$
\lim_{t \rightarrow \infty} \mathbf{P}^t x= \pi
$$
dove $\pi$ è l'unica distribuzione stazionaria della catena e $x$ è una distribuzione generica sugli $n$ stati (ossia è un vettore $n-$dimensionale le quali entrate sono minori o uguali ad $1,$ e la loro somma è proprio pari ad $1.$)
```
```ad-Osservazione
- Il teorema è vero **indipendentemente** dalla distribuzione iniziale.
- La distribuzione stazionaria di una catena di Markov ergodica può essere quindi approssimata efficientemente, moltiplicando iterativamente un vettore con la matrice della catena (Power Method).
```
### Si ok ma tanto il grafo del web non è fortemente connesso
Le analisi per il calcolo del PageRank fatte sino ad ora si sono basate sull'ipotesi che il grafo del Web fosse fortemente connesso. Nella realtà dei fatti, il grafo del web presenta la struttura in figura.
![[WebStructure.png]]
Ed è descritta dalle seguenti componenti:
1. Una componente centrale, di grandi dimensioni, fortemente connessa. 
2. Componente entrante, consistente in pagine in grado di raggiungere la componente fortemente connessa seguendo i links, ma non raggiungibili dalla componente fortemente connessa.
3. Componente uscente, consistente in pagine raggiungibili dalla componente fortemente connessa, ma non in grado di raggiungere la componente fortemente connessa.
4. I *tendrils* (in Italiano, viticci), distinguibili in due categorie. La prima comprende viticci consistenti da pagine raggiungibili dalla componente entrante, ma non in grado di raggiungere tale componente. La seconda comprende viticci in grado di raggiungere la componente uscente, ma non raggiungibili dalla componente uscente.
5. Tubi, i quali consistono in pagine raggiungibili dalla componente entrante e in grado di raggiungere la componente uscente, ma non in grado di raggiungere o di essere raggiunti dalla componente fortemente connessa.
6. Componenti isolati, irraggiungibili dalle porzioni di grande dimensione del grafo.

Queste strutture violano le assunzioni necessarie affinché la power iteration converga ad un limite. In particolar modo, si ha che tale grafo *non risulta essere ergodico*. Ad esempio, quando un navigatore aleatorio entra nella componente uscente, questo non può mai abbandonarla. Quindi, i navigatori che iniziano l'esplorazione della rete nella componente fortemente connessa o nella componente entrante tenderanno ad accumularsi o nella componente uscente, o in un viticcio della componente entrante. Quindi, nessuna pagina nella componente fortemente connessa o nella componente entrante avrà alcuna probabilità di essere raggiunta a partire da tali stati. Se si interpreta questa probabilità come misura dell'importanza di una pagina, si concluderebbe (in maniera incorretta) che nessuna delle pagine nella componente fortemente connessa e nella componente entrate siano di rilevanza.
Bisogna quindi modificare il PageRank affinché si prevengano queste anomalie. I problemi principali sono due:
- I **vicoli ciechi (dead ends)**, ossia pagine non aventi link uscenti. I navigatori che raggiungono tali pagine spariscono, e il risultato è che nel vettore di distribuzione stazionaria nessuna pagina che raggiunge un vicolo cieco può avere un PageRank. In particolar modo, la passeggiata aleatoria non può proseguire e si ha una "fuori uscita" di importanza.
- **Trappole di ragno (spider traps)**, gruppi di pagine aventi tutti archi uscenti che permettono di navigare tale porzione del grafo, ma che non escono mai da tale componente. Ciò comporta che le passeggiate aleatorie si "blocchino" all'interno di tali trappole, ossia, che i navigatori aleatori possono solamente viaggiare sugli stati della catena associati a tali pagine, le quasi assorbiranno poi tutta l'importanza.
Entrambi questi problemi possono essere risolti da un metodo chiamato *teleports*, o teletrasporto, per il quale si assume che un navigatore aleatorio abbia una probabilità finita di abbandonare il grafo del Web ad ogni passo del processo e, a risposta di ciò, che un nuovo agente inizi una passeggiata aleatoria partendo da un nodo casuale. 
#### Evitare vicoli ciechi e trappole di ragno 
Per evitare i problemi appena definiti, si può modificare il calcolo del PageRank dando ad ogni navigatore aleatorio una piccola probabilità di teletrasportarsi in una pagina casuale, piuttosto che seguire un link dalla loro pagina uscente. Il passo iterativo, dove si calcola un nuovo vettore $\mathbf{r'}$ che stima del PageRank calcolato a partire dal PageRank corrente $\mathbf{r}$ e la matrice di transizione $M$ è 
$$
\mathbf{r'} = \beta M \mathbf{r} + \left[\frac{1- \beta}{n}\right]_{n}
$$
dove $\beta$ è una costante scelta, tipicamente contenuta tra $0.8$ e $0.9$ ed $n$ è il numero di nodi nel grafo del Web. Il termine $\beta M \mathbf{r}$ rappresenta il caso in cui, con probabilità $\beta,$ il navigatore aleatorio decide di seguire un arco uscente dalla pagina in cui si trova. Il termine $\left[\frac{1- \beta}{n}\right]_{n}$ è un vettore con tutte le componenti aventi valore $\frac{1-\beta}{n}$ e rappresenta la l'introduzione, con probabilità $1-\beta$, di un nuovo navigatore aleatorio in una pagina casuale. Si nota che se il grafo non ha vicoli ciechi, allora la probabilità di introdurre un nuovo navigatore aleatorio è esattamente uguale alla probabilità che il navigatore decida di non seguire un link uscente dalla pagina in cui si trova. In questo caso, si può visualizzare il navigatore come se, ad ogni passo, dovesse decidere di seguire un link uscente dalla pagina in cui si trova, oppure teletrasportarsi su una pagina casuale. Se invece vi sono vicoli ciechi, allora vi è una terza possibilità, per la quale il navigatore non transita dalla pagina in cui si trova. Essendo che il termine $\left[\frac{1- \beta}{n}\right]_{n}$ non dipende dalla somma delle componenti del vettore $\mathbf{r},$ vi sarà sempre una frazione di un viaggiatore che opera nella rete, ossia, quando vi sono vicoli ciechi, la somma delle componenti di $\mathbf{r}$ può essere minore di $1,$ ma non raggiungerà mai $0$.
Dunque la soluzione per uscire dai vicoli cechi consiste nel teletrasportate il random surfer con probabilità 1 su una pagina casuale.
In conclusione, ad ogni passo il random surfer con probabilità $\beta$ segue uno dei link uscenti dalla pagina in cui si trova, mentre con probabilità 1-$\beta$  salta ad una pagina random. Dunque l'equazione del PageRank diventa
$$
r_{j} = \sum_{i \rightarrow j} \beta\frac{ r_{i}}{d_{i}} + (1-\beta) \frac{1}{n}
$$
che è l'equazione del PageRank di Google definita da Brin e Page nel 1998. Questa formulazione assume che non ci siano vicoli ciechi in $M$. Se in $M$ sono presenti vicoli ciechi, allora si hanno due opzioni:
- Preprocessare la matrice $M$ in modo da eliminare tutti i vicoli ciechi;
- In modo esplicito, seguire un random teleport link con probabilità 1 dai vicoli ciechi.
Implementando il teleporting, la matrice $M$ diventa la matrice
$$
A = \beta M + (1-\beta)\left[ \frac{1}{n} \right]_{n\times n}
$$
dove $\left[ \frac{1}{n} \right]_{n\times n}$ è una matrice $n\times n$ con tutte le entrate pari a $\frac{1}{n}$.
Si ottiene dunque l'equazione
$$
\mathbf{r} = A\mathbf{r}
$$
dove $\mathbf{r}$ si può ottenere utilizzando il Power Method.
Per calcolare il PageRank $\mathbf{r}$, l'operazione cruciale è il **prodotto matrice vettore**: facile se si ha sufficiente memoria per memorizzare $A,\mathbf{r}_{n},\mathbf{r}_{o}$ dove $\mathbf{r}_n = A\mathbf{r}_o$.

Rearrangiando l'equazione $\mathbf{r}=A\mathbf{r}$, dove $A_{ji} = \beta M_{ji} + \frac{1-\beta}{n}$, si ottiene
$$
\begin{align}
r_{j} &= \sum_{i=1}^n A_{ji}r_{i} \\
r_{j} &= \sum_{i=1}^n \left[ \beta M_{ji} + \frac{1-\beta}{n} \right] \cdot r_{i} \\
&= \sum_{i=1}^n \beta M_{ji} r_{i} + \frac{1-\beta}{n} \underbrace{\sum_{i=1}^n r_{i}}_{\sum_{i=1}^n r_{i} = 1} \\
&= \sum_{i=1}^n \beta M_{ji} r_{i} + \frac{1-\beta}{n} \\
\end{align}
$$
dunque si ottiene il vettore
$$
\mathbf{r} = \beta M \mathbf{r}+ \left[ \frac{1-\beta}{n} \right]_{n}
$$
dove $\left[ \frac{1-\beta}{n} \right]_{n}$ è un vettore ad $n$ entrate di valore $\frac{1-\beta}{n}$.
Dunque ad ogni iterazione del power method, si deve:
- Calcolare $\mathbf{r}_n = \beta M \cdot \mathbf{r}_o$;
- Sommare il valore costante $\frac{1-\beta}{n}$ ad ogni entrata del vettore $\mathbf{r}_n$;
Si fanno ora considerazioni sullo spazio occupato e sui tempi di esecuzione dell'algoritmo.
La matrice $M$ è **sparsa**, dunque è sufficiente memorizzare in memoria solo le entrate della matrice diverse da zero. Dato che il numero di entrate diverse da zero è proporzionale al numero di link, possibile memorizzare $M$ con spazio proporzionale al numero di link. Generalmente i link sono miliardi, dunque si possono memorizzare solamente in **memoria secondaria**.
Dunque si analizza il costo di una iterazione del Power method.
**Assumendo** che il vettore $\mathbf{r}_n$ sia **mantenibile in memoria centrale**, e che $M$ ed $\mathbf{r}_o$ siano memorizzati su disco, si ottiene che ad ogni iterazione si deve
- leggere $\mathbf{r}_o$ e $M$ dal disco;
- scrivere $\mathbf{r}_n$ su disco;
dunque ogni iterazione del Power method costa
$$|\mathbf{r}_{n}|+|\mathbf{r}_{o}|+|M| = 2|\mathbf{r}| + |M|$$
## Topic page rank
Tra i diversi miglioramenti che si possono fare al PageRank, si studia il **topic-specific page rank**, con lo scopo di valutare le pagine web non solo in base alla loro popolarità, ma anche in base a quanto sono vicine a un particolare argomento, ad esempio "sport" o "storia". Il meccanismo per applicare questa valutazione consiste nel modificare il comportamento dei random surfer, favorendo lo spostamento su pagine che trattano un particolare argomento.
Questo meccanismo permette al motore di ricerca di rispondere alle query in base agli interessi dell'utente. Ad esempio, spesso interessi diversi vengono espressi utilizzando lo stesso termine in una query. Per esempio, la query "jaguar" potrebbe fare riferimento all'animale, all'automobile o a una versione del sistema operativo MAC. Se il motore di ricerca può dedurre che l'utente è interessato alle automobili, ad esempio, può fare un lavoro migliore nel restituire pagine rilevanti all'utente.
### Biased Random Walk
Per il problema dei punti ciechi e spider-trap, la soluzione è rendere il random walker in grado di teletrasportarsi su una qualsiasi pagina in modo equiprobabile, con una bassa probabilità $(1-\beta)$ ad ogni step della sua passeggiata.
Per implementare il *topic-sensitive page rank*, si utilizza la stessa tecnica del teletrasporto applicata però ad un *insieme specifico di pagine* rilevanti per un certo topic. Dunque il random walker con probabilità $(1-\beta)$ si teletrasporta su una pagina appartenente ad uno specifico sottoinsieme di pagine $S$ detto **teleport set**. Si osserva che $S$ contiene solamente delle pagine che sono rilevanti rispetto ad uno specifico topic, dunque il vettore risultate $r_S$ del PageRank con teleport set $S$ sarà differente da un qualsiasi altro vettore risultante $r_{S'}$ per un differente teleport set $S'$.
In particolare, si modifica la parte del teleporting nella formulazione del page rank per renderlo topic specific nel modo seguente.
$$
A_{ij} = \begin{cases}
\beta M_{ij} + \frac{1-\beta}{|S|} \quad &\text{se } i \in S \\
\beta M_{ij} + 0 \quad &\text{altrimenti}
\end{cases}
$$
In questa formulazione, un random walker si teletrasporta su una delle pagine in $S$ con probabilità uniforme: è possibile anche assegnare diversi pesi alle pagine in $S$, cioè rendere, nel momento del teletrasporto, più probabile il salto su specifiche pagine di $S$.

Per integrare il topic specific PageRank in un motore di ricerca, si deve:
1. Decidere $k$ argomenti per i quali creare vettori PageRank specifici per tali topic.
2. Scegliere un teleport set $S_k$ per ciascuno di questi argomenti e utilizzare tale insieme per calcolare il vettore topic specific PageRank $\mathbf{r}_k$ per ogni argomento.
3. Trovare un modo per determinare l'argomento o l'insieme di argomenti più rilevanti per una particolare query di ricerca.
4. Utilizzare i vettori PageRank per quell'argomento o quegli argomenti nell'ordinamento delle risposte alla query di ricerca.
Il terzo passo è probabilmente il più complicato, e sono stati proposti diversi metodi. Alcune possibilità sono:
1. Consentire all'utente di selezionare un argomento da un menu;
2. Inferire l'argomento/i dalle parole che appaiono nelle pagine web recentemente cercate dall'utente, o dalle query recenti emesse dall'utente;
3. Inferire l'argomento/i dalle informazioni sull'utente, ad esempio i loro segnalibri o i loro interessi dichiarati su Facebook.
## Combattere il Web Spam: TrustRank
### Le pagine Spam
Con **spamming**, si intende un qualsiasi tentativo intenzionale mirato a migliorare il posizionamento di una pagina web nei risultati dei motori di ricerca, senza essere in linea con il vero valore della pagina stessa. Tutte le pagine web che sono il risultato dello spamming sono dette pagine **spam**. Approssimativamente, il 15% delle pagine web sono spam.
### Tecniche di spamming
#### Term spam
Le tecniche di spam prima dell'introduzione del PageRank consistevano nel cosidetto **term spam**, cioè inserire nelle pagine termini che non sono in linea con il topic della pagina in modo da far apparire la pagina anche in query di ricerca che riguardano topic differenti da quelli trattati dalla pagina stessa. In questo modo, la pagina viene potenzialmente visitata da più utenti, anche quelli che non sono direttamente interessati. Questa tecnica era efficace rispetto ai primi motori di ricerca, che indicizzavano le pagine rispetto alle parole contenute all'interno di esse e rispondevano alle query di ricerca, cioè una lista di parole, con la lista delle pagine che contenevano tali parole.
Con l'introduzione del PageRank questa tecnica perde di efficacia. Infatti, il PageRank misura l'*importanza* di una pagina in base al numero e all'importanza delle pagine web che puntano a tale pagina. Intuitivamente, con il PageRank una pagina viene valutata in base a cosa dicono gli altri della pagina, e non in base al contenuto.
#### Link spam
Quando è diventato evidente che il PageRank rendeva inefficace il term spam, gli spammer hanno cominciato a utilizzare metodi progettati per ingannare l'algoritmo del PageRank facendo sovrastimare il ranking di determinate pagine. Le tecniche per aumentare artificialmente il PageRank di una pagina sono collettivamente chiamate *link spam* e consistono nel creare strutture di link che aumentano il PageRank di una particolare pagina.
#### Spam Farm
Un'insieme di pagine il cui scopo è aumentare il PageRank di una determinata pagina o pagine è detta **spam farm**. La figura sotto mostra una semplice struttura di una spam farm. 
![](Pasted%20image%2020240213154249.png)
Dal punto di vista dello spammer, il Web è diviso in tre parti:
1. **Pagine inaccessibili**: le pagine che lo spammer non può influenzare. La maggior parte del Web si trova in questa parte.
2. **Pagine accessibili**: quelle pagine che, sebbene non siano controllate dallo spammer, possono essere influenzate da quest'ultimo.
3. **Pagine proprie**: l'insieme delle pagine che lo spammer detiene e controlla.
La spam farm consiste:
- Nelle pagine detenute dallo spammer, organizzate come si vede a destra in figura;
- Un insieme di link dalle pagine accessibili alle pagine dello spammer. Senza questi link dall'esterno, la spam farm sarebbe inutile, poiché non verrebbe nemmeno indicizzata da un tipico motore di ricerca.
Si osserva che nonostante possa sembrare sorprendente che si possano influenzare delle pagine senza possederle, in realtà ad oggi molti siti permettono (e invitano) la pubblicazioni di commenti sul sito. Sfruttando questo fatto, per esempio, uno spammer può ottenere il maggior flusso di PageRank verso le proprie pagine pubblicando molti commenti del tipo "Sono d'accordo. Si prega di vedere il mio articolo su [www.mySpamFarm.com](http://www.mySpamFarm.com)."
##### Analisi di una Spam Farm
Nella spam farm è presente una pagina **target** $t$; lo spammer ha l'obiettivo di **massimizzare** il valore del PageRank di $t$. Inoltre, ci sono un gran numero $m$ di **pagine di supporto**, che accumulano una porzione del PageRank che viene distribuita equamente a tutte le pagine (la frazione $1 - \beta$ del PageRank). Le pagine di supporto impediscono anche che il PageRank di $t$ venga perso, per quanto possibile, poiché una parte verrà tassata ad ogni round. Si noti che $t$ ha un link verso ogni pagina di supporto, e ogni pagina di supporto ha un solo link verso $t$.

Supponiamo che il PageRank venga calcolato utilizzando un parametro di tassazione $\beta$, cioè la frazione del PageRank di una pagina che viene distribuita ai suoi successori al turno successivo, cioè le pagine a cui punta. Sia $n$ il numero totale di pagine sul Web. Alcune di esse costituiscono una *spam farm* della forma suggerita in figura, con una pagina target $t$ e $m$ pagine di supporto. Sia $x$ la quantità di PageRank contribuita dalle pagine accessibili a $t$. Quindi, $x$ è la somma, su tutte le pagine accessibili $p$ che hanno un link verso $t$, del PageRank di $p$ moltiplicato per $\beta$, diviso per il numero di pagine a cui punta $p$. Infine, sia $y$ il PageRank sconosciuto di $t$.
Il valore del PageRank di ogni pagina di supporto nella spam farm è pari a
$$
\frac{\beta y}{m}+\frac{1-\beta}{n}
$$
- il primo termine indica il ranking dato dalla pagina $t$: il PageRank $y$ di $t$ viene tassato, dunque solo una quantità $\beta y$ viene distribuita equamente alle pagine puntate da $t$. Ma $t$ punta alle $m$ pagine di supporto, quindi la quantità $\beta y$ viene ripartita equamente tra le $m$ pagine di supporto;
- il secondo termine indica invece la quantità di ranking data dalla frazione $1-\beta$ di PageRank totale che viene divisa equamente tra tutte le pagine Web.
Si calcola ora il PageRank $y$ della pagina target $t$. Questo valore arriva da tre sorgenti:
1. Il contributo $x$ che arriva dalle pagine esterne;
2. $\beta$ volte il PageRank di ogni pagina di supporto nella spam farm, ossia$$\beta m\left(\frac{\beta y}{m}+\frac{1-\beta}{n}\right)$$
3. $\frac{1-\beta}{n}$ ovvero la quota della frazione $(1-\beta)$ del PageRank che appartiene a $t$. Questa quantità è trascurabile e verrà eliminata per semplificare l'analisi.
Dunque da (1) e (2) (non si considera (3) essendo quantità trascurabile), si ottiene
$$
y=x+\beta m\left(\frac{\beta y}{m}+\frac{1-\beta}{n}\right)=x+\beta^2y+\beta(1-\beta) \frac{m}{n}
$$
Infine, risolvendo per $y$, si ottiene
$$
y=\frac{x}{1-\beta^2}+c \frac{m}{n}
$$
dove $c =\frac{\beta(1-\beta)}{(1-\beta^2)} = \frac{\beta}{(1+\beta)}$ .
Scegliendo $\beta = 0.85$, si ottiene $1/(1 − \beta^2 ) = 3.6$, e $c = \frac{\beta}{1-\beta^2} = 0.46$.
Ciò significa che la struttura ha amplificato la contribuzione esterna del PageRank del 360%, e ha ottenuto anche una quantità di PageRank che rappresenta il 46% della frazione del Web presente nella spam farm., ossia $\frac{m}{n}$.
### Combattere il Link Spam
È diventato essenziale per i motori di ricerca individuare ed eliminare il link spam, proprio come era necessario eliminare il term spam.
Ci sono due approcci per il link spam. Uno è cercare strutture come la spam farm in figura, dove una pagina linka a un numero molto grande di pagine, ognuna delle quali linka nuovamente ad essa. I motori di ricerca dunque cercano tali strutture ed eliminano quelle pagine dal loro indice. Questo fa sì che gli spammer sviluppino diverse strutture che hanno essenzialmente lo stesso effetto di catturare il PageRank per una pagina o pagine target. Non c'è essenzialmente fine alle possibili variazioni della struttura della farm spam in figura, quindi questa guerra tra gli spammer e i motori di ricerca probabilmente continuerà per molto tempo.
Tuttavia, esiste un altro approccio che si basa sulla modifica della definizione di PageRank per *abbassare* automaticamente il rank delle pagine spam. Considereremo due definizioni differenti:
- **TrustRank**: una variazione del topic-specific PageRank, dove il *teleport set* è costituito da pagine **trusted**; ad esempio, pagine con un dominio universitario o governativo. Cosi facendo, il teleport non è più uniforme su tutte le pagine, ma è biased su queste pagine fidate.
- **Spam Mass**: un calcolo che identifica le pagine che probabilmente sono spam e consente al motore di ricerca di eliminare tali pagine o di abbassarne fortemente il loro PageRank.
#### TrustRank
**TrustRank** è un topic-specific PageRank dove il topic è un insieme di pagine ritenute *affidabili* (non spam). L'idea è che mentre una pagina di spam potrebbe facilmente essere creata per collegarsi a una pagina affidabile, è *improbabile* che una pagina affidabile si collega a una pagina di spam.
Per definire l'insieme delle pagine affidabili, si può ad esempio creare automaticamente un sample di pagine web (cioè un insieme di pagine) e passare questo insieme ad un operatore umano in grado di identificare le pagine affidabili e scartare le pagine spam: è un lavoraccio, quindi il sample di partenza deve essere il più piccolo possibile.
Dato l'insieme delle pagine affidabili, si esegue un topic-specific PageRank impostando come teleport set questo insieme di pagine affidabili: questo permette di propagare la fiducia attraverso i link alle diverse pagine, dando ad ognuna di esse un *valore di fiducia* compreso tra 0 e 1.
Il modello (semplificato) è il seguente.
Ad ogni pagina affidabile si assegna un valore di fiducia pari ad 1.
Sia $t_p$ la fiducia della pagina $p$ e sia $o_p$ l'insieme delle pagine puntate da $p$. Allora per ogni $q \in o_p$, la pagina $p$ conferisce una *fiducia* a $q$ pari a
$$
\frac{\beta t_{p}}{|o_{p}|} \quad \text{per } 0<\beta<1
$$
Infine la fiducia di una pagina $p$ è **additiva**, dunque la fiducia di $p$ è data dalla somma della fiducia assegnata a $p$ da ogni pagine che puntano verso $p$.
Questo modello rispetta due caratteristiche importanti:
- **Trust attenuation**: la quantità di fiducia conferita da una pagina affidabile *decresce* al crescere della distanza della pagina nel grafo;
- **Trust splitting**: la fiducia è divisa tra gli out-links.

Si fanno due considerazioni importanti sull'insieme delle pagine candidate ad essere affidabili, cioè l'insieme di pagine che deve essere ispezionato da un umano:
- Questo insieme deve essere il più piccolo possibile;
- Si deve assicurare che ogni pagina affidabile riceva un rank di affidabilità adeguato, dunque si devono scegliere le pagine trusted iniziali in modo che tutte le pagine non spam siano raggiungibili con percorsi brevi.
Per scegliere buoni insiemi di possibili pagine affidabili, ci sono due approcci principali:
1. supponendo di voler scegliere $k$ pagine, si prendono le prime $k$ pagine date dal PageRank tradizionale: l'idea è che le pagine spam, per quanto possano salire nel ranking, non raggiungeranno posizioni molto alte nel ranking;
2. scegliere le pagine appartenenti a domini affidabili e controllati: ad esempio pagine con dominio .edu, .gov, .mil.
#### Spam Mass
L'idea dietro lo **spam mass** è quella di misurare per ogni pagina la frazione del suo PageRank che proviene dallo spam. Tale misura si ottiene calcolando sia il PageRank ordinario che il TrustRank basato su un insieme di pagine affidabili. Si supponga che la pagina $p$ abbia un PageRank $r$ e un TrustRank $t$. Allora la *spam mass* di $p$ è $\frac{r - t}{r}$. Una *spam mass* negativa o poco positiva indica che $p$ probabilmente non è una pagina di spam, mentre una *spam mass* vicina a 1 suggerisce che la pagina probabilmente è spam.
Dunque è possibile eliminare le pagine con una *spam mass* elevata dall'indice delle pagine Web utilizzato da un motore di ricerca, eliminando così una grande quantità di link spam senza dover identificare particolari strutture utilizzate dagli spammer.