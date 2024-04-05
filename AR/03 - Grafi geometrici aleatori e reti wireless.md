I modelli finora trattati, ossia il modello di Erdos-Renyi e il modello rich-get-richer, si prestano a generare grafi che corrispondono a *reti virtuali*, cioè reti in cui gli archi rappresentano relazioni virtuali fra individui (relazioni di amicizia o collegamenti logici)
Quando si deve costruire una rete fisica, bisogna tenere in considerazione la struttura geometrica dello spazio nel quale i nodi sono inseriti. Dunque i modelli studiati in questo caso sono poco significativi
## Grafi geometrici
Sia $(\mathbb{R},\mid\mid\cdot\mid\mid)$ uno **spazio metrico** di dimensione $d\geq1$ e sia $r > 0$ una costante detta **raggio**. Un **grafo geometrico** consiste in un insieme $V$ di punti in tale spazio metrico, di cui si conoscono le coordinate; in particolare
- $V \subseteq \mathbb{R}^d$;
- $E := \{(a,b)\in V^2: \mid\mid a - b \mid\mid \leq r\}$
Per il continuo della trattazione, verranno considerati grafi geometrici in uno spazio metrico bidimensionale $\mathbb{R}^2$, dove la norma (o distanza) è quella *euclidea*. Dunque, ciascun punto $a$ viene individuato da una coppia di coordinate $a = (x_a,y_a)$ e gli archi del grafo individuato da $V$ e $r$ sono tutte e sole le coppie di punti la cui distanza euclidea è minore o uguale a $r$
$$
E = \left\{ (a,b): a,b \in V \land \sqrt{ (x_{a}-x_{b})^2 + (y_{a}-y_{b})^2 } \leq r \right\}
$$
## Grafi geometrici aleatori
Siano $n \in \mathbb{N}$ e $r>0$ due costanti.
Un **grafo geometrico aleatorio** $G(n,r)$ è un grafo in cui sono scelti $n$ punti *uniformemente a caso* nel quadrato $[0,1]\times [0,1]$.
Si osserva che per $r \rightarrow 0$ il grafo diventa sempre più sparso, mentre per $r = \sqrt{2}$ il grafo risultate sarà *completo*. L'ultima affermazione è vera perché scegliendo i nodi in un quadrato di lato uno, due nodi possono essere distanti <u>al più</u> $\sqrt{2}$, ovvero la lunghezza della diagonale di una quadrato.
Analogamente al modello di Erdos-Renyi, il problema da risolvere è scegliere il più piccolo valore di $r$ in funzione di $n$ (ossia $r=r(n)$) tale che con *buona probabilità* il grafo $G(n,r(n))$ risulti connesso.
Si vede ora, un ambito di applicazione di questo problema.
### Reti wireless ad-hoc
Si consideri lo scenario seguente. Vengono dislocati in un'area un insieme di dispositivi, ciascuno dotato di un ricetrasmettitore wireless e di una batteria di capacità limitata. Il ricetrasmettitore può essere configurato in modo da trasmettere entro un certo *raggio di trasmissione*: se il trasmettitore $x$ è configurato per trasmettere entro un raggio $r_x$, potrà inviare messaggi solo a dispositivi che distano $\leq r_x$ da esso.
Rappresentiamo tale rete di trasmettitori come un grafo diretto, dove $(x,y)$ è un arco diretto del grafo se e solo se la distanza da $x$ a $y$ è minore o uguale a $r_x$.
![|center|500](03-img01.png)
Questo grafo diretto (a destra in figura) è detto *grafo di comunicazione*.
Inoltre, questo sistema utilizza comunicazione multi-hop: un nodo può trasmettere un messaggio ad un nodo non raggiungibile da lui stesso direttamente per mezzo degli altri nodi. Ad esempio, il nodo $u_1$ può inviare un messaggio ad $u_4$ passando per $u_2$ e $u_3$. In generale, un nodo $x$ può inviare messaggi a un nodo $y$ solo se il grafo di comunicazione contiene un percorso da $x$ a $y$.
Allora tutti i nodi possono comunicare tra di loro se e solo se il grafo di comunicazione è *fortemente connesso*.
Il modo più semplice per ottenere una rete fortemente connessa consiste nel configurare ogni trasmettitore in modo da poter raggiungere il nodo più lontano: detto $V$ l'insieme dei nodi, per ogni $u \in V$ si pone
$$
r_u = \max\{ d(u,v): v \in V - \{ u \} \}
$$
In tale modo il grafo di comunicazione è completo, dunque ogni nodo può inviare messaggi direttamente al destinatario.
Però, l’energia che occorre ad un nodo per trasmettere i suoi messaggi è tanto maggiore quanto più è grande il raggio di trasmissione di quel nodo, e i nodi dispongono di batterie a capacità limitata.
Dunque, l'obiettivo è assegnare a ciascun nodo un raggio di trasmissione il più piccolo possibile.

Il problema può essere modellato come nell'esempio della precedente sezione, ovvero assumendo che tutti i nodi abbiano uno stesso raggio di trasmissione $r$, e senza perdere di generalità che la regione in cui distribuire i nodi sia un quadrato normalizzato $Q \equiv \left[ 0,1 \right]^2$.
Ovviamente $r$ deve essere una funzione di $n$, in quanto se $r$ fosse fissato la densità del grafo geometrico aleatorio risultante dipenderebbe da $n$ (molto probabilmente connessa quando $n$ molto grande, molto probabilmente non connessa quando $n$ molto piccolo).
Invece si vuole che al variare di $n$ il grafo $G(n,r(n))$ sia sempre fortemente connesso (con buona probabilità).

Si osserva inoltre che se $r$ è uguale per tutti i nodi allora non ha senso parlare di grafo diretto, in quanto se un nodo $x$ riesce a trasmettere direttamente a un nodo $y$ allora anche $y$ può trasmettere in modo diretto a $x$.

Formalmente, la domanda alla quale si vuole dare una risposta è:

> dati $n$ punti distribuiti uniformemente a caso nel quadrato unitario $Q$, calcolare il valore <u>minimo</u> di $r(n)$ tale che $G(n, r(n))$ risulti connesso con buona probabilità.
## Connessione di $G(n,r)$
In questa sezione si dimostra che:

> Detto $r^*(n)$ il minimo valore per $r(n)$ che garantisce, con buona probabilità, ch $G(n,r(n))$  è connesso, allora $r^*(n) \in \Theta\left( \sqrt{ \frac{\ln{n}}{n} } \right)$.

Per dimostrare tale affermazione, è necessario dimostrare due teoremi che mostrano rispettivamente una limitazione **superiore** ed una **inferiore** per il minimo raggio di trasmissione $r^{\star}(n)$.
### Teorema 1: delimitazione superiore
Se $n$ punti sono scelti uniformemente a caso nel quadrato $Q \equiv \left[ 0,1 \right]^2$, allora esiste una costante $\gamma_{1}>0$ tale che se $r(n) \geq \gamma_{1}\left(\sqrt{ \frac{\ln{n}+c}{n} }\right)$ allora $G(n,r(n))$ è connesso con *alta probabilità*.
#### Dimostrazione
Sia $k(n)>0$ un intero dipendente da $n$. Si procede partizionando $Q$ in $k^2(n)$ celle, ciascuna di lato $\frac{1}{k(n)}$. Inoltre, si pone $r(n)$ pari alla lunghezza della diagonale di una coppia di celle adiacenti, cioè di una coppia di celle con <u>un lato</u> in comune, ossia
$$
r(n) = \sqrt{ \left(\frac{2}{k(n)}\right)^2 + \left(\frac{1}{k(n)}\right)^2 } = \frac{\sqrt{ 5 }}{k(n)}
$$
![|center|500](03-img02.png)
In questo modo, ciascun nodo sarà certamente collegato a <u>tutti</u> i nodi nelle celle adiacenti alla sua.
Dunque, se *con alta probabilità* ciascuna cella ha <u>almeno</u> un nodo allora è anche vero che con alta probabilità $G(n,r(n))$ è connesso.
Si dimostra quindi che è possibile scegliere un $k(n)$ in modo tale che, con alta probabilità, nessuna cella è vuota.

Per calcolare la probabilità dell'evento *nessuna cella è vuota*, si calcola la probabilità dell'evento complementare, ossia *esiste almeno una cella vuota*, perché è più facile.

Sia $C$ una cella: si calcola una delimitazione superiore alla probabilità che essa sia vuota, cioè $P(C = \emptyset)$.
Per fare tale calcolo, si esprime l'evento $C = \emptyset$ come l'evento
$$
1 \not\in C \land 2 \not\in C \land \dots \land n \not\in C
$$
che corrisponde all'intersezione degli eventi $\lbrace i \not\in C \rbrace_{i \in \left[ n \right]}$.
Quindi vale
$$
\mathcal{P}(C = \emptyset) = \mathcal{P}\left(\bigcap_{1 \leq i \leq n} (i \not\in C)\right)
$$
Dato che ogni nodo è posizionato in maniera totalmente indipendente dagli altri, e dato che la probabilità dell'intersezione di eventi indipendenti è pari al prodotto dei singoli, vale
$$
\mathcal{P}(C = \emptyset) = \mathcal{P}\left(\bigcap_{1 \leq i \leq n} (i \not\in C)\right) = \prod_{1 \leq i \leq n} \mathcal{P}\left( i \not\in C \right)
$$
Non resta che calcolare la probabilità $\mathcal{P}\left( i \not\in C \right)$ che un nodo $i$ non "cada" nella cella $C$. Si osserva che la probabilità dell'evento complementare, cioè la probabilità che un nodo $I$ sia scelto all'interno della cella $C$, è pari al rapporto fra l'area di $C$ *(casi favorevoli)* e l'area del quadrato $Q$ *(casi possibili)*. Quindi, dato che l'area di $Q$ è uno, vale
$$
\mathcal{P}(i \in C) = \frac{1}{k^2(n)}
$$
e dunque
$$
\mathcal{P}(i \not\in C) = 1-\frac{1}{k^2(n)}
$$
allora vale
$$
\mathcal{P}(C = \emptyset) = \mathcal{P}\left(\bigcap_{1 \leq i \leq n} (i \not\in C)\right) = \prod_{1 \leq i \leq n} \mathcal{P}\left( i \not\in C \right) = \left(1-\frac{1}{k^2(n)}\right)^n
$$
Calcolata la probabilità che la cella $C$ sia vuota, si vuole ora calcolare la probabilità dell'evento che esista <u>almeno</u> una cella vuota, ovvero
$$\mathcal{P}(\exists C : C = \emptyset) = \mathcal{P}(\bigcup_{C \in Q} \lbrace C = \emptyset \rbrace )$$
Rimarcando il concetto di **Union Bound** che dice che la probabilità dell'unione di eventi è minore o uguale della somma delle loro probabilità si ottiene la seguente delimitazione superiore
$$\mathcal{P}(\exists C : C = \emptyset) \leq \sum_{C \in Q} \mathcal{P}(C = \emptyset ) = (k(n))^2 \left(  1 - \frac{1}{(k(n))^2} \right)^n$$
Dato che $r(n) = \frac{\sqrt{5}}{k(n)}$, si sostituisce $k(n)$ con $\frac{\sqrt{5}}{r(n)}$, ottenendo
$$\mathcal{P}(\exists C : C = \emptyset) \leq \frac{5}{(r(n))^2} \left(  1 - \frac{(r(n))^2}{5} \right)^n$$
A questo punto, si pone $r(n) = \gamma_1\left( \sqrt{\frac{\ln{n}}{n}} \right)$, ottenendo
$$
\mathcal{P}(\exists C : C = \emptyset) \leq \frac{5n}{\gamma^2_1 \ln{n}} \left(  1 - \frac{\gamma^2_1 \ln{n}}{5n} \right)^n
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
    \mathcal{P}(\exists C : C = \emptyset)
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
\mathcal{P}(\exists C : C = \emptyset) < \frac{b}{n^c} \in \left( \frac{1}{n} \right)^{\Omega(1)}
$$
quindi con alta probabilità non ci sono celle vuote
$$
\mathcal{P}\left( \forall C \subseteq Q \; \exists x \in V : x \in Q \right) > 1 - \left( \frac{1}{n} \right)^{\Omega(1)}
$$
$$\tag*{$\blacksquare$}$$
### Teorema 2: delimitazione inferiore
Per ogni costante $c>0$, se $r(n) =\sqrt{ \frac{\ln{n}+c}{n\pi} }$ allora
$$
\lim_{ n \to \infty } \mathcal{P}(G(n,r(n)) \text{ è non connesso}) > 0
$$
Per semplicità, nel corso della trattazione si dentoerà con $G$ il grafo $G(n,r(n))$ e con $r$ il valore $r(n)$.
#### Dimostrazione
Per dimostrare che $r^*(n) \geq\sqrt{ \frac{\ln{n}+c}{n\pi} }$, si deve trovare una *minorazione* della probabilità che $G$ **non** sia connesso.
A tale scopo, si definiscono i seguenti eventi:
-   $\mathcal{E}_{\geq 1}$ = $G$ contiene **almeno** un nodo isolato.
-   $\mathcal{E}_{i_1,i_2,...,i_h}$ = i nodi $i_1,i_2,...,i_h \in \left[ n \right]$ sono **tutti** isolati in $G$.
-   $\mathcal{E}_{i!}$ = $i \in \left[ n \right]$ è l'**unico** nodo isolato in $G$.
Allora è possibile dare un primo lower bound facendo le seguenti osservazioni:
1. se $G$ contiene almeno un nodo isolato allora certamente $G$ è disconnesso. In termini logici si può scrivere scrivere come $\mathcal{E}_{\geq 1} \implies G \mbox{ disconnesso}$. In termini insiemistici invece l'implicazione si può tradurre come operatore di *sottoinsieme*, ovvero $\mathcal{E}_{\geq 1} \subseteq G \mbox{ disconnesso}$. Perciò vale che $\mathcal{P}\Big(G \mbox{ disconnesso}\Big) \geq \mathcal{P}(\mathcal{E}_{\geq 1})$.
2. Se il nodo 1 è l'unico nodo isolato in $G$, oppure il nodo 2 è l'unico nodo isolato in $G$, oppure ..., oppure $n$ è l'unico nodo isolato in $G$ allora certamente $G$ contiene almeno un nodo isolato, dunque vale $$\bigcup_{i \in \left[ n \right]} \mathcal{E}_{i!} \subseteq \mathcal{E}_{\geq 1}$$ e quindi in termini probabilistici
 $$\mathcal{P}\left( \mathcal{E}_{\geq 1} \right) \geq \mathcal{P}\left( \bigcup_{i \in \left[ n \right]} \mathcal{E}_{i!} \right)$$
 3. Dato che $\mathcal{E}_{1!}, \mathcal{E}_{2!}, ..., \mathcal{E}_{n!}$ sono tutti eventi **disgiunti** (ovvero ne può capitare solo uno tra tutti) allora vale che $$\mathcal{P}\left( \bigcup_{i \in \left[ n \right]} \mathcal{E}_{i!} \right) = \sum_{i \in \left[ n \right]} \mathcal{P}\left( \mathcal{E}_{i!} \right)$$
 Combinando queste tre osservazione, si ottiene il lower bound seguente
$$
\mathcal{P}(G \text{ non connesso}) \geq \sum_{i \in [n]}\mathcal{P}(\mathcal{E}_{i!})
$$
A questo punto, si calcola $\mathcal{P}(\mathcal{E}_{i!})$ trovando un'ulteriore lower bound a tale probabilità.
A tale scopo, si osserva che $i$ è l'**unico** nodo isolato in $G$ se e solo se si verificano i seguenti eventi:
1. $i$ è un nodo isolato in $G$;
2. comunque si sceglie un altro nodo $j$, $i$ e $j$ non sono entrambi isolati in $G$, ovvero $$\mathcal{E}_{i!} \equiv
  \mathcal{E}_{i} \bigcap_{j \in \left[ n \right] \setminus \lbrace i \rbrace} \overline{\mathcal{E}_{i,j}} \equiv
  \mathcal{E}_{i} \setminus \left( \bigcup_{j \in \left[ n \right] \setminus \lbrace i \rbrace} \mathcal{E}_{i,j} \right)$$dove l'ultima uguaglianza è data dalla *legge di De Morgan*.
Da cui si ottiene
$$
\begin{align*}
	\mathcal{P}\left( \mathcal{E}_{i!} \right)
	&= \mathcal{P}\left( \mathcal{E}_{i} \setminus \bigcup_{j \in \left[ n \right] \setminus \lbrace i \rbrace} \mathcal{E}_{i,j} \right)\\
	&\geq \mathcal{P}\left( \mathcal{E}_{i} \right) - \mathcal{P}\left( \bigcup_{j \in \left[ n \right] \setminus \lbrace i \rbrace} \mathcal{E}_{i,j} \right)\\
	&\geq \mathcal{P}\left( \mathcal{E}_{i} \right) - \sum_{j \in \left[ n \right] \setminus \lbrace i \rbrace} \mathcal{P}\left( \mathcal{E}_{i,j} \right)
\end{align*}
$$
Per calcolare quest'ultima probabilità si devono ancora ricavare delle limitazioni.
Più precisamente, se si vuole minorare, si deve:
1.  trovare una **minorazione** per $\mathcal{P}\left( \mathcal{E}_{i} \right)$
2.  trovare una **maggiorazione** per $\sum_{j \in \left[ n \right] \setminus \lbrace i \rbrace} \mathcal{P}\left( \mathcal{E}_{i,j} \right)$
##### Minorazione per $\mathcal{P}\left( \mathcal{E}_{i} \right)$
Sia $t_i$ il *punto* di $Q$ in cui si trova il nodo $i$, e sia $C_r(t_i)$ la circonferenza di raggio $r$ centrata in $t_i$.
L'evento $\mathcal{E}_i$ si verifica solo se per ogni nodo $j \neq i$ è vero che $j \not\in C_r(t_i)$.
Quindi fissati $j$ e $t_i$ si ottiene
$$\mathcal{P}\left( j \not\in C_r(t_i) \right) = \frac{|Q - C_r(t_i)|}{|Q|} \geq \frac{|Q| - |C_r(t_i)|}{|Q|} = 1 - \pi r^2$$
Il *maggiore o uguale* è dato perché la circonferenza $C_r(t_i)$ potrebbe essere posizionata ai bordi, e quindi non rientrare in $Q$.
Perciò, **fissato** un $t_i$ cale che
$$\mathcal{P}\left(j \not\in C_r(t_i) \; \forall j \neq i \right) \geq \left( 1 - \pi r^2 \right)^{n-1}$$
La precedente probabilità è relativa a un dato $t_i$ fissato. Si deve però dare una stima per un qualsiasi $i$. Visto che $t_i$ è scelto uniformemente a caso in un intervallo **continuo**, allora la funzione di densità della scelta u.a.r. in $Q$ sarà $f(t) = \frac{1}{|Q|} = 1$.
Perciò la delimitazione inferiore cercata è
$$
\begin{align*}
	\mathcal{P}(\mathcal{E}_i)
	&\geq \int_{t_i \in Q} f(t_i) \left( 1 - \pi r^2 \right)^{n-1} \,dt_i\\
	&= \left( 1 - \pi r^2 \right)^{n-1} \int_{t_i \in Q} \,dt_i\\
	&= \left( 1 - \pi r^2 \right)^{n-1} \cdot |Q|\\
	&= \left( 1 - \pi r^2 \right)^{n-1}
\end{align*}
$$
##### Maggiorazione per $\sum_{j \in \left[ n \right] \setminus \lbrace i \rbrace} \mathcal{P}\left( \mathcal{E}_{i,j} \right)$