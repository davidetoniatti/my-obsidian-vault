Nei sistemi distribuiti spesso è necessario che una singola entità coordini il lavoro delle altre entità per la risoluzione dei task. La presenza di tale entità può essere necessaria per *semplificare* lo svolgimento di un task oppure perché è richiesto dalla natura stessa del problema.
# Elezione di un leader
Il problema della scelta di un **leader** tra un insieme omogeneo e simmetrico di individui di una popolazione è noto come il **Leader Election Problem**. Generalmente il problema richiede di portare la rete da una configurazione iniziale in cui tutti i nodi sono in uno stesso stato, ad una configurazione in cui tutti i nodi saranno in uno stato *follower* eccetto uno solo che dovrà essere nello stato di *leader*. Non ci sono restrizioni su quali nodi devono iniziare il task della leader election, e nemmeno su quali nodi possono diventare leader.
Si può pensare alla leader election come un metodo che **forza** la restrizione di *iniziatore unico*, infatti eletto un leader è possibile risolvere il task con protocolli a _single initiator_ semplicemente ponendo il leader come nodo iniziatore.

Si consideri il problema sotto il classico insieme $R$ di restrizioni
$$
R = \begin{cases} \text{Total Reliability (TR)} \\ \text{Bidirectional Link (BL)} \\ \text{Connectivity (CN)} \end{cases}
$$
Allora vale il seguente teorema.
```ad-Teorema
title: Teorema
Sotto le restrizioni $R$ non è possibile risolvere il problema della leader election in modo **deterministico**.
```
Questo risultato di *impossibilità* molto forte è dato dalla condizione di **perfetta simmetria** nella rete. Il fenomeno della **perfetta simmetria** occorre quando una rete ha una struttura totalmente simmetrica, in modo che ogni nodo non è in grado di distinguersi da un altro nodo. Dunque se tutti i nodi sono in uno stato interno $\sigma$ ad uno stesso istante $t$, da quel momento in poi si comporteranno in modo identico, dato che avranno la stessa visione del mondo esterno: non potendosi comportare in maniera differente, i nodi cambieranno stato alla stessa maniera, e quindi non è possibile arrivare in una situazione in cui un nodo avrà uno stato _diverso_ dagli altri (il leader).
Il problema della leader election è anche noto come **simmetry breaking**, perché si cerca di "rompere" tale simmetria tra i nodi. Il teorema ci dice però che per fare rompere la simmetria, le restrizioni $R$ non sono sufficienti. In particolare, è necessario aggiungere la restrizione **Unique Identifier**, nella quale si assume che ogni nodo $x$ è **univocamente identificato** da un valore $id(x)$, cioè che rispetta la seguente proprietà
$$
\forall x,y \in V \ \ id(x) \neq id(y)
$$
# Elezione in un albero
Si osserva che per sistemi distribuiti in cui la topologia è quella ad albero, il problema della leader election può essere facilmente risolto tramite la [saturazione](06%20-%20Saturazione.md). Infatti è sufficiente usare la saturazione per calcolare il nodo $x$ con $id(x)$ minimo. Tale nodo sarà eletto a leader, mentre i restanti diventeranno follower.
# Elezione in un anello
Si considera ora un sistema distribuito con topologia ad **anello**. In particolare sia $R=(\{ x_{1},\dots,x_{n} \},E)$ un anello con $n$ nodi ed $n$ archi in cui ogni nodo è in grado di distinguere la direzione: ogni nodo può scegliere di mandare un messaggio a destra o a sinistra. In particolare, l'insieme degli archi è
$$
E = \{ (x_{i},x_{i+1}) : i = 1,2,\dots,n-1 \} \cup \{ (x_{n},x_{1}) \}
$$
Si osserva che gli $id$ non sono globalmente **consistenti**, ossia non è detto che $id(x_i)=i$ o che se $id(x_i) = k$ allora $id(x_{i+1})=k+1$ e $id(x_{i-1})=k-1$.

Si discutono tre protocollo diversi che risolvo il problema della leader election su questa topologia. In particolare questi protocolli permettono di individuare il nodo $x$ con $id(x)$ minimo su una rete ad anello, in modo da poter eleggere tale $x$ come leader.
## Protocollo 1: All the way!
L'idea del primo protocollo, detto **all the way**, consiste nell'inviare ogni etichetta ($id(\cdot)$) ad ogni nodo.
In particolare, quando un nodo $x$ si attiva (spontaneamente o alla ricezione di un messaggio `Election`, si vedano i dettagli nel protocollo) inoltra un messaggio in direzione concorde a tutti (ad esempio destra) con all'interno $id(x)$. Quando un nodo riceve un messaggio `Election` con $id(x)$ aggiorna la sue etichetta minima vista in precedenza con quest'ultima se necessario e inoltra in avanti il messaggio. Quando un nodo $x$ riceve un messaggio contenente $id(x)$, allora vuol dire che questo messaggio ha toccato tutti i nodi dell'anello, dunque $x$ non lo inoltra nuovamente. Si osserva che nell'anello, ad un certo istante, circolano $n$ messaggi diversi contenenti tutti gli identificativi dei nodi.
In questo modo, ogni nodo è in grado di calcolare l'identificativo minimo. Il nodo a cui appartiene l'id minimo sarà il leader, mentre gli altri saranno i follower.

Per effettuare il calcolo del minimo, ogni nodo deve sapere quanti nodi ci sono nell'anello, ovvero deve conoscere il valore del parametro $n$. Questo perché, ad esempio, un nodo non può affermare di aver considerato tutti gli id quando il proprio ha fatto il giro: per i ritardi di trasmissione, il messaggio col proprio id potrebbe arrivare prima di tutti gli altri.
Per conoscere $n$, ogni nodo $x$ inserisce nel messaggio con il proprio $id$ un **contatore** che viene incrementato ad ogni inoltro: il valore del contatore quando il messaggio torna a $x$ sarà esattamente pari ad $n$. Allora quando $x$ riceve il suo messaggio, ricava il valore di $n$ e si può comportare in due modi:
- se $x$ ha ricevuto $n$ $id$ allora calcola il minimo tra tutte le etichette per decidere se essere leader o follower;
- se $x$ ha ricevuto $k<n$ etichette allora deve attendere altre $n-k$ **nuove** etichette per poter calcolare il minimo.
Allora, ogni nodo termina localmente, riuscendo ad identificare un leader globale e comune a tutti.

Formalmente il protocollo è il seguente. L'insieme degli stati è
- $S = \{ \text{asleep, awake, follower, leader} \}$
- $S_{init} = \{ \text{asleep} \}$
- $S_{term} = \{ \text{follower, leader} \}$
```python
```

Si osserva che il protocollo appena descritto funziona anche per collegamenti _unidirezionali_ e risolve il problema più generale della _data collection_.
### Correttezza e complessità
Il protocollo è corretto, in quanto l'assunzione di Total Reliability garantisce che ogni nodo prima o poi riceverà le informazioni riguardo le etichette di tutti quanti, dunque riuscirà a calcolare il minimo.
Il protocollo ha una message complexity
$$
M(\text{all-the-way}) = O(n^2)
$$
in quanto su ogni arco passano $n$ messaggi distinti.
Si può calcolare anche una **bit complexity** del protocollo. Si devono contare il numero di bit necessari per rappresentare $id(x)$ e il contatore. Se il messaggio `id+count` si codifica in binario con $\log_{2}(id(x)+n)$, allora la bit complexity è pari a
$$
M_{bit}(\text{all-the-way})=O(n^2\log(id(x)+n))
$$
### Ideal time complexity
Si assuma che il sistema sia sincrono, che tutti i nodi inizino il protocollo ad uno stesso istante $t$ e che il tempo di trasmissione è di una sola unità fissata $\Delta = 1$; allora il tempo ideale è $n$.
Il caso peggiore è quando si attiva un solo nodo $x_1$ al tempo $t_0$. Inviando il messaggio al suo vicino $x_1$, esso si attiverà al tempo $t_{1}=t_{0}+1$. Il nodo $x_3$ si attiverà al tempo successivo, e così via fino al nodo $x_n$ che si attiverà dopo $n−1$ istanti. Da quel momento in poi, l'ultimo nodo che terminerà il processo sarà proprio $x_n$, il quale dovrà aspettare che la sua etichetta faccia il giro completo dell'anello, risultando così in una time complexity di
$$
T(\text{all-the-way}) = O(n)
$$
## Protocollo 2: As far as it can!
Modificando il protocollo precedente, si può eliminare la propagazione di messaggi superflui per ottenere. In particolare, ogni nodo $x$ blocca l'inoltro di tutti i messaggi che contengono un $id$ dal valore più grande di quello che $x$ momentaneamente ritiene minimo. Allora un messaggio con un certo $id$ viaggia in avanti più a lungo che può (**as far as it can**) e non in ogni caso (all the way).
Certamente il nodo con etichetta minima sarà l'unico a veder ricevere un messaggio con la propria etichetta, perciò potrà terminare _localmente_ il suo compito. Sia questo nodo $l$. Per far terminare anche gli altri nodi, il nodo $l$ dovrà notificare (in broadcast) gli altri nodi che lui è il leader.
Si osserva che il costo aggiuntivo in bit complexity non è eccessivo, in quanto tutti i nodi certamente convergeranno su una singola etichetta minima, e quindi basterà un solo bit per terminare il task.
### Complessità
Il caso peggiore in termini di message complexity si ottiene quando gli $id$ seguono un ordine crescente di valori nel senso in cui il messaggio viene inoltrato.
Nel caso peggio, inizia il protocollo il nodo con $id$ pari a $n$, ma il suo messaggio verrà bloccato dal nodo $1$. Dopodiché si attiva in nodo $n−1$, il cui messaggio verrà bloccato dopo $2$ passi sempre dal nodo $1$. Il processo continua in questo modo fino all'attivazione del nodo $2$, il cui messaggio farà quasi un giro completo dell'anello fino a bloccarsi al nodo $1$. Infine si sveglia il nodo $1$ il cui messaggio farà il giro completo dell'anello, scatenando così il broadcast di notifica di fine protocollo.
In questa situazione critica, contando anche il broadcast finale, la message complexity è pari a
$$
 n+ \sum_{i=1}^n i = n + \frac{n(n-1)}{2}
$$
dunque
$$
M(\text{as-far}) = O(n^2)
$$
Il caso migliore invece si ottiene quando il nodo con etichetta minima inizia il protocollo per primo e la sua etichetta riesce a fare il giro completo prima che le altre vengano inoltrate. In particolare il messaggio con etichetta minima viene inoltrato $n$ volte, mentre ogni messaggio con etichetta maggiore viene inoltrato una sola volta.
In questa situazione, contando il broadcast finale, si ottiene message complexity
$$
n+(n-1)+n
$$
## Protocollo 3: Stage technique
Nel protocollo *as far as it can* e nel protocollo *all the way* gli $id$ viaggiano in modo incontrollato lungo l'anello. In particolare, nel protocollo *as far as it can* viaggiano finché non vengono filtrati da un nodo con $id$ più basso, mentre nel protocollo *all the way* viaggiano lungo l'intero anello.
Un'idea è quella di **controllare** la trasmissione di un $id$ ad una distanza **limitata** e di avere dei **feedback** per bloccarne o estenderne la trasmissione a distanze maggiori.

Più precisamente, un nodo attivo $x$ esegue il protocollo in **stage**, dove nello stage $i$ il nodo $x$ inoltra a destra e a sinistra la sua etichetta $id(x)$ fino a una distanza massima di $2^{i-1}$ salti.
![](Pasted%20image%2020240327163856.png)
Quando un nodo $y$ riceve $id(x)$ la inoltra al nodo successivo solo se $id(y) > id(x)$; in questo caso, certamente $y$ non sarà mai eletto leader, dunque $y$ passa in uno stato $\text{defeated}$, nel quale $y$ potrà solamente inoltrare messaggi inviati da altri nodi.
Se $id(x)$ arriva a distanza $2^{i-1}$ allora verrà rispedito indietro al mittente con un messaggio di *feedback*. Se $x$ riceve il feedback da entrambi i lati, allora tutti i nodi a distanza $2^{i-1}$ da lui hanno etichette maggiori, dunque deve inoltrare $id(x)$ a distanza maggiore. In tale caso, si dice che il nodo $x$ è *sopravvissuto* allo stage $i$ e parteciperà allo stage $i+1$.
Se $id(x)$ non riceve il feedback da uno o tutti e due i lati, allora esiste almeno un nodo $z$ entro $2^{i-1}$ salti che ha etichetta minore $id(z)<id(x)$: allora ad un certo punto il nodo $x$ passerà in stato $\text{defeated}$ per colpa di $id(z)$.
![](Pasted%20image%2020240327164621.png)
Infine, quando un nodo $x^*$ riceve da un lato il messaggio inviato dall'altro lato, allora $x^*$ potrà affermare di essere il leader. A questo punto $x^*$ invia una notifica in broadcast a tutti gli altri nodi, che passeranno dallo stato $\text{defeated}$ allo stato $\text{follower}$.

Riassumendo:
- in ogni **stage** $i$ ci sono dei nodi candidati al ruolo di leader (i nodi sopravvissuti allo stage $i-1$);
- ogni candidato invia il suo $id$ da entrambi i lati dell'anello;
- il messaggio $id$ viaggia finché: non incontra un nodo con $id$ minore oppure non arriva a distanza $2^{i-1}$;
- se $id$ arriva a distanza $2^{i-1}$ (aka non incontra nessun $id$ minore), allora torna indietro tramite messaggi di *feedback*;
- se un candidato riceve i messaggi di feedback da entrambi i lati, rimane candidato per lo stage $i+1$.

Nel protocollo valgono le seguenti tre *regole*:
1. Se un nodo candidato riceve il suo $id$ dal lato opposto a quello da cui l'ha mandato, diventa leader e lo notifica agli altri nodi;
2. Se un nodo candidato riceve un $id$ minore del suo allora passa nello stato $\text{defeated}$;
3. Un nodo nello stato $\text{defeated}$ può solamente inoltrare messaggi di altri nodi, e passerà nello stato $\text{follower}$  quando riceve la notifica dal leader.

Si definisce formalmente il protocollo; l'insieme degli stati è il seguente
- $S = \{ \text{asleep, candidate, defeated, follower, leader} \}$
- $S_{init} = \{ \text{asleep} \}$
- $S_{term} = \{ \text{follower, leader} \}$

```python
```

Si osserva che durante l'esecuzione del protocollo è possibile che diversi nodi si trovino in diversi stages. L'importante è che i nodi rispettino l'ordine delle etichette e si comportino come descritto nel protocollo.
### Correttezza e terminazione
Si osserva che in un tempo finito il protocollo porta sempre il sistema nello stato finale in cui si ha un leader e tutti follower. Infatti, dopo un tempo finito il nodo con etichetta minima si deve necessariamente svegliare. Tale nodo ad un certo stage $i^*$ riceverà da un lato il messaggio che ha inviato dal lato opposto, dove
$$
i^* = \min_{i} \{ 2^{i-1} \leqslant n \} \iff i^* = O(\log_{2}n)
$$
Dunque il numero di stage necessari e sufficienti per far terminare il protocollo è $O(\log n)$. Dato che ogni stage dura per un tempo finito, ne segue che in un tempo finito viene eletto un leader che notifica tutti gli altri nodi rendendoli follower, facendoli terminare tutti correttamente.
### Complessità
Per quanto riguarda la message complexity, si contano il numero di messaggi che vengono inviati in ogni stage del protocollo. Si definisce la quantità
$$
\begin{align}
n_{i} &:= \text{numero di nodi che superano lo stage } i-1  \\
&:= \text{numero di nodi che partecipano allo stage }i
\end{align}
$$
Si osserva che un nodo $x$ supera lo stage $i-1$ quando $id(x)$ è minore di $2^{i-2}$ $id$ dei nodi alla sua destra e di $2^{i-2}$ $id$ dei nodi alla sua sinistra. Dunque la fase $i-1$ viene superata da <u>al più</u> un solo nodo ogni $2^{i-2}+1$ nodi; quindi vale che
$$
n_{i} \leqslant \frac{n}{2^{i-1}+1}
$$
![](Pasted%20image%2020240328164046.png)

Sia $i>1$; si considerano i messaggi inviati nello stage $i$. Nel caso peggiore, la situazione è la seguente:
- **Messaggi** ***"forth"***: i messaggi di tipo *forth* inviati nello stage $i$ percorrono una distanza di $2^{i-1}$ salti in entrambe le direzioni. Dato che superano lo stage $i$  $n_{i+1}$ candidati, allora vengono inviati al più $n_{i+1}\cdot2\cdot2^{i-1}$ messaggi di tipo *forth*;
- **Messaggi** ***"feedback"***: Per ogni nodo che supera lo stage $i$ vengono necessariamente trasmessi $2\cdot 2^{i-1}$ messaggi *feedback*; ogni nodo che non supera la fase $i$ invece potrà ricevere al più un messaggio *feedback*, per un totale di $(n_{i}-n_{i+1})2^{i-1}$ messaggi ulteriori. Complessivamente si trasmettono un numero di messaggi *feedback* pari a $$n_{i+1}\cdot 2 \cdot 2^{i-1} + \underbrace{(n_{i}-n_{i+1})}_{\text{nodi che non superano lo stage }i}2^{i-1}$$
Dunque il totale dei messaggi inviati per ogni stage $i>1$ è al più
$$
\begin{aligned}
n_i \cdot 2 \cdot 2^{i-1}+n_{i+1} \cdot 2 \cdot 2^{i-1}+\left(n_i-n_{i+1}\right) \cdot 2^{i-1} & =2^{i-1} \cdot\left(3 \cdot n_i+n_{i+1}\right) \\
& \leq 2^{i-1} \cdot\left(3 \cdot\left\lfloor\frac{n}{2^{i-2}+1}\right\rfloor+\left\lfloor\frac{n}{2^{i-1}+1}\right\rfloor\right) \\
& <\frac{3 n \cdot 2^{i-1}}{2^{i-2}+1}+\frac{n \cdot 2^{i-1}}{2^{i-1}+1} \\
& <\frac{3 n \cdot 2^{i-1}}{2^{i-2}+1}+\frac{n \cdot 2 \text { 六 }}{2 / 1} \\
& =6 n+n \\
& =7 n
\end{aligned}
$$

Per quanto riguarda il primo stage invece $(i=1)$, il caso peggiore si ha quando tutti i nodi sono iniziatori. In questo caso si ha che
$$
n_2 \leq \frac{n}{2^0+1}=\frac{n}{2}
$$

I nodi che superano il primo stage hanno un costo individuale di
$$
\left\{\begin{array}{l}
2 \text { per msg "forth" } \\
2 \text { per msg "feedback" }
\end{array} \Longrightarrow 4 \cdot n_2\right. \text { messaggi totali }
$$
Gli altri invece, quelli che non lo superano, hanno un costo individuale di
$$
\left\{\begin{array}{l}
2 \text { per } m s g \text { "forth" } \\
1 \text { per msg "feedback" }
\end{array} \Longrightarrow 3 \cdot\left(n-n_2\right)\right. \text { messaggi totali }
$$
In totale quindi vale
$$
4 n_2+3 n-3 n_2=n_2+3 n<4 n
$$

Combinando il tutto, si ottiene che il numero totale dei messaggi inviati è pari a 
$$
\begin{aligned}
M(\text{STAGE TECHNIQUE}) & \leq \sum_{i=1}^{\log n} 7 n+\overbrace{O(n)}^{\text{stage }1} \\
& =7 n \cdot \log n+O(n) \\
& =O(n\log n)
\end{aligned}
$$
# Elezione in una mesh
#TODO cazzata da fare