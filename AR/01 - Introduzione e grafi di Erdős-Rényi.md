# Introduzione
## Analisi di reti: obiettivo
In questo corso, si parlerà di **reti**, nell'accezione del termine più ampia possibile: con rete si intende un insieme di entità interconnesse, fra le quali esistono dei legami.
Nel corso le reti verranno analizzate rispetto a diversi punti di vista, come ad esempio le prestazioni, la struttura e il loro utilizzo, utilizzando tecniche prese in prestito da numerose discipline, tra cui la matematica, la sociologia e l'economia.
Verrà inoltre riservata una certa attenzione a queste tecniche: ossia, gli argomenti trattati saranno spunto per studiare le tecniche utilizzate per analizzare le reti.
## Reti
Genericamente parlando, una rete è uno schema di interconnessione fra un insieme di entità. In base al tipo di entità si parla di reti diverse: reti fisiche, reti sociali, reti di informazioni etc.
Per il corso di AR, l'idea di rete si riduce a considerarle come un'ampia popolazione che reagisce alle azioni dei singoli.
Si è interessati a studiare il comportamento aggregato di gruppi di individui:
- Come la presenza di legami influisce sul comportamento dei singoli individui (effetti informativi, fenomeni di diffusione, ricerca decentralizzata di percorsi brevi);
- Come la presenza di legami modifica la struttura stessa della rete (stabilità, fenomeno rich get richer);
- Come la struttura dell'insieme dei legami permette di desumere informazioni (web-search, sistemi di voto).
### Struttura di una rete
In generale, risulta difficile rappresentare e studiare puntualmente reti di milioni di individui; si possono però analizzare proprietà "globali" di una rete di grandi dimensioni.
Un esempio è lo studio di componenti giganti nella rete, ossia una componente connessa della rete che contiene una frazione degli individui; oppure la ricerca di porzioni densamente connesse all'interno di una componente connessa.
Sempre come proprietà globale, si può studiare se la rete presenta una struttura di tipo centro/periferia, o si può studiare il ruolo dei nodi che costituiscono una porzione densamente connessa, suddividendoli in entità centrali / periferiche.
#### Esempio: Componente Gigante (Leskovec e Hrovitz)
In questo articolo, chiamato "Planetary-Scale Views on an Instant-Messaging Network", e presente al seguente [link](https://www.researchgate.net/publication/221023506_Planetary-Scale_Views_on_an_Instant-Messaging_Network/link/02e7e51d7c7c082404000000/download), Leskovec e Hrovitz hanno avuto accesso ai dati generati in un mese un sistema telefonico di comunicazione, sviluppato da Microsoft e chiamato ``Instant Messaging``.
I dati sono stati utilizzati per costruire una rete sociale, in cui i nodi erano gli utenti e le connessioni erano i tentativi di comunicazioni tra un utente e l'altro.
Si è scoperto, tra le altre cose, che all'interno del grafo c'era una componente fortemente connessa che conteneva un sacco di invidui. Su 240 milioni di utenti totali infatti, la componente scoperta ne conteneva 200 milioni.

Talvolta, lo studio dei fenomeni verrà portato avanti a livello di popolazione, cioè senza considerare i singoli individui della rete.
Ad esempio, all'interno delle reti sociali è presente un fenomeno, chiamato _rich get richer_, che fa sì che tanto più un nodo è popolare, e tanto più la sua popolarità aumenta.
Quando si fa riferimento a tale fenomeno, ad esempio, si studia qual'è mediamente la frazione di individui che ha un elevato grado di popolarità. Dunque non si studia la popolarità di un individuo singolarmente.
Talvolta per comprendere altri fenomeni, si considera la struttura fisica della rete.
Ad esempio, per comprendere il ruolo di una certa relazione all'interno di una data rete, si deve studiare precisamente la topologia di quella rete relazione per relazione, proprio come accade nello studio di fenomeni di diffusione.

Dato che una rete è un insieme di individui e delle relazioni che intercorrono fra essi, viene modellata attraverso un **grafo**, e si utilizzeranno tutte le nozioni derivate dalla teoria dei grafi.
# Grafi aleatori
Idealmente, si vorrebbero studiare i fenomeni che avvengono all'interno delle reti utilizzando reti reali, che vengono attualmente utilizzate dalle persone o dai calcolatori.
Per lo studio di fenomeni del tipo:
- qual'è, mediamente, il diametro di una rete sociale, espresso in funzione del numero di nodi;
- qual'è, mediamente, la frazione del numero di nodi che ha un certo grado in una rete sociale;
è necessario avere a disposizione un numero consistente di reti.

Nasce il problema di ottenere queste reti, dato che in generale le reti reali non sono sufficienti per fare indagini statistiche; inoltre, difficilmente proprietari di grandi reti le mettono a disposizione per studiarle.

La soluzione generale a questo problema è generale **casualmente** le reti, ossia una rete verrà rappresentata da un grafo generato in modo casuale, ossia un grafo in cui gli archi fra i nodi sono scelti sulla base di un evento aleatorio.
Sono stati proposti molti modelli per generare grafi casuali, ossia delle regole probabilistiche che permettono di connettere nodi, e in generale alcuni di questi modelli riprodurranno taluni fenomeni che sono stati osservati nelle reti sociali, ma non tutti. In altre parole, dato un modello ci si chiede: quanto è aderente alla realtà che si sta cercando di modellare.
Al fine di rispondere a questa domanda, verranno studiate varie proprietà di questi modelli, in modo da capire quali modelli utilizzare per riprodurre quale fenomeno.
## Il modello di Erdős-Rényi
Fissato $n \in \mathbb{N}$ e $p \in [0,1]$, si costruisce un grafo nel modo seguente:
- L'insieme dei nodi del grafo è $[n] = \{1,2,\dots,n\}$;
- Per ogni coppia di elementi distinti $i$ e $j$ in $[n]$: con probabilità $p$ viene inserito nel grafo l'arco $(i,j)$.
Il grafo cosi costruito è un evento aleatorio che indichiamo con $G_{n,p}$.
Dunque, $G_{n,p}$ è un modello per la generazione di grafi casuali conosciuto come modello di Erdős-Rényi, del quale verranno ora studiate alcune caratteristiche.
```ad-warning
title: Attenzione
$G_{n,p}$ indica un modello per la generazione di grafi, quindi non sarebbe propriamente corretto indicare con $G_{n,p}$ un grafo cosi generato, ma abusiamo di notazione dunque con $G_{n,p}$ si farà spesso riferimento a un grafo generato dal modello.
```
In particolare, si studia l'**esistenza di una componente gigante** e il **grado dei nodi** in un grafo generato con questo modello.

Si osserva che fissato il numero $n$ dei nodi, al variare di $p$ si ottengono grafi $G_{n,p}$ con caratteristiche molto differenti:
- quando $p=0$ sarà possibile ottenere un unico grafo $G_{n,0}$: quello che non contiene alcun arco;
- Analogamente, quando $p=1$ sarà possibile ottenere un unico grafo $G_{n,1}$: il grafo completo su $n$ nodi;
In generale, $G_{n,p}$ conterrà mediamente tanti più archi quanto più $p$ si avvicina a $1$, ossia mediamente un nodo avrà tanti più vicini quanto più $p$ si avvicina a $1$ e, mediamente, le componenti connesse in $G_{n,p}$ saranno tanto più grandi quanto più $p$ si avvicina a $1$. Dunque si quantifica quanto deve valere $p$ per ottenere, con alta probabilità, una componente gigante nel grafo $G_{n,p}$.
### Componenti giganti
Una componente gigante in un grafo è una componente connessa che contiene una frazione del numero dei nodi. Molte reti reali contengono una componente gigante, come ad esempio nelle reti sociali o nella rete del Web. Ci si chiede se il modello di Erdős-Rényi riesce a rappresentare questa caratteristica tipica di molte reti reali, ossia se esistono valori di $p$ per i quali $G_{n,p}$ contiene componenti giganti.

Si dimostra ora che se $p > \frac{\ln{64}}{n}$ allora con alta probabilità il grafo $G_{n,p}$ contiene una componente connessa costituita da almeno metà dei suoi nodi. Formalmente, si definisce $X$ come la variabile aleatoria corrispondente al numero di nodi nella più grande componente connessa di $G_{n,p}$ ; dunque si dimostra il teorema seguente.
```ad-Teorema
title: Teorema
se $p > \frac{\ln{64}}{n}$, allora $P( X \geq \frac{n}{2}) \geq 1-2^{-n/8}$

```
Per dimostrare il teorema, si dimostra in primis il lemma seguente.
#### Lemma
se $X < n/2$ allora esiste un insieme $A \subset [n]$ tale che $n/4 \leq |A| < 3n/4$ e non esistono archi fra i nodi in $A$ e i nodi in $[n]-A$.
##### Dimostrazione
Siano $C_1,C_2,\dots,C_k$ tutte le componenti connesse di $G_{n,p}$ e si assuma che siano ordinate per cardinalità non decrescente, ossia $|C_1| \leq |C_2| \leq \dots \leq |C_k|$.
Poiché $X < n/2$, non esiste una componente connessa $C_i$ che contiene più di $n/2$ nodi, dunque vale $|C_i| < n/2 \quad \forall i=1,\dots,k$ . 

Dunque si sceglie un indice $h$ tale che
$$ C_1+C_2+\dots+C_{h-1} < n/4 \quad\text{e}\quad C_1+C_2+\dots+C_{h-1}+C_{h} \geq n/4 $$
Si osserva che $h < k$. Infatti sia $h=k$; se le prime $k-1$ componenti contengono complessivamente meno di $n/4$ nodi, allora la somma del numero di nodi di tutte le componenti è minore di $n$, dato che la componente $k$-esima contiene per ipotesi meno di $n/2$ nodi. Ma questo è assurdo, dato che l'unione delle componenti è proprio l'insieme dei nodi di $G_{n,p}$, e $G_{n,p}$ è composto per definizione da $n$ nodi.

Sia $A$ l'unione delle prime $h$ componenti, $A = C_1 \cup \dots \cup C_h$ . Allora $A \neq \emptyset$ dato che le componenti non possono essere vuote, e $[n]-A \neq \emptyset$ dato che $h < k$.
Vale che $|A| \geq n/4$ per costruzione e vale
$$ |A| = (|C_{1}| + |C_{2}| + \dots + |C_{h-1}|) + |C_{h}| < n/4 + n/2 = 3n/4 $$
dunque $n/4 \leq |A| < 3n/4$.

Rimane da dimostrare che non esistono archi fra i nodi in $A$ e $[n]-A$ . Ma questo vale per costruzione: poiché $C_1,C_2,\dots,C_k$ sono tutte le componenti connesse di $G_{n,p}$ , non ci sono archi fra $A$ e $[n]-A$ altrimenti, se ci fosse un arco fra $C_i$ e $C_j$ con $i \leq h$ e $j>h$, allora $C_i \cup C_j$ sarebbe un'unica componente connessa.
#### Dimostrazione teorema
Sia denominato `good` l'insieme $A$ individuato dal lemma; si osserva che questo lemma assicura che la probabilità che la più grande componente connessa di $G_{n,p}$ contenga meno di $\frac{n}{2}$ nodi è minore o uguale alla probabilità che $G_{n,p}$ contenga un insieme di nodi `good`, dato l'ipotesi $X < \frac{n}{2}$ implica l'esistenza di un insieme $A$.

Per dimostrare il teorema, è sufficiente calcolare $\mathbf{Pr}\left( X < \frac{n}{2} \right)$, ossia la probabilità che la massima componente connessa in $G_{n,p}$ contenga meno di $\frac{n}{2}$ nodi. In virtù dell'osservazione precedente vale
$$
\begin{align}
\mathbf{Pr}\left( X < \frac{n}{2} \right) &\leq \mathbf{Pr}(\exists A \subseteq [n] : A\text{ è \texttt{good}})\\
&= \mathbf{Pr}\left( \bigcup_{A \subseteq [n] \text{ e } \frac{n}{4} \leq |A| < \frac{3n}{4}} \left[\text{non ci sono archi fra }A \text{ e } [n]-A\right] \right) \\
&\leq \sum_{A \subseteq [n] \text{ e } \frac{n}{4} \leq |A| < \frac{3n}{4}} \mathbf{Pr} \left(\text{non ci sono archi fra }A \text{ e } [n]-A\right)
\end{align}
$$
dato un grafo $G_{n,p}$, e un sottoinsieme $A$ dei suoi nodi: la probabilità che non ci sia un arco tra $A$ e $[n]-A$ è pari $1-p$; dato che il numero di archi possibili fra $A$ è $[n]-A$ è dato dalla cardinalità di $A$ per la cardinalità di $[n]-A$, vale
$$
\mathbf{Pr}\left(\text{non ci sono archi fra }A \text{ e } [n]-A\right) = (1-p)^{|A|\cdot |[n]-A|}
$$
dato che $(1-p)<1$, il suo valore è massimo quando l'esponente $|A|\cdot |[n]-A|$  è minimo, cioè quando $|A| = n/4$, dunque 
$$
\begin{align}
\mathbf{Pr} \left( X < \frac{n}{2} \right) &\leq \sum_{A \subseteq [n] \text{ e } \frac{n}{4} \leq |A| < \frac{3n}{4}} (1-p)^{\frac{n}{4}(n-\frac{n}{4})}
\end{align}
$$

ricordando che $[n]$ contiene $2^n$ sottoinsiemi, vale
$$
\begin{align}
\mathbf{Pr}\left( X < \frac{n}{2} \right) &\leq 2^n(1-p)^{\frac{3n^2}{16}} \\
\end{align}
$$
Applicando  l'ipotesi $p> \frac{\ln64}{n}$ a destra della disequazione, si ottiene
$$
\begin{align}
\mathbf{Pr} \left( X < \frac{n}{2} \right) &< 2^n\left(1-\frac{\ln{64}}{n}\right)^{\frac{3n^2}{16}} \\
&= 2^n\left[\left(1 - \frac{\ln{64}}{n}\right)^n\right]^{\frac{3n}{16}}\\
&= 2^n\left[\left(1 - \frac{\ln{64}}{n}\right)^{\frac{n}{\ln64}(\ln{64})}\right]^{\frac{3n}{16}}
\end{align}
$$
Poiché $\lim_{n \rightarrow \infty} \left(1 - \frac{\ln{64}}{n}\right)^{\frac{n}{\ln64}} = e^{-1}$ , allora per $n$ sufficientemente grande vale
$$ \left(1 - \frac{\ln{64}}{n}\right)^{\frac{n}{\ln64}(\ln{64})} = e^{-\ln{64}} = 64^{-1} $$
e dunque 
$$
\mathbf{Pr}) \left( X < \frac{n}{2} \right) < 2^n [64^{-1}]^{3n/16} = 2^n[2^{-6}]^{3n/16} = 2^n 2^{-18n/16} = 2^{-n/8}
$$
da cui segue il teorema.

Il teorema appena dimostrato può essere generalizzato.
```ad-Teorema
1. se $p(n-1)<1$ allora quasi sicuramente tutte le componenti connesse di $G_{n,p}$ hanno $O(\log{n})$ nodi;
2. se $p(n-1)=1$ allora quasi sicuramente $G_{n,p}$ ha una componente connessa di circa $n^{2/3}$ nodi;
3. se $p(n-1)>1$ allora quasi sicuramente $G_{n,p}$ ha una componente connessa di circa $\Omega(n)$ nodi e tutte le altre componenti connesse hanno $O(\log{n})$ nodi.
```
Dove con *quasi sicuramente* significa che, al tendere di $n$ all'infinito la probabilità dell'evento tende a $1$; in altre parole, è meno forte di *con alta probabilità* (non deve tendere per forza rispetto ad un polinomio).
In conclusione, la presenza di componenti giganti dipende dal prodotto $p(n-1)$.
### Grado dei nodi
Il prodotto $p(n-1)$ rappresenta il valore atteso del grado di un nodo di $G_{n,p}$. Per $i \in [n]$, sia $\delta_i$ la variabile aleatoria che esprime il grado del nodo $i$, vale
$$
\mathbf{E}[\delta_i] = \sum_{j \in [n]-{i}} \left[1\cdot p+0\cdot(1-p)\right] = \sum_{j \in [n]-{i}} p = (n-1)p
$$
Questo significa che, se $p$ è costante, il grado dei nodi in media cresce linearmente col numero dei nodi.

Si studia ora il potere espressivo di un grafo $G_{n,p}$, ossia se tale grafo descrive in modo corretto una rete sociale costituita da tanti individui.
Se il grado medio di un nodo è $(n-1)p$, e $p$ è costante, allora mediamente il numero di contatti di un individuo in una rete sociale è proporzionale agli individui che costituiscono tale rete.
Questo fatto non è propriamente ragionevole, dunque al fine di modellare significativamente reti reali di grandi dimensioni è opportuno che $p$ sia una funzione decrescente di $n$, del tipo $p=p(n)=\frac{\lambda}{n}$, con $\lambda$ costante positiva.

Fissato $p$ in questo modo e fissato un intero $k \leq n$, si vuole calcolare con quale probabilità un nodo in $G_{n,p}$ ha grado $k$.

Sia $i \in [n]$: la probabilità che il nodo $i$ abbia grado $k$ è la probabilità che esattamente $k$ altri nodi siano adiacenti a $i$:
- il numero di possibili $k$-uple di nodi scelti nell'insieme $[n] - {i}$ è $n-1 \choose k$;
- la probabilità che vi sua un arco fra $i$ e ciascuno dei nodi della $k$-upla è $p^k$;
- la probabilità che **non** vi sia un arco fra $i$ e ciascuno dei nodi non contenuto nella $k$-upla è $(1-p)^{n-1-k}$;
quindi 
$$
\mathbf{Pr}(\delta_i = k) = {n-1 \choose k} p^k(1-p)^{n-k-1}
$$
sostituendo $p = \frac{\lambda}{k}$ ed esplicitando il coefficiente binomiale si ottiene
$$
\mathbf{Pr}(\delta_i = k) = \frac{(n-1)\cdot(n-2)\cdot\dots\cdot(n-k)}{k!} \frac{\lambda^k}{n^k}\left(1-\frac{\lambda^k}{n^k}\right)^{n-k-1}
$$
poiché $(1-x)< e^{-x}$, allora non
$$ \left(1-\frac{\lambda}{n}\right)^{n-k-1} < e^{\frac{-\lambda(n-k-1)}{n}} $$
dunque si ottiene
$$
\mathbf{Pr}(\delta_i = k) < \frac{(n-1)\cdot(n-2)\cdot\dots\cdot(n-k)}{k!} \frac{\lambda^k}{n^k}e^{\frac{-\lambda(n-k-1)}{n}}
$$
dato che $(n-1)\cdot(n-2)\cdot\dots\cdot(n-k) < n^k$, si ottiene
$$
\mathbf{Pr}(\delta_i = k) < \frac{n^k}{k!} \frac{\lambda^k}{n^k}e^{\frac{-\lambda(n-k-1)}{n}}
$$
per $n$ sufficientemente grande, vale
$$
\mathbf{Pr}(\delta_i = k) < \frac{\lambda^k}{k!} e^{-\lambda}
$$
utilizzando l'approssimazione di Stirling, si ottiene infine
$$
\mathbf{Pr}(\delta_i = k) < \frac{(\lambda \cdot e)^k}{\sqrt{ 2\pi k } \cdot k^k } e^{-\lambda} = \frac{e^{-\lambda}}{\sqrt{ 2\pi k }} \left(\frac{\lambda \cdot e}{k}\right)^k
$$
dunque si conclude che la probabilità che un generico nodo abbia grado $k$ decresce molto velocemente al crescere di $k$; più precisamente, decresce come $k^{-k}$ ossia esponenzialmente in $k$.

Calcoliamo infine quale sia la frazione del numero di nodi che hanno grado $k$. Indichiamo con $F_k$ la variabile aleatoria che esprime tale frazione e indichiamo con $\delta_{i,k}$ la variabile aleatoria 
$$
\delta_{i,k} = \begin{cases}
1 \quad \text{ se } \delta_{i} = k \\
0 \quad \text{ altrimenti }
\end{cases}
$$
allora vale
$$
F_{k} = \frac{1}{n} \sum_{i \in [n]} \delta_{i,k}
$$
il valore atteso di $F_k$ vale
$$
\begin{align}
\mathbf{E}[F_{k}] &= \mathbf{E}\left[\frac{1}{n} \sum_{i \in [n]} \delta_{i,k}\right]  \\
&= \frac{1}{n} \sum_{i \in [n]} \mathbf{E}[\delta_{i,k}] \\
&= \frac{1}{n} \sum_{i \in [n]} \mathbf{Pr}(\delta_{i}=k) \\
&< \frac{e^{-\lambda}}{\sqrt{ 2\pi k }} \left(\frac{\lambda \cdot e}{k}\right)^k
\end{align}
$$
Ossia, mediamente, la frazione del numero di nodi che hanno grado $k$ decresce come $k^{-k}$ ossia esponenzialmente in $k$.