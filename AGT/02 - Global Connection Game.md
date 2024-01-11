La teoria dei giochi fornisce una struttura per modellare come individui indipendenti ed egoisti possano generare una rete affinché vengano ottimizzati i costi e la qualità delle operazioni che questi devono sostenere su di essa. Questi modelli facilitano a loro volta uno studio quantitativo del trade-off tra efficienza e stabilità nella formazione di reti.

In particolar modo, possono essere utilizzati per modellare:
- Formazioni di reti sociali.
- Come sottoreti si connettono in reti di computer.
- Formazione di reti che connettono i partecipanti tra loro per lo scambio di files.

In generale, i **network formation games** modellano maniere distinte nelle quali agenti egoisti possono creare e valutare una rete. Si studiano due modelli:
- **Global Connection Game**;
- **Local Connection Game**.
Entrambi i modelli hanno come obiettivo quello di risolvere due questioni concorrenti. Infatti, i giocatori vogliono:
- Minimizzare il costo impiegato nella costruzione della rete.
- Assicurarsi che la rete gli fornisca un'alta qualità di servizio.

Per tutti i giochi considerati, si usa l'equilibrio di Nash come concetto di soluzione. Le reti corrispondenti ad un equilibrio di Nash vengono definite *stabili*.
Per valutare la qualità generale della rete, si fa riferimento al *costo sociale*, ossia la somma dei costi di tutti i giocatori. Ci si riferisce alle reti che ottimizzano il costo sociale come reti *ottime* o *socialmente efficienti*.
In generale, i giocatori non sono interessati alla formazione di una rete ottima, tanto quanto a costruire una rete efficiente impiegando il minor costo individuale possibile. 
Si vuole quindi dare una delimitazione alla perdita di efficienza di una rete dovuto al comportamento egoistico dei giocatori, ossia al raggiungimento di un profilo di strategia stabile.
# Modello del Global Connection Game
Si ha un grafo diretto $G=(V,E)$, con pesi degli archi $c_e$ non negativi per tutti gli archi $e \in E$. Si hanno $k$ giocatori, e ciascun giocatore $i$ ha associato uno specifico nodo sorgente $s_i$ ed un nodo pozzo $t_i$, dove lo stesso nodo può essere una sorgente o un pozzo per molteplici giocatori.
L'obiettivo del giocatore $i$ è quello di costruire una rete nella quale $t_i$ è raggiungibile da $s_i$, pagando il minor costo possibile. Una strategia per il giocatore $i$ è un cammino $P_i$ da $s_i$ a $t_i$ in $G$. Scegliendo $P_i$, il giocatore $i$ partecipa alla costruzione di tutti gli archi in $P_i$ nella rete finale. Dato un vettore di strategia $S$, si definisce la *rete costruita* come $N(S)=\bigcup_i P_i$.
Si deve ora allocare il costo di ciascun arco nella rete ai giocatori che lo utilizzano. Questo permetterà ai giocatori di valutare l'utilità di ciascuna strategia.
Per fare ciò, si considera il meccanismo che divide il costo di un arco $e$ equamente tra tutti i giocatori per i quali la loro strategia contiene $e$, ossia, sia $k_e(S)$ il numero di giocatori per i quali il loro cammino contiene l'arco $e$, allora tale arco assegna una frazione di costo pari a $c_e/k_e(S)$ a ciascun giocatore che lo utilizza. Quindi, il costo totale impiegato dal giocatore $i$ per il vettore strategia $S$ è dato da
$$
\textnormal{cost}_i(S) = \sum_{e \in P_i}c_e/k_e(S)
$$
In altri termini, il costo della rete costruita è condiviso da tutti i giocatori mediante la formula appena definita. Si osserva quindi esplicitamente che il costo totale assegnato a tutti i giocatori è esattamente il costo della rete costruita.
Si fa presente come, per comodità di notazione, se $S$ è intuibile dal contesto, si scriverà $k_e$ invece di $k_e(S)$.
Ci si riferisce a questo schema di divisione di costi come *fair* o *Shapley cost-sharing mechanism*. 
L'obiettivo sociale di questo gioco è quindi il costo della rete costruita.

Si osserva che, a differenza dei giochi di routing, i giocatori **non** sono sensibili agli effetti di congestione. Dunque, mentre nei giochi di routing i giocatori sono incentivati a cercare cammini poco trafficati, nel global connection game la condivisione di archi è incoraggiata, dato che l'unico obiettivo è quello di minimizzare i costi.

A rigore di quanto detto in precedenza, si evidenziano i seguenti punti:
- Si usa l'equilibrio di Nash come *concetto di soluzione*.
- Un vettore di strategia $S$ è un equilibrio di Nash se nessun giocatore può migliorare il suo payoff cambiando strategia.
- Dato un vettore di strategia $S$, la rete $N(S)$ è stabile se $S$ è un equilibrio di Nash.
- Per valutare la qualità generale di una rete, si considera il *costo sociale*, ossia, la somma dei costi di tutti i giocatori:$$ \textnormal{cost}(S) = \sum_i \textnormal{cost}_i(S)$$
- Una rete è *ottimale* o *socialmente ottimale* se minimizza il costo sociale.
## Esempio
Sia $G$ il grafo mostrato in figura.
![02-agt_img01|center|500](02-agt_img01.png)
In questo esempio, la rete socialmente ottima è quella avente gli archi evidenziati in rosso. Il costo di tale rete, e quindi il costo dell'ottimo sociale, è pari alla somma dei pesi degli archi che costituiscono la rete, ossia $13$. Si osserva che, sia $N(S)$ la rete formata per l'adozione da parte dei giocatori del profilo di strategia $S$ esplicitato in figura, vale 
$$
\textnormal{cost}(S) = \sum_{e \in N(S)}c_e
$$
Inoltre, si fa presente che, dato un grafo $G$, la *rete ottimale* è il sottografo meno costoso di $G$ contenente un cammino da $s_i$ a $t_i$ per ogni $i$. Nello scenario in figura, si ha che $\textnormal{cost}_1=7$ e $\textnormal{cost}_2 =6$.
![02-agt_img02|center|500](02-agt_img02.png)
Si osserva che, per le strategie adottate dai player, la rete non risulta essere stabile. Infatti, il profilo di strategia per il quale si forma la rete ottimale non è un equilibrio di Nash, perché il primo giocatore può ridurre il suo costo scegliendo l'arco superiore. A rigore di quanto appena detto, si evidenzia ancora come l'obiettivo dei giocatori non è quello di cooperare per formare una rete ottimale, bensì quella di raggiungere il proprio nodo pozzo dal proprio nodo sorgente scegliendo il cammino che garantisce la minor spesa individuale. Avendo scelto il cammino superiore, il primo giocatore paga $\textnormal{cost}_1=6$, mentre il secondo giocatore si ritrova a sostenere individualmente il costo dei primi due archi del cammino precedente, dovendo spendere $\textnormal{cost}_2=11$. 
![02-agt_img03|center|500](02-agt_img03.png)
Anche la rete definita a partire dalla nuova scelta dei cammini non risulta essere stabile. Infatti, tale profilo di strategia non è un equilibrio di Nash. Questo perché il secondo giocatore ottimizza il suo costo individuale scegliendo l'arco inferiore. Tale strategie rende la rete stabile, portando alla definizione di una rete avente costo sociale pari a $16$.
![02-agt_img04|center|500](02-agt_img04.png)
# PoA e PoS del Global Connection Game
Come detto in precedenza, si vuole studiare come il comportamento egoistico dei giocatori, definito dalla scelta di un cammino che risulti avere costo minimo dal nodo sorgente al nodo pozzo, vada a degradare il valore della soluzione ottima, ossia della rete di costo minimo.
Per fare ciò, si cerca di dare una delimitazione al prezzo dell'anarchia e al prezzo della stabilità del gioco. In particolar modo, ci si chiede se
- Esiste sempre una rete stabile.
- E' possibile dare una delimitazione al Prezzo dell'Anarchia.
- E' possibile dare una delimitazione al Prezzo della Stabilità.
- La versione ripetuta del gioco converge sempre ad una rete stabile.

Si definiscono quindi le misure di inefficienza per un grafo $G$ per il Global connection Game.
```ad-lemma
title: Definizione
Sia $G=(V,E)$ un grafo diretto. Sia $S_G^*$ l'ottimo sociale per $G$ nel Global Connection Game.
Si definisce il prezzo dell'anarchia del Global connection Game per il grafo $G$ come
$$
\textnormal{PoA del gioco in }G = \textnormal{max}_{S \textnormal{ t.c } S \textnormal{ è un equilibrio di Nash}} \frac{\textnormal{cost}(S)}{\textnormal{cost}(S_G^*)}
$$
Si definisce il prezzo della stabilità del Global connection Game per il grafo $G$ come
$$
\textnormal{PoS del gioco in }G = \textnormal{min}_{S \textnormal{ t.c } S \textnormal{ è un equilibrio di Nash}} \frac{\textnormal{cost}(S)}{\textnormal{cost}(S_G^*)}
$$
```
Tali misure vogliono essere delimitate nel caso peggiore, si definiscono quindi PoA e PoS per il Global connection game.
```ad-lemma
title: Definizione
Il prezzo dell'anarchia del Global Connection Game è definito come
$$
\begin{align}
\textnormal{PoA del gioco }&= \textnormal{max}_G \textnormal{ PoA del gioco in }G \\
&= \textnormal{max}_G \left(\textnormal{ max}_{S \textnormal{ t.c } S \textnormal{ è un equilibrio di Nash}} \frac{\textnormal{cost}(S)}{\textnormal{cost}(S_G^*)}\right)
\end{align}
$$
Il prezzo della stabilità del Global Connection Game è definito come
$$
\begin{align}
\textnormal{PoS del gioco }&= \textnormal{max}_G \textnormal{ PoS del gioco in }G \\
&= \textnormal{max}_G \left(  \textnormal{min}_{S \textnormal{ t.c } S \textnormal{ è un equilibrio di Nash}} \frac{\textnormal{cost}(S)}{\textnormal{cost}(S_G^*)} \right)
\end{align}
$$
```
# Bound sul prezzo dell'anarchia
Si analizza ora il prezzo dell'anarchia.
## Lower bound
Per mostrare un **lower bound** al prezzo dell'anarchia, si costruisce un particolare grafo $G$ che contiene un equilibrio di Nash $S$ tale che
$$
\frac{\textnormal{cost}(S)}{\textnormal{cost}(S_G^*)} = k
$$
dove $k$ è il numero di giocatori. Cosi facendo si ottiene che il prezzo dell'anarchia è $\geq k$.

Sia $G$ il grafo seguente.
![02-agt_img05|center|500](02-agt_img05.png)
in cui ciascun giocatore ha lo stesso nodo sorgente $s$ e lo stesso nodo destinazione $t$. I costi degli archi sono $k$ per l'arco superiore e $1+\varepsilon$ per l'arco inferiore, con $\varepsilon > 0$ e arbitrariamente piccolo.
Nell'ottimo sociale, tutti i giocatori scelgono l'arco inferiore. Questo esito è anche un equilibrio di Nash. La rete costruita per questo profilo di strategia, quindi, ha costo $1+\varepsilon$ e risulta essere stabile. Inoltre è il miglior equilibrio per questa istanza, infatti il prezzo della stabilità del gioco per questo grafo è pari ad $1+\varepsilon$.
D'altra parte, si supponga che tutti i giocatori scelgano l'arco superiore. Ogni giocatore incorre quindi in costo $1$, e nessuno è incentivato a deviare verso l'arco inferiore, essendo che pagherebbe il costo maggiore di $1 + \varepsilon$. Quest'altro profilo di strategia è quindi un secondo equilibrio di Nash, ed ha costo $k$, e risulta essere il peggior equilibrio per l'istanza presentata. Il prezzo della stabilità del gioco per questo grafo è quindi esattamente pari a $k$ e dunque il prezzo dell'anarchia per il gioco deve essere **maggiore o uguale** a $k$.
## Upper Bound
Si da ora un **upper bound** sul prezzo dell'anarchia. In particolare, dato un equilibrio di Nash e l'ottimo sociale, si dimostra che il costo sociale per tale equilibrio è al più $k$ volte l'ottimo sociale.
Per fare ciò, si da un upper bound al costo di ogni singolo player, mostrando come ciascun giocatore $i$ per l'equilibrio di Nash paga al più l'ottimo. Facendo la somma dei costi di ogni giocatore, si avrà che il costo totale della rete è al più $k$ volte l'ottimo.
### Teorema
Il prezzo dell'anarchia nel Global Connection Game con $k$ giocatori è al più $k$.
#### Dimostrazione
Sia $G=(V,E)$ un grafo diretto fissato. Sia $S$ un profilo di strategia tale per cui $S$ è un equilibrio di Nash, e sia $S^*$ un profilo di strategia che minimizza il costo sociale. 
Si vuole mostrare che il costo sociale del profilo di strategia $S$ è al più $k$ volte il costo sociale di $S^*$.
Per ogni giocatore $j$, si definisce $\pi_j$ il cammino minimo in $G$ da $s_j$ a $t_j$.
Sia ora $i$ un giocatore. Allora $i$, per il profilo di strategia $S$, non può migliorare il suo payoff cambiando strategia, essendo $S$ un equilibrio di Nash. Dunque vale
$$
\textnormal{cost}_i(S) \leq \textnormal{cost}_i(S_{-i},\pi_i)
$$
Ossia, il costo del player $i$ per l'equilibrio di Nash scelto è minore o uguale del costo del profilo di strategia in cui tutti i restanti giocatori mantengono la strategia in $S$, mentre $i$ gioca come strategia un cammino minimo dalla sua sorgente al suo pozzo.

Sia ora $d_G(s_i,t_i)$ la distanza in $G$ da $s_i$ a $t_i$, ossia la lunghezza del cammino minimo $\pi_i$. Si osserva che, scegliendo come strategia $\pi_i$, il giocatore $i$ paga al più la lunghezza di $\pi_i$. Infatti, nel caso in cui la strategia di $i$ sia l'unica a comprendere gli archi di $\pi_i$, allora $i$ paga individualmente il costo del cammino. Altrimenti, alcuni degli archi di $\pi_i$ sono compresi nella strategia di altri giocatori $j \neq i$, i quali costi andranno suddivisi. Di conseguenza, vale che
$$
\textnormal{cost}_i(S) \leq \textnormal{cost}_i(S_{-i},\pi_i) \leq d_G(s_i,t_i)
$$

Sia ora $N(S^*)$ la rete ottima, ossia la rete di costo minimo contenente per ogni $j$ un cammino da $s_j$ a $t_j$. Allora, per il giocatore $i$ scelto in precedenza, $N(S^*)$ conterrà almeno un cammino da $s_i$ a $t_i$, sia questo $\pi$. Il costo di $\pi$ è sicuramente minore o uguale del costo della soluzione ottima, essendo che $\pi \subseteq N(S^*)$, ed essendo un cammino da $s_i$ a $t_i$ tale cammino non può avere costo minore del cammino minimo in $G$ tra questi due ultimi nodi, perché se così non fosse, $\pi_i$ non sarebbe un cammino minimo e ciò è, per ipotesi, assurdo. Vale quindi che
$$
d_G(s_i,t_i) \leq \textnormal{costo di }\pi \leq \textnormal{cost}(S^*)
$$
Ciò dimostra che 
$$
\textnormal{cost}_i(S) \leq \textnormal{cost}(S^*)
$$

Essendo che $\textnormal{cost}(S)=\sum_{i}\textnormal{cost}_i(S)$, ed essendo il gioco definito per $k$ giocatori, si ha che 
$$
\textnormal{cost}(S) \leq k \textnormal{ cost}(S^*)
$$
```ad-note
La notazione $\pi \subseteq N(S)$ è corretta, potendo considerare una rete come un insieme di archi.
```
# Bound al Prezzo della stabilità
Si mostra ora una delimitazione al prezzo della stabilità.
## Lower bound
Per mostrare un **lower bound** al prezzo della stabilità, si costruisce un particolare grafo $G$ che contiene un **unico** equilibrio di Nash $S$ tale che
$$
\frac{\textnormal{cost}(S)}{\textnormal{cost}(S_G^*)} = \mathcal{H}_{k}
$$
dove $k$ è il numero di giocatori. Cosi facendo si ottiene che il prezzo della stabilità è $\geq \mathcal{H}_{k}$.

Sia $\varepsilon >0$ un valore arbitrariamente piccolo e sia $G$ il grafo seguente
![02-agt_img06|center|500](02-agt_img06.png)
in cui i $k$ giocatori hanno tutti lo stesso nodo sorgente $t$ e in cui per ogni $i\in\{1,\ldots,k\}$, l'arco $(s_i,t)$ ha costo $1/i$.
Nell'esito ottimale, ogni giocatore $i$ sceglie il cammino $s_i \rightarrow v \rightarrow t$ ed il costo della rete formata è $1+\varepsilon$. Questo non è un equilibrio di Nash, perché il giocatore $k$ può diminuire la sua spesa da $(1+\varepsilon)/k$ ad $1/k$ scegliendo come strategia il cammino $s_i \rightarrow t$. Inoltre, questo cammino è proprio una strategia dominante per il $k-$esimo giocatore. Induttivamente, si può estendere lo stesso ragionamento per i giocatori $k-1,k-2,\ldots,1$, e ciò mostra che l'unico equilibrio di Nash è l'esito in cui ciascun giocatore sceglie il proprio cammino diretto verso il nodo pozzo. Il costo di questo equilibrio è esattamente il $k-$esimo numero armonico $\mathcal{H}_k = \sum_{i=1}^k (1/i)$, che è circa $\ln k$. Il prezzo della stabilità può essere quindi arbitrariamente, a seconda di $\varepsilon$, vicino a $\mathcal{H}_k$ per questo esempio. Dunque il prezzo della stabilità per il gioco è arbitrariamente, a seconda di $\varepsilon$, maggiore o uguale ad un valore vicino ad $\mathcal{H}_k$.
## Upper bound e metodo della funzione potenziale
Nelle dimostrazioni dei bound precedenti, è stato dato per vero che ogni istanza del global connection game ammette un equilibrio di Nash. 
Dunque, si dimostreranno i seguenti teoremi.
```ad-theorem
title: Teorema
Ogni istanza del GCG ha un equilibrio di Nash puro, che può essere ottenuto giocando in modo ripetuto con la better response dynamics.
```
Una dinamica di best response è un processo di gioco iterativo nel quale, ad ogni passo, i giocatori selezionano una strategia che risulta essere una best response alle strategie scegli da tutti gli altri giocatori al passo precedente.
```ad-theorem
title: Teorema
Il prezzo della stabilità nel global connection game con $k$ giocatori è al più $\mathcal{H}_k$, il $k-$esimo numero armonico.
```
Per dimostrare i due teoremi si utilizza il **metodo della funzione potenziale**; in particolare, verranno utilizzati due risultati validi per una classe di giochi detta **giochi potenziali**.
### Metodo della funzione potenziale
Il **metodo della funzione potenziale** è una tecnica che permette di dare una delimitazione sul prezzo della stabilità di un gioco utilizzando una funzione potenziale.
```ad-lemma
title: Definizione (Funzione potenziale esatta)
Per ogni gioco finito, una funzione potenziale esatta $\Phi$ è una funzione che mappa ogni vettore di strategia $S$ in valori reali e che soddisfa la seguenti condizione:
$\forall S=(S_1,\ldots,S_k),S_i^{'} \neq S_i$, sia $S^{'}=(S_{-i},S_{i}^{'})$, allora
$$
\Phi(S)-\Phi(S^{'}) = \textnormal{cost}_i(S)-\textnormal{cost}_i(S^{'})
$$
```
In altre parole, se lo stato corrente del gioco è $S$, e il giocatore $i$ cambia strategia da $S_i$ a $S_i^{'}$, allora il risparmio in cui incorre $i$ corrisponde esattamente al decremento nel valore della funzione potenziale. Un gioco che possiede una funzione potenziale esatta prende il nome di *gioco potenziale*. Un gioco può avere al più una singola funzione potenziale esatta.

La presenza di una funzione potenziale esatta per un gioco comporta diverse implicazioni per l'esistenza e la convergenza ad un equilibrio.
#### Teorema 1: esistenza NE
Ogni gioco potenziale ha almeno un equilibrio di Nash puro, ossia il vettore di strategia $S$ che minimizza $\Phi(S)$.
##### Dimostrazione
Sia $\Phi$ una funzione potenziale per il gioco, e sia $S$ un vettore di strategia puro che minimizza $\Phi(S)$. Si consideri una qualsiasi mossa da parte di un giocatore $i$, che risulta in un nuovo vettore di strategia $S^{'}$. 
Essendo $S$ il vettore che minimizza $\Phi(S)$, vale
$$
\Phi(S^{'})\geq \Phi(S)
$$
Quindi, $\Phi(S)-\Phi(S^{'}) \leq 0$.
Per definizione di funzione potenziale, si ha
$$
\Phi(S)-\Phi(S^{'}) = \textnormal{cost}_i(S)-\textnormal{cost}_i(S^{'})$$
e ciò implica che
$$
\textnormal{cost}_i(S)\leq \textnormal{cost}_i(S^{'})
$$
Quindi il giocatore $i$ non può diminuire il suo costo, e per tale motivo $S$ risulta essere un equilibrio di Nash.

Si osserva ora che ogni stato $S$, con la proprietà che $\Phi$ non può essere diminuita alterando una qualsiasi strategia in $S$, è un equilibrio di Nash per l'argomentazione appena fatta. 
Ancor di più, la dinamica di best response simula una ricerca locale su $\Phi$. Queste osservazioni implicano il seguente risultato
#### Teorema 2: convergenza NE
In ogni gioco potenziale finito, la dinamica di best response converge sempre ad un equilibrio di Nash.
##### Dimostrazione
La dinamica di best response simula una ricerca locale su $\Phi$, quindi:
1. Ogni mossa riduce strettamente $\Phi$.
2. Per definizione di gioco finito, si hanno un numero finito di soluzioni.
Per il teorema visto in precedenza, che mostra la presenza di almeno un equilibrio di Nash per un gioco potenziale, e i due punti appena illustrati, ciò dimostra il teorema in questione.
#### Teorema 3: upper bound PoS di un gioco potenziale
Si supponga di avere un gioco potenziale con funzione potenziale $\Phi$, e si assuma che per ogni esito $S$ si ha
$$
\frac{\textnormal{cost}(S)}{A} \leq \Phi(S) \leq B \cdot \textnormal{cost}(S)
$$
per delle costanti $A,B >0$. Allora il prezzo della stabilità è al più $A\cdot B$.
##### Dimostrazione
Sia $S^N$ un vettore di strategia che minimizza $\Phi(S)$. Allora, per il teorema 1, che afferma l'esistenza di un equilibrio di Nash puro per un gioco potenziale, si ha che $S^N$ è un equilibrio di Nash. 
Per dimostrare il teorema, è sufficiente mostrare che il costo di questa soluzione non è molto più grande di quello di una soluzione $S^*$ di costo minimo.
Per ipotesi, si ha che $\frac{\textnormal{cost}(S^N)}{A} \leq \Phi(S^N)$. Per definizione di $S^N$, si ha che $\Phi(S^N) \leq \Phi(S^*)$. La seconda disuguaglianza dell'assunzione fatta implica che $\Phi(S^*) \leq B \cdot \textnormal{cost}(S^*)$. Componendo le due disuguaglianze si ha che 
$$
\textnormal{cost}(S^N) \leq AB \cdot \textnormal{cost}(S^*)
$$
### Upper bound al PoS del GCG
Si torna ora al global connection game. Mediante i teoremi appena enunciati, si è dimostrato che esiste sempre un equilibrio di Nash per il gioco studiato. Bisogna ora fornire una delimitazione superiore al suo prezzo della stabilità.
Si consideri un'istanza del global connection game, ed un vettore di strategia $S=(P_1,P_2,\ldots,P_k)$ contenente un cammino $s_i - t_i$ per ogni giocatore $i$. Per ogni arco $e$, si definisce una funzione $\Psi(S)$, che mappa i vettori di strategia in valori reali come segue
$$
\Psi_e(S) = c_e \cdot \mathcal{H}_{k_e}
$$
dove $k_e$ è il numero di giocatori che usano l'arco $e$ in $S$, e $\mathcal{H_k}=\sum_{j=1}^k 1/j$ è il $k-$esimo numero armonico. Sia quindi
$$
\Psi(S) = \sum_{e} \Psi_e(S)
$$
Il seguente lemma mostra come $\Psi(S)$ sia una funzione potenziale per il global connection game.
#### Lemma 1
Sia $S=(P_1,P_2,\ldots,P_k)$, e sia $P_i^{'} \neq P_i$ un cammino alternativo per un giocatore $i$, e si definisca un nuovo vettore di strategia $S^{'}=(S_{-i},P_i^{'})$. Allora
$$
\Psi(S) - \Psi(S^{'}) = \textnormal{cost}_i(S) - \textnormal{cost}_i(S^{'})
$$
##### Dimostrazione
Questo lemma afferma che quando un giocatore $i$ cambia le sue strategie, il corrispondente cambio in $\Psi(\cdot)$ rispecchia esattamente il cambio nel payoff di $i$. Sia $k_e$ il numero di giocatori che utilizzano $e$ in $S$. Per ogni arco $e$ che appare in entrambi $P_i$ e $P_i^{'}$ o in nessuno di esi, il costo pagato da $i$ per $e$ è lo stesso sia per $S$ che per $S^{'}$. Quindi, $\Psi_e(\cdot)$ ha lo stesso valore sotto e $S$ e $S^{'}$. Per ogni arco $e$ in $P_i$ ma non in $P_i^{'}$, cambiando strategia da $S$ ad $S^{'}$, $i$ risparmia (e quindi migliora il suo payoff di) $c_e/k_e$, che risulta essere precisamente la diminuizione in $\Psi_e(\cdot)$. Similmente, per un arco $e$ in $P_i^{'}$ ma non in $P_i$, il giocatore $i$ paga un costo di $c_e/(k_e+1)$ cambiando da $S$ ad $S^{'}$, che corrisponde all'aumento in $\Psi_e(\cdot)$. Essendo $\Psi(\cdot)$ semplicemente la somma di $\Psi_e(\cdot)$ su tutti gli archi, il cambio collettivo nel payoff di $i$ è esattamente la negazione del cambio in $\Psi(\cdot)$.
#### Lemma 2
Per ogni vettore di strategia $S$, si ha che 
$$
\textnormal{cost}(S) \leq \Psi(S) \leq \mathcal{H}_k \textnormal{cost}(S)
$$
##### Dimostrazione
Sia ora $e$ un arco usato nel profilo di strategia $S$. La funzione $\Psi_e(S)$ è almeno $c_e$, perché ogni arco utilizzato è selezionato da almeno un giocatore, e non più di $\mathcal{H}_kc_e$, essendovi solo $k$ giocatori. Si ha quindi che 
$$
c_e \leq \Psi_e(S) \leq \mathcal{H}_kc_e
$$
Essendo $$\textnormal{cost}(S) = \sum_{e \in N(S)}c_e$$$$\Psi(S)= \sum_{e \in N(S)}\Psi_e(S)=\sum_{e\in N(S)}c_e \mathcal{H}_{k_e(S)}$$
e
$$
\sum_{e\in N(S)}c_e \mathcal{H}_{k_e(S)} \leq \sum_{e \in N(S)} \mathcal{H}_kc_e
$$
dove si ricorda $1\leq k_e(S) \leq k$ per ogni $e \in N(S)$ essere il numero di giocatori che usano l'arco $e$,  si ha che 
$$
\textnormal{cost}(S) \leq \Psi(S) = \sum_{e\in N(S)}c_e \mathcal{H}_{k_e(S)} \leq \sum_{e \in N(S)} \mathcal{H}_kc_e = \mathcal{H}_k\textnormal{cost}(S)
$$
# Determinare il costo di un equilibrio di Nash
Si vuole studiare ora quanto sia difficile, data un'istanza di un global connection game ed un valore $C$, determinare se il gioco possiede un equilibrio di Nash di costo al più $C$.
```ad-theorem
title: Teorema
Data un'istanza di un Global Connection Game ed un valore $C$, è $\mathbf{NP}-$completo determinare se il gioco a un equilibrio di Nash di costo al più $C$.
```
Il seguente teorema viene dimostrato mediante una riduzione polinomiale dal problema $3D-$Matching. 
## Problema 3D-Matching
Il problema **3D-Matching** è il seguente: dati tre insiemi disgiunti $X,Y,Z$, ciascuno di dimensione $n$, e dato un insieme $T \subseteq X \times Y \times Z$ di triple ordinate, esiste un insieme di $n$ triple $M \subseteq T$ tale per cui ogni elemento di $X \cup Y \cup Z$ è contenuto in esattamente una di queste triple?
## Dimostrazione di NP-completezza
Data un'istanza di 3D-Matching con insiemi di nodi $X,Y,Z$, si costruisce un grafo come segue:
- Inserisci un nodo per ogni nodo in $X,Y,Z$.
- Inserisci un nodo $v_{i,j,k}$ per ogni  arco $3D$ $(x_i,y_j,z_k)$.
- Inserisci un nodo $t$.
- Aggiungi un arco diretto da ciascun nodo $v_{i,j,k}$ verso $t$ avente costo $c_e =3$.
- Per ogni nodo $v$ in $X,Y,Z$ aggiungi un arco diretto da $v$ verso tutti i nodi che rappresentano un arco $3D$ contenente $v$, aventi costo $c_e=0$.
- Sia $k=|X|+|Y|+|Z|$. Aggiungi un giocatore per ciascun nodo $v$ in $X \cup Y \cup Z$. Questo giocatore ha due nodi terminali: $v$ nodo sorgente e $t$ nodo pozzo.

Se esite un 3D matching per l'istanza di 3D-Matching, allora esiste un equilibrio di Nash nel global connection game di costo $k$:
Sia $M$ il 3D matching, e sia $S_i$, per il giocatore avente come nodi terminali $v$ e $t$, il cammino da $v$ a $t$ contenente:
- l'arco da $v$ verso l'unico nodo $v_{i,j,k}$ corrispondente all'arco $3D$ in $M$, dove l'unicità di $v_{i,j,k}$ è data dal fatto che $M$ è un Matching.
- l'arco da questo $v_{i,j,k}$ a $t$.

Essendo $M$ un matching, il costo di $S$, dove $S$ è il profilo di strategia contenente $S_i$ per ogni giocatore $i$, è esattamente $3k/3=k$. $S$ è quindi un equilibrio di Nash, visto che una qualsiasi deviazione da questo profilo di strategia da parte di un giocatore comporta il pagamento di un arco avente costo $3$, mentre la quantità pagata secondo l'attuale profilo di strategia è $1$, essendo tale arco coinvolto in $S$ condiviso dagli altri giocatori appartenenti alla rispettiva tripla del matching.
Se non esiste un $3D$ Matching, allora ogni soluzione del gobal connection game deve costare più di $k$. Quindi, non può esistere un equilibrio di Nash avente costo al più $k$.