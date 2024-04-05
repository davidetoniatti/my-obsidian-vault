In questa sezione, si vedono dei metodi per il calcolo distribuito di una $a$-colorazione di una rete $G=(V,E)$. Formalmente, una $a$-colorazione di un grafo è una funzione
$$
C: V \to \{ 1,2,\dots, a\}
$$
tale che
$$
\forall(u,v) \in E, C(v) \neq C(u)
$$
In particolare, saranno trattati grafi $\Delta$-regolari, cioè grafi in cui ogni nodo ha al più grado $\Delta$. In particolare, si osserva che ogni grafo $\Delta$-regolare accetta una $(\Delta+1)$-colorazione.
# Il problema
Restringendo il problema al caso in cui il grafo è $\Delta$-regolare, l'obiettivo del problema è quello di calcolare in modo distribuito una sua $(\Delta+1)$-colorazione, la quale esiste per l'osservazione fatta in precedenza.
Formalmente, il problema è dato dalla tripla $P = \langle P_{init}, P_{final}, R \rangle$ dove
- $P_{init}$: una colorazione iniziale dei nodi $C_{init}: V \to [a]$ tale che $a > \Delta+1$ e tale che $v$ conosce il suo colore; questo per ogni nodo $v \in V$;
- $P_{final}$: una colorazione dei nodi $C_{final}:V\to [\Delta+1]$ tale che ogni nodo $v \in V$ conosca il suo colore finale $C_{final}(v)$;
- $R = \mathcal{LOCAL}\text{ model}$;
Si riportano ora le caratteristiche del modello distribuito nel quale si risolve il problema, il modello $\mathcal{LOCAL}$.
## $\mathcal{LOCAL}$ model
In questo modello del calcolo distribuito, la rete di comunicazione è modellata come un grafo non diretto e non pesato $G=(V,E)$ con $n$ nodi. La rete è statica, dunque la topologia non cambia. Ogni nodo viene identificato da un $id$ univoco e inizialmente ogni nodo $v$ possiede come sola conoscenza il valore di $id(v)$. Ogni nodo può comunicare con i suoi vicini inviando e ricevendo messaggi, di dimensione illimitata, in modo del tutto **sincrono**. Nello specifico, il protocollo inizia simultaneamente su tutti i nodi e procede per round **discreti** e **sincroni**. Ad ogni round un nodo può eseguire una serie, potenzialmente illimitata, di computazioni e inviare messaggi. In particolare, ogni nodo può inviare messaggi distinti a vicini distinti e tutti i messaggi inviati in un certo round vengono ricevuti entro la fine del round. Infine, la complessità del protocollo è data dal numero di round necessari per terminare il protocollo.
# Protocolli deterministici
In questa sezione si descrivono un paio di protocolli deterministici fondamentali per la colorazione in ambiente distribuito.
## Basic Color Reduction Procedure
La **Basic Color Reduction Procedure (BCP)** consente di ridurre la colorazione di $G$ da $a$ colori a $\Delta+1$ colori in tempo $a-\Delta+1$.
All'inizio di ogni step, ogni nodo $v$ manda il proprio colore a tutto il suo vicinato $N(v)$. Al primo passo, sia $v$ il nodo con colore massimo $a$. Certamente, nel vicinato $N(v)$ possono esserci al più $\Delta$ nodi di colore diverso, dato che $G$ è $\Delta$-regolare. Allora il nodo $v$ può scegliere un qualsiasi nuovo colore $j$ diverso da $a$ e diverso dai colori dei nodi in $N(v)$, garantendo che la nuova colorazione sia ancora una colorazione valida. Inoltre, questa nuova colorazione contiene un colore in meno rispetto alla precedente. Iterando il processo per i colori $a-1, a-2,\dots,\Delta+2$ si ottiene una $(\Delta+1)$-colorazione legale.
### Il protocollo
```python
for each phase k == a,a-1,...,Delta+2:
  for each node v in parallel:
    v.send(C(v)) to v.neighbours
    if C(v) == k:
	  j = any color in {1,2,...,k-1} - N(C(v))
	  C(v) = j
for each node v:
  C_final(v) = C(v)
```
### Correttezza
Il protocollo **BCP** ritorna correttamente una $(\Delta+1)$-colorazione
$$
C_{final}: V \to[\Delta+1]
$$
Questo è vero per due fatti fondamentali:
- **Fatto 1**: ad ogni fase $k$ per ogni nodo $v$ il colore $j$ *esiste sempre*. Questo fatto è vero dato che, per ogni nodo $v$, ci possono essere al più $\Delta$ nodi di colore diverso e il colore $k$ cercato è sempre maggiore di $\Delta+2$.
- **Fatto 2**: Alla fine di ogni fase $k>\Delta+1$, la colorazione $C_k: V \to [a-k]$ è *legale*. Questo è conseguenza del fatto che, per definizione di colorazione legale, i nodi che hanno colore $k$ non possono essere adiacenti. Inoltre, per il fatto 1 il colore $j$, scelto da una palette che non contiene colori dei nodi nel vicinato, esiste sempre.
### Complessità
Per costruzione il protocollo **BCP** impiega $a-(\Delta+1)$ iterazioni. Nel caso peggiore, la $a$-colorazione iniziale potrebbe essere una biezione in $[n]$ ($a=n$). In tal caso, il protocollo **BCP** converge in $O(n-\Delta)$ rounds su una $(\Delta+1)$-colorazione.
La message complexity in questo caos è $O((n-\Delta)\cdot m)$.
## Parallelizzazione di Khun-Wattenhofer
È possibile ridurre il numero di step per calcolare una $(\Delta+1)$-colorazione di un grafo $\Delta$-regolare tramite il **metodo di Khun-Wattenhofer**. In particolare, questo metodo consente di calcolare tale colorazione in tempo $O\left( \Delta \log\left( \frac{a}{\Delta} \right) \right)$.
### Il protocollo
Il protocollo utilizza il **metodo di Khun-Wattenhofer**. In particolare, l'insieme $V$ viene diviso in $k=\lceil \frac{a}{\Delta+1} \rceil$ sottoinsiemi disgiunti $V_1, V_2, \dots, V_k$, dove il sottoinsieme $V_i$ è definito come
$$
V_{i}:= \{ v \in V: (i-1)(\Delta+1)+1 \leqslant C_{init}(v) \leqslant i(\Delta+1) \} 
$$
per ogni $i \in [k]$.
Si osserva che, dati due sottoinsiemi $V_i,V_{i+1}$, la colorazione iniziale $C_{init}: V\to[a]$ induce una $2(\Delta+1)$-colorazione legale sul **sottografo indotto** $G(V_i \cup V_{i+1},E)$.
Sia $F_{i,i+1}$ la $2(\Delta+1)$-colorazione indotta da $C_{init}$ su $G(V_i \cup V_{i+1},E)$ per ogni $i \in [k]$; formalmente
$$
F_{i,i+1}: V_{i} \cup V_{i+1}\to 2[\Delta+1]
$$
Il protocollo esegue dunque **BCP** in parallelo su ogni sottografo indotto $G(V_i \cup V_{i+1},E)$, trasformando ogni $2(\Delta+1)$-colorazione $F_{i,i+1}$ in una $(\Delta+1)$-colorazione per $G(V_i \cup V_{i+1},E)$.
Il protocollo itera questa esecuzione parallela fino a che non si ottiene una $(\Delta+1)$-colorazione per tutto il grafo $G$. Dato che ad ogni iterazioni si dimezzano i colori, dopo $t=O\left( \log \left( \frac{a}{\Delta} \right) \right)$ esecuzioni parallele si ottiene una $(\Delta+1)$-colorazione del grafo $G$.
### Correttezza
Si osserva che il protocollo è corretto per i due seguenti punti.
1. Per ogni $i \in [k]$, il protocollo **BCP** su $G(V_i \cup V_{i+1},E)$ rende la sua $2(\Delta+1)$-colorazione legale $F_{i,i+1}$ una $(\Delta+1)$-colorazione legale (per la correttezza di **BCP**). 
2. Dopo una singola applicazione parallela del protocollo **BCP**, la colorazione $F_{1,2} \cup F_{3,4} \cup \dots \cup F_{k-1,k}$ risulta essere una $\left( \frac{a}{2} \right)$-colorazione legale per il grafo $G$, in quanto:
	- Ogni sottografo indotto $G(V_i \cup V_{i+1},E)$ è colorato con una $(\Delta+1)$-colorazione legale;
	- Ogni coppia di nodi $u,v$ tale che $u \in V_i \cup V_{i+1}$ e $v \in V_j \cup V_{j+1}$ con $i \neq j$ non possono avere gli stessi colori, in quanto gli insiemi dei colori usati dalle colorazioni $F_{i,i+1}$ sono **mutualmente disgiunti**;
	- Dato che le $(\Delta+1)$-colorazioni, dopo una singola applicazione, sono $\frac{k}{2}$, si ottiene che la funzione globale utilizza un numero di colori pari a
$$
\frac{k}{2} (\Delta+1) = \frac{a}{2(\Delta+1)} (\Delta+1)= \frac{a}{2}
$$

In generale, dopo $t$ applicazioni del protocollo **BCP**, si ottiene una $\left( \frac{a}{2^t} \right)$-colorazione legale per il grafo $G$: per $t=O\left( \log \left( \frac{a}{\Delta} \right) \right)$, si ottiene una $(\Delta+1)$-colorazione legale.
### Complessità
Ogni applicazione del **BCP** in parallelo richiede tempo $O(\Delta)$. Dato che sono necessarie $t=O\left( \log \left( \frac{a}{\Delta} \right) \right)$ esecuzioni di **BCP** per ottenere una $(\Delta+1)$-colorazione del grafo $G$, la complessità temporale del protocollo è pari a
$$
O\left( \Delta \log\left( \frac{a}{\Delta} \right) \right)
$$
Dunque nel caso peggiore in cui $a=n$, si ottiene che la complessità temporale è pari a
$$
O\left( \Delta \log\left( \frac{n}{\Delta} \right) \right)
$$
# Protocolli probabilistici
