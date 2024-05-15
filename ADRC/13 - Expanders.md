La motivation di questo topic è: esistono modelli di grafo diversi da una clique, cioè con $o(n^2)$ archi, nei quali però il broadcasting di un'informazione tramite protocolli di tipo gossip termina in un tempo polilogaritmico?
Grafi di questo tipo devono avere un **diametro** piccolo e una buona connettività tra i nodi.
# Expanders e loro proprietà
```ad-Definizione
title: Definizione (node-exapnsion)
Dato un grafo $G=(V,E)$ $\Delta$-regolare con $V=[n]$, per ogni sottoinsieme di nodi $S \subset V$ la node-expansion di $S$ è definita come $|N(S)|$ dove
$$
N(S) := \{w \in V \setminus S: (v,w) \in E \land v \in S \}
$$
```

```ad-Definizione
title: Definizione ($\alpha$-expander)
Per ogni costante fissata $\alpha \geqslant 0$, $G$ è detto $\alpha$-expander se ogni sottoinsieme $S \subset V$, con $|S| \leqslant \frac{n}{2}$, ha espansione $|N(S)|$ tale che
$$
|N(S)| \geqslant \min\{n, \alpha|S|\}
$$
```
## Proprietà
Si è interessati ai grafi $\Omega(1)$-expanders per il seguente teorema.
### Teorema
Si consideri la seguente famiglia infinita di grafi
$$
\{ G_{n} = (V_{n},E_{n}): V_{n} = [n], E_{n} \subseteq V_{n}^2, n \geqslant 1 \}
$$
Se esiste una costante assoluta $\alpha > 0$ tale che per un valore abbastanza grande di $n$ il grafo $G_{n}$ è un $\alpha$-expanders, allora il suo diametro è $O(\log n)$.
#### Dimostrazione
Si consideri un qualsiasi grafo della famiglia $G=G_n = (V_n,E_n)$.
Per la dimostrazione, si effettua una visita in ampiezza a partire da un qualsiasi nodo sorgente $s \in V$ al fine di calcolare le varie distanza $d_G(s,v), v \in V$. Si definiscono i seguenti insiemi:
- $L_{t} := \{ v \in V: d_{G}(s,t) = t \},\ \forall t = 0,1,\dots,n-1;$
- $L_0 := \{ s \} = I_{0};$
- $I_{t} := I_{t-1} \cup L_{t}, \ \forall t = 0,1,\dots,n-1.$
dunque $L_t$ è l'insieme dei nodi nel livello $t \geqslant 0$ dell'albero BFS risultante dalla visita e $I_t$ è l'insieme di tutti i nodi che appartengono ad un livello al più $t$. 
Si osserva che $L_1 = N(s) \geqslant \alpha$ in quanto $|N(s)|$ è un intero. Inoltre, per costruzione, $N(I_{t-1}) = L_t$ e dunque $|I_{t}| = |I_{t-1}| + |L_{t}|$. Dato che $G$ è un $\alpha$-expander, si ottiene che se $|I_{t-1}| \leqslant \frac{n}{2}$, allora vale
$$
|I_{t}| = |I_{t-1}|+|L_{t}| = |I_{t-1}|+|N(I_{t-1})| \geqslant |I_{t-1}|+\alpha|I_{t-1}| = (1+\alpha)|I_{t-1}|
$$
srotolando questa relazione ricorsiva, si ottiene
$$
\begin{align}
|I_{t}| &\geqslant (1+\alpha)|I_{t-1}| \\
&\geqslant(1+\alpha)^2 |I_{t-2}| \\
& \ \ \vdots \\
&\geqslant(1+\alpha)^{t-1}
\end{align}
$$
Sia $L_{\tau}$ un livello tale che il numero di nodi che appartengono ad un livello al più $\tau$ è maggiore della metà dei nodi
$$
\tau = \min\left\{  t\geqslant 1 : |I_{t}| > \frac{n}{2} \right\}
$$
![|center](expanders01.png)
Allora $\tau$ indica la distanza entro la quale almeno la metà dei nodi del grafo sono distanti da $s$. Usando la relazione ricorsiva, si ottiene che
$$
|I_{\tau}| \geqslant (1+\alpha)^{\tau-1} = \frac{n}{2} \iff \tau = \log_{1+\alpha}\left( \frac{n}{2} \right) +1
$$
dunque il numero di nodi a distanza $\tau \in O(\log n)$ da $s$ sono almeno $\frac{n}{2}$.
Si consideri ora un qualsiasi altro nodo $w \in V \setminus I_{\tau}$, cioè un nodo $w$ che si trova a distanza maggiore di $\tau$ da $s$. Ripetendo la visita in ampiezza partendo da $w$ e sempre per il fatto che $G$ è un $\alpha$-expander, si ottiene che i nodi a distanza $\tau' \in O(\log n)$ da $w$ sono almeno $n/2$. Dato che entrambe le visite (quella da $s$ e quella da $w$) raggiungono <u>almeno</u> la metà dei nodi, allora i due alberi costruiti dalle BFS devono necessariamente condividere almeno un nodo. 
![|center](expanders02.png)
Allora data una qualsiasi coppia di nodi $u,v \in V_n$ esiste sempre un cammino di lunghezza $O(\log n)$ tra $u$ e $v$. Ne segue che il diametro di $G_n$ è $O(\log n)$.
## Sull'esistenza di expanders sparsi
Ci si chiede se esistono protocollo efficienti che permettano di generare dei grafi expander **sparsi**. Il lemma seguente permette di dare una risposta positiva a tale questione.
### Lemma
Il grafo aleatori e statico generato dal seguente protocollo randomizzato

> ogni nodo sceglie $d$ nodi vicini u.a.r.

è un $\Theta(1)$-expander con alta probabilità, per ogni $d\geqslant 3$.
#### Dimostrazione
Si consideri un qualsiasi grafo aleatorio generato dal protocollo $G=(V,E)$ e sua $S\subset V$ un sottoinsieme dei nodi con $|S|=s$. Sia $T\subseteq V \setminus S$ un sottoinsieme di nodi arbitrario disgiunto da $S$ e tale che $|T| = \frac{1}{10}s$. In tale modello vale che per ogni coppia $u,v \in V$
$$
\mathbf{Pr}((u,v) \in E) = \frac{1}{n-1} 
$$
Sia $\mathcal{E}$ l'evento "tutti gli archi che partono da nodi in $S$ finiscono in nodi in $S \cup T$". Vale che
$$
\mathbf{Pr}(\mathcal{E})  = \left( \frac{|S|}{n-1} \right)^{d \cdot |S \cup T|} = \left( \frac{|S\cup T|}{n-1} \right)^{d|S|} = \left( \frac{\frac{11}{10}s}{n-1} \right)^{ds}
$$
Si osserva che l'evento $\mathcal{E}$ è equivalente all'evento "il vicinato uscente di $S$ è incluso in $T$: $\delta_{out}(S) \subseteq T$ ".
Facendo union bound su tutti i possibili sottoinsiemi $T$ disgiunti da $S$ e con $|T|=\frac{1}{10}s$, su tutti i possibili sottoinsiemi $S$ con $s$ elementi e tutte le possibili dimensioni $s=1,\dots,\frac{n}{2}$ di $s$ si ottiene
$$
\mathbf{Pr}(G \ \text{non è un expander}) \leqslant \sum_{s=1}^{n/2} \binom{n}{s} \binom{n-s}{\frac{1}{10}s} \left( \frac{\frac{11}{10}s}{n-1} \right)^{ds}
$$
Si può dimostrare attraverso trick matematici che non voglio scrivere e che non stanno scritti sulle dispense che sta robaccia tende a $0$ come l'inverso di un polinomio. Ciò implica che con alta probabilità il grafo $G$ generato dal protocollo randomizzato è un expander.

Il grafo $G$ generato dal protocollo è sparso, in particolare
$$
|E| \leqslant 2d|V|
$$
Si osserva che il grafo non è regolare. Con algoritmi più complessi e analisi più cazzuti si può dimostrare che si possono generare grafi expanders sparsi e regolari
# Protocollo PULL su expanders $\Delta$-regolari
Si considera il protocollo PULL broadcast visto nel [Modello GOSSIP](12%20-%20Modello%20GOSSIP.md), sotto l' assunzione che la rete di comunicazione del sistema corrisponde ad un grafo $G=(V,E)$ che è un $\alpha$-expander $\Delta$-regolare, con $\Delta \in O(1)$.
Data una sorgente $s$ dalla quale far partire il broadcast di un messaggio $M$, il protocollo PULL Broadcast segue il seguente comportamento.
## Il protocollo
I nodi, durante l'esecuzione del protocollo, possono trovarsi in due stati: $\text{INFORMED}$ e $\text{NOT-INFORMED}$ in accordo con il valore del registro $c_x$. Si descrive dunque ad altissimo livello il protocollo:
- Inizialmente, la sorgente $s \in V$ è nello stato $\text{INFORMED}$ ed è l'unico nodo informato nella rete. Il resto dei nodi non è informato e sono nello stato $\text{NOT-INFORMED}$.
- Ad ogni round $t\geqslant 1$, ogni nodo $v$ $\text{NOT-INFORMED}$ esegue una operazione di *pull* su un altro nodo $u \in_{U} N(v)$:
	- se $u$ è un nodo $\text{INFORMED}$, allora $v$ si fa inviare una copia del messaggio $M$ e passa nello stato $\text{INFORMED}$;
- Il protocollo termina globalmente quando tutti i nodi si trovano nello stato $\text{INFORMED}$.
## Analisi (SOLO IN MEDIA AHAH)
### Teorema
Sia $G=(V,E)$ un grafo $\Delta$-regolare con $\alpha$-espansione, dove $\alpha$ è una qualsiasi costante positiva fissata. Per una qualsiasi sorgente $s$, il protocollo **PB** termina il task in $O(\log n)$ round con alta probabilità.
#### Dimostrazione
Si definiscono i seguenti insiemi
$$
\begin{align}
I_{0} &\equiv \{ s \} \\
I_{t} &\equiv \{ v \in V: v \text{ è informato in qualche round } t' \leqslant t\}
\end{align}
$$
Si dimostra che la dimensione di $I_t$, in aspettazione, cresce esponenzialmente al crescere di $t$.
Sia $t\geqslant 1$ un round fissato. Sia $N(I_t)$ l'insieme dei nodi al di fuori di $I_t$ che hanno almeno un vicino in $I_t$, ovvero
$$
N(I_t) = \{ v \in V \setminus I_{t}: \exists u \in I_{t} \land (u,v) \in E\}
$$
![](expanders03.png)

Sia $Y_v$, per ogni nodo $v \in N(I_{t})$, la variabile aleatoria binaria
$$
Y_{v}= 
  \left\{ 
    \begin{aligned}
      &\ 1 \ \ \ \ \text{se } v \text{ diventa informato al round } t+1\\
      &\ 0 \ \ \  \textnormal{ altrimenti}  
    \end{aligned}
  \right.
$$
Allora la probabilità che un nodo $v \in N(I_t)$ sia informato al round $t+1$ corrisponde alla probabilità di scegliere tra tutti i suoi $\Delta$ vicini uno di quelli che si trovano in $I_t$. Dato che $v \in N(I_t)$, per definizione deve avere almeno un vicino in $I_t$, dunque
$$
\mathbf{E}[Y_{v}] = \mathbf{Pr}(Y_{v} = 1) = \frac{|I_{t} \cap N(v)|}{\Delta} \geqslant \frac{1}{\Delta} 
$$
Data l'$\alpha$-espandibilità di $G$, fintantoché $|I_t|\leqslant \frac{n}{2}$, vale che
$$
|N(I_{t})| \geqslant \alpha |I_{t}|
$$
Applicando queste ultime due disuguaglianze, è possibile dare un lower bound al numero atteso di nuovi nodi informati al round $t+1$:
$$
\begin{align}
\mathbf{E}[|I_{t+1}|] &= |I_{t}| + \sum_{v \in N(I_{t})} \mathbf{E}[Y_{v}] \\
&\geqslant |I_{t}| + \alpha |I_{t}| \frac{1}{\Delta} \\
&= \left( 1+\frac{\alpha}{\Delta} \right)|I_{t}| \\
&= (1+\Omega(1))|I_{t}|
\end{align}
$$
Questa disuguaglianza dimostra che in media $|I_t|$, fintantoché non è più grande di $\frac{n}{2}$, incrementa di un fattore costante dato che $G$ è un expander $\Delta$-regolare con $\Delta=O(1)$. Dunque in media, dopo $O(\log n)$ round, almeno $\frac{n}{2}$ nodi saranno informati.

Si dovrebbe dimostrare che il numero di nodi informati passa da $\frac{n}{2}$ ad $n$ in un numero logaritmico di round, ma sta parte è omessa quindi VAFFANCULO PORCO DIO!