# Hashing: un'implementazione randomizzata dei dizionari
### Il problema del dizionario
Dato un universo $U$ di possibili elementi, si vuole definire una struttura dati in grado di tener traccia di un suo sottoinsieme $S \subseteq U$ arbitrario di $n$ elementi, dove $n$ è una frazione di $|U|$, con l'obiettivo di inserire, eliminare e cercare elementi in $S$ in maniera efficiente.

Tale struttura dati prende il nome di ***dizionario***, che supporta le seguenti operazioni, alle quali gli elementi di $S$ sono quindi soggetti:
- $\texttt{MakeDictionary()}$: Inizializza un dizionario in grado di mantenere un sottoinsieme $S$ di $U$. Inizialmente, $S = \emptyset$. 
- $\texttt{Insert}(u)$: Inserisci un elemento $u \in U$ in $S$.
- $\texttt{Delete}(u)$: Se $u \in S$, rimuovi $u$ da $S$. 
- $\texttt{Lookup}(u)$: Determina se $u\in S$ o meno.

Si vuole studiare il caso in cui la dimensione di $U$ sia estremamente grande rispetto ad $n$, per il quale la definizione di un array di size $|U|$ non risulti essere ammissibile. Si vogliono trovare soluzioni proporzionali ad $|S|=n$. 

Per fare ciò, si farà riferimento all'***hashing***, una tecnica utilizzabile per mantenere un insieme di elementi che subisce modifiche dinamicamente.
 
Si definisce un array $H$ di dimensione $m\approx n$ per memorizzare tali informazioni, e si utilizza una funzione $h:U \longrightarrow \{0,1,\ldots,m-1\}$ che mappa ciascun elemento di $U$ in una delle posizioni dell'array. Tale funzione $h$ prende il nome di **funzione hash**, mentre l'array $H$ prende il nome di **tabella hash**. 
Per aggiungere un elemento $u$ all'insieme $S$, si inserisce $u$ nella posizione $h(u)$ di $H$.
Questo approccio funziona molto bene nel caso in cui, $\forall u,v \in S$ tali che $u\neq v$ vale $h(u) \neq h(v)$. Per tale scenario, l'operazione di ricerca di un elemento $u$ impiega tempo costante: basta semplicemente controllare l'array in posizione $H[h(u)]$, la quale o è vuota o contiene $u$.

In generale, possono esservi però elementi $u,v\in S$ con $u\neq v$ tali per cui $h(u)=h(v)$. Quando ciò accade, si dice che tali elementi *collidono*, essendo mappati nella stessa entrata in $H$.
Per gestire le collisioni, si assume che ogni posizione $H[i]$ della tabella hash memorizzi una lista concatenata di tutti gli elementi $u \in S$ tali per cui $h(u)=i$. 
Le operazioni descritte in precedenza per il dizionario su un elemento $u\in U$ prevedono quindi il calcolo di $h(u)$ precedentemente alla loro esecuzione. In particolar modo
- $\texttt{Insert}(u)$: Calcola $h(u)$ e Inserisci $u$ nella lista in $H[h(u)]$.
- $\texttt{Lookup}(u)$: Calcola $h(u)$ e scandisci la lista in posizione $H[h(u)]$ per vedere se $u$ è presente in essa.
- $\texttt{Delete}(u)$: Calcola $h(u)$ e scandisci la lista in posizione $H[h(u)]$ per vedere se u è presente in essa, ed eventualmente rimuovilo. 

Il tempo per eseguire le operazioni appena descritte è proporzionale al tempo necessario per il calcolo di $h(u)$ sommato alla lunghezza della lista concatenata in posizione $H[h(u)]$, dove quest'ultima quantità è pari al numero di elementi in $S$ che collidono con $u$.

```ad-Osservazione
L'efficienza del dizionario è basata sulla scelta della funzione hash $h$.
```

Si vuole definire quindi una funzione hash $h$ che distribuisca bene gli elementi, ossia che renda sufficientemente rara la presenza di collisioni, per la quale nessuna entrata della tabella hash $H$ possa contenere troppi elementi. Per fare ciò risulta necessario un approccio probabilistico alla costruzione di tale funzione. 
Ma prima, si mostra perché l'approccio deterministico non risulti essere buono nel caso in cui $|U| >> m$.  
```ad-Proprieta
Sia $|U|>m^2$. Allora, per ogni funzione hash deterministica $h$, esiste un insieme $S$ di dimensione $n$ tale per cui ogni suo elemento viene mappato nella stessa entrata di $H$.  
```
```ad-Dimostrazione
Sia $h:U\longrightarrow \{0,1,...,m-1\}$ una funzione hash fissata che mappa ciascun elemento di $U$ in $H.$ Si vuole scegliere un insieme $S$ di $n$ elementi tale per cui ogni elemento venga mappato nella stessa entrata dell'array.
Sia $S=\{u\in U: h(u)=i\}$. Allora $H[i]$ conterrà tutti gli elementi di $S$.
```

Quindi, nel caso peggiore, tutti gli elementi di $U$ inseriti in $H$ apparterrebbero a $S=\{u\in U: h(u)=i\}$, e l'esecuzione delle varie operazioni richiederebbe tempo $\Theta(n)$.
### Hashing deterministico
Si vede ora brevemente un esempio di hashing deterministico.
Sia $U$ l'insieme universo tale per cui $|U| = N >> m \approx n :=|S|$.
Si supponga di rappresentare gli elementi $u$ di $U$ come interi in $\{0,1,...,N-1\}$.
Sia ora $p$ un numero primo tale per cui $m \leq p \leq 2m$.
Si definisce ora $h$ tale per cui, $\forall u \in U$
$$h(u) = u \ (\textnormal{mod } p)$$ 
Allora la proprietà vista in precedenza rimane vera, ma gli elementi di $U$ risultano essere ben distribuiti.
Tale approccio risulta buono per applicazioni "statiche".

### Hashing probabilistico
Si utilizza la probabilità per la costruzione della funzione hash $h$.
In generale, si vuole che tale funzione possegga le seguenti caratteristiche
1. $h(u)$ deve esse il "più casuale" possibile. Idealmente, $h$ dovrebbe mappare gli elementi di $U$ in maniera completamente uniforme. Si vedrà però che tale proprietà presenterà un costo estremamente eccessivo.
2. $h(u)$ deve essere veloce da calcolare algoritmicamente. Idealmente, si vorrebbe calcolare $h(u)$ in tempo proporzionale al tempo necessario alla lettura di $u$ , ossia $\mathcal{O}(1)$ se $u$ è un intero, $\mathcal{O}(l)$ se $u$ è una stringa di lunghezza $l$.
3. $h$ occupa dello spazio in memoria, essendo essa implementata mediante una struttura dati. Questo spazio deve essere il più piccolo possibile, idealmente $\mathcal{O}(1)$.

Si formalizza il concetto di *famiglia di funzioni hash*.
Una famiglia di funzioni hash è un insieme $\mathcal{H} \subseteq \{0,1,...,m-1\}^{N}$ tale per cui ogni $h \in \mathcal{H}$ assegna un valore da $\{0,1,...,m-1\}$ a ciascuno degli $N$ elementi di $U$ come segue:
Sia $h=(y_1,y_2,...,y_N)$, allora per ogni $u_i \in U=\{u_1,u_2,...,u_N\}$ si definisce $h(u_i)=y_i$, dove ovviamente $\forall i=1,...,N,y_i \in \{0,1,...,m-1\}$. 

Si considera inizialmente l'approccio probabilistico banale:
Per ogni $u\in U$, si sceglie un valore $h(u)$ uniformly at random nell'insieme $\{0,1,...,m-1\}$, indipendentemente dalle scelte fatte per gli altri elementi dell'insieme universo, ossia, $\forall i \in \{0,1,...,m-1\},$ $$\mathbf{Pr}[h(u)=i] = \frac{1}{m}$$  
In questo caso, la probabilità che due valori $h(u)$ e $h(v)$ scelti casualmente siano uguali (e che quindi collidano) è relativamente bassa.
```ad-Proprieta
Con questo schema di hashing probabilistico uniforme, la probabilità che due valori $h(u)$ e $h(v)$ collidano, ossia $h(u)=h(v)$ è esattamente $1/m$
```
```ad-Dimostrazione
Per ogni coppia $(h(u),h(v))$ vi sono $m^2$ possibili scelte, ciascuna delle quali equiprobabile, ed esattamente $m$ di queste risultano in una collisione.
```
Nonostante questo approccio definisca una buona distribuzione degli elementi indipendentemente da $S$, la sua efficienza non risulta essere buona. Infatti, per effettuare l'operazione di ricerca $\texttt{Lookup}(u)$ risulta necessario mantenere l'insieme di tutte le coppie $\{(u,h(u)):u\in S\}$ per tener traccia dell'indice al quale è stato associato l'elemento $u$. In particolar modo, si dovrebbero memorizzare $n$ indirizzi indipendenti, e la ricerca su tale insieme risulterebbe ancora pari a $\Theta(n)$, non portando quindi al miglioramento cercato.

La selezione di un valore $h(u)$ u.a.r. nell'insieme $\{0,1,...,m-1\}$ per ogni $u\in U$ equivale ad estrarre una funzione hash $h$ u.a.r. dall'insieme di tutte le possibili funzioni hash che mappano gli elementi di $U$ in $\{0,1,...,m-1\}$. Sia ora $\mathcal{H} \subseteq \{0,1,...,m-1\}^{N}$ una generica famiglia di funzioni hash. L'analisi del caso atteso dell'algoritmo che estrae uniformemente una funzione hash da questo insieme deve prendere in considerazione la struttura di $\mathcal{H}$ e il fatto che $h$ è stata scelta uniformemente da esso. Nel worst-case, sono necessari quindi almeno $\log_2|\mathcal{H}|$ bits per rappresentare, e memorizzare, $h$. Se, per contraddizione, si usassero $t < \log_2|\mathcal{H}|$ bits per ogni $h \in \mathcal{H}$, allora si potrebbero distinguere solo $2^t < |\mathcal{H}|$ funzioni hash, i quali non sono sufficienti visto che, per via del fatto che la scelta della funzione hash avviene u.a.r., una qualsiasi $h$ può essere scelta dall'insieme. 
Idealmente, si vorrebbe che la funzione hash sia completamente uniforme:
```ad-Definizione
title: Definizioni (Funzione hash uniforme)
Sia $h\in\mathcal{H}\subseteq \{0,1,...,m-1\}^{N}$ scelta uniformemente. Si dice che $\mathcal{H}$ è uniforme se, per ogni $y_1,...,y_N\in \{0,1,...,m-1\}$ si ha che 
$$
\mathbf{Pr}\bigr[h=(y_1,...,y_N)\bigr] = \frac{1}{m^N}
$$
```
Si osserva esplicitamente che una famiglia di funzioni hash $\mathcal{H}$ è definita uniforme se e solo se $\mathcal{H}=\{0,1,...,m-1\}^N$, dove $\forall h\in \mathcal{H}$, $h:U \longrightarrow \{0,1,...,m-1\}$ con $|U|=N$.

I tre requisiti precedentemente enunciati sono in conflitto. Infatti, non è possibile ottenere tutti e tre simultaneamente. Ad esempio, si supponga che si voglia ottenere una funzione hash uniforme. Allora, per ogni scelta di $y_1,...,y_N \in \{0,1,...,m-1\}$, $\mathbf{Pr}\bigr[ h=(y_1,...,y_N) \bigr]= \frac{1}{m^N}$. Questo però è possibile solo se $\mathcal{H}=\{0,1,...,m-1\}^N$. In ogni altro caso, esisterebbe almeno una scelta di $x_1,...,x_N \in \{0,1,...,m-1\}$ tale per cui $\mathbf{Pr}\bigr[ h=(y_1,...,y_N) \bigr] < \frac{1}{m^N}$. Allora, una funzione hash uniforme richiederebbe $\log_2|\mathcal{H}|= \log_2m^N = N \log_2 m$ bits di memoria, una quantità tipicamente eccessiva.
Una funzione hash di questo tipo può essere implementata come segue:
Si riempie un vettore $V[1,N]$ con interi da $0$ a $m-1$ e si definisce $h(x)= V[x]$. Tale funzione hash soddisfa i requisiti $1$ e $2$ ma non il $3$.

Si vuole mostrare come un uso più controllato della randomizzazione possa portare ad un'implementazione efficiente del dizionario.
### Universal hashing
L'idea è quella di scegliere una funzione hash casualmente, ma non dall'insieme di tutte le possibili funzioni hash che assumono valori in $\{0,1,...,m-1\}$, bensì da una specifica famiglia di funzioni $\mathcal{H}.$
Ciascuna di queste funzioni $h\in \mathcal{H}$ mappa l'universo $U$ in $\{0,1,...,m-1\}$ e deve essere definita in maniera tale che rispetti le due seguenti proprietà:

```ad-Proprieta
Per ogni coppia di elementi $u,v \in U$, la probabilità che una funzione $h\in\mathcal{H}$ scelta casualmente soddisfi $h(u)=h(v)$ è al più $1/m$.
```
```ad-Proprieta
Ogni $h \in \mathcal{H}$ può essere rappresentata in maniera compatta e, per un dato $h\in \mathcal{H}$ e $u \in U$, il valore $h(u)$ può essere calcolato in maniera efficiente.
```

Quest'ultima proprietà è stata definita informalmente, e verrà resa più precisa successivamente.

Una famiglia di funzioni $\mathcal{H}$ che soddisfa la prima proprietà prende il nome di **famiglia di funzioni universale**.

```ad-Definizione
title: Definizione (Famiglia di funzioni hash universale)
Una famiglia $\mathcal{H}$ di funzioni è *universale* se $\forall u,v\in U$ tali che $u\neq v$:
$$
\mathbf{Pr}_{h\in \mathcal{H}}[h(u)=h(v)] \leq \frac{1}{m}
$$
```

```ad-Osservazione
La classe di tutte le possibili funzioni da $U$ in $\{0,1,...,m-1\}$ è universale. 
Questo perché, pescando casualmente una funzione hash da tale classe, la probabilità che $\forall u,v \in U$ $h(u)=h(v)$ è esattamente $1/m$ per l'argomentazione vista in precedenza. 
```

Si vuole quindi individuare una famiglia di funzioni che rispetti le due proprietà precedentemente enunciate. Per fare ciò, si rende precisa la proprietà di base che caratterizza una classe di funzioni hash universale, e che motiva la scelta di funzioni appartenenti a queste classi.  Si vuole mostrare come, sia $h$ una funzione selezionata casualmente da una famiglia di funzioni hash universale $\mathcal{H}$, allora in ogni insieme $S \subseteq U$ di $n$ elementi e per ogni $u\in S$, il numero atteso di elementi in $S$ che collidono con $u$ è costante per $m=\Theta(n)$.

```ad-Teorema
Sia $\mathcal{H}$ una famiglia di funzioni hash universale. Sia $S \subseteq U$ un insieme di $n$ elementi, e sia $u\in S$. Sia $h$ una funzione scelta uniformly at random da $\mathcal{H}$ e sia $X$ la variabile aleatoria che conta il numero di elementi di $S$ mappati in $h(u)$, ossia 
$$
X = |\{s\in S: h(s) = h(u)\}|
$$
Allora
$$
E[X] \leq 1 + \frac{n}{m}
$$
```
```ad-Dimostrazione
Sia $u$ un elemento di $S$ fissato.
Per ogni $s\in S$ si definisce la variabile aleatoria $X_s$ come segue
$$
\begin{equation*}
X_s = 
  \left\{ 
    \begin{aligned}
      &\ 1 \ \ \ \textnormal{se }\ h(s)=h(u) \\
      &\ 0 \ \ \  \textnormal{altrimenti }\   
    \end{aligned}
  \right.
\end{equation*}
$$
Si ha che 
$$
E[X_s] = \mathbf{Pr}[X_s=1] \leq \frac{1}{m}
$$
essendo la classe di funzioni hash $\mathcal{H}$ universale.
Allora, essendo
$$
X = \sum_{s\in S}X_s
$$
Per la linearità del valore atteso si ha
$$
\begin{align*}
E[X] =& E\Bigr[\sum_{s\in S}X_s\Bigr] = \sum_{s\in S}E[X_s] = \sum_{s\in S}\mathbf{Pr}\bigr[h(s)=h(u)\bigr] \\
\\
=& \ 1 + \sum_{s\in S\backslash \{u\}}\mathbf{Pr}\bigr[h(s)=h(u)\bigr] \leq 1 + \frac{n}{m}
\end{align*}
$$
```

Per $m=\Theta(n)$ ci si aspetta quindi che il numero di collisioni per un dato elemento $u$ sia costante.

Si definisce ora una particolare classe di funzioni hash universale per la risoluzione del problema del dizionario.
Si sceglie un numero primo $p$ tale per cui $n\leq p\leq 2n$ (il quale esiste sempre). La dimensione della tabella hash $H$ utilizzata per memorizzare gli elementi di $S$ è pari a tale numero $p$.
Si identifica ciascun elemento $x\in U$ con un intero base $p$ di $r$ cifre:

$$
x = (x_1,x_2,...,x_r) \ :\  0\leq x_i < p \textnormal{ per }  i=1,...,r
$$

Se  $|U|=N$, allora $r$ deve essere tale per cui $p^r \geq N$, perché a ciascun elemento di $U$ deve essere rappresentato con $r$ cifre in base $p$ in maniera univoca, quindi
$$
r \geq \frac{\log(N)}{\log(p)}
$$

Sia $\mathcal{A}$ l'insieme di tutti i vettori della forma $a=(a_1,...,a_r)$, dove $a_i \in \{0,1,...,p-1\}$ per $i=1,...,r$.
Per ogni $a \in \mathcal{A}$ si definisce la funzione lineare
$$
h_a(x) \ = \ \Bigr[\sum_{i=1}^{r}a_ix_i\Bigr] \ (\textnormal{mod } p)
$$
Cosi facendo, si definisce quindi una famiglia di funzioni hash
$$
\mathcal{H} = \{h_a:a \in\mathcal{A}\}
$$
Si osserva esplicitamente che il numero di funzioni così costruite è pari a $p^r$, questo perché ad ogni funzione in $\mathcal{H}$ è associato un vettore $a$ univoco di $r$ cifre, ciascuna che assume valori da $0$ a $p-1$, di conseguenza esistono $p^r$ vettori distinti.
Quindi, essendo $p=\Theta(n)$ (si ricorda che $n\leq p \leq 2n$)
$$
|\mathcal{H}| = p^r = \Theta(n^r)
$$
Per eseguire $\texttt{MakeDictionary}()$ si sceglie un vettore casuale $a=(a_1,a_2,...,a_r)$ da $\mathcal{A}$. Questo è possibile scegliendo ciascun $a_i$ uniformly at random da $\{0,1,...,p-1\}$, formando così $h_a$. 
Tale funzione rispetta la seconda proprietà precedentemente enunciata: ha una rappresentazione compatta, essendo che, scegliendo e memorizzando un $a$ casuale, è possibile calcolare $h_a(u)$ per ogni $u\in U$. Per la scelta e la memorizzazione di $a$, e quindi di $h$, sono necessari $r=\Theta(\log (N)/\log (p))$ cifre, ciascuna avente lunghezza di $\log(p)$ bits. 
La si usa successivamente per implementare le operazioni di $\texttt{Insert}(u),\texttt{Delete}(u)$ e $\texttt{Lookup(u)}$ come enunciato in precedenza. Si ricorda che queste operazioni vengono precedute dal calcolo della funzione hash, il quale è stato mostrato essere efficiente. Per mostrare quindi che tale implementazione del dizionario risulti essere efficiente, rimane da dimostrare che gli elementi di $U$ vengano distribuiti "bene" all'interno dell'hash table, e per fare ciò, bisogna mostrare che $\mathcal{H}$ sia una famiglia di funzioni hash universale.

```ad-Osservazione
Una collisione $h_a(x)=h_a(y)$ definisce un'equazione lineare modulo $p$.
```

Per analizzare questa equazione, si usa la seguente "legge di cancellazione":

```ad-Lemma
Per ogni primo $p$ ed ogni intero $z \neq 0$ (mod $p$), e per ogni intero $\alpha,\beta$
$$
\alpha z = \beta z \textnormal{ (mod } p) \implies \alpha = \beta \textnormal{ (mod } p)
$$
```
```ad-Dimostrazione
Sia $\alpha z = \beta z \textnormal{ (mod } p)$. Allora
$$
z(\alpha -\beta) = 0 \textnormal{ (mod }p)
$$
Ossia, $z(\alpha -\beta)$ è divisibile per $p$. Ma per ipotesi $z \neq 0 \textnormal{ (mod }p)$, quindi $z$ non è divisibile per $p$. Essendo $p$ un primo, quindi, deve essere necessariamente che $\alpha -\beta$ risulti essere divisibile per $p$, ossia
$$
\alpha -\beta= 0 \textnormal{ (mod }p) \implies \alpha = \beta \textnormal{ (mod }p)
$$
```

```ad-Osservazione
*inserire argomentazione sui numeri primi*
 perché se così non fosse $z(\alpha -\beta) \neq 0 \textnormal{ (mod }p)$.
```
Tale lemma risulta necessario per la dimostrazione del seguente teorema

```ad-Teorema
La classe di funzioni hash $\mathcal{H}=\{h_a:a\in \mathcal{A}\}$ è universale.
```
```ad-Dimostrazione
Siano $x=(x_1,x_2,...,x_r)$ e $y=(y_1,y_2,...,y_r)$ due elementi distinti in $U$.
Si vuole mostrare che la probabilità che $h_a(x)=h_a(y)$, per un $a\in \mathcal{A}$ scelto casualmente, è al più $1/p$.

Essendo $x \neq y$ allora deve esistere un indice $j$ tale per cui $x_j \neq y_j$.
Si sceglie ora un vettore casuale $a \in \mathcal{A}$ nella seguente maniera:
si scelgono prima tutte le coordinate $a_i$ dove $i\neq j$. Successivamente, si sceglie la coordinata $a_j$ casualmente.
Si può quindi assumere che $a_i$ sia fissato per tutte le coordinate $i\neq j$.
Si mostra quindi come, indipendentemente dalla scelte di tutte le altre coordinate $a_i$, la probabilità che $h_a(x)=h_a(y)$, considerando la scelta finale di $a_j$, sia esattamente $1/p$. Da ciò seguirà che la probabilità di $h_a(x)=h_a(y)$ per la scelta casuale di tutto il vettore $a$ dovrà necessariamente essere anch'essa $1/p$.

Ciò è chiaro intuitivamente: se la probabilità è $1/p$ indipendentemente dalla scelta di tutti gli altri $a_i$, allora sarà $1/p$ in generale. 
Si da una dimostrazione diretta usando la probabilità condizionata:

Sia $\mathcal{E}$ l'evento
$$
h_a(x) = h_a(y)
$$

e sia $\mathcal{F}_b$ l'evento 
$$
\textnormal{Tutte le coordinate }a_i\ (\textnormal{per }i\neq j)\textnormal{ ricevono una sequenza di valori }b.
$$
Si vuole mostrare che $\mathbf{Pr}\bigr[\mathcal{E}\ |\ \mathcal{F}_b\bigr] = 1/p$ per ogni $b$. Da ciò seguirà che 
$$
\mathbf{Pr}\bigr[\mathcal{E}\bigr] = \sum_b \mathbf{Pr}\bigr[\mathcal{E}\ |\ \mathcal{F}_b\bigr]\cdot \mathbf{Pr}\bigr[ \mathcal{F}_b\bigr] = (1/p)\sum_b \mathbf{Pr}\bigr[\mathcal{F}_b\bigr] = 1/p
$$

Si assuma quindi che tutti i valori per le coordinate $a_i$ (eccetto $a_j$) siano state scelte arbitrariamente, e si consideri la probabilità di selezionare un $a_j$ tale per cui $h_a(x)=h_a(y)$. Riordinando i termini, si osserva che
$$
h_a(x) = h_a(y) \iff a_j(y_j - x_j) = \sum_{i\neq j}a_i(x_i-y_i) \textnormal{ mod }p
$$

Essendo le scelte per tutte le coordinate $a_i$ $(i \neq j)$ fissate, si può vedere la parte destra dell'equazione come una quantità fissata $\alpha$. Sia ora $z=y_j -x_j$.

Bisogna mostrare che esiste esattamente un solo valore $0\leq a_j <p$ che soddisfa $a_j z = \alpha$ (mod $p$). Infatti, cosi facendo, essendo $a_j$ scelto uniformly at random da $\mathbb{Z}_p$, allora la probabilità di scegliere tale valore per $a_j$ è esattamente $1/p$.
Si supponga quindi che esistano due valori che soddisfino l'equazione vista in precedenza, siano essi $a_j$ ed $a_j^{'}$, ossia
$$
a_j z = \alpha \textnormal{ (mod }p)
$$
$$
a_j^{'} z = \alpha \textnormal{ (mod }p)
$$
Dunque si avrebbe che $a_j z =a_j^{'} z$ (mod $p$), e per il lemma enunciato in precedenza, si avrebbe $a_j = a_j^{'}$ (mod $p$). Ma essendo per ipotesi $a_j,a_j^{'} <p$, allora $a_j$ ed $a_j^{'}$ sono esattamente lo stesso numero. Da ciò segue che esiste un singolo $a_j$ in $\mathbb{Z}_p$ che soddisfa $a_jz=\alpha$ (mod $p$).

Questo vuol dire che la probabilità di scegliere $a_j$ tale per cui $h_a(x)=h_a(y)$ è $1/p$, indipendentemente dalla scelta delle altre coordinate $a_i$ in $a$. Quindi la probabilità che $x$ e $y$ collidano è $1/p$, e ciò dimostra che $\mathcal{H}$ è una classe di funzioni hash universale.
```
```ad-Osservazione
Si vuole mostrare esplicitamente che 
$$
h_a(x) = h_a(y) \iff a_j(y_j - x_j) = \sum_{i\neq j}a_i(x_i-y_i) \textnormal{ mod }p
$$

Si ricorda per definizione che 
$$
	h_a(x) = \sum_{i=1}^r a_ix_i\ \ \ \textnormal{ e }\ \ \ h_a(y) = \sum_{i=1}^r a_iy_i
$$

Quindi 
$$
h_a(x) = h_a(y) \iff h_a(x) - h_a(y) = 0 \iff \sum_{i=1}^r a_ix_i - \sum_{i=1}^r a_iy_i = 0 \textnormal{ (mod }p)
$$
$$
\iff \sum_{i=1}^r a_i(x_i-y_i) = 0 \textnormal{ (mod }p) \iff \sum_{i=1,i\neq j}^r a_i(x_i-y_i) + a_j(x_j - y_j) = 0 \textnormal{ (mod }p)
$$
$$
\iff \sum_{i=1,i\neq j}^r a_i(x_i-y_i) = -a_j(x_j - y_j) \textnormal{ (mod }p) \iff \sum_{i=1,i\neq j}^r a_i(x_i-y_i) = a_j(y_j - x_j) \textnormal{ (mod }p)
$$
```
```ad-Osservazione
*inserire argomentazione sui numeri primi per il fatto che dato un numero primo $p$ e l'anello $Z_p$, essendo $z$ diverso da 0 ha un unico inverso in Z_p*
```

Si conclude osservando che, mediante l'utilizzo di una funzione appartenente ad $\mathcal{H}$ per l'implementazione del dizionario, si usa uno spazio pari a $\Theta(n)$, {$\mathcal{O}(n \log N)$ se gli elementi sono interi -> DA VERIFICARE}.
L'esecuzione delle operazioni relative a tale struttura dati hanno costo $\mathcal{O}(1)$, questo perché la memorizzazione di $h_a \in \mathcal{H}$ richiede la memorizzazione di una singola chiave $a$, che richiede spazio costante, e il calcolo di tale funzione per ogni elemento in $U$, che avviene per l'esecuzione di ogni operazione definita per il dizionario, richiede tempo costante.
### K-wise independent hashing
Siano gli $N$ elementi dell'insieme universo $U$ gli interi da $0$ ad $N-1$, ossia  $U=\{0,1,...,N-1\}$. In questa sezione vengono trattate le funzioni hash su interi definite come $h:\{0,1,...,N-1\}\longrightarrow\{0,1,...,m-1\}$. 
Il $k$-wise independent hashing è una versione più debole dell'hashing uniforme.
```ad-Definizione
title: Definizione (Famiglia di funzioni hash $k$-wise independent)
Si dice che una famiglia di funzioni hash $\mathcal{H}$ è $k$-wise independent se e solo se, per una scelta uniforme di $h \in \mathcal{H}$, si ha che
$$
\mathbf{Pr}\Biggr[\bigwedge\limits_{i=1}^k h(x_i)=y_i \Biggr] = m^{-k}
$$

per ogni scelta di $x_1,...,x_k \in \{0,1,...,N-1\}$ distinti e $y_1,...,y_k \in \{0,1,...,m-1\}$ non necessariamente distinti.
```
Quindi, una famiglia $\mathcal{H}$ uniforme è il caso particolare in cui $k=N$. Ciò implica che:
- Per ogni scelta di $x_1,...,x_t$ tutti distinti con $t \geq k$, le variabili aleatorie $h(x_1),...,h(x_t)$ sono $k$-wise indipendenti.
- $h$ mappa le $k$-tuple uniformemente: la $k$-tupla $(h(x_1),...,h(x_k))$ è una variabile aleatoria uniforme su $\{0,1,...,m-1\}^k$ quando $x_1,...,x_k$ sono distinti.
```ad-Osservazione
La $2$-wise indipendenza (o pairwise indipendenza) di una famiglia di funzioni hash implica la sua universalità.
Si consideri il sottoinsieme $\{h(x_2)=y\}_{y\in [m]}$ dello spazio di campionamento 
$$
\Omega = \bigcup_{i=0}^{N-1}\Bigl(\{h(x_i)=y\}_{y\in [m]}\Bigl).
$$
Per il teorema della probabilità totale e per la $2$-wise indipendenza si ha che

$$
\mathbf{Pr}\bigr[h(x_1) = h(x_2) \bigr] = \sum_{y=0}^{m-1} \mathbf{Pr}\bigr[h(x_1)=h(x_2) \wedge h(x_2) = y\bigr]
$$
$$
\sum_{y=0}^{m-1} \mathbf{Pr}\bigr[h(x_1)=y \wedge h(x_2) = y\bigr] = \sum_{y=0}^{m-1} m^{-2} \ = \ \frac{1}{m}
$$
```
Si mostra ora la costruzione una famiglia di funzioni pairwise indipendente.

Sia $p \geq N$ un numero primo. Si definisce la funzione hash $h_{a,b}: U \longrightarrow \{0,1,...,p-1\}$ come

$$
h_{a,b}(x) = (a \cdot x + b) \ \ \textnormal{ (mod }p)
$$

Si definisce la famiglia di funzioni hash $\mathcal{\hat{H}}$ come segue
$$
\mathcal{\hat{H}} = \{h_{a,b}\ :\ a,b \in \{0,1,...,p-1\} \}
$$
In altre parole, una funzione $h_{a,b}\in \mathcal{\hat{H}}$ uniforme è un polinomio di grado $1$ in $\mathbb{Z}_p$ scelto uniformly at random. Tale funzione è specificata completamente da $a,b,p$ e quindi può essere memorizzata in $\mathcal{O}(\log p)$ bits. Inoltre, può essere calcolata in tempo $\mathcal{O}(1)$. Si mostra ora che tale famiglia è pairwise indipendente:

```ad-Lemma
$\mathcal{\hat{H}}$ è una famiglia di funzioni hash pairwise indipendente.
```
```ad-Dimostrazione
Siano $x_1,x_2 \in \{0,1,...,N-1\}$ due elementi tali per cui $x_1 \neq x_2$ e siano $y_1,y_2 \in \{0,1,...,p-1\}$.

Si osserva esplicitamente che essendo $x_1 \neq x_2$ e $p \geq N$, allora $x_1 \not\equiv_p x_2$.

Allora, si ha che 
$$
\mathbf{Pr}\Bigr[h(x_1)=y_1 \wedge h(x_2) = y_2\Bigr] = \mathbf{Pr}\Bigr[ax_1 +b \equiv_p y_1 \wedge ax_2 + b\equiv_p y_2\Bigr]
$$
$$
= \mathbf{Pr}\Bigr[b \equiv_p y_1 - x_1 \cdot \frac{y_2 - y_1}{x_2 - x_1} \wedge a \equiv_p \frac{y_2 - y_1}{x_2-x_1} \Bigr]\ \ \ \ \ \ (1)
$$
$$
= \mathbf{Pr}\Bigr[b \equiv_p y_1 - x_1 \cdot \frac{y_2 - y_1}{x_2 - x_1}\Bigr] \cdot \mathbf{Pr}\Bigr[a \equiv_p \frac{y_2 - y_1}{x_2 - x_1} \Bigr] \ \ \ \ (2)
$$
$$
= p^{-2}
$$

Dove
$(1)$ è dato risolvendo il seguente sistema nelle variabili $a$ e $b$:
$$
\begin{equation*}
  \left\{ 
    \begin{aligned}
      & ax_1 + b \equiv_p y_1 \\
      & ax_2 + b \equiv_p y_2 \   
    \end{aligned}
  \right.
\end{equation*}
$$
Si osserva che $(x_2 - x_1)^{-1}$ esiste perché $x_2 \not\equiv_p x_1$ e $\mathbb{Z}_p$ è un campo, quindi ogni elemento (eccetto $0$) ha un inverso moltiplicativo. [Nota: vedere questa questione]
$(2)$ $a$ e $b$ sono variabili aleatorie indipendenti.
```

```ad-Osservazione
I termini noti del sistema nella dimostrazione sono $x_1,x_2,y_1,y_2$, mentre $a,b$ sono variabili aleatorie, e sono l'incognita del sistema.
```

La funzione $h_{a,b}$ appena definita non può essere utilizzata per l'implementazione di un dizionario. Questo perché $p \geq N$, di conseguenza risulterebbe necessario un array $H$ di dimensione $p$, e ciò sarebbe di dimensioni spropositate. Si definisce quindi un'altra funzione hash partendo da $h_{a,b}$:

Sia $m << N$ la dimensione dell'hash table e sia $p>N$ un numero primo, dove si ricorda che $U=[N]$ e quindi $|U|=N$. Siano $a \in \{1,..,p-1\}$ e $b \in \{0,1,...,p-1\}$ due valori scelti uniformly at random dai rispettivi insiemi di appartenenza.
Si definisce la funzione hash:

$$
\bar{h}(x) = ((a \cdot x + b) \textnormal{ mod }p) \textnormal{ mod m}
$$

e si mostra che tale funzione $\bar{h}$ è universale.
```ad-Lemma
La funzione $\bar{h}(x)$ è universale, ossia per ogni $x \neq y$, vale
$$
\mathbf{Pr} \left[ \bar{h}(x)=\bar{h}(y)\right] \leq 1/m
$$
```
```ad-Dimostrazione
Siano $x_1,x_2 \in [N]$ tali per cui $x_1 \neq x_2$. Si vuole dare una delimitazione superiore a $\mathbf{Pr} \left[ \bar{h}(x_1)=\bar{h}(x_2)\right]$. Siano $\hat{X}_1 = ax_1 + b$ (mod $p$) e $\hat{X}_2 = ax_2 + b$ (mod $p$), e quindi $\bar{h}(x_1)= \hat{X}_1$ (mod $m$) e $\bar{h}(x_2)= \hat{X}_2$ (mod $m$). Allora si può riscrivere la probabilità vista in precedenza come
$$
\mathbf{Pr} \left[ \bar{h}(x_1)=\bar{h}(x_2)\right] = \mathbf{Pr}\left[\hat{X}_1 \equiv_m \hat{X}_2\right]
$$

Bisogna mostrare prima di tutto che $\hat{X}_1 \neq \hat{X}_2$. Se, per contraddizione, fosse $\hat{X}_1 = \hat{X}_2$, allora $ax_1+b \equiv_p ax_2+b$. Essendo $a\neq 0$, ciò è equivalente a $x_1 \equiv_p x_2$. Ma $p \geq N \geq x_1$ e $p \geq N \geq x_2$, quindi $x_1 \equiv_p x_2$ implica $x_1 = x_2$, il che, per ipotesi iniziale, è assurdo.

Si torna a dare un upper bound a $\mathbf{Pr}\left[\hat{X}_1 \equiv_m \hat{X}_2\right]$. Applicando il teorema di probabilità totale su $\hat{X}_2$ si ha
$$
\mathbf{Pr}\left[\hat{X}_1 \equiv_m \hat{X}_2\right] = \sum_{i=0}^{p-1} \mathbf{Pr}\left[ \hat{X}_1 \equiv_m \hat{X}_2 \wedge \hat{X}_2 = i\right]  
$$
$$
\sum_{i=0}^{p-1} \mathbf{Pr}\left[ \hat{X}_1 \equiv_m i \wedge \hat{X}_2 = i\right]  
$$
Applicando di nuovo il teorema di probabilità totale su $\hat{X}_1$ si ha
$$
\sum_{i=0}^{p-1} \sum_{j=0,..,p-1:j\neq i} \mathbf{Pr}\left[ \hat{X}_1 \equiv_m i \wedge \hat{X}_2 = i \wedge \hat{X}_1 = j\right] 
$$
Sia ora $W= \{j \in [p]: j\neq i \wedge j \equiv_m i\}$ l'insieme di tutti i valori diversi da $i$ ma equivalenti ad $i$ modulo $m$. La probabilità $\mathbf{Pr}\left[ \hat{X}_1 \equiv_m i \wedge \hat{X}_2 = i \wedge \hat{X}_1 = j\right]$ è uguale a $0$ per $j\notin W$, e quindi la sommatoria precedente diventa
$$
\sum_{i=0}^{p-1} \sum_{j \in W} \mathbf{Pr}\left[\hat{X}_2 = i \wedge \hat{X}_1 = j\right]
$$
Si da ora un bound sulla dimensione di $W$. Prima di tutto, si nota che $|W| \leq \lceil p/m \rceil -1$. Questo perché nell'insieme $\{0,...,p-1\}$ ci sono al più $\lceil p/m \rceil$ valori equivalenti ad $i$ modulo $m$, e da questi bisogna escludere $i$. Si mostra ora che $\lceil p/m \rceil -1 \leq (p-1)/m$.
Se $m$ divide $p$, allora $\lceil p/m \rceil -1 = p/m -1 \leq (p-1)/m$.
Altrimenti, sia $k= \lfloor p/m \rfloor$, allora $p=km+k^{1}$ per $k^{1}=p-km$.
Si osserva che $1\leq k^{1} < m$ essendo che $m$ non divide $p$. In particolar modo, $1/m \leq k^{1}/m$. Allora
$$
\frac{p-1}{m} = \frac{p}{m} - \frac{1}{m} \geq \left( \frac{km+k^{1}}{m}\right) - \frac{k^{1}}{m} = k = \lfloor p/m \rfloor \geq \lceil p/m \rceil - 1
$$
Si conclude che $|W| \leq \frac{p-1}{m}$.
Usando la stessa argomentazione del lemma precedente, ed osservando che $a \neq 0$, si può ottenere facilmente che
$$
\mathbf{Pr}\left[\hat{X}_2 = i \wedge \hat{X}_1 = j\right] \leq \frac{1}{p(p-1)}.
$$
Mettendo tutto assieme si ha
$$
\mathbf{Pr} \left[ \bar{h}(x_1)=\bar{h}(x_2)\right] \leq \sum_{i=0}^{p-1} \sum_{j \in W} \mathbf{Pr}\left[\hat{X}_2 = i \wedge \hat{X}_1 = j\right]
$$
$$
\leq p \cdot \frac{p-1}{m} \cdot \frac{1}{p(p-1)} = 1/m
$$
E ciò dimostra l'universalità di $\bar{h}$.
```
### Collision-free hashing
In alcune applicazioni risulta necessaria una funzione hash *collision-free*
```ad-Definizione
title: Definizione (Funzione hash collision-free)
Una funzione hash $h:\{0,1,...,N-1\}\longrightarrow \{0,1...,m-1\}$ si dice collision free su un insieme $A \subseteq \{0,1,...,N-1\}$ se, per ogni $x_1,x_2 \in A$,$x_1 \neq x_2$, si ha $h(x_1)\neq h(x_2)$.
```
In generale, basta che una funzione soddisfi tale proprietà con alta probabilità. Si riporta nuovamente tale definizione
```ad-Definizione
title: Definizione (Alta probabilità)
Si dice che un evento accade con alta probabilità rispetto ad una quantità $n$ se la sua probabilità è almeno $1-n^{-c}$ per una costante $c$ arbitrariamente grande.
Equivalentemente, si dice che l'evento accade con probabilità che decresce come l'inverso di un polinomio.
```
### Scegliere dinamicamente la dimensione dell'hash table
Si fa presente come, nel problema del dizionario, $S$ è un insieme dinamico, ossia subisce modifiche nel tempo, e si vuole utilizzare spazio pari a $\mathcal{O}(|S|)$.
Si vuole mostrare una tecnica per scegliere la dimensione della tabella hash in maniera dinamica.
Siano
- $n$ : numero di elementi correntemente nella tabella, ossia $n=|S|$.
- $N$: dimensione virtuale della tabella/dimensione dell'universo
- $m$: dimensione reale della tabella, dove $m$ è un primo tale per cui $N \leq m \leq 2N$.

Double/Halving technique:
1. Inizializza $n=N=1$;
2. Quando $n>N$: //dopo che sono stati aggiunti elementi in $S$
	21. Poni $N=2N$;
	22. Scegli un nuovo primo $m$ tale per cui $m \in \Theta(n)$;
	23. Ri-esegui il calcolo della funzione hash per tutti gli elementi; //in tempo $\mathcal{O}(n)$ per via del fatto che il costo ammortizzato di ogni inserimento/cancellazione è $\mathcal{O}(1)$
3. Quando $n<N/4$:
	31. Poni $N=N/2$;
	32. Scegli un nuovo primo $m$ tale per cui $m \in \Theta(n)$;
	33. Ri-esegui il calcolo della funzione hash per tutti gli elementi; 
### Perfect hashing (Double hashing)
Si considera il problema del dizionario statico: 
Dato un insieme $S$ di $n$ elementi (o chiavi) da un universo $U$ di dimensione $N$, si vuole costruire una struttura dati di dimensione $\mathcal{O}(n)$ che supporti le operazioni di ricerca (dato $x \in U$, sapere se $x \in S$) in tempo costante. Si vuole un tempo di costruzione atteso per tale struttura dati che sia polinomiale con alta probabilità.

L'idea è quella di costruire una tabella a due livelli:

- Passo 1: Pesca uniformly at random una funzione hash $h_1:U\longrightarrow[m]$ da una famiglia di funzioni hash universale per $m=\Theta(n)$ (ad esempio $m$ numero primo vicino ad $n$). Esegui l'hashing su tutti gli elementi di $S$ usando $h_1$, ossia costruisci un'array $H$ di liste concatenate e inserisci ogni $x\in S$ in posizione $H[h_1(x)]$.
- Passo 2: Per ogni $j\in [m]$, sia $l_j$ il numero di elementi in posizione $j$ in $H$. $$ l_j = |\{i\ |\ h(x_i)\ =\ j \ \}|$$Pesca uniformly at random una funzione hash $h_{2,j}:U \longrightarrow [m_j]$ da una famiglia di funzioni hash universale per $l_j^2 \leq m_j \leq \mathcal{O}(l_j^2)$ (ad esempio $m_j$ numero primo vicino ad esso). Sostituisci la lista concatenata in posizione $j$ con una tabella hash $H_j$ di dimensione $m_j$,  mappando gli elementi in posizione $j$ di tale lista in $H_j$ utilizzando $h_{2,j}$.

La complessità spaziale è $$\mathcal{O}\left(n+ \sum_{j=0}^{m-1}l_j^2 \right)$$Per ridurla a $\mathcal{O}(n)$ bisogna aggiungere due passi intermedi:
- Passo 1.5: Se $\sum_{j=0}^{m-1}l_j^2>cn$ dove $c$ è una costante scelta, riesegui il passo $1$.
- Passo 2.5: Quando $h_{2,j}(u)=h_{2,j}(v)$ per ogni $u,v \in S$ tali per cui $u\neq v$ e $h_1(u)=h_1(v)$, ossia quando si verifica una collisione al secondo livello di hash, pesca una nuova funzione $h_{2,j}$ e rimappa tutti gli $l_j$ elementi in $H_j$. 

Questi due passi garantiscono che non si verifichino collisioni al secondo livello, e che la complessità spaziale sia di $\mathcal{O}(n)$. Ciò garantisce che il tempo di ricerca di un elemento sia $\mathcal{O}(1)$.
Si osserva esplicitamente che risulta improbabile che si verifichino le condizioni espresse nel passo $2.5$, essendovi $\Theta(l_j^2)$ celle per mappare $l_j$ elementi.

Si studia ora il tempo di costruzione della struttura dati impiegato dall'algoritmo.
Ricordando che il calcolo di una delle funzioni hash universali viste in precedenza richiede tempo costante, si ha che i passi $(1)$ e $(2)$ richiedono tempo $\mathcal{O}(n)$.
Per il passo $(2.5)$ si ha che
$$
\begin{align*}
\mathbf{Pr}_{h_{2,j}}\left[h_{2,j}(u)=h_{2,j}(v), \textnormal{ per }u\neq v\right] &\leq \sum_{u,v\in S, u\neq v} \mathbf{Pr}\left[h_{2,j}(u)=h_{2,j}(v)\right]  \\
\\
&\leq \binom{l_j}{2} \cdot \frac{1}{l_j^2} < \frac{1}{2}
\end{align*}
$$
Dove si osserva esplicitamente che, per l'universalità di $h_{2,j}$
$$
\mathbf{Pr}\left[h_{2,j}(u)=h_{2,j}(v)\right] \leq \frac{1}{l_j^2}
$$
Quindi, ogni prova è come un lancio di moneta (distr geom?) . Se l'esito è "testa", si passa allo step successivo. Si ha quindi che $\mathbf{E}\left[\textnormal{numero di prove} \right]\leq 2$ e $\textnormal{(numero di prove)}=\mathcal{O}(\log n)$ con alta probabilità. Ogni prova richiede tempo $\mathcal{O}(l_j)$, che, per un Chernoff bound (rivedere quale e fare la dim per esercizio), $l_j = \mathcal{O}(\log n)$ con alta probabilità, quindi ogni prova richiede tempo $\mathcal{O}(\log n)$. 
Dovendo compiere tale passo per ogni $j$, si ha che la complessità temporale totale è 
$\mathcal{O}(\log n)\cdot \mathcal{O}(\log n) \cdot \mathcal{O}(n)= \mathcal{O}(n \log^2 n)$ con alta probabilità.

Per il passo $(1.5)$, si vuole mostrare che $\mathbf{E}\left[\sum_{j=0}^{m-1} l_j^2\right]= \Theta(n)$, applicando poi la disuguaglianza di Markov.
Si definisce quindi la variabile aleatoria
$$
\begin{equation*}
X_{u,v} = 
  \left\{ 
    \begin{aligned}
      &\ 1 \ \ \ \textnormal{se }\ h_1(v)=h_1(u) \\
      &\ 0 \ \ \  \textnormal{altrimenti }\   
    \end{aligned}
  \right.
\end{equation*}
$$
```ad-Osservazione
$$
\sum_{j=0}^{m-1} l_j^2= \sum_{u\in S}\sum_{v \in S}X_{u,v}
$$
Questo perché -rivedere hash lucio+andy-
```

Si ha quindi che 
$$
\begin{align*}
\mathbf{E}\left[\sum_{j=0}^{m-1} l_j^2\right] &= \mathbf{E}\left[\sum_{u\in S}\sum_{v \in S}X_{u,v}\right] = \sum_{u \in S}\sum_{v\in S} \mathbf{E}\left[X_{u,v}\right] \\
\\
&= \sum_{u \in S}\sum_{v\in S} \mathbf{Pr}\left[h_1(u)=h_1(v)\right] \ \ \ \ \ \ \ \ \ \textnormal{(1)}
\\
\\
&\leq \sum_{u \in S} \left[ 1 + \frac{n}{m}\right] \ \ \ \ \ \ \ \ \ \textnormal{(2)}
\\
\\
&= n +   \frac{n^2}{m} \leq 2n \ \ \ \ \ \ \ \ \ \textnormal{(3)}
 
\end{align*}
$$ 
Dove:
$(1)$ $\mathbf{E}\left[X_{u,v}\right] = \mathbf{Pr}\left[ h_1(u) = h_1(v)\right]$.

$(2)$ Si ha che $\sum_{v \in S}X_{u,v}=X_u$ dove $X_u=|\{v \in S\ |\ h_1(v) = h_1(u)\}|$ è la v.a. che conta gli elementi in $S$ mappati in $h_1(u)$. Si ha quindi  $$\sum_{v\in S} \mathbf{E}\left[X_{u,v}\right] = \mathbf{E}\left[ X_u \right] \leq 1 + \frac{n}{m}$$essendo $h_1$ universale.
$(3)$ Vero perché $m \geq n$ 

Per la disuguaglianza di Markov si ha
$$
\mathbf{Pr}\left[\sum_{j=0}^{m-1} l_j^2 > cn\right] \leq \frac{\mathbf{E}\left[\sum_{j=0}^{m-1} l_j^2\right]}{cn} \leq \frac{2n}{cn} \leq 1/2
$$
Scegliendo un $c$ adatto, ad esempio $c\geq 4$.
Si ha quindi che $\mathbf{E}\left[\textnormal{numero di prove} \right]\leq 2$ e $\textnormal{(numero di prove)}=\mathcal{O}(\log n)$ con alta probabilità. 
Da ciò, si ha che il passo $(1)$ ed il passo $(1.5)$ combinati richiedono tempo atteso $\mathcal{O}(n \log n)$ con alta probabilità.
