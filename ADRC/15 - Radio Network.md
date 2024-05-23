# Il modello Radio Network
Una **radio network** è un insieme di _dispositivi_ collocati in uno **spazio euclideo**, ognuno dei quali è dotato di un trasmettitore _radio_. Ogni nodo $v$ della rete ha **raggio di trasmissione** $r(v)>0$ fissato, entro il quale può trasmettere il suo segnale. Un qualsiasi nodo $w$ per ricevere un messaggio da $v$ deve trovarsi ad una _distanza_ minore o uguale del raggio di trasmissione $r(v):$
$$
\| v-w\| \leq r(v)
$$
Inoltre il nodo $v$ non può selezionare a quale dei suoi nodi vicini inviare il messaggio: il messaggio verrà trasmesso in a **tutti** i nodi entro il raggio di trasmissione di $v.$ 
Data una rete radio, si può costruire una **grafo diretto delle comunicazioni**, in cui esiste l'arco diretto $(u,v)$ solo se $v$ è coperto dal raggio di trasmissione di $u$.
![[radioNetworkUNO.png]]
Le reti radio sono anche dette **unknown** in quanto i nodi non hanno nessuna conoscenza in merito agli altri nodi e alla loro posizione, perciò è come considerare un grafo in cui i nodi non conoscono il loro vicinato, in cui hanno una sola porta dalla quale trasmettere il messaggio.

Infine le reti radio sono considerate essere un sistema completamente **sincrono**. Tutti i nodi condividono un **clock globale**, dunque i nodi agiscono in **time slots**. Inoltre si assume che i messaggi vengono ricevuti dai nodi nella propria area di trasmissione in un solo time slot.
## Modello di interferenza
In un contesto reale, in genere, se troppi dispositivi trasmettono nello stesso momento e in una stessa area si crea troppo _rumore_, o _interferenza_. In una rete radio quindi, se un nodo $v$ riceve più di un messaggio in uno stesso time slot, si assume che si crei una interferenza e che quindi $v$ non riceva nessun messaggio.

Più formalmente, il nodo $v$ viene informato al time slot $t>0$ se e solo se esiste un unico suo vicino che gli trasmette il messaggio al tempo $t$.
# Broadcast su reti radio
Uno dei _task_ più comuni in un ambiente reale è quello del broadcast di un segnale su una rete radio. Come accennato, questo task soffre del problema dell'interferenza, perciò si vuole progettare un protocollo che porti a termine il broadcast di un'informazione considerando questa ulteriore difficoltà.  
## Il problema
Si modella il problema considerando come grafo il _grafo di comunicazione_ $G=(V,E),$ un grafo diretto derivato dalla rete radio, dove $V$ è l'insieme dei nodi partecipanti alla rete, $\forall u,v \in V: (u,v) \in E \iff \| u-v\| \leq r(u)$. Inoltre, valgono le seguenti assunzioni:
1. **Sistema sincrono:** ogni nodo condivide uno stesso clock globale che scandisce le azioni.
2.  **Unica sorgente** $s$ che avvia il protocollo.
3. **Connettività** di $G$, non necessariamente forte, quantomeno ogni nodo deve essere raggiungibile dalla sorgente $s$. (Mi sa che deve essere fortemente connesso)
4. **Presenza di interferenze:**  un nodo $v$ viene informato al tempo $t$ se esiste un unico nodo che al tempo $t−1$ gli trasmette il messaggio.
5. **Sistema fault tolerant:** non si verificano faults (eccetto le interferenze).
## Protocollo FLOOD: BLEAH
Si considera inizialmente il protocollo FLOOD visto per la risoluzione del problema Broadcast nel modello classico. È facile trovare un controesempio in cui il protocollo non informerà mai tutti i nodi.
![[RadioNEZWORKDUE.png]]
Si consideri il grafo di comunicazione nella precedente in figura, e si supponga che la sorgente sia il nodo $s$. Al time slot $t_0$ la sorgente $s$ invia il messaggio ai nodi $u,v,w,$ e al time slot successivo i nodi $u,v,w,$  inviano il messaggio al nodo $x$, causando un'interferenza. Dopo che $u,v,w$ avranno trasmesso, entrano nello stato `done` e smettono di trasmettere. Allora, il nodo $x$ non riceve mai il messaggio, e il task non viene mai risolto.

Si osserva quindi che un protocollo completa il broadcasting *correttamente* partendo da una sorgente $s$ se esiste un time slot $t$ tale per cui ogni nodo è informato in merito al messaggio propagato, e il protocollo *termina* se esiste un istante di tempo $t$ tale per cui ogni nodo termina di eseguire ogni azione entro il time slot $t.$

Si vogliono quindi evitare le collisioni di messaggi.
## Protocollo Round-Robin (RR)
Siano i nodi univocamente identificati da un valore nell'intervallo in $[0,n-1]$. Si assume che ogni nodo conosce la propria etichetta (nota: ciascun nodo non conosce i propri vicini), e che tutti i nodi conoscano una **buona approssimazione** di $n$. Per semplicità, sia il valore esatto di $n$ noto a tutti i nodi.  
Il protocollo è scandito in **fasi** con durata complessiva di $n$ time slots ciascuno. In particolare, la fase $k$ del protocollo coinvolge i time slots che vanno da $kn$ a $(k+1)n-1$.
Ogni nodo con etichetta $i$ trasmette, nel caso in cui conosca il messaggio, solamente al time slot $i$-esimo di ogni fase. Formalmente per ogni $k≥0$, il nodo $i$ può trasmettere solo nei time slots $kn+i$. Si osserva che è possibile definire una scansione temporale delle azioni da svolgere grazie all'assunzione di sincronismo del sistema.  
Questo protocollo garantisce la non possibilità di una situazione d'interferenza, in quanto, in ogni time slot, può trasmettere un solo nodo.
Il protocollo è descritto formalmente dal seguente pseudocodice.
```python
for phase k = 0,1,2,...:
	for time = kn,kn+1,...,(k+1)n-1:
		sia x il nodo tale che id(x) = time (mod n): 
			x invia msg (se informato)
```
Si osserva esplicitamente che, per come definito, questo protocollo non termina. Si mostrerà nell'analisi del protocollo che è sufficiente eseguire $n$ fasi.
### Correttezza RR
Nel protocollo $RR$ sotto le precedenti assunzioni, tutti i nodi a distanza $k$ (distanza nel numero di hops e non euclidea) dalla sorgente $s$ vengono informati entro $k$ fasi.
#### Dimostrazione
Si dimostra per _induzione_ sulla distanza $k$.  
##### Passo base
Nella prima fase, il primo nodo a trasmettere sarà la sorgente $s$ nel time slot $id(s)$; inoltre, in tale time-slot $s$ è l'unico nodo che trasmette. Allora tutti i vicini di $s$ nel grafo delle comunicazioni, ossia i nodi a distanza $1$ da $s,$ per via del modello radio network, ricevono correttamente il messaggio.
##### Passo induttivo
Sia per ipotesi induttiva vero che ogni nodo nell'insieme $L_k$ (ovvero i nodi a distanza esattamente $k$ dalla sorgente) viene informato entro la fine della fase $k$, per ogni $k>0$. Si dimostra che l'ipotesi è vera per $k+1$. 
Sia $w \in L_{k+1}$. Per definizione di $L_{k+1}$, esiste almeno un nodo in $v \in L_k$ tale che esiste l'arco diretto $(v,w)$. Per ipotesi induttiva, entro la fine della fase $k$ il nodo $v$ è informato. Allora, se $v$ ha ricevuto il messaggio in un time slot $kn+i<kn+id(v)$, $v$ può trasmettere durante la fase $k$ stessa al tempo $kn+id(v)$; altrimenti, se ha ricevuto il messaggio a un time slot $kn+i>kn+id(v)$, $v$ può trasmettere certamente nella fase $k+1$, più precisamente al time slot $(k+1)n+id(v)$. In ogni caso, per definizione del protocollo $\text{RR}$, il nodo $v$ sarà l'unico che in quel momento trasmette il messaggio a $w$. Perciò $w$ riceve almeno una volta il messaggio senza alcuna interferenza entro il termine della fase $k+1$, e questo vale per ogni $w \in L_{k+1}.$  

Infine, per ipotesi di connettività di $G$, vale che per ogni nodo $x \in V$ esiste un intero $h$ tale che $x \in L_h.$
### Terminazione e time complexity
Sia $D$ l'eccentricità della sorgente $s$, ovvero il più lungo dei cammini minimi che partono da $s$. Sicuramente in $D$ fasi tutti i nodi della rete verranno informati grazie al protocollo $\text{RR}$. Ma nessun nodo conosce $D.$ Però è noto che in una rete connessa di $n$ nodi il diametro è al più $n−1.$ Perciò, dato che ogni nodo conosce il valore $n$, ed essendovi un clock condiviso, ognuno può stabilire che entro al più $n−1$ fasi tutti i nodi saranno informati, e quindi potranno _terminare_ l'esecuzione del protocollo.

Vale quindi il seguente teorema.
```ad-Teorema
title: Teorema
Sia la rete $G$ con $n$ nodi e sorgente $s$ di eccentricità $D$. Sotto le precedenti assunzioni il protocollo $\text{RR}$ **completa** il task in esattamente $nD \in O(n^2)$ time slots e tutti i nodi **termineranno** l'esecuzione del protocollo in $O(n^2)$ time slots.
```

Si osserva esplicitamente che $D$ non è noto, ma se lo fosse, si potrebbe terminare l'esecuzione in tempo $nD.$
Inoltre, si fa presente che, per come è stato definito il protocollo, sia $i<n$ la fase in cui un nodo $v$ viene informato, allora questo sicuramente trasmetterà nelle fasi $i+1,\ldots,n$ successive (anche nella $i-$esima se viene informato prima del time slot ad esso assegnato).
Si osserva che ciò non è necessario: non essendovi possibilità di interferenze, quando il nodo $v$ trasmette la prima volta, allora *tutti* i suoi vicini riceveranno correttamente il messaggio alla fine del time slot ad esso dedicato. Di conseguenza, è inutile far trasmettere ulteriormente il messaggio a $v$ nelle fasi successive. Il protocollo può essere quindi modificato, arrestando l'esecuzione di un agente immediatamente dopo la trasmissione.

```ad-note
Sia $v_j$ un nodo che viene informato alla fase $i,$ avente etichetta $j.$ Ci si chiede quando questo nodo può trasmettere il suo messaggio.
Lo può fare in due fasi:
- Nella fase $i$ stessa, nel caso in cui venisse informato da un suo vicino entrante $v_k$ avente etichetta $k$ minore dell'etichetta $j,$ il quale a sua volta è stato informato o da un altro nodo, sempre nella fase $i,$ avente etichetta minore di $k,$ o nella fase $i-1$ da un nodo avente etichetta maggiore di $k.$ Questo perché quando si arriverà al time slot $j$ dell'$i-$esima fase, allora $v_j$ è già informato.
- Nella fase $i+1$ nel caso in cui venisse informato nella fase $i$ da un agente avente etichetta $k$ maggiore dell'etichetta $j.$
```
### Message Complexity
La message complexity di questo protocollo è banale, in quanto ogni nodo trasmetterà lungo tutti i suoi archi uscenti una volta sola il messaggio, perciò la message complexity sarà pari al numero di archi del grafo di trasmissione:
$$
M[RR] = |E|
$$
## Generalizzazione del problema
Fin ora valeva l'assunzione che tutti i nodi hanno una conoscenza di $n$, oppure una sua _buona approssimazione_. Per esempio, se tutti i nodi hanno come approssimazione di $n$ un valore del tipo $3.5n$, l'efficienza del protocollo $\text{RR}$ in termini asintotici non cambia.  

Si supponga ora invece che i nodi abbiano un'approssimazione $N$ del numero di nodi $n$. Col protocollo $\text{RR}$ il tempo di terminazione globale, dove si evidenzia che ci si riferisce al tempo in cui tutti i nodi terminano di eseguire il protocollo, e non il tempo in cui viene completato il task del problema, che rimane invariato, sarà $O(N^2)$. Se $N \approx n^2$ il tempo di terminazione del protocollo sarà dell'ordine di $O(n^4)$, e se $N \approx 2^n$ il tempo di completamento sarà $O(2^{2n}).$

Sia il numero di nodi $n$ non noto al sistema. Si ricorre alla tecnica della **simulazione**, nella quale si provano tutti i possibili valori che i parametri mancanti (in questo caso il numero di nodi) possono assumere.  

Dato che il sistema è totalmente sincrono, si esegue il protocollo $\text{RR}$ con $n=1$, poi si ripete con $n=2$, e così via… Così facendo, esisterà un tempo $t$ nel quale verrà eseguito il protocollo col corretto valore di $n$, portando a termine così il task del broadcast su reti radio.
```python
for stage k=1,2,...:
	for phase j = 0,1,..,k:
		for time = jk,jk+1,...,(j+1)k-1:
			sia x il nodo tale che id(x) = time (mod k): 
				se x è informato, invia il msg e termina
```
Importante specificare però, che qualora un nodo abbia una etichetta $x$ maggiore del corrente valore di $n$, esso non dovrà mai essere in grado di mandare il messaggio, in quanto potrebbe esistere un altro nodo con etichetta $y\leq n$ tale che $x \equiv_n y$ e quindi potrebbero occorrere interferenze.

In questo caso, il task richiede un tempo dell'ordine di:
$$
TIME = \sum_{j=1}^n TIME(\text{time slots\_robin}(j)) = \sum_{j=1}^n\Theta(j^2) \in \Theta(n^3)
$$
### Terminazione locale
Si consideri il nodo $v_j$ con etichetta $id(v_j)=j,$ e si consideri l'esecuzione del protocollo di $RR$ di parametro $n=i$ in cui $v_j$ viene informato. Se $j<i$, si ha che $v_j$ può ricevere il messaggio da un nodo $v_k$ collegato ad esso, che ha ricevuto il messaggio in precedenza, e in tal caso lo trasmetterà direttamente nella fase $i$, altrimenti dovrà attendere la fase successiva $i+1$. Se $j>i$, allora $v_j$ non parlerà mai nella simulazione del $\text{RR}$ $i$-esima. Esisterà comunque un tempo $i^*\geq j$ in cui  $v_j$  avrà opportunità di trasmettere il messaggio ai suoi vicini.  
In entrambi i casi, per come è costruito il protocollo, quando $v_j$ trasmetterà non ci potranno essere altri nodi che trasmetteranno nello stesso istante. Perciò $v_j$ informerà tutti i suoi vicini senza interferenze, e potrà concludere localmente il suo compito, passando allo stato $\text{done}.$

Si osserva esplicitamente, che quando un nodo trasmette l'informazione, allora questo deve necessariamente terminare. Questo perché, essendo $n$ non noto, un agente non può sapere quale sia lo stage $k=n,$ ossia quello stage tale per cui si ha la certezza che il task è stato portato a termine. Di conseguenza, non si può far continuare a trasmettere continuamente come nel protocollo precedente.

```ad-note
Un nodo trasmette quando, il turno è modulo la sua etichetta, non ha mai trasmesso ed è informato.
```
### Ottimizzazione
Una ottimizzazione intuitiva, è quella di non far crescere il valore simulato di $n$ in maniera lineare, bensì in maniera _esponenziale_. Ovvero, le simulazioni del protocollo $\text{RR}$ si ripetono ponendo il parametro $n=2^i,$ per ogni $i>0$.  
```python
for stage i = 0, 1, 2, ... :
	k = 2^i
	for phase j = 0, 1, 2, ..., k:
	  for time = jk, jk+1, ... , jk+k-1:
	   if sia x il nodo tale che id(x) = time (mod k): 
		    se x è informato, invia il msg e termina
```
Per quanto riguarda il completamento del _task_, esisterà certamente un $i$ tale che $2^{i−1}<n\leq2^i$, garantendo quindi che entro $2^i$ simulazioni tutti i nodi verranno informati.  

Per il tempo di completamento invece, basta osservare che ci saranno al più $\lceil \log_2 n\rceil$ simulazioni, ognuna delle quali con complessità temporale di $O(n^2)$. Perciò vale che
$$
TIME = \sum_{i=1}^{\lceil \log_2 n\rceil}TIME(\text{time slots\_robin}(2^i)) = O(n^2 \log n) \in o(n^3)
$$
### Risultati più efficienti
Non si entra nel dettaglio, ma si fa presente che con i metodi selettivi, si può risolvere il task distribuito in tempo $O(d \cdot D \log n)$ su un grafo qualsiasi, ed è stato dimostrato che non si può fare meglio: dato un qualsiasi protocollo deterministico, l'avversario può costruire una rete deterministica tale per cui il protocollo impiega almeno $\Omega(d \cdot D \log (n/D)),$ stabilendo quali etichette assegnare agli agenti e cercando di creare interferenze. 
## Protocollo probabilistico
È possibile mostrare che, per ogni rete tale per cui il diametro è pari a $D=3,$ e il grado della rete è $d=O(n),$ si ha che per risolvere il problema del broadcast sul modello radio network è necessario tempo almeno $\Omega(n \log n).$ Tale fattore $n$ è necessario perché si deve schedulare l'invio dei messaggi da parte di ciascun agente (si ricorda che i nodi non conoscono la topologia).
### $d$-regular layered graph
Un grafo $d-$regolare layered è un grafo che può essere organizzato a livelli $L_0,L_1,\ldots,L_D,$ dove tutti gli archi vanno da qualsiasi nodo di un livello $j$ ad un qualsiasi nodo del livello $j+1,$ e il grado entrante di ciascun nodo è esattamente $d.$  
![](RadioNetwork3.png)
### Il protocollo
Si mostra un protocollo randomizzato che, su un qualunque grafo $d-$regolare layered, ottiene un tempo di completamento della forma $O(D \cdot \log n)$ con alta probabilità.
Il protocollo procede in stages, dove ogni stage è composto da $c\log{n}$ time slots. Ad ogni time slots, ogni nodo della rete, se informato, trasmette l'informazione ai vicini con probabilità $\frac{1}{d}$.
```python
BGI PROTOCOL:
for k=1,2,... stages:
	for j=1,2,...,c log(n)
		for ogni nodo x:
			if il nodo x è informato:
				x trasmette con probabilità 1/d
```
Si osserva che ad ogni iterazione interna, parlano tutti i nodi informati.
### Analisi 
Il protocollo $\text{BGI}$, con alta probabilità, completa il broadcast in  $O(D)$ stages. Dato che ogni stage richiede $O(\log n)$ time steps, il protocollo $\text{BGI}$ completa il task in $O(D \log n)$ time steps con alta probabilità.
#### Dimostrazione
##### Step 1: induzione
Per prima cosa, si dimostra che tutti i nodi in $L_{k}$ risultano informati entro la fine dello stage $k$ con alta probabilità.
Essendo il grafo organizzato a livelli $L_0,L_1,\ldots,L_D$, lo si dimostra per induzione sui livelli. 
###### Passo base
Il livello $L_0$ contiene solo la sorgente. Dato che $s$ sa di essere la sorgente, ed essendo l'unico nodo inizialmente informato, allora si può assumere che tale agente trasmetta con probabilità $1$ invece che con probabilità $1/d.$ Allora il primo livello sarà informato entro la fine del primo stage.
###### Passo induttivo
Per **ipotesi induttiva**, si assuma che entro lo stage $j>1$ tutti i nodi al livello $L_j$ sono informati (ovvero entro $jc\log n$ time slots).
Si vuole dimostrare che l'ipotesi sia vero anche per $j+1.$ 
Sia $y \in L_{j+1}.$ Si studia la probabilità con cui $y$ viene informato in un time slot della fase $j+1$.
Per definizione di grafo $d-$regolare layered, $y$ ha esattamente $d$ vicini nel livello precedente, i quali sono tutti informati per ipotesi induttiva.
Il nodo $y$ viene informato se e solo se, tra i suoi $d$ vicini al livello precedente, solo uno di questi inoltra il messaggio mentre gli altri no.
Un suo vicino trasmette correttamente con probabilità
$$
\frac{1}{d}\left(1-\frac{1}{d} \right)^{d-1}
$$
Prendendo in considerazione tutti i vicini, sia $\mathcal{E}$ l'evento: "il nodo $y$ riceve correttamente il messaggio in un time slots", allora
$$
\mathbf{Pr}\left[\mathcal{E}\right] = d\cdot\frac{1}{d}\left(1-\frac{1}{d} \right)^{d-1} = \left(1-\frac{1}{d} \right)^{d-1}
$$
E per ogni $d>1$ vale che
$$
\mathbf{Pr}\left[\mathcal{E}\right] = d\cdot\frac{1}{d}\left(1-\frac{1}{d} \right)^{d-1} > \frac{1}{8}
$$
E quindi per ogni time slots della fase $j-$esima, ogni nodo che sta nel livello $L_{j+1}$ ha probabilità costante, ad ogni time slots, di venir informato.
Sia $\mathcal{E'}$ l'evento 
$$
\mathcal{E'}:=y \ \text{non viene informato in uno stage}
$$
ossia $y$ **non** viene informato in $c \log n$ time slots indipendenti; la probabilità di tale evento vale 
$$
\mathbf{Pr}[\mathcal{E'}] < \left(1-\frac{1}{8}\right)^{c \log n} < e^{-\frac{c\log n}{8}} < \frac{1}{n^{c/8}}
$$
dove la prima disuguaglianza è vera essendo che le scelte casuali sono indipendenti, e la seconda perché vale $(1-x) < e^{-x}$ per ogni $0<x<1.$ 
Questa appena studiata è la probabilità che **un** nodo non venga informato.
Si vuole che **tutti** i nodi del livello $j+1$ vengano informati entro la fine dello stage $j+1$ con alta probabilità, dove si osserva che $|L_{j+1}|<n,$ e quindi applicando una prima volta l'union bound si ottiene che
$$
\begin{align}
\mathbf{Pr}\left[\exists x\in L_{j+1} : x\text{ non viene informato entro lo stage }j+1\right] &< n\left(\frac{1}{n^{c/8}}\right) \\
&< \frac{1}{n^{\frac{c}{8}-1}}
\end{align}
$$
Dunque si è dimostrato che tutti i nodi in $L_{k}$ sono informati entro la fine dello stage $k$ con alta probabilità.
##### Step 2: Union Bound sui livelli
Per concludere la dimostrazione, bisogna mostrare che per ogni livello $k = D < n$, tutti i nodi in $L_k$ sono informati entro la fine dello stage $k$ con alta probabilità. Per fare ciò, si calcola la probabilità dell'evento $\exists \text{ BAD STAGE}$, cioè l'evento per cui esiste uno stage in cui non tutti i nodi del relativo livello vengono informati. Si calcola tale probabilità applicando lo Union Bound sui livelli (o stage):
$$
\begin{align}
\mathbf{Pr}\left[\exists \text{ BAD STAGE} \right] &<  \sum_{j=1}^n \mathbf{Pr}(\exists x\in L_{j} : x\text{ non viene informato entro lo stage }j) \\
&< n\left(\frac{1}{n^{\frac{c}{8}-1}}\right) = \left(\frac{1}{n^{\frac{c}{8}-2}}\right) 
\end{align}
$$
Scegliendo $c>24,$ si ottiene che per ogni livello $k$ tutti i nodi in $L_k$ risultano informati entro la fine dello stage $k$ con alta probabilità.

Si osserva esplicitamente che si è assunto che i nodi conoscano $d.$
Se i nodi non conoscono $d,$ si può ricorrere nuovamente alla tecnica della simulazione. Si osserva che provando tutti i $d=2,\ldots,n,$ se il vero valore $d$ è costante, non si ha uno spreco di risorse eccessivo, ma se $d = \Omega(\log n),$ essendo che il protocollo viene simulato per tutti i $d,$ la complessità per il completamento del task aumenta a $O(d \cdot D \log n),$ mentre per la sua terminazione, dovendo provare tutti i $d,$ sale a $O(n \cdot D \log n).$
Si può effettuare, come fatto in precedenza, la ricerca binaria, trasmettendo con probabilità $1/2^i$ per $i=0,1,\ldots,\log_2n.$   
### Grafi generali
Si vuole studiare il problema anche per grafi non regolari generici. Si assuma di eseguire il protocollo $\text{BGI}$ definito in precedenza, e di trovarsi nel seguente scenario.
Si assuma, per induzione, che alla fine della fase $(j-1)-$esima, tutti i nodi in $L_{j-1}$ sono informati. Si fa presente come, in questa situazione, un agente al livello $k$ può avere archi entranti provenienti da agenti presenti nel livello $k$ stesso. Si vuole mostrare che $\text{BGI}$ con la probabilità definita in precedenza, non funziona.
![](RadioNetwork4.png)
Si consideri un nodo $v \in L_j$ non ancora informato, sia $1/d$ la probabilità con cui i nodi parlano, e $d^{'}= \deg^{(in)}(v).$ La probabilità che un suo vicino entrante informato trasmetta, e che tutti gli altri rimangano in silenzio, è
$$
\frac{1}{d}\left(1 - \frac{1}{d}\right)^{d^{'}-1}
$$
Ogni volta che il rapporto tra $d$ e $d^{'}$ è un fattore costante, si ha che esiste sempre una costante $c>0$ tale per cui
$$
\frac{1}{d}\left(1 - \frac{1}{d}\right)^{d^{'}-1} \geq c 
$$
E' fondamentale che, durante la fase $(j-1)-$esima, il numero nodi attivi che partecipano alla trasmissione (ossia il grado $d^{'}$ del nodo $v$) devono essere un fattore costante rispetto alla probabilità di trasmissione. Se il numero dei nodi è troppo distante (troppo grande o troppo piccolo), il protocollo impiegherà un tempo atteso estremamente eccessivo (o probabilità troppo alta di interferenze, o troppo bassa la probabilità che avvenga una trasmissione). I problemi sono i seguenti:
- Se i nodi hanno un vicinato variabile, ci saranno alcuni nodi in $L_j$ che avranno un vicinato interno proveniente da $L_{j-1}$ di cardinalità proporzionale a $d,$ mentre altri possono avere la maggioranza dei vicini provenienti dal livello $L_j$ stesso, e al termine della fase $j-1,$ non si può dire nulla in merito alla conoscenza dell'informazione dei nodi del livello $L_j,$ essendo queste delle variabili aleatorie.

Bisogna quindi modificare il protocollo come segue:
- Tutti i nodi che partecipano alla fase $j,$ sono quelli che sono stati informati alla fase $j-1$ precedente. Per capire il motivo di questo vincolo, si fa presente il seguente scenario: si supponga di trovarsi all'inizio della fase $j-$esima, e che per ipotesi, tutti i nodi alla fase $j-1$ sono stati informati. Allora, può esistere un agente al livello $j-$esimo che viene informato al primo passo della fase $j-$esima. Tale agente, se non si pone il vincolo in questione, può cominciare a trasmettere con probabilità $1/d,$ rendendo non valida l'analisi fatta in precedenza. L'analisi non sarebbe valida perché, per ipotesi induttiva si può dire che tutti i nodi nella fase $j-1$ sono stati informati al termine della fase $j-1$, ma non si può dire nulla in merito a quelli della fase $j.$ Ponendo questo vincolo sulla trasmissione da parte degli agenti si annulla questa problematica.
- Non si conosce il grado entrante dal livello precedente di un nodo $v\in L_j$, sia questo $|N_{j-1}^{(in)}(v)|.$ Si osserva che $1 \leq |N_{j-1}^{(in)}(v)| \leq \max_{v\in V}deg(v).$ Si esegue quindi la simulazione binaria, trasmettendo con probabilità $1/2^i$ con $i=0,\ldots, \log (\max_{v\in V}{deg(v)})$ (o $i=0,\ldots, \log n$ se non si conosce il grado massimo di un nodo). 

```python
BGI GENERAL GRAPH:
for k=1,2,... stages:
  for i = 0,1,...,log(max deg(v)):
 	for j=1,2,...,c log(n):
		for ogni nodo x:
			if il nodo x è stato informato alla fase k-1:
				x trasmette con probabilità 1/2^i

```

Si ha quindi che la probabilità che un vicino di $v$ al livello $k-1$ trasmetta correttamente in una fase $k$, alla sotto-fase $i$ è 
$$
\frac{1}{2^i} \left(1 - \frac{1}{2^i}\right)^{|N_{k-1}^{(in)}(v)|}
$$
Si ha che $|N_{k-1}^{(in)}(v)| \in \{1,2,\ldots,n-1\}.$ Allora si prende la sotto-fase in cui il protocollo trasmette con probabilità
$$
\bar{i} = \lfloor \log_2 |N_{k-1}^{(in)}(v)| \rfloor
$$
dove per tale $\bar{i}$ vengono ovviamente fatti $c \log n$ tentativi.
Si osserva che
$$
 \frac{1}{2} \leqslant \frac{|N_{k-1}^{(in)}(v)| }{2^{\bar{i}}} \leqslant 2
$$
E la binary search funziona perché
$$
d \cdot \frac{1}{d} \left(1 - \frac{1}{d}\right)^{d'} \geqslant c > 0 : \forall \frac{d^{'}}{d} = \Theta(1)
$$
E facendo la ricerca binaria ci sarà sempre un $d$ proporzionale a $d^{'}.$ 