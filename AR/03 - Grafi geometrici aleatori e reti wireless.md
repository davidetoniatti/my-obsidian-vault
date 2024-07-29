I modelli finora trattati, ossia il modello di [Erdős-Rényi](01%20-%20Introduzione%20e%20grafi%20di%20Erdős-Rényi.md#Il%20modello%20di%20Erdős-Rényi) e il modello rich-get-richer, si prestano a generare grafi che corrispondono a *reti virtuali*, cioè reti in cui gli archi rappresentano relazioni virtuali fra individui (relazioni di amicizia o collegamenti logici).
Si vogliono ora studiare modelli in grado di rappresentare reti fisiche ( ad esempio un sistema distribuito di calcolatori), per le quali bisogna tenere in considerazione la struttura geometrica dello spazio nel quale i nodi sono inseriti.
## Grafi geometrici
Sia $(\mathbb{R},\mid\mid\cdot\mid\mid)$ uno **spazio metrico** di dimensione $d\geq1$ e sia $r > 0$ una costante detta **raggio**. Un **grafo geometrico** consiste in un insieme $V$ di punti in tale spazio metrico, di cui si conoscono le coordinate; in particolare
- $V \subseteq \mathbb{R}^d$;
- $E := \{(a,b)\in V^2: \mid\mid a - b \mid\mid \leq r\}$
Si assume che lo spazio metrico sia bidimensionale $\mathbb{R}^2$: ciascun punto $a \in V$ è individuato da una coppia di coordinate $a = (x_a,y_a)$ con $x_a, y_a \in \mathbb{R}$. Gli archi del grafo individuato da $V$ ed $r$ sono tutte e sole le coppie di punti la cui distanza euclidea è minore o uguale a $r$
$$
E = \left\{ (a,b): a,b \in V \land \mid\mid a-b \mid\mid \leq r \right\}
$$
dove
$$
\mid \mid a-b\mid\mid= \sqrt{ (x_{a}-x_{b})^2 + (y_{a}-y_{b})^2 }
$$
## Grafi geometrici aleatori
Si definisce un modello per la generazione di grafi geometrici casuali.
Siano $n \in \mathbb{N}$ e $r>0$ due costanti. Un **grafo geometrico aleatorio** $G(n,r)$ è un grafo in cui sono scelti $n$ punti *uniformemente a caso* nel quadrato unitario $[0,1]\times [0,1]$ e per ogni coppia $u,v \in V$ si inserisce l'arco $(u,v)$ in $E$ se e solo se $\mid\mid u-v \mid\mid \leqslant r$.
Si osserva che per $r \rightarrow 0$ il grafo diventa sempre più sparso, mentre per $r = \sqrt{2}$ il grafo risultate sarà *completo*. L'ultima affermazione è vera perché scegliendo i nodi in un quadrato di lato uno, due nodi possono essere distanti <u>al più</u> $\sqrt{2}$, ovvero la lunghezza della diagonale di una quadrato.
Il problema da risolvere, conosciuto con il nome di **minimo raggio di connessione** consiste nello scegliere il più piccolo valore di $r$ in funzione di $n$ (ossia $r=r(n)$) tale che con *buona probabilità* il grafo $G(n,r(n))$ risulti connesso.
### Reti wireless ad-hoc
Una rete wireless ad-hoc è costituita da nodi che dispongono di ricetrasmettitori wireless mediante i quali realizzano comunicazioni peer-to-peer.
Tali nodi sono dislocati in un'area e ciascun ricetrasmettitore può essere configurato in maniera tale da trasmettere ai suoi vicini entro un certo *raggio di trasmissione*: se il trasmettitore $x$ è configurato per trasmettere entro un raggio $r_x$, allora $x$ potrà inviare messaggi a tutti i dispositivi che distano al più $r_x$ da $x$.
In una rete wireless ad-hoc possono avvenire comunicazioni *multi-hop*: dati due nodi i quali non possono connettersi direttamente mediante i propri ricetrasmettitori, questi possono comunicare mediante nodi intermedi, i quali inoltrano il messaggio dalla sorgente sino alla destinazione.
Si osserva che, affinché le informazioni possano circolare ovunque nella rete e raggiungere ciascun nodo, è necessario che la rete sia fortemente connessa: per ogni coppia di nodi $a,b$ deve esistere una sequenza di nodi $u_1,u_2,\dots,u_k$ tale per cui:
- $a$ può trasmettere in un hop ad $u_1$;
- $\forall i \in [u_{k-1}]$, $u_i$ può trasmettere in un hop a $u_{i+1}$;
- $u_k$ può trasmettere in un hop a $b$.
Si vuole studiare quali siano le condizioni necessarie al fine di garantire che una rete di wireless ad-hoc sia connessa.
Si assume che $n$ nodi vengono distribuiti uniformemente a caso nel quadrato unitario $Q\equiv [0,1]^2$. Si assume che tutti i nodi abbiano lo stesso raggio di trasmissione $r$. Quindi, due nodi $a,b \in V$ possono comunicare direttamente se e solo se $\mid\mid a-b\mid\mid \leqslant r$.
Si vuole quindi determinare, sotto queste ipotesi, quale sia il valore minimo di $r$, in funzione di $n$, tale per cui è possibile generare grafi di comunicazione connessi con alta probabilità.
## Formalizzazione del modello
La rete è costituita da $n$ nodi, dove ciascuno dei nodi è un punto di uno spazio euclideo $d$-dimensionale. Il parametro $d$ è detto *dimensione della rete* e si considera $d=2$. Il parametro $n$ è detto ordine della rete. Si indica con $V$ l'insieme dei nodi della rete, con $|V|=n$.
```ad-Definizione
title: Definizione (Nodo e rete stazionaria)
Un nodo $u \in V$ si dice **stazionario** se la sua collocazione fisica non varia nel tempo. Se tutti i nodi di una rete sono stazionari, la rete si dice essere stazionaria.
```
```ad-Definizione
title: Definizione (Configurazione di una rete stazionaria)
La **configurazione** di una rete ad-hoc stazionaria $d$-dimensionale è rappresentata da una coppia $M_d = (V,P)$, dove $V$ è l'insieme dei nodi e $P:V \rightarrow [0,1]^d$ è la funzione di posizionamento. La **funzione di posizionamento** assegna ad ogni nodo in $v$ le coordinate di un punto nel cubo $d$-dimensionale di lato $1$, rappresentando in tal modo la posizione fisica del nodo.
```
```ad-Definizione
title: Definizione (Assegnazione di raggio)
Un'assegnazione di raggio per una rete $d$-dimensionale $M_d=(V,P)$ è una funzione $RA:V \rightarrow (0,r_{\text{max}}]$ che assegna ad ogni elemento di $V$ un valore in $(0,r_{\text{max}}]$. Per ogni nodo $u \in V$, il valore $RA(u)$ rappresenta il raggio di trasmissione di $u$.
```
Il parametro $r_{\text{max}}$ è il massimo raggio di trasmissione dei nodi, e si assume che questo valore sia lo stesso per tutti i nodi della rete. Un'assegnazione di raggio in cui tutti i nodi hanno lo stesso raggio di trasmissione $r$, per qualche $0\leqslant r\leqslant r_{\text{max}}$, prende il nome di **assegnazione omogenea**:
$$
RA(u) = r_{u} = r, \ \ \forall u \in V \ \text{con} \ r \in (0,r_{\text{max}}]
$$
```ad-Definizione
title: Definizione (Grafo di comunicazione)
Data una configurazione $M_d =(V,P)$ e un'assegnazione $RA: V \rightarrow \mathbb{R}^+$ di raggi di trasmissione agli elementi di $V$, il **grafo di comunicazione** indotto da $RA$ su $M_d$ è definito come il grafo $G=(V,E)$, dove $(u,v) \in E$ se e solo se $RA(u) \geqslant d(P(u),P(v))$ dove $d(P(u),P(v))$ denota la distanza tra i punti $u$ e $v$.
```
In altre parole, l'arco $(u,v)$ esiste se e solo se i nodi $u$ e $v$ sono a una distanza non superiore a $RA(i)$.
Il grafo di comunicazione cosi definito è un grafo diretto. Si osserva che, se $\forall u \in V, RA(u) = r$ allora, essendo la distanza una funzione simmetrica, allora l'esistenza dell'arco $(u,v) \in E$ implica l'esistenza dell'arco $(v,u) \in E$ e quindi si può modellare il grafo di comunicazione come un grafo non diretto. Se la funzione $RA$ è costante, ossia assegna lo stesso raggio di trasmissione a tutti i nodi della rete, allora il grafo di comunicazione coincide con il modello Unit Disk Graph. Infine, se le posizioni dei nodi sono scelte in base a qualche distribuzione di probabilità, il modello UDG coincide con quello di grafo geometrico casuale.
### Problema del minimo raggio di trasmissione
Prima di entrare nei dettagli del problema del minimo raggio di trasmissione, si osserva che per garantire che il grafo di comunicazione risulti essere fortemente connesso, è possibile configurare il trasmettitore di ogni nodo ad un raggio di trasmissione pari alla distanza tra tale nodo e il nodo più distante da esso, ossia $\forall u \in V$ si pone
$$
RA(u)=r_{u}=\max \{ d(u,v): c \in V \setminus \{ u \} \}
$$
Cosi facendo non dolo si ottiene un grafo fortemente connesso, ma proprio una *clique*, rendendo non necessaria la comunicazione multi-hop. Ma tale strategia non risulta essere funzionale per via della batteria di capacità limitata presente nei nodi. Dunque, l'obiettivo è assegnare a ciascun nodo un raggio di trasmissione il più piccolo possibile.
```ad-Problema
title: Problema MTR
Dati $n$ nodi distribuiti casualmente e in modo uniforme in $[0,1]^2$, qual è il valore minimo $r^*$ tale che l'assegnazione omogenea di raggio di trasmissione $r^*$ a tutti i nodi induce un grafo di comunicazione che risulti essere connesso con alta probabilità?
```
Si osserva che la soluzione al problema MTR dipende dalle informazioni disponibili riguardo la posizione dei nodi. Se la collocazione dei nodi è nota a priori, il valore minimo del raggio di trasmissione che garantisce la connettività è il più lungo arco del MST del grafo di comunicazione.
Se le posizioni dei nodi non sono note a priori, in $[0,1]^d$ il valore del raggio di trasmissione che garantisce la connettività è $r \approx l\sqrt{ d }$, dove $l$ è il lato del cubo $d$-dimensionale e $d$ è la dimensione della rete.
Facendo riferimento alle osservazioni fatte in precedenza riguardo i grafi geometrici aleatori, si ricorda che la misura del minimo raggio di trasmissione che garantisce connettività della rete dipende dal numero $n$ dei nodi della rete.
Si riformula quindi il problema nel modo seguente: dati $n$ punti distribuiti uniformemente a caso nel quadrato $Q\equiv [0,1]^2$, calcolare il valore minimo di $r(n)$ affinché $G(n,r(n))$ sia connesso.
## Connessione di $G(n,r)$
In questa sezione si dimostra che:
> Sia $r^*(n)$ il minimo valore per $r(n)$ che garantisce, con buona probabilità, ch $G(n,r(n))$ è connesso, allora $r^*(n) \in \Theta\left( \sqrt{ \frac{\ln{n}}{n} } \right)$.

Per dimostrare tale affermazione, è necessario dimostrare due teoremi che mostrano rispettivamente una limitazione **superiore** ed una **inferiore** per il minimo raggio di trasmissione $r^{\star}(n)$.
### Teorema 1: delimitazione superiore
Se $n$ punti sono scelti uniformemente a caso nel quadrato $Q \equiv \left[ 0,1 \right]^2$, allora esiste una costante $\gamma_{1}>0$ tale che se $r(n) \geq \gamma_{1}\left(\sqrt{ \frac{\ln{n}+c}{n} }\right)$ allora $G(n,r(n))$ è connesso con *alta probabilità*.
### Teorema 2: delimitazione inferiore
Per ogni costante $c>0$, se $r(n)\geqslant\sqrt{ \frac{\ln n+c}{n} }$ allora la probabilità che $G(n,r(n))$ sia non connesso è strettamente maggiore di zero.
## Dimostrazione Teorema 1: delimitazione superiore
Sia $k(n)>0$ un intero dipendente da $n$. Si partiziona $Q \equiv [0,1]^2$ in $k^2(n)$ celle, ciascuna di lato $\frac{1}{k(n)}$. Inoltre, si pone $r(n)$ pari alla lunghezza della diagonale di una coppia di celle adiacenti, cioè di una coppia di celle con <u>un lato</u> in comune, ossia
$$
r(n) = \sqrt{ \left(\frac{2}{k(n)}\right)^2 + \left(\frac{1}{k(n)}\right)^2 } = \frac{\sqrt{ 5 }}{k(n)}
$$
Così facendo, ciascun nodo in una qualsiasi cella sarà certamente collegato a <u>tutti</u> i nodi eventualmente contenuti in tutte le celle adiacenti alla sua.
Dunque, se si dimostra che *con alta probabilità* ciascuna cella ha <u>almeno</u> un nodo allora è anche vero che con alta probabilità $G(n,r(n))$ è connesso.
Si dimostra quindi che è possibile scegliere un $k(n)$ in modo tale che, con alta probabilità, nessuna cella è vuota.
Per calcolare la probabilità dell'evento *nessuna cella è vuota*, si calcola la probabilità dell'evento complementare, ossia *esiste almeno una cella vuota*.

Sia $C$ una cella: si calcola una delimitazione superiore alla probabilità che essa sia vuota, cioè $\mathbf{Pr}(C = \emptyset)$.
Per fare tale calcolo, si esprime l'evento $C = \emptyset$ come l'evento
$$
\bigcap_{1 \leqslant i \leqslant n} (i \not\in C)
$$
Quindi vale
$$
\mathbf{Pr}(C = \emptyset) = \mathbf{Pr} \left(\bigcap_{1 \leq i \leq n} (i \not\in C)\right)
$$
Dato che ogni nodo è posizionato in maniera totalmente indipendente dagli altri, e dato che la probabilità dell'intersezione di eventi indipendenti è pari al prodotto dei singoli, vale
$$
\mathbf{Pr} (C = \emptyset) = \mathbf{Pr} \left(\bigcap_{1 \leq i \leq n} (i \not\in C)\right) = \prod_{1 \leq i \leq n} \mathbf{Pr} \left( i \not\in C \right)
$$
Non resta che calcolare la probabilità $\mathbf{Pr}\left( i \not\in C \right)$. Si osserva che la probabilità dell'evento complementare, cioè la probabilità che un nodo $i$ sia scelto all'interno della cella $C$, è pari al rapporto fra l'area di $C$ *(casi favorevoli)* e l'area del quadrato $Q$ *(casi possibili)*. Quindi, dato che l'area di $Q$ è uno, vale
$$
\mathbf{Pr}(i \in C) = \frac{1}{k^2(n)}
$$
e dunque
$$
\mathbf{Pr} (i \not\in C) = 1-\frac{1}{k^2(n)}
$$
allora vale
$$
\begin{align}
\mathbf{Pr} (C = \emptyset) &= \mathbf{Pr} \left(\bigcap_{1 \leq i \leq n} (i \not\in C)\right)  \\
&= \prod_{1 \leq i \leq n} \mathbf{Pr} \left( i \not\in C \right) = \left(1-\frac{1}{k^2(n)}\right)^n
\end{align}
$$
Calcolata la probabilità che la cella $C$ sia vuota, si vuole ora calcolare la probabilità dell'evento che esista <u>almeno</u> una cella vuota, ovvero
$$\mathbf{Pr} (\exists C : C = \emptyset) = \mathbf{Pr} \left(\bigcup_{C \in Q} \lbrace C = \emptyset \rbrace\right)$$
applicando lo Union Bound e ricordando che $Q$ è suddiviso in $k^2(n)$ celle, si ottiene
$$\mathbf{Pr} (\exists C : C = \emptyset) \leqslant \sum_{C \in Q} \mathbf{Pr} (C = \emptyset ) = k^2(n) \left(  1 - \frac{1}{k^2(n)} \right)^n$$
Dato che $r(n) = \frac{\sqrt{5}}{k(n)}$, si ottiene
$$\mathbf{Pr}(\exists C : C = \emptyset) \leqslant \frac{5}{(r(n))^2} \left(  1 - \frac{(r(n))^2}{5} \right)^n$$
Ponendo $r(n) = \gamma_1\left( \sqrt{\frac{\ln{n}}{n}} \right)$, si ottiene
$$
\mathbf{Pr} (\exists C : C = \emptyset) \leqslant \frac{5n}{\gamma^2_1 \ln{n}} \left(  1 - \frac{\gamma^2_1 \ln{n}}{5n} \right)^n
$$
Per concludere la dimostrazione, si utilizza il seguente lemma
```ad-Lemma
Per ogni $x \in \mathbb{R}$ vale $1 - x \leqslant e^{-x}$. Inoltre, se $x \neq 0$ allora vale $1 - x < e^{-x}$.
**Dimostrazione**: sia $G(x) = 1-x-e^{-x}$. La derivata prima di $G(x)$ vale
$$ G'(x) = e^{-x}-1 $$
Studiando il segno di $G'(x)$, vale
$$
e^{-x}-1 \geqslant 0 \iff e^{-x} \geqslant 1 \iff e^{-x} \geqslant e^{0} \iff x \leqslant 0
$$
Dunque $G'(x) \geqslant 0$ per $x \leqslant 0$.
Si osserva che $G'(x)$ si annulla solo quando $x=0$, dunque è un punto di massimo assoluto.
Infine, dato che $G(0) = 0$ (e questo è il punto di massimo), vale che $1-x \leq e^{-x}$ per ogni $x \in \mathbb{R}$.
$$G(x) \leq G(0) \implies 1 - x - e^{-x} \leq 0 \implies 1 - x \leq e^{-x}$$
$\blacksquare$
```
In virtù del precedente lemma, si pone $x = \frac{\gamma^2_1 \ln{n}}{5n}$, e dato che $\frac{\gamma^2_1 \ln{n}}{5n} \neq 0$, vale che
$$
1 - \frac{\gamma^2_1 \ln{n}}{5n} < e^{-\frac{\gamma^2_1 \ln{n}}{5n}}
$$
Dunque dopo qualche passaggio algebrico si ottiene
$$
\begin{align*}
    \mathbf{Pr}(\exists C : C = \emptyset)
    &\leq \frac{5n}{\gamma^2_1 \ln{n}} \left(  1 - \frac{\gamma^2_1 \ln{n}}{5n} \right)^n\\
    &< \frac{5n}{\gamma^2_1 \ln{n}} \left( e^{-\frac{\gamma^2_1 \ln{n}}{5n}} \right)^n\\
    &= \frac{5n}{\gamma^2_1 \ln{n}} e^{-\frac{\gamma^2_1 \ln{n}}{5}}\\
    &= \frac{5n}{\gamma^2_1 \ln{n}} n^{-\frac{\gamma^2_1}{5}}\\
    &< \frac{5n}{\gamma^2_1} n^{-\frac{\gamma^2_1}{5}}\\
    &= \frac{5}{\gamma^2_1} n^{1-\frac{\gamma^2_1}{5}}
\end{align*}
$$
Si osserva che l'esponente $1-\frac{\gamma^2_1}{5} < 0$ per $\gamma_1 > \sqrt{5}$.

In conclusione, scegliendo un qualsiasi $\gamma_1 > \sqrt{5}$ e ponendo $b = \frac{5}{\gamma^2_1}$ e $c = - \left( 1-\frac{\gamma^2_1}{5} \right) = \frac{\gamma^2_1}{5} - 1$,  la probabilità che esista almeno una cella vuota è
$$
\mathbf{Pr}(\exists C : C = \emptyset) < \frac{b}{n^c} \in \left( \frac{1}{n} \right)^{\Omega(1)}
$$
quindi con alta probabilità non ci sono celle vuote
$$
\mathbf{Pr}\left( \forall C \subseteq Q \; \exists x \in V : x \in Q \right) > 1 - \left( \frac{1}{n} \right)^{\Omega(1)}
$$
$$\tag*{$\blacksquare$}$$