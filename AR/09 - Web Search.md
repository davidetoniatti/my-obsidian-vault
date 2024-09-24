In questa sezione, si tratta il problema di individuare all'interno di una rete, ed estrarre da essa, informazioni rilevanti ad una data richiesta. In particolare, si tratta il problema nel caso in cui la rete sia il Web, cioè una rete costituita da documenti collegati mediante hyperlink.
# World Wide Web
Il Web è un'applicazione sviluppata da _Tim Berners-Lee_ nel periodo 1989-1991 per consentire alle persone di condividere informazioni tramite Internet. L'architetture del web è di tipo client-server e il suo funzionamento può essere descritto come segue:
- delle informazioni sono memorizzate su delle macchine **server** e sono rese disponibili tramite internet sotto forma di **pagine web**;
- un'applicazione **client** richiede tali pagine Web, pubblicamente accessibili;
Il principio logico fondativo del web è l'**ipertesto**, nella quale l'informazione è organizzata in una struttura di rete, ossia, i documenti che costituiscono il web sono nodi di un grafo diretto. Si tratta perciò di una organizzazione **non lineare** dell'informazione
# HITS
Una volta individuato un sottoinsieme di pagine inerenti alla ricerca effettuata, si considerano maggiormente rilevanti quelle pagine che sono molto puntato (o **indicizzate**) all'interno dell'insieme.
Potrebbe capitare che una pagina venga puntata da tantissime pagine che in realtà sono molto poco rilevanti ai fini della ricerca.

> quando e quanto una pagina è **autorevole** nel conferire rilevanza ad un'altra pagina che essa punta?

L'algoritmo **HITS** consente di valutare la rilevanza delle pagine in funzione dei link che la puntano.

Ad ogni pagina si associano due indici:
1. un indice di **autorità** che esprime la *rilevanza* della pagina ai fini della ricerca;
2. un indice di **hub** che invece esprime l'attitudine della pagina a *conferire autorità* alle pagine alle quali punta.

Per ogni pagina $i$ si indica con $a_i$ il valore di **autorità** come il numero di pagine, nell'insieme di pagine attinenti alla ricerca, che puntano ad $i$
$$
a_{i} = |\{ j: j \text{ è attinente alla ricerca } \land j\to i \}|
$$
dove con $j\to i$ indica che la pagina $j$ è puntata dalla pagina $i$.
Si assuma che, a fronte di una ricerca si ottiene un sottoinsieme di $n$ pagine, $M$ sia la matrice di adiacenza del sottografo del web indotto dalle $n$ pagine inerenti alla ricerca, ovvero
$$
\forall 1 \leq i, j \leq n \quad M[i,j] = \begin{cases}
1 \quad \text{se } i\to j \\ \\
0 \quad \text{altrimenti}
\end{cases}
$$
allora
$$
a_{i} = \sum_{j=1}^n M[j,i]
$$
Analogamente, si indica con $h_i$ l'indice di hub di una pagina $i$. Tale indice si potrebbe definire come il numero di pagine (sempre all'interno del sottoinsieme di nodi inerenti alla ricerca) alle quali $i$ punta. Ovvero
$$
h_{i} = |\{j: j \text{ è attinente alla ricerca} \land i\to j  \}|
$$
e dunque
$$
h_{i} = \sum_{j=1}^n M[i,j]
$$
Si osserva che si possono *raffinare* i due indici appena definiti tramite le seguenti idee:
- il valore di autorità di una pagina $i$ dovrebbe essere tanto più elevato quanto più sono **autorevoli** le pagine $j$ che la puntano;
- simmetricamente, il valore di hub di una pagina $i$ dovrebbe essere tanto più elevato quanto più elevata è la **rilevanza** delle pagina $j$ a cui punta.
Allora, se in qualche modo fosse suggerito un valore di hub iniziale $h_{i}^{(0)}$, si può raffinare il valore di $a_i$ nel modo seguente
$$
a_{i}^{(1)} =  \sum_{j=1}^{n} M[j,i] \cdot h_{j}^{(0)}
$$
ovvero l'autorità $a^{(1)}_{i}$ è influenzata dall'autorevolezza delle pagine $j$ che la puntano.
Dato che l'indice di autorità è stato raffinato, si può raffinare anche $h_{i}^{(0)}$ allo stesso modo
$$
h_{i}^{(1)} = \sum_{j=1}^{n} M[i,j] \cdot a_{j}^{(1)}
$$
Allora per raffinare gli indici di autorità e di hub si può applicare il seguente metodo iterativo, ponendo in maniera convenzionale che $h_{i}^{(0)} = 1$ per ogni $i$ o, in forma vettoriale, $h^{(0)} = \underline{1}$:
$$
\begin{align}
a_{i}^{(k+1)} &= \sum_{j=1}^{n} M[j,i] \cdot h_{j}^{(k)} \\
h_{i}^{(k+1)} &= \sum_{j=1}^{n} M[i,j] \cdot a_{j}^{(k+1)}
\end{align}
$$
oppure, in forma matriciale
$$
\begin{align}
a_{i}^{(k+1)} &= M^T \cdot h^{(k)} \\
h_{i}^{(k+1)} &= M \cdot a^{(k+1)}
\end{align}
$$
L'obiettivo è ottenere ad ogni iterazione valori di autorità e di hub che descrivano meglio, rispetto all'iterazione precedente, la rilevanza di una pagina nella ricerca. Dunque si cerca una sorta di "convergenza" ai valori reali di autorità e di hub.  

Si osserva che non si può parlare di convergenza vera e propria in quanto ogni volta sommiamo valori non negativi. Perciò si considera una convergenza in presenza di una **opportuna normalizzazione**.
In effetti, vale il seguente teorema.

Comunque si scelga un vettore $h^{(0)}$ con valori positivi, esistono un valore $c \in \mathbb{R}^+$ e un vettore $z \in \mathbb{R}^n$ non nulla tali che
$$
\lim_{k \rightarrow \infty} \frac{h^{(k)}}{c^k} = z
$$

Prima di dimostrare il teorema, qualche richiamo di algebra lineare.
## Richiamo algebra lineare
### Definizioni
#### Autovalori e autovettore
Data una matrice quadrata $A \in \mathbb{C}^{n \times n}$, un vettore $x \in \mathbb{C}^{n}$ e un numero $\lambda \in \mathbb{C}$ non nullo sono rispettivamente **autovettore** e **autovalore** della matrice $A$ se
$$
Ax = \lambda x
$$
#### Base
Una base per uno spazio vettoriale $\mathbb{R}^n$ è un insieme di $n$ vettori $z_{1},\dots,z_{n} \in \mathbb{R}^{n}$ tali che ogni altro vettore $x \in \mathbb{R}^{n}$ può essere espresso come una **combinazione lineare** della base, ovvero
$$
\forall x \in \mathbb{R}^n, \exists p \in \mathbb{R}^n : x = \sum_{i = 1}^{n} p_i z_i
$$
#### Vettore normale
Dato un vettore $z \in \mathbb{R}^{n}$ esso si dice **normale** se
$$
z \cdot z = z_1^2 + z_2^2 + ... + z_n^2 = 1
$$
#### Vettore ortogonale
Dati due vettori $z_{i}, z_{j} \in \mathbb{R}^{n}$ essi si dicono **ortogonali** se
$$
z_i \cdot z_j = z_{i1}z_{j1} +  z_{i2}z_{j2} + ... +  z_{in}z_{jn} = 0
$$
#### Base ortonormale
Data una base $z = (z_1, ..., z_n) \in \mathbb{R}^{n \times n}$ essa è detta **ortonormale** se tutti i vettori sono normali e reciprocamente ortogonali, ovvero se
$$
\begin{align*}
  \forall i = 1, ..., n &: z_i \cdot z_i = z_{i1}^2 + z_{i2}^2 + ... + z_{in}^2 = 1\\
  \forall i \neq j &: z_i \cdot z_j = z_{i1}z_{j1} +  z_{i2}z_{j2} + ... +  z_{in}z_{jn} = 0
\end{align*}
$$
#### Matrice Semidefinita positiva
Una matrice $A$ di dimensione $n \times n$ è detta **semidefinita positiva** se
$$
\forall x \in \mathbb{R}^n, x^TAx \geqslant 0
$$
si osserva che $x^TAx$ è un numero reale.
### Teoremi utili
#### Teorema A
Se $A$ è una matrice reale e simmetrica di dimensione $n \times n$, allora $A$ ha $n$ autovettori e $n$ autovalori reali e, inoltre, gli autovettori formano una base ortonormale per $\mathbb{R}^n$.
#### Teorema B
Se $A$ è una matrice semidefinita positiva, allora:
1. Gli autovalori di $A$ sono non negativi;
2. se $A$ è non nulla, allora almeno un autovalore $\lambda$ di $A$ è strettamente positivo.
#### Teorema C
Se $A$ ha solamente elementi reali, allora $AA^T$ è una matrice **simmetrica** e **semidefinita positiva**.
## Dimostrazione teorema
Prima di procedere con la dimostrazione, si osserva che:
- la matrice $(MM^T)$ è reale e simmetrica $$ (MM^T)_{ij} = \sum_{k=1}^{n} M_{ik} M_{kj} = (MM^T)_{ji} $$ allora, $(MM^T)$ ha una base ortonormale di autovettori e tutti gli autovalori reali;
- la matrice $(MM^T)$ è semidefinita positiva: comunque scelto $x \in \mathbb{R}^n$ si ha $x^T(MM^T)x = (x^TM)(M^Tx)$ e dunque ponendo $(M^Tx) = \underbrace{y}_{1 \times n}$ si ottiene $$ x^T(MM^T)x = y^Ty = \sum_{k=1}^n y^2_{k} \geqslant 0 $$
- poiché $(MM^T)$ è non nulla, il suo autovalore massimo è strettamente positivo

Si riscrivono le formule per calcolare $h^{(k)}$ nel modo seguente
$$
\begin{align*}
  h^{(1)} &= M a^{(1)} = MM^T h^{(0)}\\
  h^{(2)} &= M a^{(2)} = MM^T h^{(1)} = (MM^T)(MM^T) h^{(0)} = (MM^{T})^2 h^{(0)}\\
  h^{(3)} &= M a^{(3)} = MM^T h^{(2)} = (MM^T)(MM^T)^2 h^{(0)} = (MM^{T})^3 h^{(0)}\\
  &\vdots\\
  h^{(k)} &= M a^{(k)} = MM^T h^{(k-1)} = (MM^{T})^k h^{(0)}
\end{align*}
$$
Dato che la matrice di adiacenza $M$ ha valori in $\{ 0,1 \}$, allora $MM^T$ è una matrice **simmetrica**, **semidefinita positiva** e **non nulla**, questo per il teorema C.
Essendo nelle ipotesi del teorema A, la matrice $MM^T$ ha $n$ autovettori reali $z_{1},z_{2},\dots,z_{n} \in \mathbb{R}^n$ distinti, i quali formano una base **ortonormale** per $\mathbb{R}^n$.
Per il teorema B, ogni autovettore $z_{i}$ di $MM^T$ ha un relativo autovalore **reale non negativo** $c_{i} \in \mathbb{R}$. Senza perdita di generalità si assuma che tali autovalori rispettino il seguente ordine
$$
c_{1}\geqslant c_{2} \geqslant \dots \geqslant c_{n}
$$
Allora sempre per il teorema B esiste almeno un autovalore strettamente positivo, dunque $c_{1} > 0$.

Dato che gli autovettori $z_{1},\dots ,z_{n}$ formano una base di $\mathbb{R}^n$, si può scrivere $h^{(0)}$ come combinazione lineare di questi ultimi
$$
h^{(0)} = \sum_{i=1}^n q_{i}z_{i}
$$
e dunque
$$
\begin{align}
h^{(k)} &= (MM^T)^k h^{(0)} = (MM^T)^k \sum_{i=1}^n q_{i}z_{i} \\
&= (MM^T)^{k-1} \sum_{i=1}^n q_{i}(MM^T)z_{i} = (MM^T)^{k-1} \sum_{i=1}^n q_{i}c_{i}z_{i} \\
&= (MM^T)^{k-2} \sum_{i=1}^n q_{i}c_{i}(MM^T)z_{i} = (MM^T)^{k-2} \sum_{i=1}^n q_{i}c_{i}^2z_{i} \\
&\vdots \\
&= \sum_{i=1}^n q_{i}c_{i}^kz_{i}
\end{align}
$$
dunque
$$
h^{(k)} = \sum_{i=1}^n q_{i}c_{i}^kz_{i}
$$
ora si normalizza dividendo ambi i membri per $c_{1}^k$, ovvero per l'autovalore massimo, ottenendo
$$
\frac{h^{(k)}}{c_{1}^k} = \sum_{i=1}^n q_{i} \left(\frac{c_{i}}{c_{1}}\right)^k z_{i}
$$
Sia $l\leqslant n$ il primo indice tale che $c_l > c_{l+1}$, ovvero tale che
$$
c_{1}=c_{2}=\dots=c_{l} > c_{l+1} \geqslant\dots\geqslant c_{n}
$$
allora per ogni $i\leqslant l$ vale che $\left( \frac{c_{i}}{c_{1}} \right)^k = 1$, e per ogni $i>l$ vale che $\left( \frac{c_{i}}{c_{1}} \right)^k < 1$ dunque vale che
$$
\begin{align}
\lim_{ k \to \infty } \frac{h^{(k)}}{c_{1}^k} &= \lim_{ k \to \infty } \sum_{i=1}^n q_{i} \left(\frac{c_{i}}{c_{1}}\right)^k z_{i} \\
&= \lim_{ k \to \infty } \left(q_{1}z_{1} + \dots q_{l}z_{l} + q_{l+1}\overbrace{\left(\frac{c_{l}}{c_{1}}\right)^k}^{\to 0}  z_{l+1} + \dots + q_{n}\overbrace{\left(\frac{c_{n}}{c_{1}}\right)^k}^{\to 0} z_{n}\right) \\
&= q_{1}z_{1}+\dots+q_{l}z_{l}
\end{align}
$$
resta da dimostrare che $q_{1}z_{1}+\dots+q_{l}z_{l}$ è un vettore non nullo, ossia ha almeno una componente non nulla: si dimostra solo nel caso particolare $l=1$, ossia $c_{1}>c_{2}$, ovvero quando vale
$$
\lim_{k \rightarrow \infty} \frac{h^{(k)}}{c_1^k} = \lim_{k \rightarrow \infty} \frac{(MM^T)^k h^{(0)}}{c_1^k} = q_1z_1
$$
Per concludere la dimostrazione è sufficiente mostrare che, comunque si scelga un vettore $h^{(0)}$ a coordinate positive, $q_{1} \neq 0$. Infatti, $z_{1}$ è un vettore reale perché elemento di una base di $\mathbb{R}^n$, e non nullo in quanto la base è ortonormale. Se fosse nullo, avremo che $z_{1}$ non sarebbe normale, ovvero $z_{1}= 0 \implies z_{1}z_{1}=0\cdot 0 \neq 1$.
Si osserva inoltre che $q_{1}=h^{(0)}z_{1}$, infatti
$$
\begin{align}
h^{(0)}z_{1} &= \left(\sum_{i=1}^{n} q_{i}z_{i}\right)z_{1} = \sum_{i=1}^{n} q_{i}\left(z_{i}z_{1}\right) \\
&= q_{1}\underbrace{(z_{1}z_{1})}_{1}+q_{2}\underbrace{(z_{2}z_{1})}_{0}+\dots+q_{n}\underbrace{(z_{n}z_{1})}_{0} = q_{1}
\end{align}
$$
dunque $q_{1}\neq 0$ se e solo se $h^{(0)}z_{1}\neq 0$.
Si dimostra quindi che comunque si sceglie $h^{(0)}$ a coordinate positive, risulta $h^{(0)}z_{1}\neq 0$.
### Parte 1
Si dimostra ch esiste un vettore $y$ a coordinate positive tale $y\cdot z_{1}\neq 0$. Si supponga per assurdo che per ogni vettore $x \in \mathbb{R}^n$ a coordinate positive vale che $xz_{1}=0$. Si sceglie ora un qualsiasi vettore $y$ a coordinate positive $y_{i}>0$ per ogni $i=1,\dots,n$. Sia, per ogni $l=1,\dots,n, \ y^{[l]}$ il vettore tale che $y^{[l]}_{i} = y_{i}$ per ogni $i\neq l$ e $y^{[l]}_{l} = y_{i}+1$.
Dato che si assume che per ogni vettore $x$ a coordinate positive vale $xz_{1}=0$, allora il seguente sistema è soddisfatto per ogni $l=1,\dots,n$
$$
\begin{cases}
  y \cdot z_1 &= 0\\
  y^{\left[ \ell \right]} \cdot z_1 &= 0
\end{cases}
$$
allora si ha
$$
\begin{cases}
  y_1 \cdot z_{11} +  y_2 \cdot z_{12} + ... +  y_{\ell} \cdot z_{1\ell} + ... +  y_n \cdot z_{1n} &= 0\\
  y_1 \cdot z_{11} +  y_2 \cdot z_{12} + ... +  (y_{\ell} + 1) \cdot z_{1\ell} + ... +  y_n \cdot z_{1n} &= 0\\
\end{cases}
$$
e sottraendo la prima equazione con la seconda, si ottiene che $z_{1l}=0$, ossia per ogni $l=1,\dots,n$ vale che $z_{1l}=0$: questo è assurdo in quanto $z_1$ è un vettore reale e non nullo.
### Parte 2
In conclusione si mostra che per ogni vettore $x$ a coordinate positive vale che $xz_{1}\neq 0$.
Sia $y \in \mathbb{R}^n$ un vettore a coordinate positive tale che $yz_{1}\neq0$ (esiste per la parte 1); si esprime $y$ come combinazione lineare della base ortonormale composta dagli autovettori di $(MM^T)$
$$
y=p_{1}z_{1}+\dots+p_{n}z_{n}
$$
allora l'espressione $\frac{(MM^T)^ky}{c_{1}^k}$ converge a $p_{1}z_{1}$ e poiché l'espressione $\frac{(MM^T)^ky}{c_{1}^k}$ contiene solo valori non negativi allora anche i valori in $p_{1}z_{1}$ sono tutti non negativi. Poiché $yz_{1}\neq 0$ e $p_{1}=yz_{1}$ allora $p_{1}z_{1}$ ha almeno una coordinata strettamente positiva. Ricapitolando
- $p_{1} \neq 0$ dato che $yz_{1}\neq 0$;
- il vettore $p_{1}z_{1}$ ha almeno una coordinata strettamente positiva e tutte le altre coordinate non negative
Sia ora $x=(x_{1},\dots,x_{n})$ un qualunque vettore in $\mathbb{R}^n$ a coordinate positive e tale che $x\neq y$. Allora nell'espressione
$$
p_{1}z_{1}x = p_{1}z_{1}x_{1}+p_{1}z_{12}x_{2}+\dots+p_{1}z_{1n}x_{n}
$$
tutti gli addendi sono non negativi e almeno uno di essi è positivo. Quindi
$$
xz_{1}=z_{1}x=\frac{1}{p_{1}}(p_{1}z_{1}x_{1}+p_{1}z_{12}x_{2}+\dots+p_{1}z_{1n}x_{n}) \neq 0
$$
## Considerazioni
Nel teorema precedente si è dimostrato che la quantità $\frac{h^{(k)}}{c_{k}^1}$ tende ad un vettore $z$ non nullo. Si è dimostrato solamente nel caso $c_1>c_2$, però con tecniche analoghe si può dimostrare in qualsiasi caso generale.  

In altre parole, il precedente teorema dice che, nel caso in cui $c_1>c_2$, qualunque sia il vettore di hub iniziale $h^{(0)}$ purché a coordinate **positive**, si ha che $\frac{h^{(k)}}{c_{k}^1}$ convergerà al vettore $q_{1}z_{1}$, dove $q_{1}=h^{(0)}z_{1}$. Ciò significa che comunque sia scelto un vettore di hub iniziale, esso convergerà ad un altro vettore **parallelo** all'autovettore $z_{1}$. Perciò la convergenza del vettore di hub dipende solamente dalla matrice $M$.  

Ricapitolando, l'algoritmo **HITS** valuta la rilevanza di una pagina presente nel sottoinsieme di pagine inerenti alla ricerca rispetto a due ruoli:

- rispetto al ruolo di **autorità** che indica la rilevanza della pagina rispetto alla ricerca, e tale quantità è determinata mediante lo studio degli link **entranti** nella pagina.
- rispetto al ruolo di **hub** che indica quanta rilevanza da tale pagina alle pagine che essa punta, e tale quantità è invece determinata mediante lo studio dei suo link **uscenti**.

Ciò significa che si può incorrere in situazioni in cui una pagina poco rilevante ai fini di una ricerca conferisca invece rilevanza ad altre pagine.  

In poche parole l'intuizione alla base di _Hubs & Authorities_ si basa sull'idea che le pagine svolgano molteplici ruoli nella rete e, in particolare, che le pagine possano svolgere un potente ruolo di **approvazione** senza che esse siano fortemente sostenute. Perciò l'algoritmo `HITS` si presta bene a modellare tutte quelle situazioni in cui le pagine sono naturalmente partizionate in due sottoinsiemi **semanticamente** distinti. Per esempio quando si ricercano degli oggetti da comprare, le pagine dei rivenditori non si puntano a vicenda in quanto sono concorrenti: se si puntano allora si sta dando rilevanza al concorrente. Viceversa, alcuni siti come eBay puntano ad altri rivenditori.
# PageRank
Mentre con l'algoritmo HITS la rilevanza di una pagina dipende da quante pagine è puntata e a quante punta, esistono contesti in cui la rilevanza non dipende dal numero bensì dalla **qualità** dei link. Ovvero ci sono situazioni in cui l'approvazione è vista come un passaggio diretto da una pagina importante ad un'altra. Per esempio, se un articolo `X` molto importante cita un articolo `Y`, allora `Y` prenderà rilevanza. Simmetricamente, se `X` cita molti articoli importanti allora probabilmente il suo contenuto sarà rilevante per la ricerca. L'algoritmo **PageRank** modella bene queste situazioni.
Tale algoritmo è ancora una volta un metodo iterativo, basato però sull'analisi dei soli link entranti in una pagina. Ovvero solamente i link che puntano ad una pagina `X` saranno utili per definirne la sua rilevanza.
Tale algoritmo parte dall'assunzione che nella porzione di rete attinente alla ricerca sia presente una unità di **flusso** inizialmente distribuita in maniera equa fra tutti i nodi, ovvero se ci sono $n$ allora inizialmente ogni nodo possiede frazione $\frac{1}{n}$ di flusso.
Intuitivamente, durante le iterazioni dell'algoritmo, ogni nodo redistribuisce la propria frazione di flusso alle pagine alle quali punta. Alla fine, dopo un certo numero di iterazioni, la pagina che avrà una frazione più alta sarà la pagina più rilevante per la ricerca

Formalmente, si indica con
$$
f^{(0)}_i = \frac{1}{n}
$$
la quantità iniziale di flusso del nodo $i$ per ogni $i =1,\dots,n$. Ad ogni passo, ogni nodo redistribuisce la propria quantità di flusso lungo i suoi archi uscenti in modo **uniforme**. Perciò si ha che
$$
f_{i}^{(k+1)} = \sum_{1\leqslant j \leqslant n: j\to i} \frac{f_{j}^{(k)}}{d_{j}^{out}}
$$
dove $d_{j}^{out}$ indica il grado uscente del nodo $j$, ovvero il numero di nodi puntati da $j$ all'interno del sottografo indotto dalle sole pagine inerenti alla ricerca.
Si indica con $f^{(k)} = (f_{1}^{(k)},\dots,f_{n}^{(k)})$ il vettore delle quantità di flusso degli $n$ nodi al tempo $k$.

```ad-Teorema
title: Teorema
Se il sottografo indotto dalle pagine attinenti alla ricerca è fortemente connesso allora esiste ed è unico il limite
$$
\lim_{k \rightarrow \infty} f^{(k)} = f^*
$$
```

Sotto l'assunzione di connessione forte, si osserva che ad ogni iterazione $k$ la quantità di flusso totale presente nel grafo è sempre pari ad 1. Allora si può pensare al vettore limite $f^*$ come una **configurazione di equilibrio** in cui per ogni $i=1,\dots,n$ vale che
$$
f_{i}^* = \sum_{1 \leqslant j \leqslant n: j\to i} \frac{f_{j}^*}{d_{j}^{out}}
$$
![](Pasted%20image%2020240910122458.png)

La condizione che il grafo sia fortemente connesso è **necessaria** per il corretto funzionamento, in quanto se non fosse fortemente connesso tutto il flusso andrebbe a finire tutto in **componenti pozzo**.

Dunque è necessario modificare opportunamente il PageRank per ch evitare tutto il flusso si accumuli in componenti pozzo. Una versione modificata è il **Scaled PageRank**, nel quale ogni pagina preserva un propria porzione di flusso per evitare che esso si accumuli nei _"vicoli ciechi"_.
Formalmente, nello scaled pagerank viene fissato un parametro $s\in[0,1]$ e, ad ogni iterazione:
- una frazione $s$ del flusso di ogni nodo viene redistribuito uniformemente sugli archi uscenti come nel PageRank base;
- la frazione $(1-s)$ di ogni nodo viene ridistribuita su tutti i nodi del grafo in modo uniforme.
Dunque, dato che il flusso globale di tutta la sottorete è sempre 1, ad ogni iterazione ogni nodo riceve una quantità di flusso almeno pari a $\frac{1−s}{n}$.

Sia $r_{i}^{(k)}$ la quantità di **rank** posseduta dal nodo $i$ all'iterazione $k$ del pagerank scalato. Allora
$$
r_{i}^{(k+1)} = \left(\sum_{1 \leqslant j \leqslant n: j\to i} s \frac{r_{j}^{(k)}}{d_{j}^{out}}\right) + \frac{1-s}{n} 
$$
Dato un vettore di ranking $r^{(k)} = (r^{(k)}_{1},\dots,r^{(k)}_{n})$ al tempo $k$, si definisce con $N$ la matrice $n \times n$ che descrive il processo iterativo, ovvero tale che
$$
r^{(k+1)}=Nr^{(k)}
$$
Per definizione la matrice $N$ è la seguente: per ogni $1\leqslant i,j \leqslant n$:
$$
N [ i,j ] = \begin{cases}
    \frac{s}{d^{(out)}_j} + \frac{1-s}{n} &\mbox{se } j \rightarrow i\\
    \frac{1-s}{n} &\mbox{altrimenti}
  \end{cases}
$$
allora il ranking $i$-esimo di $r^{(k+1)}$ sarà
$$
\begin{align*}
  r^{(k+1)}_{i} = \sum_{j = 1}^{n} N [ i,j ] r^{(k)}_j &= \left( \sum_{1 \leq j \leq n :\\ j \rightarrow i} \left( \frac{s}{d^{(out)}_j} + \frac{1-s}{n} \right) r^{(k)}_j \right) + \left( \sum_{1 \leq j \leq n :\\ j \not\to i} \frac{1-s}{n}r^{(k)}_j \right)\\
  &= \left( \sum_{1 \leq j \leq n :\\ j \rightarrow i} \frac{s}{d^{(out)}_j}r^{(k)}_j \right) + \left( \sum_{j = 1}^{n} \frac{1-s}{n}r^{(k)}_j \right)\\
  &= \left( \sum_{1 \leq j \leq n :\\ j \rightarrow i} \frac{s}{d^{(out)}_j}r^{(k)}_j \right) + \frac{1-s}{n}\underbrace{\left( \sum_{j = 1}^{n} r^{(k)}_j \right)}_{\scriptsize{\mbox{tutto il flusso}}}\\
  &= \left( \sum_{1 \leq j \leq n :\\ j \rightarrow i} \frac{s}{d^{(out)}_j}r^{(k)}_j \right) + \frac{1-s}{n}
\end{align*}
$$
ovvero la definizione data all'inizio.
Si osserva che ad ogni iterazione $k$ la quantità di flusso totale presente nel grafo è sempre pari ad 1. Allora si può pensare al vettore limite $r^*$ come una **configurazione di equilibrio**, ovvero tale che
$$
r^*=Nr^*
$$
Si osserva che il vettore $r^*$, se esiste, è un autovettore di $N$ con le seguenti proprietà:
- il rispettivo autovalore è $\lambda=1$;
- la somma degli elementi di $r^{*}$ è pari a 1;
- ha tutti elementi **non negativi**;
- possibilmente che $r^{*}$ è l'**unico** autovettore con tali proprietà, cosi da avere un unico punto di convergenza

L'esistenza di tale autovettore è conseguenza dei seguenti teoremi.
```ad-Teorema
title: Teorema di Perron
Sia $A$ un matrice $n\times n$ con valori reali positivi, allora

- $A$ ha un autovalore $c \in \mathbb{R}^+$ tale che $c>|c^{'}|$ per ogni altro autovalore $c^{'}$ di $A$.
- l'autovettore di $A$ corrispondente a $c$ è **unico** ed ha elementi **reali positivi** la cui somma è pari a 1.

```
La matrice $N$ rispetta tali condizioni, perciò si può applicare il teorema di _Perron_, dunque esiste una coppia autovalore-autovettore $(r^∗,c)$ che rispetta le proprietà. Dunque se $c=1$ si può concludere che $r^*=Nr^*$.

```ad-Teorema
title: Teorema 
Sia $A$ una matrice stocastica, ovvero una matrice quadrata $n×n$ la cui somma degli elementi su ciascuna riga (o colonna) è pari a 1. Allora $A$ ha un autovalore $\lambda$ tale che $\lambda=1$ e $\lambda$ è l'autovalore di modulo massimo.

```

La matrice $N$ è stocastica per colonne. Infatti, poiché vale che fissato un nodo $j$
$$
\sum_{1 \leq i \leq n :\\ j \to i} \frac{1}{d^{(out)}_j} = 1
$$
allora
$$
\begin{align*}
  \sum_{1 \leq i \leq n} N [ i,j ] &= \left( \sum_{1 \leq i \leq n :\\ j \to i} \frac{s}{d^{(out)}_j} + \frac{1 - s}{n} \right) + \left( \sum_{1 \leq i \leq n :\\ j \not\to i} \frac{1 - s}{n} \right)\\
  &= \left( \sum_{1 \leq i \leq n :\\ j \to i} \frac{s}{d^{(out)}_j} \right) + \left( \sum_{1 \leq i \leq n} \frac{1 - s}{n} \right)\\
  &= s + (1-s) = 1
\end{align*}
$$
In conclusione, per il teorema di _Perron_ e per quello sulle matrici stocastiche, l'algoritmo Scaled PageRank converge ad un **punto fisso** $r^*=Nr^∗$.
## PageRank e Random Walk
Concettualmente, l'idea dietro al PageRank è quella di essere presente nella rete una sorta di fluido che _"scorre"_ tra le pagine inerenti alla ricerca.
Esiste una relazione tra questo concetto di fluido e le cosidette **Random Walk**, o **percorso aleatorio**. Una Random Walk funziona nella seguente maniera:
- dato un grafo $G$ si sceglie uniformemente a caso un nodo $u$ da cui iniziare il percorso.
- al passo successivo si passa da $u$  ad uno dei suoi vicini in maniera del tutto uniforme.
- e così via…
Si calcola la probabilità che la random walk si trovi su un dato nodo $i$ al passo $k$. Formalmente, per ogni nodo $i \in V$ si indica con $w_{i}^{(k)}$ la variabile aleatoria binaria che:
- vale **uno** se al passo $k$ la random walk si trova esattamente sul nodo $i$;
- **zero** altrimenti;
Scegliendo il primo nodo della random walk in modo uniforme, vale che
$$
\mathbf{Pr}(w_{i}^{(0)}=1) = \frac{1}{n} = f_{i}^{(0)}
$$
al passo $k=1$ vale che la probabilità che il cammino aleatorio finisca sul nodo $i$ è pari alla probabilità che un suo vicino $j$ sia il nodo iniziale del cammino, moltiplicata per la probabilità che da $j$ si passi esattamente al nodo $i$.
$$
\begin{align}
\mathbf{Pr}(w_{i}^{(1)}=1) &= \sum_{1 \leqslant j \leqslant n: j\to i} \mathbf{Pr}(w_{j}^{(0)}=1) \frac{1}{d_{j}^{(out)}} \\
&= \sum_{1 \leqslant j \leqslant n: j\to i} \frac{f_{j}^{(0)}}{d_{j}^{(out)}} = f_{i}^{(1)}
\end{align}
$$
Induttivamente per ogni $k>0$
$$
\begin{align}
\mathbf{Pr}(w_{i}^{(k+1)}=1) &= \sum_{1 \leqslant j \leqslant n: j\to i} \mathbf{Pr}(w_{j}^{(k)}=1) \frac{1}{d_{j}^{(out)}} \\
&= \sum_{1 \leqslant j \leqslant n: j\to i} \frac{f_{j}^{(k)}}{d_{j}^{(out)}} = f_{i}^{(k+1)}
\end{align}
$$
Ovvero la quantità di flusso del nodo $i$ dopo $k$ iterazioni è pari alla probabilità di finire esattamente sul nodo $i$ dopo $k$ passi della random walk.

Si consideri ora la versione *scalata* del random walk per il PageRank scalato. Le regole di questa versione del random walk sono:
- fissare un parametro $s \in [0,1]$;
- scegliere *uniformemente a caso* un nodo iniziale $u$ da cui iniziare il percorso;
- dal nodo $u$:
	- con probabilità $s$ si prosegue il random walk scegliendo un vicino di $u$ uniformemente a caso sul quale transitare;
	- con probabilità $1-s$ si ricomincia la random walk, ovvero si sceglie un nodo uniformemente a caso e si inizia un nuovo random walk da esso.
- e cosi via...

Per ogni nodo $i \in V$ si indica con $w_{i}^{(k)}$ la variabile aleatoria binaria tale che:
- vale **uno** se al passo $k$ la random walk si trova esattamente sul nodo $i$;
- **zero** altrimenti;
Scegliendo il primo nodo della random walk in modo uniforme, vale che
$$
\mathbf{Pr}(w_{i}^{(0)}=1) = \frac{1}{n} = r_{i}^{(0)}
$$
per $k=1$ vale
$$
\begin{align}
\mathbf{Pr}(w_{i}^{(1)}=1) &= s \left( \sum_{1 \leqslant j \leqslant n: j\to i} \mathbf{Pr}(w_{j}^{(0)}=1) \frac{1}{d_{j}^{(out)}} \right) + (1-s) \frac{1}{n} \\
&= \sum_{1 \leqslant j \leqslant n: j\to i} \left(s\frac{r_{j}^{(0)}}{d_{j}^{(out)}}\right) + \frac{1-s}{n} = r_{i}^{(1)}
\end{align}
$$
e quindi, più in generale
$$
\begin{align}
\mathbf{Pr}(w_{i}^{(k+1)}=1) &= s \left( \sum_{1 \leqslant j \leqslant n: j\to i} \mathbf{Pr}(w_{j}^{(k)}=1) \frac{1}{d_{j}^{(out)}} \right) + (1-s) \frac{1}{n} \\
&= \sum_{1 \leqslant j \leqslant n: j\to i} \left(s\frac{r_{j}^{(k)}}{d_{j}^{(out)}}\right) + \frac{1-s}{n} = r_{i}^{(k+1)}
\end{align}
$$
Ovvero il rank scalato di una pagina $i$ dopo $k$ iterazioni dell'algoritmo _Scaled PageRank_ equivale alla probabilità di trovarsi esattamente sul nodo $i$ dopo $k$ passi di questo scaled random walk.
# Modern Web Search
L'enorme crescita del Web, in termini di numero di pagine e numero di argomenti trattati, ha richiesto col tempo di raffinare le vecchie tecniche di link analysis e web search. Una tecnica è quella di analizzare i link combinando il **contenuto** delle pagine, per esempio usando gli **anchor text**, ovvero delle parole chiave che descrivono in qualche maniera il contenuto di un link. Oppure ancora considerando con quale frequenza o quante volte una pagina viene effettivamente aperta.  
Spesso ci sono anche motivi commerciali per cercare di apparire primi nei ranking di una ricerca. Infatti è nata una vera e propria industria dietro le tecniche di ranking. Da una parte ci sono i web designer che cercano di comprendere gli algoritmi di ranking usati dai motori di ricerca, e cercano un modo per poterli sfruttare a loro vantaggio. D'altra parte ci sono i progettisti di motori di ricerca che vengono indotti nel cercare di modificare e migliorare i propri algoritmi di ranking. Ecco perché gli algoritmi di ranking effettivamente usati dai motori di ricerca sono spesso tenuti segreti.