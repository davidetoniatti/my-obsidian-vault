In questa sezione, si vedono dei metodi per il calcolo distribuito di una $a$-colorazione di una rete $G=(V,E)$. Formalmente, una $a$-colorazione di un grafo è una funzione
$$
C: V \to \{ 1,2,\dots, a\}
$$
tale che
$$
\forall(u,v) \in E, C(v) \neq C(u)
$$
In particolare, saranno trattati grafi $\Delta$-regolari, cioè grafi in cui ogni nodo ha al più grado $\Delta$. Si osserva che ogni grafo $\Delta$-regolare **accetta** una $(\Delta+1)$-colorazione.
# Il problema
Restringendo il problema al caso in cui il grafo è $\Delta$-regolare, l'obiettivo diventa quello di calcolare in modo distribuito una sua $(\Delta+1)$-colorazione, la quale esiste sempre.
Formalmente, il problema è dato dalla tripla $P = \langle P_{init}, P_{final}, R \rangle$ dove
- $P_{init}$: una colorazione iniziale dei nodi $C_{init}: V \to [a]$ tale che $a > \Delta+1$ e tale che $v$ conosce il suo colore; questo per ogni nodo $v \in V$;
- $P_{final}$: una colorazione dei nodi $C_{final}:V\to [\Delta+1]$ tale che ogni nodo $v \in V$ conosca il suo colore finale $C_{final}(v)$;
- $R = \mathcal{LOCAL}\text{ model restriction}$, cioè le restrizioni definite per tale modello;
Si riportano ora le caratteristiche del modello distribuito nel quale si risolve il problema, il modello $\mathcal{LOCAL}$.
## $\mathcal{LOCAL}$ model
In questo modello, la rete di comunicazione è modellata come un grafo non diretto e non pesato $G=(V,E)$ con $n$ nodi. La rete è statica, dunque la topologia non cambia nel tempo. Ogni nodo viene identificato da un $id$ univoco e inizialmente ogni nodo $v$ possiede come sola conoscenza il valore di $id(v)$. Ogni nodo può comunicare con i suoi vicini inviando e ricevendo messaggi, di dimensione illimitata, in modo del tutto **sincrono**. Nello specifico, il protocollo inizia simultaneamente su tutti i nodi e procede per round **discreti** e **sincroni**. Ad ogni round un nodo può eseguire una serie, potenzialmente illimitata, di computazioni e inviare messaggi. In particolare, ogni nodo può inviare messaggi distinti a vicini distinti e tutti i messaggi inviati in un certo round vengono ricevuti entro la fine del round. Infine, la complessità del protocollo è data dal numero di round necessari per terminare il protocollo.
# Protocolli deterministici
In questa sezione si descrivono un paio di protocolli deterministici per la colorazione in ambiente distribuito.
## Basic Color Reduction Procedure
La **Basic Color Reduction Procedure (BCP)** consente di ridurre la colorazione di $G$ da $a$ colori a $\Delta+1$ colori in $a-\Delta+1$ round.
All'inizio di ogni round, ogni nodo $v$ manda il proprio colore a tutto il suo vicinato $N(v)$. Al primo passo, sia $v$ il nodo con colore massimo $a$. Certamente, nel vicinato $N(v)$ possono esserci al più $\Delta$ nodi di colore diverso, dato che $G$ è $\Delta$-regolare. Allora il nodo $v$ può scegliere un qualsiasi nuovo colore $j$ minore di $a$ e diverso dai colori dei nodi in $N(v)$. Così facendo, il nuovo colore assunto da $v$ mantiene comunque la colorazione valida. Inoltre, la nuova colorazione contiene un colore in meno rispetto alla precedente. Iterando il processo per i colori $a-1, a-2,\dots,\Delta+2$ si ottiene una $(\Delta+1)$-colorazione legale.
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
- **Fatto 1**: ad ogni round $k$, per ogni nodo $v$ con $C(v)=k$ *esiste sempre* un colore $j$ minore di $k$ e diverso dai colori dei nodi in $N(v)$. Questo è vero perché, per ogni nodo $v$ con $C(v)=k$, ci sono al più $\Delta$ vicini e $k$ è sempre maggiore di $\Delta+2$.
- **Fatto 2**: Al termine di ogni round $k>\Delta+1$, la colorazione $C_k: V \to [a-k]$ è *legale*. Questo è conseguenza del fatto che, per definizione di colorazione legale, i nodi che hanno colore $k$ non possono essere adiacenti. Inoltre, per il fatto 1 il colore $j$, scelto da una palette che non contiene colori dei nodi nel vicinato, esiste sempre.
### Complessità
Per costruzione il protocollo **BCP** impiega $a-(\Delta+1)$ round. Nel caso peggiore, la $a$-colorazione iniziale potrebbe essere una biezione in $[n]$ ($a=n$). In tal caso, il protocollo **BCP** converge in $O(n-\Delta)$ rounds su una $(\Delta+1)$-colorazione.
La message complexity in questo caso è $O((n-\Delta)\cdot m)$.
## Parallelizzazione di Khun-Wattenhofer
È possibile ridurre il numero di step per calcolare una $(\Delta+1)$-colorazione di un grafo $\Delta$-regolare tramite il **metodo di Khun-Wattenhofer**. In particolare, consente di calcolare tale colorazione in tempo $O\left( \Delta \log\left( \frac{a}{\Delta} \right) \right)$.
### Il protocollo
L'insieme dei nodi $V$ viene partizionato in $k=\lceil \frac{a}{\Delta+1} \rceil$ sottoinsiemi disgiunti $V_1, V_2, \dots, V_k$, dove il sottoinsieme $V_i$ è definito come
$$
V_{i}:= \{ v \in V: (i-1)(\Delta+1)+1 \leqslant C_{init}(v) \leqslant i(\Delta+1) \} 
$$
per ogni $i \in [k]$.
Si osserva che, dati due sottoinsiemi $V_i,V_{i+1}$, la colorazione iniziale $C_{init}: V\to[a]$ induce una $2(\Delta+1)$-colorazione legale sul **sottografo indotto** $G(V_i \cup V_{i+1},E)$.
Se $k$ pari, sia $F_{i,i+1}$ la $2(\Delta+1)$-colorazione indotta da $C_{init}$ su $G(V_i \cup V_{i+1},E)$ per ogni $i \in \{ 1,3,5,\dots, k-1 \}$; formalmente
$$
F_{i,i+1}: V_{i} \cup V_{i+1}\to 2[\Delta+1]
$$
Se $k$ dispari, sia  $F_{i,i+1}$ la $2(\Delta+1)$-colorazione indotta da $C_{init}$ su $G(V_i \cup V_{i+1},E)$ (come caso pari) per ogni $i \in \{ 1,3,5,\dots,k-4 \}$, e sia $F_{k-2,k-1,k}$ l'ultima colorazione indotta da $C_{init}$ su $G(V_{k-2} \cup V_{k-1} \cup V_{k},E)$, formalmente
$$
F_{k-2,k-1,k}: V_{k-2} \cup V_{k-1} \cup V_{k} \to 3[\Delta+1]
$$
(ques'ultimo penso mappi su meno di 3(delta+1) colori, ma tanto è un fattore costante in più, la complessità del protocollo bcp su quest'ultima colorazione indotta rimane ordine di delta).
Nel resto della trattazione si assume in generale $k$ pari per non appesantire la notazione e la spiegazione.
Il protocollo esegue dunque **BCP** in parallelo su ogni sottografo indotto $G(V_i \cup V_{i+1},E)$, trasformando ogni $2(\Delta+1)$-colorazione $F_{i,i+1}$ in una $(\Delta+1)$-colorazione per $G(V_i \cup V_{i+1},E)$.
Il protocollo itera questa esecuzione parallela fino a che non si ottiene una singola $(\Delta+1)$-colorazione per tutto il grafo $G$. Dato che ad ogni iterazioni si dimezzano i colori, dopo $t=O\left( \log \left( \frac{a}{\Delta} \right) \right)$ esecuzioni parallele si ottiene una $(\Delta+1)$-colorazione del grafo $G$.
! spiegare meglio questo "itera", il protocollo sembra una roba a la divide et impera: dopo che c'hai k/2 colorazioni F_i,i+1 per i k/2 grafi indotti V_i cup V_i+1, fai le k/4 delta+1 colorazioni (usando bcp) di V_i cup V_i+1 cup V_i+2 cup V_i+3 con i in {1,5,9,...,k-3}, e cosi via fino ad avere una delta+1 colorazione per V_i cup V_iè1 cup ... cup V_k-1 cup V_k.
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
Si studiano ora dei protocolli probabilistici per il problema del coloramento distribuito, i quali ammettono una piccola probabilità di errore, ma abbattono drasticamente la complessità temporale rispetto ai protocolli deterministici appena affrontati.
## Protocollo Rand-$2\Delta$
Il protocollo $\text{Rand-}2\Delta$ calcola in maniera distribuita una $2\Delta$ colorazione di un grafo $G$ che è $\Delta$ regolare.
### Il problema
Formalmente, il problema è dato dalla tripla $P = \langle P_{init}, P_{final}, R \rangle$ dove
- $P_{init}$: una colorazione iniziale dei nodi $C_{init}: V \to [a]$ tale che $a > 2\Delta$ e tale che $v$ conosce il suo colore; questo per ogni nodo $v \in V$;
- $P_{final}$: una colorazione dei nodi $C_{final}:V\to [2\Delta]$ tale che ogni nodo $v \in V$ conosca il suo colore finale $C_{final}(v)$;
- $R = \mathcal{LOCAL}\text{ model}$;
### Il protocollo
Il protocollo $\text{Rand-}2\Delta$ è basato su una tecnica più generale detta **Phase Technique**. Si presenta tale tecnica, che viene usata per risolvere con un approccio probabilistico diversi problemi distribuiti, applicata al problema in analisi.
La tecnica consiste in un protocollo che procede per **fasi**, dove in ciascuna di esse ogni nodo sceglie **casualmente** un valore da un insieme appropriato di colori. In base al colore scelto, e a quello scelto dai propri vicini, ogni nodo deve prendere la decisione finale di adottare il colore scelto e terminare il protocollo, oppure di continuare alla fase successiva. Si osservi che tutti i nodi che prendono una decisione finale formano un sottoinsieme della soluzione finale; perciò il calcolo della soluzione può continuare rimuovendo i nodi che hanno terminato il protocollo e procedendo con la nuova fase nella stessa maniera ma sul **grafo residuo**, cioè sul grafo dal quale sono rimossi i nodi che hanno terminato. Un requisito importante che una soluzione parziale deve necessariamente avere è che essa non solo deve essere corretta nel relativo sottoinsieme di vertici, ma anche rispetto all'*unione di tutti i sottoinsiemi* rimossi nelle fasi successive. Il protocollo termina quando verranno rimossi tutti i nodi dal grafo, ovvero quando tutti i nodi avranno scelto definitivamente il proprio colore.

Il protocollo $\text{Rand-}2\Delta$ dettagliato è il seguente. Si osserva che lo pseudocodice riporta un algoritmo che viene eseguito da ogni vertice $v \in V$ della rete distribuita.
![|center](adrc_img29.png)
Si osserva che il protocollo segue esattamente la tecnica appena descritta. In particolare, ad ogni fase ogni nodo $v$ sceglie un colore $c_v$ dalla palette di colori $[2\Delta]$ *uniformemente a caso*. Con $F_v$ viene indicato l'insieme dei colori scelti definitivamente dai vicini di $v$, mentre con $T_v$ viene indicato l'insieme dei colori scelti temporaneamente dai vicini di $v$ nella fase corrente. Allora se $c_v \not\in F_v \cup T_v$, significa che $c_v$ non è scelto in modo definitivo da nessun vicino di $v$, ne è stato pescato casualmente in quella fase da qualche vicino di $v$, dunque $c_v$ può diventare il colore definitivo per il nodo $v$: in tal caso, $v$ termina il protocollo. Altrimenti, il nodo $v$ aggiorna l'insieme $F_v$ con i nuovi colori definitivi impostati nella fase corrente, scarta il colore $c_v$ e partecipa alla fase successiva del protocollo.
### Correttezza
Si osserva che se tutti i nodi *terminano*, allora il protocollo $\text{Rand-}2\Delta$ calcola una $2\Delta$-colorazione legale del grafo $G$ in input.
#### Dimostrazione
La colorazione finale $C_{final}$ è una $2\Delta$-colorazione, in quanto per costruzione tutti i colori scelti dai nodi fanno parte della palette $[2\Delta]$, dunque si deve mostrare che la colorazione $C_{final}$ è una colorazione valida.
Siano $u,v$ due nodi vicini e sia per assurdo $C_{final}(v) = C_{final}(u)$. Allora i due nodi $u,v$ non possono aver terminato il protocollo nella stessa fase, in quanto $c_u = c_v$ e $c_v \in T_u, c_{u}\in T_{v}$ e dunque per la linea 8 del protocollo i due nodi non terminano. Allora siano $i,j$ le due fasi in cui i due nodi $u,v$ terminano, e sia w.l.o.g. $i<j$, ovvero $u$ termina prima di $v$. Allora dal round $i$ in poi, il colore definitivo $C_{final}(v) = c_u =c_v$ sarà presente all'interno di $F_v$ (riga 12). Dunque per costruzione del protocollo, il colore definitivo $C_{final}(v)$ scelto al tempo $j$ certamente non può appartenere a $F_v$, dunque $C_{final}(u) \neq C_{final}(v)$, contraddicendo l'ipotesi iniziale.
### Complessità
Durante l'esecuzione del protocollo $\text{Rand-}2\Delta$ tutti i nodi terminano la loro esecuzione entro $O(\log{n})$ fasi con alta probabilità $1-\frac{1}{n^c}$, per un $c>0$ sufficientemente grande.
#### Dimostrazione
Sia $v \in V$ un nodo fissato. Si calcola la probabilità che $v$ termini nella fase $j$ condizionato al fatto che non ha terminato prima, per ogni $j>0$.
Si osserva che $|T_{v} \cup F_{v}|<\Delta$ perché ogni vicino di $v$ contribuisce con al più un colore a tale insieme e $G$ è $\Delta$-regolare, cioè $|N(v)|<\Delta$. Di conseguenza, $v$ ha a disposizione *almeno* $\Delta$ colori validi per la colorazione finale nell'insieme $[2\Delta] \setminus (T_{v} \cup F_{v})$. Perciò la probabilità che $v$ scelga un colore valido per la colorazione finale, e dunque terminare, è pari a
$$
\mathbf{Pr}(c_{v} \not\in T_{v} \cup F_{v})  = \frac{|[2\Delta] \setminus (T_{v} \cup F_{v})|}{|2\Delta|} \geqslant \frac{\Delta}{2\Delta} = \frac{1}{2}
$$
Quindi, se $v$ non termina prima della fase $j$, la probabilità che termini proprio alla fase $j$ è pari alla probabilità che scelga un colore valido per terminare, dunque almeno $\frac{1}{2}$ **indipendentemente** dagli altri nodi
$$
\mathbf{Pr}(v\text{ termina alla fase }j|v \text{ non termina prima di }j) \geqslant \frac{1}{2}
$$
Allora, se $\mathcal{B}_{v,j}$ è l'evento complementare al precedente, vale che
$$
\mathbf{Pr}(\mathcal{B}_{v,j}) \leqslant \frac{1}{2}
$$
per ogni $j>0$.
Sia $\mathcal{E}_{v,i}$ l'evento
$$
\mathcal{E}_{v,i}=\text{``$v$ non termina entro la fase $i$'' }
$$
Allora per ogni $i>0$, la probabilità di tale evento è uguale alla probabilità dell'intersezione degli eventi $\mathcal{B}_{v,j}$ con $j=1,\dots,i$. Allora per la regola della catena si ottiene
$$
\begin{align}
\mathbf{Pr}\left(\mathcal{E}_{v,i}\right) &= \mathbf{Pr}\left(\bigcap_{j=1}^i \mathcal{B}_{v,j}\right)  \\
&= \mathbf{Pr}\left(\mathcal{B}_{v,i}|\bigcap_{j=1}^{i-1} \mathcal{B}_{v,j}\right) \mathbf{Pr}\left(\bigcap_{j=2}^{i-1} \mathcal{B}_{v,j}\right)  \\
&=\mathbf{Pr}\left(\mathcal{B}_{v,i}|\bigcap_{j=1}^{i-1} \mathcal{B}_{v,j}\right)\mathbf{Pr}\left(\mathcal{B}_{v,i}|\bigcap_{j=1}^{i-2} \mathcal{B}_{v,j}\right)\mathbf{Pr}\left(\bigcap_{j=2}^{i-2} \mathcal{B}_{v,j}\right) \\
&=\mathbf{Pr}\left(\mathcal{B}_{v,i}|\bigcap_{j=1}^{i-1} \mathcal{B}_{v,j}\right)\mathbf{Pr}\left(\mathcal{B}_{v,i}|\bigcap_{j=1}^{i-2} \mathcal{B}_{v,j}\right)\dots \mathbf{Pr}\left(\mathcal{B}_{v,3}|\mathcal{B}_{v,1} \cup \mathcal{B}_{v,2}\right) \mathbf{Pr}\left(\mathcal{B}_{v,2}|\mathcal{B}_{v,1}\right)\mathbf{Pr}\left(\mathcal{B}_{v,1}\right)  \\
&= \prod_{j=1}^i \mathbf{Pr}\left(\mathcal{B}_{v,j}| \bigcap_{k=1}^{j-1}\mathcal{B}_{v,k}\right) \leqslant 2^{-i}
\end{align}
$$
A questo punto, sia $\mathcal{E}_{i}$ l'evento
$$
\mathcal{E}_{i} = \text{``$\exists v \in V: v$ non termina entro la fase $i$''}
$$
Applicando l'union bound si calcola la probabilità di $\mathcal{E}$
$$
\begin{align}
\mathbf{Pr}\left(\mathcal{E}_{i}\right) &= \mathbf{Pr}\left(\bigcup_{v \in V}\mathcal{E}_{v,i}\right)  \\
&\leqslant \sum_{v \in V}\mathbf{Pr}\left(\mathcal{E}_{v,i}\right) \\
&\leqslant n 2^{-i}
\end{align}
$$
allora per $i = (c+1) \log_2{n}$, si ottiene
$$
\mathbf{Pr}(\mathcal{E}_{i}) \leqslant \frac{n}{n^{c+1}} = \frac{1}{n^c}
$$
Concludendo, dopo $i = (c+1) \log_2{n} = O(\log{n})$ fasi tutti i nodi terminano con probabilità $1-\frac{1}{n^c}$, cioè con **alta probabilità** per $c>0$.
## Protocollo $\text{Rand-}\Delta^+$
Il protocollo $\text{Rand-}2\Delta$ permette di calcolare in $O(\log{n})$ passi un $2\Delta$-coloramento di $G$ con alta probabilità. Ma $G$ è $\Delta$-regolare, dunque l'obiettivo è calcolare un $(\Delta+1)$-coloramento, abbattendo la complessità del protocollo **BCP**.
In particolare, è possibile modificare leggermente il protocollo $\text{Rand-}2\Delta$ per ottenere un protocollo che in $O(\log{n})$ passi con alta probabilità, calcola un $(\Delta+1)$-coloramento di $G$.
### Il protocollo
Tale protocollo modificato prende il nome di $\text{Rand-}\Delta^+$ e si ottiene facendo due modifiche al protocollo $\text{Rand-}2\Delta$; ad ogni fase:
1. Ogni nodo $v \in V$ che partecipa ad una fase sceglie *uniformly at random* un bit $c(v) \in\{ 0,1 \}$; se $c(v)=0$, allora $v$ non partecipa alla scelta di un colore e viene spedito per direttissima alla fase successiva;
2. Ogni nodo $v \in V$ tale che $c(v)=1$ partecipa alla scelta di un colore, in particolare ognuno di questi nodi sceglie *uniformly at random* un colore dalla palette $[\Delta+1] \setminus F_{v}$; a questo punto il protocollo prosegue come $\text{Rand-}2\Delta$, osservando che il colore zero non può essere mai scelto come colore definitivo da un nodo.
Formalmente, il protocollo $\text{Rand-}\Delta^+$ è il seguente.
![|center](adrc_img31.png)
Si osserva che se tutti i nodi *terminano*, allora il protocollo calcola una $(\Delta+1)$-colorazione legale del grafo $G$ in input. La dimostrazione è del tutto analoga a quella per il protocollo $\text{Rand-}2\Delta$.
### Complessità
Durante l'esecuzione del protocollo $\text{Rand-}\Delta^+$ tutti i nodi terminano la loro esecuzione entro $O(\log{n})$ fasi con alta probabilità $1-\frac{1}{n^c}$, per un $c>0$ sufficientemente grande.
#### Dimostrazione
Sia $v \in V$ un nodo fissato. Si calcola la probabilità che $v$ termini nella fase $j$ condizionato al fatto che non ha terminato prima, per ogni $j>0$.
Quest'ultima è la probabilità che il nodo $v$ selezioni un colore $c_v > 0$ differente dalle scelte di tutti i suoi vicini $N(v)$.
Sia per ipotesi $c_v > 0$. Allora $c_v$ deve essere diverso da ogni altro colore in $F_v$. Inoltre, per ogni $u \in N(v)$ fissato vale che
$$
\mathbf{Pr}(c_{v} = c_{u}) = \frac{1}{2} \frac{1}{\Delta+1-|F_{v}|}
$$
dato che la probabilità che $u$ scelga un colore maggiore di zero è $\frac{1}{2}$, e la probabilità di scegliere proprio il colore $c_v$ è pari a $\frac{1}{\Delta+1-|F_{v}|}$, dove $\Delta+1-|F_v|$ è la dimensione della palette dalla quale $u$ sceglie un colore a caso. Allora la probabilità che esista un nodo $u \in N(v)$ tale che $c_v = c_u$ è pari alla probabilità dell'unione degli eventi "$c_v = c_u$" per ogni $u \in N(v)$; applicando lo Union Bound, questa probabilità vale
$$
\begin{align}
\mathbf{Pr}(\exists u \in N(v): c_{v} = c_{u}) &= \mathbf{Pr}\left(\bigcup_{u \in N(v)} c_{v} = c_{u}\right)  \\
&\leqslant \sum_{u \in N(v)} \mathbf{Pr}(c_{v} = c_{u})  \\
&\leqslant (\Delta+1-|F_v|) \frac{1}{2(\Delta+1-|F_{v}|)} \\
&\leqslant \frac{1}{2}
\end{align}
$$
dove $\Delta+1-|F_v|$ è anche il numero di vicini che partecipano alla fase di $v$.
Dunque la probabilità che, se $c_v > 0$, il colore scelto da $v$ sia diverso dai colori scelti dai suoi vicini è almeno $\frac{1}{2}$. Ricordando che $c_v$ partecipa alla scelta del colore con probabilità $\frac{1}{2}$, si ottiene che la probabilità che il nodo $v$ selezioni un colore $c_v > 0$ differente dalle scelte di tutti i suoi vicini $N(v)$ vale
$$
\mathbf{Pr}(c_{v}>0 \land \forall u \in N(v): c_{v} \neq c_{u}) \geqslant \frac{1}{2} \cdot \frac{1}{2} = \frac{1}{4} 
$$
Quindi la probabilità che $v$ termini nella fase $j$ condizionato al fatto che non ha terminato prima, per ogni $j>0$, è almeno $\frac{1}{2}$: se $\mathcal{B}_{v,j}$ è l'evento complementare a questo, vale che
$$
\mathbf{Pr}(\mathcal{B}_{v,j}) \leqslant \frac{3}{4}
$$
per ogni $j>0$. Sia $\mathcal{E}_{v,i}$ l'evento
$$
\mathcal{E}_{v,i}=\text{``$v$ non termina entro la fase $i$'' }
$$
Allora per ogni $i>0$, la probabilità di tale evento è uguale alla probabilità dell'intersezione degli eventi $\mathcal{B}_{v,j}$ con $j=1,\dots,i$, e per la regola della catena si ottiene
$$
\begin{align}
\mathbf{Pr}\left(\mathcal{E}_{v,i}\right) &= \mathbf{Pr}\left(\bigcap_{j=1}^i \mathcal{B}_{v,j}\right)
= \prod_{j=1}^i \mathbf{Pr}\left(\mathcal{B}_{v,j}| \bigcap_{k=1}^{j-1}\mathcal{B}_{v,k}\right) \leqslant \left( \frac{3}{4}\right)^i
\end{align}
$$
A questo punto, sia $\mathcal{E}_{i}$ l'evento
$$
\mathcal{E}_{i} = \text{``$\exists v \in V: v$ non termina entro la fase $i$''}
$$
Applicando l'union bound si calcola la probabilità di $\mathcal{E_{i}}$
$$
\begin{align}
\mathbf{Pr}\left(\mathcal{E}_{i}\right) &= \mathbf{Pr}\left(\bigcup_{v \in V}\mathcal{E}_{v,i}\right)  \\
&\leqslant \sum_{v \in V}\mathbf{Pr}\left(\mathcal{E}_{v,i}\right) \\
&\leqslant n \left( \frac{3}{4} \right)^i
\end{align}
$$
allora per $i = (c+1) 4\log_2{n}$, si ottiene
$$
\mathbf{Pr}(\mathcal{E}_{i}) \leqslant \frac{n}{n^{c+1}} = \frac{1}{n^c}
$$
Concludendo, dopo $i = (c+1) 4\log_2{n} = O(\log{n})$ fasi tutti i nodi terminano con probabilità $1-\frac{1}{n^c}$, cioè con **alta probabilità** per $c>0$.