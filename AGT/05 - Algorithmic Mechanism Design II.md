Si riprende la trattazione del mechanism design esaminando una applicazione del meccanismo VCG ad un problema utilitario su grafo: il problema del cammino minimo tra due nodi in un grafo con archi privati.
# Shortest Path con Selfish-Edges
Si può associare a questo problema il seguente scenario: in un grafo gli archi sono controllati da *agenti egoistici*, per cui solo l'agente conosce il peso associato al proprio arco. Dati due nodi del grafo, l'obiettivo è quello di calcolare un cammino minimo tra i due nodi rispetto ai pesi *reali*. A tale scopo, si deve progettare un *meccanismo truthful*, che con un pagamento opportuno incentiva gli agenti a dire la *verità*.
Intuitivamente, il problema è cosi descritto:
- Input composto da un grafo $G=(V,E)$ in cui ogni arco $e \in E$ è un agente egoistico; inoltre sono dati due nodi $s,t \in V$ di *sorgente* e di *destinazione*, rispettivamente. Il **tipo** di un agente è il costo di utilizzo dell'arco: questa è l'informazione privata dell'agente, che non viene necessariamente riportata. La **valutazione** di un agente è uguale al valore del tipo.
- La **social-choice function** è un *vero* cammino minimo in $G=(V,E,\tau)$ tra $s$ e $t$, dove $\tau$ è il vettore dei tipi degli agenti (aka il peso reale di ogni arco).
Più formalmente:
- Le **soluzioni ammissibili**, cioè i possibili outcome del gioco, è l'insieme $\mathcal{F}$ dei cammini minimi tra $s$ e $t$;
- Il **tipo** dell'agente $e$ è il valore $\tau_e$ che rappresenta il peso *reale* dell'arco $e$ in $G$. Intuitivamente, $\tau_e$ è il costo che l'agente sostiene per utilizzare $e$. In generale, il tipo riportato $r_e$ dall'agente $e$ può essere diverso da $\tau_e$.
- Dato un outcome $P \in \mathcal{F}$, la **valutazione** dell'agente $e$ nel cammino $P$ è data da $$ v_{e}(\tau_{e},P) = \begin{cases}
\tau_{e}, \quad e \in P \\
	0, \quad \text{altrimenti}
\end{cases}$$ cioè l'agente $e$ paga al sistema $\tau_e$ se il suo arco fa parte del cammino minimo, zero altrimenti.
- Dato il vettore dei tipi riportati $\tau$, o i pesi *reali* degli archi, la social-choice function calcola lo **shortest path** in $G=(V,E,\tau)$ tra i due nodi $s,t$.
## Un meccanismo truthful per il problema
Si osserva che il problema presentato è **utilitario**. Infatti, calcolare il cammino minimo significa calcolare il cammino di costo minimo. Dato un cammino $P$, la sua vera lunghezza è data dalla somma dei pesi (tipi reali) degli archi presenti nel cammino, che per come è definita la funzione di valutazione, è proprio pari alla somma delle valutazioni degli agenti nell'outcome $P$. In formule
$$ \sum_{e \in P}\tau_{e} = \sum_{e \in E}v_{e}(\tau_{e},P) $$
Allora, la social-choice function è pari a
$$ f(t) = \arg\min_{P \in \mathcal{F}} \sum_{e \in E} v_{e}(\tau_{e},P) $$
Dunque il problema è utilitario, per cui è possibile utilizzare i meccanismi VCG per risolvere il problema in modo truthful.
### Meccanismo VCG
Si ricorda che un meccanismo $M = \langle g(r),p(r) \rangle$ è VCG se è della forma:
- $g(r) = x = \arg\min_{y \in F} \sum_{j} v_{j}(r_{j},y)$
- $p_{i}(r) = \sum_{j \neq i}v_{j}(r_{j},g(r_{-i})) - \sum_{j\neq i} v_{j}(r_{j},x)$
Nel caso di questo problema dunque:
- L'algoritmo $g(r)$ calcola un cammino minimo $P_G(s,t)$ in $G=(V,E,r)$ pesato con i tipi riportati;
- Fissato un outcome $g(r) = P_G(s,t)$, l'agente $e \in E$ viene pagato una quantità pari a $$p_{e}(r) = \sum_{j \neq e}v_{j}(r_{j},g(r_{-e})) - \sum_{j\neq e} v_{j}(r_{j},P_{G}(s,t))$$ cioè $$p_{e}(r) = \begin{cases}
d_{G-e}(s,t) - (d_{G}(s,t)- r_{e}), \quad &e \in P_{G}(s,t) \\
0, \quad &\text{altrimenti}
\end{cases}$$
Dunque per calcolare il pagamento ad ogni arco $e \in P_G(s,t)$, è necessario calcolare il **cammino minimo di rimpiazzo** $P_{G-e}(s,t)$ nel grafo $G-e = (V, E \setminus \{e\},r_{-e})$, cioè il cammino minimo da $s$ a $t$ che non contiene l'arco $e$.
#### Esempio
Si consideri il seguente grafo
![[agt_img02.png]]
Per calcolare il pagamento di $e \in P_G(s,t)$, si deve prima calcolare li **cammino minimo di rimpiazzo** per $e$
![[agt_img03.png]]
Dunque vale
$$P_{e} = d_{G-e}(s,t) - (d_{G}(s,t) - r_{e}) = 12 - (11 - 2) = 3$$
### Analisi della complessità
Sia $n = |V|$ e $m = |E|$ e sia $d_G(s,t)$ la **distanza** in $G$ da $s$ a $t$.
Inoltre, si lavora sotto l'ipotesi che nel grafo i nodi $s$ e $t$ sono **2-edge connessi**, cioè esistono in $G$ almeno due cammini tra $s$ e $t$ che sono disgiunti sugli archi. Dunque, per ogni arco $e$ del  cammino $P_G(s,t)$ che viene rimosso esiste almeno un cammino alternativo in $G-e$.
Se così non fosse, allora ci sarebbe almeno un arco $e$ in $P_G(s,t)$ che è un **ponte**, cioè un arco che se rimosso spezza $G$ in due componenti $C_1,C_2$ con $s \in C_1$ e $t \in C_2$. In questo caso, non esiste un cammino minimo da $s$ a $t$ che *non* passa per $e$, cioè $d_{G-e}(s,t) = +\infty$. In altre parole, il possessore di questo arco ponte *tiene in pugno* il sistema e può chiedere una qualsiasi cifra.

Per calcolare i pagamenti si applica per ogni arco $e \in P_G(s,t)$ l'algoritmo di **Dijkstra** al grafo $G-e$. La complessità con questo approccio è pari a
$$ \underbrace{O(n)}_{\text{nro di archi in }P_{G}(s,t)} \cdot \underbrace{O(m+n\log{n})}_{\text{Calcolo $P_{G-e}(s,t)$ fissato $e \in P_{G}(s,t$)}} = O(nm+n^2\log{n}) $$
In realtà vale un risultato ancora migliore, in cui si dimostra che il problema è risolvibile in tempo $O(m+m\log{n})$.
# Meccanismi One-Parameter (OP)
Si definisce ora una seconda classe di meccanismi truthful, utilizzati per risolvere una categoria di problemi detta **one-parameter (OP)**.
```ad-theorem
title: Definizione
Un problema è **one-parameter** se:
1. L'informazione posseduta da ogni agente $a_i$ è un **singolo parametro** $t_i \in \mathbb{R}$;
2. La valutazione di $a_i$ ha la forma $$ v_i(t_i,o) = t_i \cdot w_i(o) $$ dove $w_i(o)$ è una quantità detta **carico di lavoro** per $a_i$ nell'outcome $o$.
```
Per questi problemi esiste una classe di meccanismi truthful detta **meccanismi one-parameter**. A differenza dei meccanismi VCG, dove le valutazioni (costi) e i tipi sono *arbitrari* ma possono essere applicati solo a problemi utilitari (aka sono vincolati a problemi con funzione di scelta sociale ben precisa), i meccanismi OP ammettono una funzione di *scelta sociale arbitraria* ma i tipi devono essere a singolo parametro e le valutazioni vincolate.
Si descrivono ora le caratteristiche dei meccanismi one-parameter.
## Meccanismo truthful se e solo se $g(\cdot)$ monotona
Si introduce una definizione utile per dimostrare una condizione necessaria per la truthfulness di un meccanismo per un problema OP.
```ad-theorem
title: Definizione
Un algoritmo $g(\cdot)$ per un problema OP di minimizzazione è **monotono** se per ogni agente $a_i$, il carico di lavoro $w_i(g(r_{-i},r_i))$ è **non crescente** rispetto a $r_i$ per tutti gli $r_{-i} = (r_1,\dots,r_{i-i},r_{i+i},\dots,r_n)$.
```
dunque, vale il seguente teorema.
### Teorema 1: CN truthfulness (Mayerson '81)
Una *condizione necessaria* affinché un meccanismo $M=\langle g(r), p(r) \rangle$ per un problema OP sia *veritiero* è che $g(r)$ sia monotono.
#### Dimostrazione
Si suppone per assurdo che l'algoritmo $g(\cdot)$ non sia monotono e si mostra che nessuno schema di pagamento può rendere $M$ veritiero.
Se $g(\cdot)$ non è monotono, esiste un agente $a_i$ e un vettore $r_{-i}$ tale che $w_i(g(r_{-i},r_i))$ è non *non crescente* rispetto a $r_i$. Graficamente,
![[agt_img05.png]]
Si considerano i quattro casi seguenti:
1. $t_i = x, r_i = x \implies v_i(t_i,o) = x \cdot w_i(g(r_{-i},x))$
2. $t_i = y, r_i = y \implies v_i(t_i,o) = y \cdot w_i(g(r_{-i},y))$
3. $t_i = x, r_i = y \implies v_i(t_i,o) = x \cdot w_i(g(r_{-i},y)) \implies a_i \text{ aumenta il suo costo di } A$
4. $t_i = y, r_i = x \implies v_i(t_i,o) = y \cdot w_i(g(r_{-i},x)) \implies a_i \text{ ha un risparmio di } A+k$
Dove $A$ e $k$ sono le due aree in figura:
![[agt_img06.png]]
Sia $\Delta p = p_i(r_{-i},y) - p_i(r_{-i},x)$, se $M$ è veritiero deve essere:
- $\Delta p \leqslant A$: se cosi non fosse, nel caso in cui $t_i = x$ all'agente $a_i$ converrebbe mentire e dichiarare $r_{i}=y$ in quanto l'aumento di pagamento ($> A$) supererebbe l'aumento di costo ($A$), dunque la sua utilità aumenta.
- $\Delta p \geqslant A+k$: se cosi non fosse, nel caso in cui $t_i = y$ all'agente $a_i$ converrebbe mentire e dichiarare $r_{i}=x$ in quanto l'aumento di pagamento nel dichiarare $y$ non supererebbe il risparmio che si ottiene riportando $x$ $, dunque la sua utilità aumenta.
Ma $k$ è un valore strettamente positivo, dunque non possono valere le due condizioni contemporaneamente.
$$\tag*{$\blacksquare$}$$
## Meccanismo one-parameter
Dato un problema one-parameter, un meccanismo per risolvere il problema è $M = \langle g(r),p(r) \rangle$ dove $g(r)$ è un qualsiasi algoritmo **monotono** (condizione necessaria per la truthfulness) e, fissato un outcome $o$, il pagamento per l'agente $a_i$ quando dichiara $r_i$ è pari a
$$
p_{i}(r) = h_{i}(r_{-i}) + r_{i}w_{i}(r) - \int_{0}^{r_{i}} w_{i}(r_{-i},z)  \, dz
$$ 
con $h_i(r_{-i})$ funzione arbitraria indipendente da $r_i$.
Si osserva che il meccanismo presentato è **truthful**, infatti vale il teorema seguente.
### Teorema 2:  (Mayerson '81)
Un meccanismo OP, per un problema OP, è veritierio.
#### Dimostrazione
Per dimostrare il teorema, bisogna mostrare che l'utilità di un agente $a_i$ può *solo decrescere* se $a_i$ dichiara il falso.
Siano $r_{-i}$ le dichiarazioni degli altri agenti fissate. Il pagamento fornito ad $a_i$ quando dichiara $r_i$ è pari a
$$
p_{i}(r) = h_{i}(r_{-i}) + r_{i}w_{i}(g(r)) - \int_{0}^{r_{i}} w_{i}(g(r_{-i},z))  \, dz
$$ 
dato che $h_i(r_{-i})$ non dipende dalla scelta $r_i$ dell'agente $a_i$, si ignora questo fattore ponendo $h_i(r_{-i}) = 0$.
L'utilità dell'agente $a_i$ quando dichiara la verità $t_i$ è pari a
$$
\begin{align}
u_{i}(t_{i},g(r_{-i},t_{i})) &= p_{i}((r_{-i},t_{i})) - v_{i}(t_{i},g(r_{-i},t_{i})) \\
&= \left(t_{i} \cdot w_{i}(g(r_{-i},t_{i})) - \int_{0}^{t_{i}} w_{i}(g(r_{-i},z))  \, dz\right) - t_{i} \cdot w_{i}(g(r_{-i},t_{i})) \\
&= - \int_{0}^{t_{i}} w_{i}(g(r_{-i},z))  \, dz
\end{align}
$$
Graficamente,
![[agt_img07.png]]
Se l'agente $a_i$ mente, allora dichiara un valore $r_i \neq t_i$: sia questo valore denotato con $x = r_i$. In questo caso, la valutazione è pari a
$$ 
v_{i}(t_{i},g(r_{-i},x)) = t_{i} \cdot w_{i}(g(r_{-i},x)) = C
$$
il pagamento ad $a_i$  è pari a
$$
p_{i}((r_{-i},x)) = x \cdot w_{i}(g(r_{-i},x)) -
\int_{0}^{x} w_{i}(g(r_{-i},z))  \, dz = P
$$
dunque, l'utilità di $a_i$ è pari a
$$
\begin{align}
U &= u_{i}(t_{i},g(r_{-i},x))  \\
&= p_{i}((r_{-i},x)) - v_{i}(t_{i},g(r_{-i},x)) \\
&= \left(x \cdot w_{i}(g(r_{-i},x)) - \int_{0}^{x} w_{i}(g(r_{-i},z))  \, dz\right) - t_{i} \cdot w_{i}(g(r_{-i},x)) \\
&= (x - t_{i}) \cdot w_{i}(g(r_{-i},x)) - \int_{0}^{t_{i}} w_{i}(g(r_{-i},z))  \, dz \\
&= P-C
\end{align}
$$

Allora l'agente $a_i$, se mente, dichiara o $x > t_i$ o $x < t_i$:
- se $x > t_i$, graficamente vale, <br>![[agt_img08.png]] cioè l'area sottesa al grafico presa in considerazione è più grande di una quantità $G$. Dato che l'area definita dall'integrale viene sottratta nel calcolo dell'utilità, si conclude che $a_i$ sta perdendo $G$ di utile, dunque non ha convenienza a mentire.
- se $x < t_i$, graficamente vale, <br>![[agt_img11.png]] cioè l'area sottesa al grafico presa in considerazione è più grande di una quantità $G$. Come nel caso precedente, l'area viene sottratta nel calcolo dell'utilità. Si conclude che $a_i$ sta perdendo $H$ di utile, dunque non ha convenienza a mentire.
Allora in entrambi i casi, l'agente $a_i$ non ha convenienza a mentire, cioè a dichiarare un tipo diverso da $t_i$.
 $$\tag*{$\blacksquare$}$$
### Sulla funzione $h_i(r_{-i})$
Per ottenere la **volontaria partecipazione (VP)** ad un gioco, il meccanismo deve garantire che l'utilità di un qualsiasi agente che dichiara il vero ha sempre un utile non negativo.
Per ottenere VP nei meccanismi one-parameter, è sufficiente scegliere la costante
$$
h_{i}(r_{-i}) = \int_{0}^{\infty} w_{i}(g(r_{-i},z)) \, dz
$$
in questo modo, il pagamento diventa
$$
p_{i}(r) = r_{i}w_{i}(g(r)) + \int_{r_{i}}^{\infty} w_{i}(g(r_{-i},z)) \, dz
$$
e l'utilità di un agente che dichiara il vero diventa
$$
u_{i}(t_{i},g(r)) = \int_{r_{i}}^{\infty} w_{i}(g(r_{-i},z)) \, dz \geqslant 0
$$
# Meccanismi OP: il problema Shortest Path Tree con Selfish-Edges
Come esempio applicativo dei meccanismi one-parameter, si studia ora la versione *egoistica* del problema di calcolare l'albero dei cammini minimi di un grafo.
## SPT non cooperativo
Si considera il seguente problema di *broadcasting*. Data una rete $G=(V,E)$, una sorgente $s \in V$ vuole spedire un messaggio ai restati nodi in $V \setminus \{s\}$. Ad ogni arco della rete $e \in E$ è associato un agente egoistico, il cui dato privato è il *tempo di attraversamento* del messaggio attraverso il collegamento che controlla. L'obiettivo del problema è quello di minimizzare il tempo di consegna di ogni messaggio. 
Formalmente:
- L'input è composto da un grafo $G=(V,E)$ biconnesso sugli archi, in cui ogni arco corrisponde in modo biunivoco ad un insieme di agenti egoisti, e da un nodo $s \in V$;
- Il **tipo** di un agente è un valore strettamente positivo che indica il costo di utilizzo dell'arco che gestisce;
- L'insieme dei possibili outcome $\mathcal{F}$ è l'insieme degli alberi ricoprenti radicati in $s$;
- Fissato un vettore dei tipi $t$, la funzione di scelta sociale è definita come $$ f(t) = \arg\min_{T \in \mathcal{F}} \sum_{v \in V} d_{T}(s,v) = \arg\min_{T \in \mathcal{F}} \sum_{e \in E(T)} t_{e} \cdot \mid\mid e \mid\mid$$ dove $\mid\mid e\mid\mid$ è la **molteplicità** dell'arco $e$ nell'albero (soluzione) $T$, intesa come il il numero di cammini ai quali appartiene in $T$. 
```ad-note
title: Notazione
$E(T)$ indica gli archi di $G$ che sono presenti nell'albero ricoprente $T$
```
Per classificare il problema come utilitario o meno, è necessario definire la funzione di valutazione: si osserva che fissato un albero (outcome) $T \in \mathcal{F}$, la funzione di valutazione dipende da come vengono trasmessi i messaggi nella rete $T$.
Si suppone di utilizzare un protocollo **multicast**, nel quale una sola copia del messaggio viene spedita su ogni arco: eventualmente, il messaggio viene duplicato dai nodi che devono ritrasmettere il messaggio su più archi incidenti a loro.
![[agt_img09.png]]
Fissato un outcome $T \in \mathcal{F}$, la funzione di **valutazione** dell'agente associato all'arco $e$ $a_e$ è definita come
$$
v_{e}(t_{e},T) = \begin{cases}
t_{e}, \quad &e \in E(T) \\
0, \quad &\text{altrimenti}
\end{cases}
$$
dunque il problema è **non utilitario**, in quanto
$$
f(t) \neq \arg\min_{T \in \mathcal{F}} \sum_{e \in E} v_{e}(t_{e},T)
$$
Il problema però è di tipo **one-parameter**, in quanto il tipo di ogni agente $a_e$ è un singolo parametro $t_e \in \mathbb{R}$, mentre la valutazione sotto ipotesi di trasferimento multicast è data da
$$
v_{e}(t_{e},T) = t_{e} \cdot w_{e}(T)
$$
con
$$
w_{e}(T) = \begin{cases}
1, \quad &e \in E(T) \\
0, \quad &\text{altrimenti}
\end{cases}
$$
Dunque si può utilizzare un meccanismo one-parameter per risolvere il problema.
## Progettazione del meccanismo
Un meccanismo one-parameter per l'SPT non utilitario è il meccanismo $M_{\text{SPT}}=\langle g(r),p(r) \rangle$ con:
- $g(r)$ algoritmo che dato il grafo e le dichiarazioni, calcola uno Shortest Path Tree $S_G(s)$ del grafo $G=(V,E,r)$ pesato con i pesi riportati $r$ utilizzando l'**algoritmo di Dijkstra**.
- Il pagamento per ogni agente $a_e$ che controlla l'arco $e \in E$ è dato da $$p_{e}(r) = r_{e} \cdot w_{e}(g(r)) + \int_{r_{e}}^{\infty} w_{e}(g(r_{-e},z)) \, dz$$ così da garantire la **partecipazione volontaria**.
Si osserva che il meccanismo $M_{\text{SPT}}$ è **truthful**, in quanto l'algoritmo di Dijkstra è monotono. Fissato $r_{-e}$ il carico di lavoro per un agente $a_e$ ha sempre la forma
![[agt_img10.png]]
dove il valore $\theta_e$ per cui il carico di lavoro passa da uno a zero è detta **soglia**: se $a_e$ dichiara al più $\theta_e$, allora l'arco $e$ è selezionato; se $a_e$ dichiara più di $\theta_e$, allora l'arco $e$ non è selezionato.
Questo valore di soglia è fondamentale nel calcolo dei pagamenti, infatti
- Se $e$ non viene selezionato, cioè $e \not\in E(T)$ ,allora $$p_{e}(r) = r_{e} \cdot w_{e}(g(r)) + \int_{r_{e}}^{\infty} w_{e}(g(r_{-e},z)) \, dz = 0+0=0$$
- Se $e$ viene selezionato, cioè $e \in E(T)$ ,allora $$p_{e}(r) = r_{e} \cdot w_{e}(g(r)) + \int_{r_{e}}^{\infty} w_{e}(g(r_{-e},z)) \, dz = r_{e}+(\theta_{e}-r_{e})=\theta_{e}$$
### Sul calcolo delle soglie
Per il calcolo dei valori soglia, si consideri $e=(u,v)$ un arco in $S_G(s)$ con $u$ più vicino a $s$ che $v$. Allora $e$ resta in $S_G(s)$ fintantoché si utilizza l'arco $e$ per raggiungere $v$. In particolare, $e$ non viene più scelto nel momento in cui $r_e$ è tale che
$$
d_{G}(s,u) + r_{e} > d_{G-e}(s,v)
$$
quindi il valore soglia per l'arco $e$ è tale che
$$
d_{G}(s,u) + \theta_{e} = d_{G-e}(s,v) \iff \theta_{e} = d_{G-e}(s,v) - d_{G}(s,u)
$$
### Una soluzione banale: risultati di complessità
Una soluzione banale per il problema dello SPT non cooperativo consiste nel calcolare $S_G(s)$ utilizzando l'algoritmo di Dijkstra sul grafo con i pesi riportati, per poi calcolare, per ogni $e = (u,v)$, $d_{G-e}(s,v)$ applicando Dijkstra al grafo $G-e$ in modo tale da calcolare il pagamento per ogni $a_{e}$.
La complessità di tale soluzione è pari a
$$
\underbrace{O(n)}_{\text{nro di archi in }P_{G}(s,t)} \cdot \underbrace{O(m+n\log{n})}_{\text{Calcolo $d_{G-e}(s,v)$ fissato $e \in S_{G}(s)$}} = O(nm+n^2\log{n}) 
$$
Infine, vale il seguente teorema.
```ad-theorem
title: Teorema
$M_{\text{SPT}}$ è calcolabile in tempo $O(m+n\log{n})$.
```
## Caso speciale dei problemi OP: Binary Demand
Il problema dello SPT non cooperativo appena affrontato appartiene ad una classe particolare di problemi one-parameter, nota con il nome di problemi **binary demand (BD)**.
Un problema è BD se
1. L'informazione posseduta da ogni agente $a_i$ è un **singolo parametro** $t_i \in \mathcal{R}$;
2. La **valutazione** di $a_i$ ha la forma $$v_{i}(t_{i},o) = t_{i} \cdot w_{i}(o)$$ dove $w_i(o) \in \{0,1\}$ è il **carico di lavoro** per $a_i$ nell'outcome $o$. Quando $w_i(o)=1$ diremo che l'agente $a_i$ è **selezionato** nell'outcome $o$.
Si osserva che per problemi BD, l'algoritmo $g(\cdot)$ deve avere la caratteristica di monotonia vista nel meccanismo per il problema SPT, ossia
```ad-theorem
title: Definizione
Un algoritmo $g(\cdot)$ per un problema BD di minimizzazione è **monotono** se per ogni agente $a_i$ e per tutti gli $r_{-i}=(r_1,\dots,r_{i-1},r_{i+1},\dots,r_n)$ la funzione $w_i(g(r_{-i},r_i))$ è della forma
![[agt_img10.png|500|center]]
Dove $\theta_i(r_{-i}) \in \mathcal{R}\cup\{+\infty\}$ è detto **valore soglia** e il **pagamento** per l'agente $a_i$ diventa
$$p_i(r) = \theta_i(r_{-i})$$
```
