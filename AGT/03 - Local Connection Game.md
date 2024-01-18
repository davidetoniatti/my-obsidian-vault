Si studia un altro gioco di formazione di reti, il **local connection game**, dove ciascun giocatore può creare dei collegamenti verso altri giocatori. Un **local connection game** è un gioco che modella quindi la creazione di reti. Informalmente, i giocatori hanno due obiettivi:
- Minimizzare il costo da pagare per la costruzione della rete;
- Assicurarsi che tale rete assicuri un'alta qualità di servizio.
I giocatori sono nodi che:
- Pagano per gli archi che costruiscono;
- Ottengono beneficio da cammini brevi.
# Modello del LCG
I giocatori nel **local connection game** sono identificati con dei nodi in un grafo $G$, sul quale la rete deve essere costruita. Una strategia per il giocatore $u$ è un insieme di archi non diretti che il giocatore $u$ costruirà, i quali avranno tutti $u$ come uno dei due estremi. Dato un vettore di strategia $S$, l'insieme di archi dato dall'unione delle strategie di tutti i giocatori formerà una rete $G(S)$ su i nodi giocatori. In particolar modo, si fa presente che la rete conterrà l'arco $(u,v)$ se questo è comprato da $u$ o $v$ (o entrambi).
Sia $dist_{G(S)}(u,v)$ il cammino minimo tra $u$ e $v$ in $G(S)$. Si utilizzerà la notazione $dist(u,v)$ quando $S$ risulterà essere chiaro dal contesto. Il costo della costruzione di un arco è specificato da un singolo parametro $\alpha$.
Ciascun giocatore ha come obiettivo:
- Rendere corta la distanza verso gli altri nodi;
- Pagare il meno possibile.
Più precisamente, l'obiettivo del giocatore $u$ è quello di minimizzare la somma dei costi e delle distanze definita come
$$
\textnormal{cost}_u(S) = \alpha n_u \ +\ \sum_{v}dist_{G(S)}(u,v) 
$$dove:
- $n_u$ è il numero di archi comprati da $u$.
- $\alpha n_u$ è il costo di costruzione.
- $\sum_{v}dist_{G(S)}(u,v)$ è il costo di utilizzo.
## Esempio
Si consideri la seguente rete, in cui la freccia da un nodo $i$ ad un nodo $j$ indica che il nodo $i$ ha comprato il link diretto verso il nodo $j$. Si considera il nodo $u$, e il numero all'interno di ogni altro nodo indica il numero di archi da utilizzare per raggiungere quel nodo a partire da $u$.
![](03-agt_img07.png)
In questa rete il costo del nodo $u$ è dato da
$$
\text{cost}_{u} = \alpha + (1+1+2+2+3+4) = \alpha+13
$$
se poi $u$ costruisce un arco verso il nodo più distante, si ottiene la seguente rete
![](03-agt_img08.png)
e il costo per il nodo $u$ diventa
$$
\text{cost}_{u} = 2\alpha + (1+1+2+2+1+2) = 2\alpha+9
$$
## Obiettivi analisi
Anche per questo problema, si fa presente che
- Si usa l'equilibrio di Nash come concetto di soluzione.
- Per valutare la qualità generale di una rete, si considera il costo sociale, ossia la somma dei costi di tutti i giocatori.
- Una rete $G(S)$ si dice ottimale o socialmente efficiente se minimizza i costi sociali.
- Un grafo $G=(V,E)$ si dice *stabile* per un valore $\alpha$ se esiste un vettore di strategia $S$ tale per cui $S$ è un equilibrio di Nash per il local connection game ed $S$ forma $G$, ossia $G(S)=G$. 

Si osserva ora che essendo gli archi non diretti, quando un nodo $u$ compra un arco $(u,v)$, quell'arco è disponibile per l'utilizzo anche da $v$ a $u$, e quindi, in particolar modo, risulta essere disponibile per $v$. Quindi, in un equilibrio di Nash, al più uno dei due nodi $u$ e $v$ paga per l'arco $(u,v)$ che li collega. Se così non fosse, uno dei due nodi potrebbe diminuire il costo rinunciando alla costruzione dell'arco, e quindi il profilo di strategia in questione non sarebbe un equilibrio di Nash.

Inoltre si fa presente che, essendo la distanza $dist(u,v)$ pari ad infinito quando $u$ e $v$ non sonno connessi, nell'equilibrio bisogna avere necessariamente un grafo connesso. Quindi, ogni rete stabile deve essere connessa. Il costo sociale di una rete stabile $G$ è
$$
SC(G) = \alpha|E|+\sum_{u,v \in V}dist_{G(S)}(u,v)
$$
ossia la somma dei costi dei giocatori. Si evidenzia che la distanza $dist_{G(S)}(u,v)$ contribuisce alla qualità generale della soluzione due volte, una per $u$ ed una per $v$.

Si vogliono quindi confrontare soluzioni stabili per questo problema con soluzioni ottimali.
In particolar modo:
- Si vuole dare una delimitazione al prezzo della stabilità.
- Si vuole dare una delimitazione al prezzo dell'anarchia.
Cosi facendo si potrà dare una delimitazione alla perdita di efficienza della rete dovuta alla stabilità.
# Struttura delle reti ottime
Si vuole studiare la struttura di una soluzione ottimale in funzione di $\alpha$. Si ricorda che una rete è ottimale se minimizza il costo sociale $SC(G)$.

Si definiscono due tipologie di grafi.
```ad-lemma
title: Definizione (Grafo completo):
Un grafo $G=(V,E)$ si dice completo se $\forall u,v \in V$ tali per cui $u\neq v$, $(u,v) \in E$.
![03-agt_img01|center|500](03-agt_img01.png) 
```
```ad-lemma
title: Definizione (Stella)
Un grafo semplice $G=(V,E)$ con $|V|=n$ si dice stella di ordine $n$ se gode delle seguenti proprietà:
- Esiste un solo $u \in V$ tale per cui $d(u)=n-1$, dove $d(u)$ è il grado del nodo $u$.
- $\forall v \in V, v \neq u$ si ha che $d(v)=1$ e $v$ è adiacente solamente ad $u$.

![03-agt_img02|center|400](03-agt_img02.png)
```
## Reti ottime
La struttura della rete ottima dipende quindi da $\alpha$. Più $\alpha$ è piccolo, più ai nodi conviene comprare tanti archi. Viceversa, più $\alpha$ è grande e più ai nodi conviene comprare pochi archi.
Intuitivamente ci si aspetta quindi la seguente situazione:
- Se $\alpha >> 1$, ovvero se $\alpha$ è grande, allora la rete ottima è il grafo a stella;
- Se $\alpha << 1$, ovvero se $\alpha$ è piccolo, allora la rete ottima è il grafo completo.
Volendo formalizzare tale intuizione è possibile dimostrare il seguente teorema.
### Lemma 1
Se $\alpha \geq 2$ allora ogni stella è una soluzione ottimale, e se $\alpha \leq 2$ allora il grafo completo è una soluzione ottimale.
#### Dimostrazione
Sia $G=(V,E)$ una soluzione ottimale con $|E|=m$ archi.
Si ha che $m \geq n-1$, se cosi non fosse il grafo sarebbe disconnesso, ed avrebbe quindi costo pari ad infinito.
Tutte le coppie ordinate di nodi non adiacenti devono avere una distanza di almeno $2$ tra di essi, e vi sono $n(n-1)-2m$ di tali coppie. Aggiungendo le rimanenti $2m$ coppie con distanza $1$ si ottiene il seguente lower bound per il costo sociale di $G$
$$
\begin{align*}
SC(G)\ &\geq\ \ \alpha m + 2m + 2(n(n-1)-2m)\ \ =\ \ \alpha m + 2n(n-1) - 4m+2m \\
\\
&= (\alpha -2)m + 2n(n-1)\ =\ LB(m)
\end{align*}
$$
Dove $\alpha m$ è il costo necessario per costruire $m$ archi.
Da ciò segue quindi che 
$$
SC(G) \geq LB(m) \geq \textnormal{min}_m LB(m)
$$
Sia una stella che il grafo completo rispettano questo bound.
Si analizzano i due casi separati, a seconda del valore di $\alpha$:
- Se $\alpha \geq 2$, allora $LB(m)$ raggiunge il minimo quando $m=n-1$ e quindi $$SC(G) \geq (\alpha -2)(n-1) + 2n(n-1)$$dove $(\alpha -2)(n-1) + 2n(n-1)$ è il costo sociale di un qualsiasi grafo a stella con $n$ nodi. Dunque se $\alpha \geq 2$, non si può fare di meglio di una stella.
- Se $\alpha \leq 2$, allora per minimizzare $LB(m)$ risulta necessario massimizzare $m$. Dato che il massimo valore assumibile ad $m$ è proprio $m=n(n-1)/2$, il numero degli archi di un grafo completo ad $n$ nodi, si ha che $$SG(C) \geq LB \left( \frac{n(n-1)}{2}\right)$$dove $LB \left( n(n-1)/2 \right)$ è il costo sociale di un grafo completo con $n$ nodi. Dunque se $\alpha \geq 2$, non si può fare di meglio di un grafo completo.
## Reti ottime socialmente stabili
Ci si chiede ora quando, per le due topologie prese in esame, i grafi appartenenti ad esse siano stabili. 
Sia la stella che il grafo completo possono essere ottenuti come un equilibrio di Nash per certi valori di $\alpha$, mostrati nel seguente lemma.
### Lemma 2
Se $\alpha \geq 1$, allora ogni stella è un equilibrio di Nash,  e se $\alpha \leq 1$, allora il grafo completo è un equilibrio di Nash.
#### Dimostrazione
##### Per $\alpha \geq 1$
Sia $S$ un profilo di strategie che costruisce una rete a stella.
Risulta che ogni assegnazione di archi a giocatori incidenti corrisponde ad un equilibrio di Nash.
Per questo risultato è necessario dimostrare una singola soluzione. In particolare, si consideri il profilo di strategia nel quale il giocatore 1, al quale è associato il centro della stella, compra tutti gli archi verso gli altri i restanti $n-1$ giocatori, mentre questi non comprano niente. Il giocatore 1 non ha incentivo a deviare strategia, perché così facendo il grafo risulterebbe disconnesso, portandolo a dover pagare una penalità pari ad infinito.
Ogni altro giocatore può invece deviare da questo profilo di strategia solamente comprando archi. Per ciascuno di essi, aggiungere $k$ archi comporta un risparmio di $k$ in distanza, ma comporta un costo di $\alpha k$, ed essendo $\alpha \geq 1$, non risulta essere una deviazione che migliora il suo payoff. Quindi la stella è un equilibrio di Nash.
##### Per $\alpha \leq 1$
Sia $S$ un profilo di strategie che costruisce un grafo completo, con ogni arco assegnato ad un giocatore incidente. Un giocatore che smette di pagare per un insieme di $k$ archi risparmia $\alpha k$ in costo, ma aumenta la distanza totale di $k$, quindi, questo esito è stabile.
# Upper bound al PoS
Questi particolari equilibri di Nash, assieme alle soluzioni ottimali mostrate, permettono di dare un **upper bound** al prezzo della stabilità.
## Teorema: upper bound al PoS
Se $\alpha \geq 2$ o $\alpha \leq 1$, il prezzo della stabilità è $1$. Per $1 < \alpha <2$, il prezzo della stabilità è al più $4/3$.
### Dimostrazione
#### Per $\alpha \geq 2$ o $\alpha \leq 1$
Il teorema risulta essere vero per il lemma 1 ed il lemma 2.
Infatti, per $\alpha \geq 2$ si ha che ogni stella è sia un equilibrio di Nash, sia una soluzione ottimale, e per definizione, $\mathbf{PoS}=1$.
Analogamente, per $\alpha \leq 1$, si ha che ogni grafo completo è sia un equilibrio di Nash e sia una soluzione ottimale, e per definizione, $\mathbf{PoS}=1$.
#### Per $1 < \alpha < 2$
La stella è un equilibrio di Nash, mentre il grafo completo è la struttura ottima. Per determinare il prezzo della stabilità, bisogna calcolare il rapporto del costo di queste due soluzioni. Il peggior caso per questo rapporto avviene quando $\alpha$ tende ad $1$. Infatti, sia $K_n$ il grafo completo con $n$ nodi e $T$ una stella con $n$ nodi, si ha che
$$
\begin{align*}
\mathbf{PoS}\ &\leq\ \frac{SC(T)}{SC(K_n)}\ =\ \frac{(\alpha-2)(n-1)+2n(n-1)}{\alpha \frac{n(n-1)}{2} + n(n-1)} \textnormal{ [massimizzato per } \alpha \rightarrow 1\textnormal{]}\\
\\
&\leq \frac{-1(n-1)+2n(n-1)}{\frac{n(n-1)}{2} + n(n-1)} = \frac{(n-1)(-1+2n)}{(n-1)(\frac{n}{2}+n)} \\
\\
&= \frac{2n-1}{\frac{3n}{2}} = \frac{4n-2}{3n} < \frac{4}{3}
\end{align*}
$$
## Esercizio
Dimostrare per esercizio che il grafo completo è l'unico equilibrio per $\alpha<1$. Ciò implicherà che il prezzo dell'anarchia è $1$ per questo range. 

**Soluzione**: Se $\alpha <1$, il grafo completo è l'unica rete stabile. Infatti se, per il profilo di strategia $S$, $G(S)$ non generasse un grafo completo, allora esistono due nodi $u,v \in V$ tali per cui $(u,v) \notin E$. Quindi al giocatore $u$ converrebbe deviare strategia pagando l'arco $(u,v)$, in quanto spenderebbe $\alpha$ risparmiando $1$. Dato che per $\alpha < 1$ il grafo completo è anche ottimo, si ha che in questo caso il $\mathbf{PoA}$ del gioco è pari ad $1$.
# Upper bound al PoA
Si vuole dimostrare che il prezzo dell'anarchia del local connection game è al più $\mathcal{O}(\sqrt{\alpha})$.
Tale delimitazione viene ottenuta mediante due passaggi:
1. Si da un bound sul diametro del grafo ottenuto mediante il raggiungimento di una soluzione stabile.
2. Si usa il diametro per dare un bound al costo.
## Definizioni preliminari
Prima di dimostrare l'upper bound al prezzo dell'anarchia, si danno le seguenti definizioni.
```ad-lemma
title: Definizione (Diametro)
Il diametro di un grafo $G=(V,E)$ è la massima distanza tra due nodi in $V$
$$
\textnormal{max}_{u,v\in V} dist(u,v)
$$
```
```ad-lemma
title: Definizione (Cut-edge)
Sia $G=(V,E)$ un grafo. Un arco $e$ è definito cut edge di $G$ se il grafo $G-e=(V,E\backslash\{e\})$ risulta essere disconnesso.
```
Si osserva che ogni grafo ha al più $n-1$ cut edges, dove il grafo con esattamente $n-1$ cut edges è la stella.

## Passo 1: diametro di una rete stabile
Si studia ora il primo passaggio, dimostrando il lemma seguente.
### Lemma 1
Il diametro di una qualsiasi rete stabile è al più 
$$
2\sqrt{\alpha}+1
$$
#### Dimostrazione
Sia $G$ una rete stabile, siano $u,v \in V$ due nodi nel grafo e si consideri l'intero $k$ tale per cui 
$$
2k \leq dist_G(u,v) \leq 2k+1
$$
Graficamente, lo scenario studiato può essere rappresentato come segue, con $v = v_0$.
![03-agt_img03|center](03-agt_img03.png)
Si valuta come cambia il costo di $u$ se quest'ultimo decidesse di comprare l'arco $(u,v)$:
- Il nodo $v_0$ passerebbe da una distanza $\geq 2k$ ad una distanza pari ad $1$, con un risparmio conseguito $\geq 2k-1$.
- Il nodo $v_1$ passerebbe da una distanza $\geq 2k-1$ ad una distanza pari a $2$, con un risparmio conseguito $\geq 2k-3$
Procedendo analogamente per i gli altri nodi, si conclude che
- Il nodo $v_{k-1}$ passerebbe da una distanza $\geq k+1$ ad una distanza pari a $k$, con un risparmio conseguito $\geq 1$.

Si hanno quindi $k$ vertici per i quali la loro distanza da $u$ risulta ridotta.
Sia $r$ il risparmio complessivo, si ha quindi che 
$$
r \geq \sum_{i=0}^{k-1}(2i+1)=k^2
$$

Essendo $G$ una rete stabile per ipotesi, si ha che $\alpha$, il costo per l'acquisto dell'arco $(u,v)$, deve essere maggiore del risparmio complessivo.
Quindi
$$
\alpha \geq r \geq k^2 \iff k \leq \sqrt{\alpha}
$$
E ciò implica che
$$
dist_G(u,v) \leq 2k+1 \leq 2\sqrt{\alpha}+1
$$
## Passo 2: costo sociale di una rete stabile
### Reti stabili e Non-cut edges
Per dimostrare il secondo passo, si fa riferimento a due proposizioni, le quali forniscono una delimitazione al numero di non-cut edges che vengono comprati da un nodo in una rete stabile.
#### Proposizione 1
Sia $G$ una rete con diametro $d$ e sia $e=(u,v)$ un non-cut edge. Allora in $G-e$ ciascun nodo $w$ incrementa la propria distanza da $u$ di al più $2d$.
##### Dimostrazione
Si consideri il seguente albero radicato in $u$, ottenuto mediante BFS su $G$.
![03-agt_img04|center|500](03-agt_img04.png)
Essendo che l'arco $(u,v)$ non è un cut edge, deve esistere un arco $(x,y)$ che attraversa il taglio nell'albero BFS indotto dalla rimozione dell'arco $(u,v)$. Allora si ha che 
$$
\begin{align*}
dist_{G-e}(u,w) &\leq dist_G(u,x)+1+dist_G(y,v)+dist_G(v,w) \\
\\
& \leq d + 1 + d + dist_G(u,w) - 1 \ \ \textnormal{[essendo } dist_G(v,w) = dist_G(u,w) - 1\textnormal{]} \\
\\
& \leq dist_G(u,w)+2d
\end{align*}
$$
e quindi
$$
dist_{G-e}(u,w) \leq dist_G(u,w)+2d
$$
#### Proposizione 2
Sia $G$ una rete stabile e sia $F$ l'insieme dei non-cut edges comprati da un nodo $u$. Allora
$$
|F| \leq \frac{(n-1)2d}{\alpha}
$$
##### Dimostrazione
Si consideri il seguente albero radicato in $u$, ottenuto mediante BFS su $G$.
![03-agt_img05|center|500](03-agt_img05.png)
dove $k=|F|$, ed $n_i$ è il numero dei nodi nel sottoalbero radicato in $v_i$.
Se $u$ rimuove l'arco $(u,v_i)$ risparmia $\alpha$, e facendo riferimento alla proposizione precedente si ha che la sua distanza dai nodi contenuti nel sottoalbero radicato in $v_i$ aumenta al più di $2d$ per ogni nodo. L'incremento totale nel sottoalbero radicato in $v_i$ è quindi $2dn_i$.
Essendo $G$ stabile, si ha che $\alpha \leq 2dn_i$. Estendendo il ragionamento per $i=1,\ldots,k$ e sommando per ogni $i$, si ha che
$$
k\alpha \leq \sum_{i=1}^k 2dn_i \leq 2d(n-1)
$$
e quindi
$$
k = |F| \leq \frac{2d(n-1)}{\alpha}
$$
### Costo sociale di una rete stabile
Si fornisce ora un lemma riguardante il costo sociale di una rete stabile.
#### Lemma 2
Il costo sociale di una qualsiasi rete stabile $G=(V,E)$ con diametro $d$ è al più $\mathcal{O}(d)$ volte il costo sociale ottimo.
##### Dimostrazione
In qualsiasi soluzione stabile, in totale si hanno almeno $n-1$ archi. Se così non fosse, almeno un nodo sarebbe sconnesso, e l'acquisto di un arco verso tale nodo, o di un arco da parte di quel nodo, porterebbe ad un profilo di strategia migliore, rendendo quindi la precedente non stabile.

Nel caso migliore, ogni coppia di nodi si trova a distanza $1$ tra essi. Si ha quindi il seguente lower bound
$$
OPT \geq \alpha(n-1)+n(n-1)
$$
Dove $OPT$ è il costo della rete ottima.
Il costo sociale della rete è invece dato da 
$$
SC(G) = \sum_{u,v \in V}dist_G(u,v) + \alpha \cdot |E|
$$
Si osserva ora che
1.
$$
 \sum_{u,v \in V}dist_G(u,v) \leq d \cdot n(n-1) \leq d \cdot OPT
$$
2.
$$
\begin{align*}
\alpha \cdot |E| &= \alpha \cdot |E_{cut}| + \alpha \cdot |E_{non-cut}| \\
\\
& \leq \alpha(n-1) + \alpha \cdot \frac{n(n-1)2d}{\alpha} \\
\\
&\leq 2d \cdot OPT
\end{align*}
$$ 
Dove il secondo punto è stato ottenuto facendo riferimento alla proposizione 2 e al fatto che un grafo $G$ può avere al più $n-1$ cut edges. Componendo il tutto si ha che 
$$
SC(G) = \sum_{u,v \in V}dist_G(u,v) + \alpha \cdot |E| \leq d\cdot OPT + 2d \cdot OPT = 3d \cdot OPT
$$
## Risultato finale
Facendo riferimento quindi al lemma 1 ed al lemma 2, si dimostra il seguente teorema, che definisce una delimitazione superiore al prezzo dell'anarchia.
### Teorema (Upper bound PoA)
Il prezzo dell'anarchia per il local connection game con parametro $\alpha$ è al più $\mathcal{O}(\sqrt{\alpha})$.
#### Dimostrazione
Sia $G$ una qualsiasi rete stabile. Dal lemma 1 segue che il diametro di $G$ è $\mathcal{O}(\sqrt{\alpha})$. Dal lemma 2 invece si ha che il costo sociale di $G$ è $\mathcal{O}(\sqrt{\alpha})$ volte quello ottimo. Allora, per definizione, si ha che 
$$
\mathbf{PoA} = \mathcal{O}(\sqrt{\alpha})
$$
# La Best Response è NP-Hard
Si mostra ora il seguente teorema riguardante la complessità del calcolo della best response di un dato giocatore:
```ad-theorem
title: Teorema
E' NP-hard, date le strategie degli altri agenti, calcolare la best response di un dato giocatore.
```
Il seguente teorema viene dimostrato mediante una riduzione polinomiale dal problema Dominating Set. 
```ad-lemma
title: Definizione (Dominating Set)
Sia $G=(V,E)$ un grafo non orientato e sia $U\subseteq V$. $U$ è un insieme dominante per $G$ se ogni nodo in $V-U$ ha un vicino in $U$. 
Formalmente, $\forall v \in V-U$, $\exists u \in U$ tale che $(u,v) \in E$.
```
## Problema: Dominating Set
Il problema **dominating set** consiste nel trovare, dato un grafo $G=(V,E)$, un insieme dominante di cardinalità minima.
Formalmente:
- **Input**: Grafo $G=(V,E)$.
- **Soluzione**: $U\subseteq V$ tale per cui $U$ è un insieme dominante.
- **Obiettivo**: Minimizzare la cardinalità di $U$.
## Dimostrazione NP-Hardness
Sia $1 < \alpha < 2$ e si consideri un'istanza $I$ del problema Dominating Set.
![03-agt_img06|center|500](03-agt_img06.png)
Sia $i$ un giocatore e si consideri il grafo dell'istanza $I$ come se fosse il grafo costruito dalle strategie dei restanti giocatori. Si dimostra quindi come, per $1 < \alpha < 2$ il giocatore $i$ ha una strategia che lo porta ad un costo $\leq \alpha k + 2n-k$ se e solo se esiste un Dominating Set di dimensione $\leq k$.
- $(\impliedby)$: Dato un dominating set $U$ di dimensione $|U|\leq k$, se il giocatore $i$ si connette a tutti i nodi del dominating set paga al più $$\alpha \cdot k + 2(n-k)+k = \alpha k + 2n -k$$dove $\alpha \cdot k$ è il costo per la costruzione degli archi e $2(n-k)+k$ è il costo per le distanze.
- $(\implies)$: Sia $S_i$ una strategia di costo $\leq \alpha k + 2n-k$. Se in $G(S_i)$ esiste un nodo $v$ che si trova ad una distanza $\geq 3$ dal giocatore $i$ (sia questo associato al nodo $x$), allora si modifica la strategia del giocatore $i$ aggiungendo l'arco $(x,v)$ ad $S_i$. Essendo che $\alpha < 2$, questa modifica non andrà ad aumentare il costo per $i$. Al termine di ciò, si ha che in $G(S_i)$ tutti i nodi distano da $i$ o di $1$ o di $2$. Sia quindi $U$ l'insieme dei nodi che distano $1$ da $x$. Allora $U$ è un dominating set, e inoltre$$
COST_i(S) = \alpha \cdot |U| + 2n-|U| \leq \alpha \cdot k + 2n-k \iff |U| \leq k$$
Avendo dimostrato entrambi i lati, la dimostrazione è conclusa.