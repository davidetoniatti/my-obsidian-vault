---
data: 2023-12-14
---
Si termina con questa lezione la trattazione della progettazione di meccanismi, discutendo una generalizzazione del problema dell'[*asta a singolo oggetto*](): il problema dell'**asta combinatoria**.
# Combinatorial Auction Game
Nell'asta combinatoria si hanno un insieme di oggetti da vendere, e ogni agente che partecipa all'asta ne vuole un certo sottoinsieme. Il *tipo* (informazione privata) dell'agente $a_i$ è un valore $t_i$ che intuitivamente rappresenta il valore massimo che egli è disposto a pagare per ottenere il proprio bundle di oggetti. Ogni oggetto può essere assegnato ad un solo agente.
Il meccanismo deve decidere come allocare gli oggetti (quindi l'*outcome*) e quanto dovranno pagare gli agenti. Un outcome ammissibile è l'insieme $W \in \mathcal{F}$ dei vincitori dell'asta: un sottoinsieme di agenti che sono accontentati e compatibili tra loro, cioè non hanno oggetti desiderati in comune.
L'obiettivo del meccanismo è quello di ottenere il miglior outcome possibile, cioè quello il cui valore totale è massimo; il valore di un outcome è pari alla somma dei tipi dei vincitori.
Infine, l'obiettivo di ogni agente $a_{i}$ è quello di massimizzare la propria utilità pari a $u_{i} = t_{i} - p$, dove $p$ è il prezzo pagato da $a_i$.

Formalmente,
- In input si hanno un insieme di $n$ agenti e $m$ oggetti indivisibili;
- Ciascun agente $a_i$ vuole un sottoinsieme $S_i$ degli oggetti, e il **tipo** dell'agente è un numero $t_i \in \mathbb{R}$, che rappresenta il valore complessivo degli oggetti in $S_i$ secondo $a_i$.
- Un possibile *outcome* è un sottoinsieme $W \subseteq \{1, \dots ,n\}$ tale che $$\forall i,j \in W: i \neq j, S_{i} \cap S_{j} = \emptyset$$
- Fissato un outcome $W \in \mathcal{F}$, la **valutazione** di $a_i$ per questo è pari a $$v(t_{i},W) = \begin{cases}
t_{i}, \quad i \in W \\
0, \quad \text{altrmenti}
\end{cases}$$
- La **social-choice function** è definita come segue $$ f(t) := \arg\max_{W \in \mathcal{F}} \sum_{i \in W} t_{i}$$ cioè si devono allocare gli oggetti (equivalentemente, scegliere gli agenti da soddisfare) in modo da massimizzare la somma dei tipi reali dei vincitori.

Essendo un gioco, si ricorda che:
- ogni agente (o giocatore) $a_i$ è egoista;
- solo l'agente $a_i$ conosce il proprio tipo $t_i$, mentre $S_i$ è un'informazione pubblica;
- si vuole massimizzare la somma dei tipi dei vincitori rispetto ai tipi reali degli agenti;
- Il meccanismo deve:
	- Chiedere ad ogni agente il tipo (riportato) $v_i$;
	- Calcolare un outcome sui tipi riportati usando un algoritmo $g(\cdot)$;
	- Prendere un pagamento $p_i$ dagli agenti dato da una funzione di pagamento $p(\cdot)$.
 
Non resta che progettare un meccanismo truthful per il problema.
## Meccanismo VCG
Per come è definito il valore reale di un outcome $W$ ammissibile, si ha che
$$ \sum_{i \in W} t_{i} = \sum_{i}v_{i}(t_{i},W) $$
dunque il problema è un **problema utilitario**, ed è possibile applicare il **[meccanismo VCG]** della forma $M= \langle g(r),p(x) \rangle$ con:
- l'algoritmo $g(r)$ definito come $$ g(r) = x^* := \arg\max_{x \in \mathcal{F}} \sum_{j} v_{j}(r_{j},x)$$
- la funzione di pagamento che paga ogni agente $a_i$ la quantità $$ p_{i}(r) := \sum_{j\neq i}v_{j}(r_{j},g(r_{-i})) - \sum_{j\neq i}v_{j}(r_{j},x^*) $$

### Vuoi calcolare l'outcome ottimo? Col cazzo
Si osserva che per poter applicare questo meccanismo è necessario calcolare, dato il vettore $r$ dei tipi riportati, la soluzione ottima $g(r)$.
Questo risulta però essere un problema computazinalmente difficile, come mostra il seguente risultato.
```ad-theorem
title: Teorema 1
Approssimare Combinatorial Auction con un fattore migliore di $m^{1/2-\epsilon}$ è un problema NP-hard per ogni $\epsilon > 0$
```
per dimostrare il teorema si effettua una **riduzione** a partire dal problema **maximum indipendent set**, per il quale vale il seguente risultato di impossibilità.
```ad-theorem
title: Teorema 2 (J. Hastod, 2002)
Approssimare il problema IS con un fattore migliore di $n^{1-\epsilon}$ è un problema NP-hard per ogni $\epsilon > 0$
```
#### Dimostrazione teorema 1
Si mostra una riduzione che trasforma istanze di IS in istanze di CA.
Si consideri una qualsiasi istanza $I = \langle G=(V,E),k \rangle$ del problema IS e si costruisca la relativa istanza $I'$ di CA nel modo seguente:
- ogni *arco* in $E$ diventa un oggetto in $I'$;
- ogni *nodo* in $V$ diventa un agente in $I'$  con:
	- $S_i := \{ \text{oggetti associati ad archi incidenti ad }i \}$;
	- $t_i := 1$.
 
 Allora l'istanza $I'$ di CA ha una soluzione di valore $\geqslant k$ se e soltanto se esiste un Independent Set di size $\geqslant k$ per l'istanza $I$.
 Da questo, segue che una soluzione di valore $k$ per $I'$ (CA) con, per qualche $\epsilon > 0$,
 $$ \frac{\text{OPT}_{\text{CA}}}{k} \leq m^{1/2-\epsilon} $$
 *implicherebbe* l'esistenza di una soluzione di valore $k$ per $I$ con
 $$ \frac{\text{OPT}_{\text{IS}}}{k} = \frac{\text{OPT}_{\text{CA}}}{k} \leq m^{1/2-\epsilon} \leq n^{1-2\epsilon} $$
 in quanto $m \leq n^2$ e quindi 
 $$m^{1/2-\epsilon} \leq (n^2)^{(1/2-\epsilon)} = n^{1-2\epsilon}$$
 $$\tag*{$\blacksquare$}$$
 ---
## Meccanismo One Parameter 
Anche se il meccanismo VCG non è computabile in tempo polinomiale, notiamo che il problema Combinatorial Auction è anche un **problema ad un parametro**. In particolare, è un problema di *binary demand* dato che:
- Il tipo di  $a_i$ è un **singolo parametro** $t_i \in \mathbb{R}$;
- La **valutazione** di $a_i$ dato un *outcome* $W \in \mathcal{F}$ è della forma $$v_{i}(t_{i},W) = w_{i}(W) \cdot t_{i}$$ dove $w_{i}(W) \in \{ 0,1 \}$ è detto **workload** di $a_i$ nell'outcome $W$ e vale $$w_{i}(W) = \begin{cases}
1, \quad i \in W \\
0, \quad \text{altimenti}
\end{cases}$$ cioè pari ad 1 quando l'agente è selezionato in $W$.

Dato che bisogna progettare un algoritmo *monotono* per applicare il meccanismo OP e il problema alla base della combinatorial auction è un problema di *massimizzazione*, si introduce la seguente definizione di algoritmo monotono.
```ad-theorem
title: Definizione
Un algoritmo $g()$ per un problema Binary Demand di massimizzazione è **monotono** se per ogni agente $a_i$ e er ogni scelta fissata degli altri giocatori $r_{-i} = (r_1,\dots,r_{i-1},r_{i+1},\dots,r_{n})$, il *carico di lavoro* $w_i(g(r_{-i},r_i))$ è della forma

![[agt_img01.png|center|500]]

dove $\Theta_i(r_{-i}) \in \mathbb{R}\cup\{+\infty\}$ è detto **valore di soglia**
```

L'obiettivo ora è quello di definire un meccanismo tale che:
- L'algoritmo $g(\cdot)$ è **monotono** e calcola una *buona* soluzione: una soluzione **approssimata**;
- Sia $g(\cdot)$  che $p(\cdot)$ sono **calcolabili** in tempo polinomiale.
### Un algoritmo greedy $\sqrt{m}$-apx
Si definisce il seguente algoritmo *greedy* $g(\cdot)$ approssimante per calcolare l'outcome.
![[agt_img04.png]]
L'algoritmo calcola una soluzione ammissibile, dato che un agente $i$ è inserito nella soluzione se e soltanto se nessuno degli elementi del suo bundle è già stato assegnato a un qualche altro agente. Inoltre si vede facilmente che l'algoritmo calcola la soluzione in tempo polinomiale: l'operazione dominante è l'ordinamento dei valori riportati.

Dunque si deve dimostrare che l'algoritmo è appena descritto sia *monotono* e sia in grado di calcolare effettivamente una soluzione *$\sqrt{m}$-approssimante*. Inoltre, si deve mostrare che è possibile calcolare per ogni $a_i$ il pagamento $p_i$, equivalente al valore della soglia $\theta_{i}(r_{-i})$, in tempo polinomiale.
#### Lemma: l'algoritmo è monotono
```ad-lemma
L'algoritmo $g(\cdot)$ è monotono.
```
##### Dimostrazione
Si deve dimostrare che, fissato un qualsiasi agente $a_i$ che viene selezionato da $g(\cdot)$ , $a_i$ viene comunque selezionato quando si aumenta il suo tipo riportato. Ma questo è vero per come funziona l'algoritmo, in quanto per come vengono ordinati i valori riportati
$$
\frac{v_{1}}{\sqrt{ |S_{1}|}} \geqslant\dots\geqslant \frac{v_{i}}{\sqrt{ |S_{i}|}} \geqslant\dots\geqslant \frac{v_{n}}{\sqrt{ |S_{n}|}} \geqslant
$$
se si aumenta il valore $v_i$, l'agente $a_i$  si può muovere solo a sinistra nell'ordinamento: viene selezionato ancora più velocemente di prima.
 $$\tag*{$\blacksquare$}$$
#### Calcolare i pagamenti
Dato che il problema è di tipo One Parameter Binary Demand, il pagamento per l'agente $a_i$ selezionato dall'algoritmo equivale al valore della soglia $\theta_{i}(r_{-i})$ che determina il valore riportato per cui $a_i$ viene inserito tra i vincitori.

Per calcolare il pagamento $p_i$ dell'agente $a_i$, si consideri l'ordinamento dell'algoritmo senza $i$
$$
\frac{v_{1}}{\sqrt{ |S_{1}|}} \geqslant\dots\geqslant \frac{v_{i}}{\sqrt{ |S_{i}|}} \geqslant\dots\geqslant \frac{v_{n}}{\sqrt{ |S_{n}|}}
$$
e si esegua l'algoritmo su tale ordinamento. Si consideri il primo indice $j$ a destra di $i$ tale che $S_i \cap S_j \neq \emptyset$: l'agente $a_i$ non è più vincitore non appena viene selezionato come nuovo vincitore un agente $a_j$ che rende $a_i$ non compatibile, se $\frac{v_{i}}{\sqrt{ S_{i} }} < \frac{v_{j}}{\sqrt{ S_{j} }}$.
Allora la soglia dipenderà dalla posizione di $a_{j}$ nell'ordinamento: l'agente $a_i$ si troverà nella posizione $j$, se esiste $j$, quando
$$
\frac{v_{i}}{\sqrt{ |S_{i}| }} = \frac{v_{j}}{\sqrt{ |S_{j}| }}
$$
da cui segue
$$
v_{i} = v_{j} \cdot \frac{\sqrt{ |S_{i}| }}{\sqrt{ |S_{j}| }}
$$
Allora, il pagamento $p_i$ dell'agente $a_i$ è pari a
$$
p_{i} = \theta_{i}(r_{-i}) = \begin{cases}
0, \quad j \text{non esiste} \\
v_{j} \cdot \frac{\sqrt{ |S_{i}| }}{\sqrt{ |S_{j}| }}, \quad \text{altrimenti}
\end{cases}
$$
#### Risultato di approssimazione
```ad-lemma
Sia $\text{OPT}$ la soluzione ottima per l’asta combinatorica, e sia $W$ la   soluzione calcolata dall’algoritmo $\sqrt{m}$-approssimante, allora nel caso peggiore il valore dell’ottimo è più grande di al più di un fattore $\sqrt{m}$.
$$
\sum_{i\in\text{OPT}} v_i \leqslant \sqrt{m}\sum_{i\in W} v_i
$$
Dato il risultato di hardness, questa è la migliore approssimazione che si può ottenere.
```
##### Dimostrazione
Si vuole mostrare che, quando l'algoritmo greedy seleziona un agente $i$ che non è parte della soluzione ottima, il valore della soluzione non è troppo lontano rispetto all'ottimo.
Sia $W$ la soluzione calcolata dall'algoritmo e sia $\text{OPT}$ la soluzione ottima.
Per ogni $i \in W$ si definisce l'insieme
$$
\text{OPT}_{i} = \{ j \in \text{OPT}: j \geqslant i \land S_{j} \cap S_{i} \neq \emptyset \}
$$
cioè l'insieme degli agenti $a_{j}$ che fanno parte della soluzione ottima $\text{OPT}$ ma che non vengono selezionati in $W$ dall'algoritmo greedy perché seleziona l'agente $a_{i}$ come vincitore, rendendo incompatibili tutti gli $a_j$. In altre parole, si prendono tutti gli indici $j$ dell'ottimo che sono dopo $i$ nell'ordinamento e che l'algoritmo non ha selezionato perché ha inserito $a_{i}$ nella soluzione, ossia quelli che hanno degli elementi in comune con $a_{i}$.
Si osserva che
$$
\bigcup_{i \in W} \text{OPT}_{i} = \text{OPT}
$$
infatti, dato un $k \in \text{OPT}$, consideriamo l'indice $h$ più piccolo tale che $h \in W$ e $S_h \cap S_k \neq \emptyset$; necessariamente $k \geqslant h$ e quindi $k \in OPT_h$.
Allora si deve dimostrare che
$$
\sum_{j \in \text{OPT}_{i}} v_{j} \leqslant \sqrt{ m }\cdot v_{i}, \quad  \forall i \in W
$$
infatti, se così fosse, si può valutare la disequazione per ogni agente ottenendo
$$
\sum_{i \in W}\sum_{j \in \text{OPT}_{i}} v_{j} \leqslant \sum_{i \in W} \sqrt{ m }\cdot v_{i}
$$
e poiché in $\text{OPT}_i$ c'è almeno un'elemento dell'ottimo, la disequazione è una maggiorazione della soluzione ottima, per cui vale
$$
\sum_{i \in \text{OPT}} v_{i} \leqslant \sum_{i \in W}\sum_{j \in \text{OPT}_{i}} v_{j} \leqslant \sum_{i \in W} \sqrt{ m }\cdot v_{i} = \sqrt{ m }\cdot \sum_{i \in W} v_{i}
$$

Per come è stato definito l'algoritmo greedy, vale che per ogni $j \in \text{OPT}_{i}$
$$
\frac{v_{j}}{\sqrt{ |S_{j}| }} \leqslant \frac{v_{i}}{\sqrt{ |S_{i}| }} \iff v_{j} \leqslant v_{i} \cdot \frac{\sqrt{ |S_{j}| }}{\sqrt{ |S_{i}| }}
$$
dunque per ogni $i \in W$ vale
$$
\sum_{j \in \text{OPT}_{i}} v_{j} \leqslant \sum_{j \in \text{OPT}_{i}} v_{i} \cdot \frac{\sqrt{ |S_{j}| }}{\sqrt{ |S_{i}| }} =  \frac{v_{i}}{ \sqrt{ |S_{i}| }} \cdot \sum_{j \in \text{OPT}_{i}}\sqrt{ |S_{j}| }
$$
A questo punto si utilizza la disugualianza di Cauchy-Schwarz:
```ad-info
La disugualianza di Cauchy-Schwarz dice che dati due vettori $x = (x_i,\dots,x_n)$ e $y = (y_i,\dots,y_n)$, vale che
$$
\sum_{j=1}^n x_i y_j \leqslant \left( \sum_{j=1}^n x_j^2 \right)^{\frac{1}{2}} \cdot \left( \sum_{j=1}^n y_j^2 \right)^{\frac{1}{2}}
$$
```
Ponendo $n = |\text{OPT}_i|$ e per ogni $j = 1,\dots,n$: $x_j = 1$ e $y_j = \sqrt{|S_j|}$ si ottiene
$$
\sum_{j \in |\text{OPT}_{i}|} \sqrt{ |S_{j}| } \leqslant \sqrt{ |\text{OPT}_{i}| } \cdot \sqrt{ \sum_{j \in |\text{OPT}_{i}|} |S_{j}| }
$$
inoltre, vale che:
- $|\text{OPT}_i| \leqslant |S_i|$; se cosi non fosse, ci sarebbero due agenti interessati allo stesso oggetto; ma gli agenti in $\text{OPT}_i$ fanno parte della soluzione ottima, ossia una soluzione ammissibile, e due agenti in una soluzione ammissibile non possono avere un oggetto in comune.
- $\sum_{j \in |\text{OPT}_{i}|} |S_{j}| \leqslant m$, dato che gli agenti in $\text{OPT}_i$ fanno parte della soluzione ottima, ossia una soluzione ammissibile, e due agenti in una soluzione ammissibile non possono avere un oggetto in comune. Allora il numero di oggetti a cui sono interessati gli agenti in $\text{OPT}_i$ è al più pari al numero $m$ di oggetti totali nell'asta.
Dunque si ottiene
$$
\begin{align}
\sum_{j \in \text{OPT}_{i}} v_{j} &\leqslant \frac{v_{i}}{ \sqrt{ |S_{i}| }} \cdot \sum_{j \in \text{OPT}_{i}}\sqrt{ |S_{j}| } \\
&\leqslant \frac{v_{i}}{ \sqrt{ |S_{i}| }} \cdot \sqrt{ |\text{OPT}_{i}| } \cdot \sqrt{ \sum_{j \in |\text{OPT}_{i}|} |S_{j}| } \\
&\leqslant \frac{v_{i}}{ \sqrt{ |S_{i}| }} \cdot \sqrt{ |S_{i}| } \cdot \sqrt{ m }  \\
&\leqslant v_{i} \cdot \sqrt{ m }
\end{align}
$$
 $$\tag*{$\blacksquare$}$$