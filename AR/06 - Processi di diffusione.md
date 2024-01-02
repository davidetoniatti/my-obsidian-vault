La presenza di una rete influenza il comportamento degli individui che la popolano, modificando il loro comportamento, come visto con l'esperimento di Granovetter, perché gli individui, in virtù della presenza di una rete, interagiscono fra loro anche solamente osservando il comportamento gli uni degli altri.
Molte delle iterazioni avvengono a livello locale piuttosto che globale; si è interessati al comportamento degli individui con i quali si è in relazione, piuttosto che al comportamento dell'intera popolazione.

```ad-note
Gli individui che possono influenzare il nostro comportamento sono quelli con i quali siamo in relazione.
```

## Il concetto di omofilia
Il fenomeno della chiusura triadica è strettamente connesso al concetto di omofilia che si esplicita in due direzioni:
- La tendenza di individui a connettersi con altri individui che gli assomigliano; ad esempio, un individuo tende a non stabilire relazioni con chi ha idee politiche opposte alle sue. Oppure, se un individuo ama il mare, diventerà amico con gente che incontra al mare (dunque, che ama il mare come lui). Dunque le ragioni di tale tendenza sono:
	- **Selezione**: si tende ad essere amici a chi ci somiglia;
	- **Opportunità**: se si ama il mare, difficilmente si incontreranno amanti della montagna.
-  La tendenza di individui nel diventare *simili* agli individui con i quali stanno in relazione: ad assumere i loro stessi gusti, adeguarsi ai loro comportamenti e a diventare a amici dei loro amici. La motivazione soggiacente a questa tendenza è l'esigenza di ridurre a tensione sociale, ma c'è anche una motivazione assolutamente razionale: se si vedono gli individui con cui si ha una relazione utilizzare un certo prodotto, allora si tenderà ad acquistare quel prodotto, sia perché si assume essere una buona idea dato che c'è fiducia in tali individui, sia per mantenere la similitudine con questi individui, dunque per non indebolire la relazione con loro.

## Processi di diffusione
Il processo di diffusione delle innovazioni è stato studiato in sociologia già a partire dalla metà del secolo scorso da [Ryan e Gross (1943)](https://www.proquest.com/openview/7de2b2276a089fe888071663de12b6a0/), osservando i processi di adozione di nuovi semi ibridi di mais da parte di un gruppo di agricoltori dell'Iowa: la maggior parte degli agricoltori iniziava a usare i nuovi semi solo dopo aver osservato che un certo numero di vicini li stava utilizzando.

Si vuole modellare il processo di diffusione in una rete, dunque è necessario stabilire le regole in base alle quali un nodo decide di cambiare. Si definisce un modello di decisioni **individuali**, ossia non vi è coalizzazione di gruppi di nodi per prendere collettivamente la stessa decisione, nel quale le scelte dei nodi sono guidate da **motivazione di puro interesse personale**, cioè la spinta a cambiare è tanto maggiore quanto maggiore è il vantaggio che si prevede che deriverà dal cambiamento.
Si assume che nella rete sia stabilizzato un certo stato delle cose $B$, cioè tutti gli individui della rete hanno un prodotto/opinione/comportamento denominato con $B$. Ad un certo istante, nella rete alcuni individui cambiano il loro stato in $A$. Allora, <u>assumendo che chi è nello stato A non torni mai in B</u> in quale casi un individuo decide di cambiare il proprio stato da $B$ a $A$?

### Modello del processo di diffusione

Il modellamento del processo di diffusione individuale basato sul vantaggio personale avviene mediante un *Network Coordination Game*. Sia $(u,v)$ un arco della rete, si assume che il beneficio reciproco di adottare $A$ o $B$ sia il seguente:
- se $u$ e $v$ adottano entrambi $A$ allora entrambi hanno un beneficio pari ad $a$;
- se $u$ e $v$ adottano entrambi $B$ allora entrambi hanno un beneficio pari ad $b$;
- altrimenti, nessuno dei due ha alcun beneficio dalla reciproca relazione.
Dato che in genere un nodo nella rete ha più di un vicino, si adotta un modello lineare.
Detti $N_A$ il numero di vicini di $u$ nello stato $A$ e $N_B$ il numero di vicini di $u$ nello stato $B$: se $u$ rimane nello stato $B$ ha un beneficio pari a $b \cdot N_B$, se $u$ passa allo stato $A$ ha un beneficio pari ad $a \cdot N_A$.
Allora $u$ rimane nello stato $B$ se $b \cdot N_B > a \cdot N_A$ , mentre $u$ passa allo stato $A$ se $a \cdot N_A \geqslant b \cdot N_B$.

```ad-note
A partià di beneficio, $u$ passa ad $A$, ossia l'innovazione è preferibile al vecchio stato.
```

Poiché $N_B = | N(u) | - N_A$, $u$ passa allo stato $A$ se
$$
a \cdot N_A \geqslant b \cdot (|N(u)| - N_A)
$$
ossia, se
$$
\frac{N_A}{ |N(u)| } \cdot a \geqslant \frac{(|N(u)| - N_A)}{ |N(u)| } \cdot b
$$
ossia, detto $p_A = \frac{N_A}{ |N(u)| }$, se
$$
p_{A} \cdot a \geqslant (1-p_{A}) \cdot b
$$
```ad-important
$u$ passa allo stato $A$ se, detta $p_A$ la frazione dei vicini di $u$ che è in $A$, vale che 
$$
p_A \geqslant \frac{b}{a+b}
$$
dove $q = \frac{b}{a+b}$ è la **soglia di adozione** di $A$.
```

Quando $q$ è molto piccolo, occorrono pochi vicini nello stato $A$ per indurre un nodo a cambiare stato, e $q$ è molto piccolo quando $a$ è molto più grande di $b$, ossia, quando lo stato $A$ è molto migliore dello stato $B$.
Quando $a = b$ occorrono almeno la metà dei vicini nello stato $A$ per indurre un nodo a cambiare stato, e questo accade quando lo stato $A$ è confrontabile con lo stato $B$.
Infine, quando $a$ è molto più piccolo di $b$, occorrono molti vicini nello stato $A$ per indurre un nodo a cambiare stato, e questo accade quando lo stato $A$ è peggiore dello stato $B$, e quindi è faticoso/rischioso/costoso adottare $A$.

### Game e configurazione di equilibrio

Con configurazioni di equilibrio si intendono quelle configurazioni del network coordination game definito in cui nessun nodo cambia il proprio stato da $B$ ad $A$.
Si osserva che esistono almeno due configurazioni di equilibrio:
- quando $A$ non è stato introdotto nella rete, cosicché tutti nodi sono nello stato $B$;
- quando, dopo che $A$ è stato introdotto nella rete, tutti i nodi sono passati nello stato $A$;

Si vedono ora alcuni esempi di processi di diffusione, nell'ottica di studiare quando termina il processo di diffusione, ossia quali configurazioni di equilibrio ha il network coordination game e perché proprio in tali configurazioni la diffusione si interrompe.

#### Esempio 1
![[AR/imgs/06-img01.png|center|500]]
In questo esempio, lo stato $A$ viene forzato all'inizio sui nodi $v$ e $w$; in questo caso $a = 3$ e $b = 2$, quindi $q = \frac{2}{3+2}$, ossia, per adottare $A$, un nodo deve avere i $2/5$ dei vicini nello stato $A$. Perciò in questo esempio, tutti i nodi della rete adottano $A$.

#### Esempio 2
![[AR/imgs/06-img02.png|center|500]]
1. se $a = 2$, $b = 3$, ossia $q = 2/5$, in questo caso non tutti i nodi della rete adottano $A$; infatti $A$ non riesce a raggiungere i nodi fuori dall'esagono.
2. se $a = 4$, $b = 2$, ossia $q = 1/3$, in questo caso dopo aver raggiunto tutti i nodi dell'esagono, $A$ è adottato dai nodi 2, 11 e 14; poi da 1, 3, 12, 13, 17 e infine da 15 e 16; tutti i nodi adottano $A$.

### Diffusioni e cascate complete

Dai due esempio si possono trarre una serie di conclusioni:
- non sempre tutti i nodi si adeguano all'innovazione;
- se questo accade, è possibile aumentare il beneficio derivante dall'adozione di $A$ per indurre tutti i nodi ad adottarlo, ad esempio abbassando il costo dell'acquisizione di $A$.
Inoltre, l'eventualità che tutti i nodi arriveranno ad adottare $A$ dipende dai nodi sui quali si forza lo stato $A$ all'inizio del processo: dal loro numero e dalla loro posizione all'interno della rete.

Questo insieme di nodi $V_0$ si identifica come insieme dei nodi **iniziatori**.
Una volta che $A$ viene introdotto nella rete, inizia un processo di diffusione che procede in una sequenza di passi discreti: si indica con $V_i$ l'insieme dei nodi che adottano $A$ al passo $i$ di tale sequenza, ossia l'insieme di vicini dei nodi $V_0 \cup V_1 \cup \dots \cup V_{i-1}$ che adotta $A$.
Viene generata una **cascata completa** se ad un certo passo $t$ tutti i nodi hanno adottato $A$, ossia se esiste un $t \geq 0$ tale che $\bigcup_{0 \leq i \leq t} V_i = V$.

### Cascate e cluster

Dagli esempi svolti, si nota che l'innovazione ha difficoltà ad uscire da gruppi coesi di nodi.
Si definisce **cluster di densità** $p$ un sottoinsieme di nodi $V' \subseteq V$ tale che la frazione di vicini che ogni suo nodo ha in $V'$ è almeno $p$.
$$
\forall u \in V' \left[ \frac{ |N(u) \cap V'| }{ |N(u)| } \geqslant p \right]
$$
Nell'esempio 2, il sottoinsieme di nodi $\{4,5,6,7,8,9,10\}$ è un cluster di densità $p=2/3$. 

```ad-info
Sia $G=(V,E)$ un grafo e sia $V' \subseteq V$: si indica con $G-V'$ il grafo ottenuto rimuovendo da $G$ tutti i nodi in $V'$ e tutti gli archi incidenti su nodi in $V'$.
```

Valgono i seguenti teoremi.
#### Teorema 1

```ad-theorem
title: Teorema
Sia $G=(V,E)$ un grafo e siano $V_0 \subseteq V$ l'insieme di iniziatori e $q$ la soglia di adozione di $A$: se $V_0$ non genera una cascata completa allora $G-V_0$ contiene un cluster di densità $1-q$.
```

```ad-proof
title: Dimostrazione
Se $V_0$ non genera una cascata completa, allora esistono nodi che non adottano $A$.
Sia $t$ il passo tale che $V_t \neq \emptyset$ e $V_{t+1} = \emptyset$, ossia $t+1$ è il passo in cui $A$ non si diffonde più.
Sia $V_A = \bigcup_{0 \leq i \leq t} V_i$ , cioè l'insieme di tutti e soli i nodi che adottano $A$. Poiché esistono nodi che non adottano $A$, vale $V-V_A \neq \emptyset$. Poiché i nodi in $V-V_A$ non adottano $A$, allora vale per ogni $v \in V-V_A$:
$$
\frac{ |N(v) \cap V_A| }{ |N(v)| } < q
$$
e quindi, poiché 
$$
\frac{ |N(v) \cap V_A| }{ |N(v)| } + \frac{ |N(v) \cap (V-V_{A})| }{ |N(v)| } = 1
$$
allora vale
$$
\frac{ |N(v) \cap (V-V_{A})| }{ |N(v)| } \geqslant 1-q
$$
ossia, $V-V_A$ è un cluster di densità $1 - q$ ed è contenuto in $G – V_0$.
```

#### Teorema 2

```ad-theorem
title: Teorema
Sia $G=(V,E)$ un grafo e siano $V_0 \subseteq V$ l'insieme di iniziatori e $q$ la soglia di adozione di $A$: se $G-V_0$ contiene un cluster di densità maggiore di $1-q$ allora $V_0$ non genera una cascata completa.
```

```ad-proof
Se $G-V_0$ contiene un cluster di densità maggiore di  $1-q$, cioè, esiste $C \subseteq V - V_0$ tale che, per ogni $v \in C$,
$$
\frac{ |N(v) \cap C| }{ |N(v)| } > 1 - q
$$
Si suppone per assurdo che si generi una cascata completa: allora i nodi in $C$, prima o poi, adotteranno $A$. Sia $t$ il primo passo tale che $V_t \cap C \neq \emptyset$: ossia, per $i < t, V_i \cap C = \emptyset$ e esiste $u \in V_t \cap C$.
Poniamo $V' = \bigcup_{0 \leq i \leq t-1} V_i$, ossia $V'$ sono tutti i nodi che al passo $t-1$ sono nello stato $A$ e $V'$ non contiene nodi di $C$.
Allora,
1. poiché $C$ è un cluster di densità $> 1 - q$ e $u \in C$, allora $\frac{ |N(u) \cap C| }{ |N(u)| } > 1 - q$ ;
2. poiché $u \in V_t$ , allora $\frac{ |N(u) \cap V'| }{ |N(u)| } > 1 - q$
ossia, poiché $V' \cap C = \emptyset$
$$
1 = \frac{N(u)}{N(u)} \geqslant \frac{ |N(u) \cap (C \cup V')| }{ |N(u)| } = \frac{ |N(u) \cap C| }{ |N(u)| } + \frac{ |N(u) \cap V'| }{ |N(u)| } > 1
$$

dunque un assurdo.
```


I teoremi appena dimostrati mettono in risalto il fatto che le innovazioni si diffondono con relativa facilità all'interno dei cluster; invece, incontrano difficoltà ad uscire dai cluster.
Perciò, si può concludere che, mentre l'esperimento di Granovetter ha permesso di mettere in luce la forza dei weak ties, in quanto fonte di vantaggi informativi, lo studio dei processi di diffusione ne evidenzia la debolezza, in quanto ostacolo alla diffusione.

Ci si chiede ora se è possibile superare lo stallo nel quale, a causa della presenza di cluster sufficientemente densi, uno stato $A$ smette di diffondersi. In generale, si possono scegliere gli iniziatori in posizioni tali che ciascun cluster ne contenga almeno uno, oppure è possibile aumentare l'appetibilità di $A$.
Quanto alta può essere tenuta la soglia di adozione perché si generi una cascata completa?
### Capacità di cascata
Quanto alta può essere tenuta la soglia di adozione perché si generi una cascata completa?
Innanzi tutto, dipende dalla struttura della rete; alcune strutture ostacolano maggiormente di altre la generazione di cascate complete.

**PROBLEMA**: dato un grafo $G=(V,E)$, qual è la soglia di adozione massima $q_{max}$ in $G$ affinché un *piccolo* insieme $V_0$ di iniziatori di un nuovo stato $A$ generi una cascata completa?

Il valore $q_{max}$ prende il nome di **capacità di cascata di** $G$.
Con $V_0$  *piccolo* si intende che $|V_0| \ll |V|$ .

Per studiare questo problema, si consideri la seguente ipotesi di lavoro.
$G$ è un grafo infinito, ossia contiene un numero **infinito** di nodi e l'insieme $V_0$ degli iniziatori può essere un qualsiasi insieme finito di nodi di $G$.
Dunque dato un grafo infinito, qual è la soglia di adozione massima $q_{max}$ in $G$ affinché esista un insieme finito $V_0$ di iniziatori di un nuovo stato $A$ che generi una cascata completa.  

#### Esempio: $G$ è una catena infinita
Se $|V_0| = 1$, ossia, $V_0$ contiene un solo nodo, allora occorre $q = 1/2$ per generare una cascata completa
![[AR/imgs/06-img03.png|center]]
Anche scegliendo un insieme più grande, non è possibile generare una cascata con $q < 1/2$, perché i nodi al confine con $V_0$ ($x$ e $y$) hanno comunque bisogno di $q = 1/2$ per passare ad $A$.
![[AR/imgs/06-img04.png|center]]
Allora in una catena infinita: $q_{max} = 1/2$.

#### Esempio: $G$ è una griglia infinita
Se $|V_0| = 1$, allora occorre $q = 1/8$ per generare una cascata completa.

![[AR/imgs/06-img05.png|center|500]]

Se $|V_0| = 2$, allora scegliendo i due nodi come nodi adiacenti, occorre $q = 1/4$ per influenzare $u,v,x,y$ e poi per generare una cascata completa.

![[AR/imgs/06-img06.png|center|500]]

Se $|V_0| = 3$, allora scegliendo i tre nodi come nodi adiacenti, occorre $q = 3/8$ per influenzare $v$ e $y$, poi $u,v,x,y$ e cosi via fino a generare una cascata completa.

![[AR/imgs/06-img06.png|center|500]]

Aumentando $|V_0|$ non si riesce ad aumentare la soglia di adozione: una volta influenzati tutti i nodi nel rettangolo (giallo) che contiene gli iniziatori, occorre uscire da esso e per farlo è necessario $q = 3/8$.

![[AR/imgs/06-img07.png|center|500]]

Allora, in una griglia infinita: $q_{max} = 3/8$.

Dai due esempi si osserva che:
- la soglia di adozione massima è più bassa nella griglia, che ha una topologia più ricca, che non nella catena;
- in entrambi i casi, essa non supera $1/2$, cioè affinché si generi una cascata completa, $A$ deve essere almeno tanto appetibile quanto $B$.

Ci si chiede, poiché la soglia di adozione massima è una caratteristica della rete (ovvero, della sua topologia), se esistono topologie nelle quali la soglia di adozione massima è maggiore di $1/2$;
ossia, se esistono topologie nelle quali innovazioni di qualità mediocre soppiantino uno status quo di qualità maggiore. Il prossimo teorema risponde a questa domanda.

#### Teorema
Poiché la soglia di adozione massima è una caratteristica della rete, indicheremo come $q_G$ **la soglia di adozione massima di un grafo** $G$.

```ad-theorem
title: Teorema
Per ogni grafo infinito $G=(V,E)$ i cui nodi hanno grado finito, vale
$$
q_G \leqslant \frac{1}{2}
$$
```

```ad-proof
title: Dimostrazione
Si supponga per assurdo che esista un insieme finito di iniziatori $V_0$ che, con soglia di adozione $q > 1/2$, generi una cascata completa nel grafo $G$.
Sia $V_t$ l'insieme dei nodi che adottano $A$ al passo $t$ e sia, per ogni $t \geqslant 0, S_t = \bigcup_{0 \leqslant i \leqslant t} V_i$ l'insieme dei nodi che, al passo $t$, sono nello stato $A$.

Sia **l'interfaccia al passo** $t$ l'insieme $I_t$ degli archi che, al passo $t$, hanno un estremo in $S_t$ e l'altro estremo in $V-S_t$
$$
I_t = \{ (u,v) \in E: u \in S_t \land v \in V-S_t \}
$$
Allora si dimostra che, per ogni $t \geqslant 0, |I_t| > |I_{t+1}|$ oppure $I_t = I_{t+1}$.

Per fare ciò, si dimostra che se $I_t \neq I_{t+1}$, allora $|I_t| > |I_{t+1}|$.
Se $I_t \neq I_{t+1}$, allora esiste un nodo $v$ che adotta $A$ al passo $t+1$, ossia $V_{t+1} \neq \emptyset$ e perché un nodo $v$ appartenga a $V_{t+1}$ deve esistere $u \in N(v)$ tale che $u \in S_t$, ossia, per ogni $v \in V_{t+1}$ esiste (almeno) un arco $(u,v) \in I_t$ tale che $u \in S_t$.
Allora, per ogni nodo $v \in V_{t+1}$:
- gli archi incidenti su $v$ il cui altro estremo è in $S_t$ sono in $I_t$ ma non in $I_{t+1}$, come $(u,v)$ in figura;
- gli archi incidenti su $v$ il cui altro estremo è in $S_{t+1}$ sono in $I_{t+1}$ ma non in $I_t$, come $(v,z)$ in figura.

![[06-img08.png|center|500]]

ossia
$$
I_{t+1} = I_t - \left[ \bigcup_{v \in V_{t+1}} \{ (u,v) \in E: u \in S_t \}  \right] \cup \left[ \bigcup_{v \in V_{t+1}} \{ (z,v) \in E: z \in V - S_{t+1} \} \right]
$$

- se $v \neq z$ con $v,z \notin S_t$, allora $\{ (u,v) \in E : u \in S_t \} \cap \{ (u,z) \in E : u \in S_t \} = \emptyset$; dunque $| \bigcup_{v \in V_{t+1}} \{ (u,v) \in E : u \in S_t \}| = \sum_{v \in V_{t+1}} | \{ (u,v) \in E : u \in S_t \} |$
- se $v \neq z$ con $v,z \in V_{t+1}$, allora $\{ (u,v) \in E : u \in V - S_{t+1} \} \cap \{ (u,z) \in E : u \in V - S_{t+1} \} = \emptyset$; dunque $| \bigcup_{v \in V_{t+1}} \{ (u,v) \in E : u \in V - S_{t+1} \}| = \sum_{v \in V_{t+1}} | \{ (u,v) \in E : u \in V - S_{t+1} \} |$

ossia
$$
|I_{t+1}| = |I_t| - \sum_{v \in V_{t+1}} | \{ (u,v) \in E : u \in S_t \} | + \sum_{v \in V_{t+1}} | \{ (u,v) \in E : u \in V - S_{t+1} \} |
$$

ma vale che
$$
| \{ (u,v) \in E : u \in S_t \} | = | N(v) \cap S_t |
$$
$$
| \{ (u,v) \in E : u \in V - S_{t+1} \} | = | N(v) - S_{t+1} |
$$
perché per ogni $v \in V_{t+1}, (u,v) \in I_t$ se e solo se $u \in N(v) \cap S_t$ e $(u,v) \in I_{t+1}$ se e solo se $u \in N(v) - S_{t+1}$, ossia:
$$
|I_{t+1}| = |I_t| - \sum_{v \in V_{t+1}} | N(v) \cap S_t | + \sum_{v \in V_{t+1}} | N(v) - S_{t+1} |
$$

Per ogni $v \in V_{t+1}$, deve essere
$$
\frac{ |N(v) \cap S_t| }{ |N(v)| } \geqslant q > \frac{1}{2}
$$
ma dato che $S_t \subseteq S_{t+1}$ e vale
$$
1 = \frac{ |N(v) \cap S_t| }{ |N(v)| } + \frac{ |N(v) - S_t| }{ |N(v)| } > \frac{1}{2} + \frac{ |N(v) - S_t| }{ |N(v)| }
$$
allora vale
$$
|N(v) \cap S_t| > |N(v) - S_t| > |N(v) - S_{t+1}|
$$
dunque
$$
\begin{align}
|I_{t+1}| &= |I_t| - \sum_{v \in V_{t+1}} |N(v) \cap S_t| + \sum_{v \in V_{t+1}} |N(v) - S_{t+1}| \\
&= \sum_{v \in V_{t+1}} \left( |N(v) \cap S_t| - |N(v) - S_{t+1}| \right) \\
&< \sum_{v \in V_{t+1}} \left( |N(v) \cap S_t| - |N(v) - S_t| \right) \\
&< |I_t|
\end{align}
$$ 

Dunque è stato dimostrato che, per ogni $t \geqslant 0, |I_t| > |I_{t+1}|$ oppure $I_t = I_{t+1}$.
Poiché $V_0$ è un insieme finito e i nodi di $G$ hanno grado finito, allora l'interfaccia iniziale ha dimensione finita, ossia $|I_0| = k$ per qualche $k \in \mathbb{N}$.
Allora l'eventualità $I_t \neq I_{t+1}$ non può verificarsi per più di $k$ passi, perché ogni volta che $I_t \neq I_{t+1}$ è anche $|I_t| > |I_{t+1}|$ e la dimensione dell'interfaccia non può essere minore di zero (neanche uguale a zero in tempo finito su un grafo infinito).
Allora, esiste $T$ tale che, per ogni $t \geqslant T, I_t = I_{t+1}$; ossia, dal passo $T$ la diffusione si interrompe.
Ma in un grafo infinito una cascata completa può verificarsi solo in seguito a un processo di diffusione infinito, allora $V_0$ non genera una cascata completa.
```


### Nodi eterogenei
Il modello di diffusione considerato fino ad ora è un modello uniforme, cioè tutti i nodi associano lo stesso beneficio reciproco nell'adottare $A$ o $B$ che è, rispettivamente $a$ o $b$.
Tuttavia, questo modello è poco realistico: ciascun individuo nella rete ha un proprio beneficio nell'adottare $A$ o $B$, che può dipendere, ad esempio, dalle sue capacità di adattarsi al nuovo stato.

Allora, per ogni nodo $u$ nella rete, $a_u$ e $b_u$ sono il beneficio che $u$ ottiene nel relazionarsi, rispettivamente, con un nodo che adotta $A$ o con un nodo che adotta $B$.
Si assume, cioè, che il beneficio reciproco di adottare $A$ o $B$ da parte dei nodi adiacenti $u$ e $v$ sia quello illustrato nella seguente tabella.

![[AR/imgs/06-img09.png|center|500]]

Allora, un nodo $v \in V$ nello stato $B$ passa allo stato $A$ sulla base del valore $q_v = \frac{b_v}{a_v + b_v}$.

![[AR/imgs/06-img10.png|center|500]]

Osservando la figura, in cui accanto ad ogni nodo $v$ è indicata la sua soglia di adesione $q_v$, che anche se il nodo $1$ è in posizione centrale, non riuscirebbe a portare nessuno in $A$ se non fosse che $q_3 = 0.1$ è molto piccolo.
Allora, **non è sufficiente scegliere gli iniziatori in base alla loro centralità nella rete**, occorre anche considerare *la loro possibilità di avere accesso a nodi facilmente influenzabili*.
Così, nel caso di nodi eterogenei, a struttura che impedisce la generazione di una cascata completa è il blocking cluster.

```ad-info
$V' \subseteq V$ è un **blocking cluster** se, per ogni $v \in V'$
$$
\frac{ |N(v) \cap V'| }{ |N(v)| } \geqslant 1 - q_v
$$
```

Infatti

```ad-theorem
title: Teorema
Sia $G=(V,E)$ un grafo. Nel modello a nodi eterogenei, l’insieme di iniziatori $v_0 \subseteq V$ non genera una cascata completa se e solo se $G – V_0$ contiene un blocking cluster.
```

```ad-proof
title: Dimostrazione
ESERCIZIO.
```

## Azione collettiva
Si vuole ora mostrare come modellare mediante processi di diffusione situazioni nelle quali è richiesto che un'azione abbia luogo *collettivamente*. Si supponga di voler organizzare una protesta contro un regime dittatoriale. Ciascun individuo decide di aderire alla protesta solo se sa con certezza che un numero sufficientemente elevato di individui aderirà alla protesta. Poiché l'ambientazione è quella di una dittatura, la libertà di stampa è ostacolata e, in generale, le comunicazioni sono rese difficoltose.
### Modello
Ogni nodo $v$ sceglie una *soglia di confidenza* $k_v$ : aderirà alla protesta solo se almeno $k_v$ individui aderiranno alla protesta, ossia se oltre a lui, aderiranno altri $k_v - 1$ individui.
Poiché le comunicazioni circolano con difficoltà nella rete, le uniche informazioni che $v$ può ottenere sono circa l'adesione o meno alla protesta da parte degli individui con i quali h una relazione personale, ovvero, i suoi vicini nel grafo.
Inoltre, da ciascun nodo $u$ in $N(v)$, $v$ può sapere quale sia la soglia di adesione di $u$ e, assumendo che siano strong ties, $v$ sa che i vicini gli comunicano la loro vera soglia di adesione.
Ma $v$ non può sapere se $u$ ha o meno dei vicini che non siano anche suoi vicini, e non può neanche sapere se $u$ aderirà alla protesta: in base a quello che vede, $v$ può solo provare a dedurre cosa faranno i suoi vicini.
Si vede ora con degli esempi, in che modo i nodi arrivano a prendere una decisione.
#### Esempio 1
Si osservi la figura seguente:
![[AR/imgs/06-img11.png|center|400]]

- $x$ non aderisce, non ha abbastanza vicini;
- $v$ ha bisogno di due vicini che aderiscano: ma vede che $x$ vuole che almeno 3 vicini aderiscano per aderire a sua volta e, poiché $v$ vede di $x$ i soli vicini che hanno in comune (il nodo $u$), non può sapere se $x$ aderirà o meno e, quindi, $v$ non aderisce;
- ad $u$ sarebbe sufficiente che uno solo dei suoi vicini aderisse. Per lo stesso ragionamento fatto da $v$, $u$ non può sapere se $x$ aderirà o meno. Inoltre, $u$ sa che $v$ ha bisogno di almeno due vicini che aderiscano per aderire, ma non può sapere se $v$ dispone di informazioni supplementari circa l’adesione di $x$: perciò, non può dedurre che $v$ parteciperà. Di conseguenza, neanche $u$ aderisce.
#### Esempio 2

![[AR/imgs/06-img12.png|center|400]]

In questo esempio, $u$ vede che $k_{v} = k_{x} = 3$ e capisce che loro tre ($u, v$ e $x$) potrebbero aderire, ma $u$ non vede $y$, non sa se $v$ ha o meno altri vicini oltre sé stesso e, quindi, non sa se $v$ può dedurre che almeno due dei suoi vicini aderiranno, e poiché $u$ ha bisogno di certezze, $u$ non aderisce!
Poiché il grafo è perfettamente simmetrico, nessuno aderisce alla protesta anche se, qualora avessero avuto accesso a informazioni complete, la protesta avrebbe avuto luogo!
#### Esempio 3

![[AR/imgs/06-img13.png|center|400]]

In questo esempio invece, $u, v, x$ si vedono l’un l’altro, così, $u$ sa che $v$ e $x$, per partecipare, hanno bisogno che altri due partecipino, ma $u$ sa anche che anche $v$ e $x$ sanno esattamente le stesse cose che sa egli stesso e, poiché si fidano uno dell’altro, partecipano tutti e tre, senza aver bisogno di conoscere altro della rete, ossia indipendentemente da $y$.

### Conclusioni
In assenza di comunicazioni adeguate che abbiano luogo nella rete, l'azione collettiva si verifica difficilmente, ed ecco perché i regimi dittatoriali tendono a favorire *l’ignoranza pluralistica* che permette di concludere erroneamente che pochi individui abbiano una certa opinione, come accadeva nell’esempio 2.
Invece, l’esempio 3 permette di osservare l’importanza di disporre di una base di conoscenza comune, cioè l'importanza dei mezzi di diffusione delle informazioni, permettendo che tutti siano coscienti del fatto che un certo messaggio è conosciuto da tutti.

## Diffusione in presenza di compatibilità
Non è infrequente che due stat $A$ e $B$ possano coesistere, dunque ora si studieranno i processi di diffusione in presenza di compatibilità, cioè quando un nodo può essere nello stato $A$, nello stato $B$ o nello stato $AB$ nel modello *nodi omogenei*, cioè i benefici del trovarsi in un certo stato, dipendono soltanto dallo stato e sono gli stessi per tutti i nodi.
Naturalmente, un nodo adotta lo stato misto $AB$ ogniqualvolta ne trae beneficio, ma non può adottarlo sempre, perché adottare sia $A$ che $B$ ha un costo maggiore. In ogni caso, mentre i benefici ottenuti dall'adottare $AB$ sono proporzionali al numeri di vicini nel grafo, ossia si guadagna per ogni vicino che adotta $A$ e per ogni vicino che adotta $B$, il costo di adozione di $AB$ viene pagato una sola volta.

Sia $(u,v)$ un arco della rete: il beneficio reciproco di adottare $A$,$B$ o $AB$ sia quello illustrato nella tabella seguente.

![[AR/imgs/06-img14.png|center|500]]

Sia invece, per ogni nodo $u$, il costo per $u$ di essere nello stato $AB$ pari a $c$.
Siano $V_{A},V_{B},V_{C}$ gli insiemi dei nodi che sono, rispettivamente, negli stati $A,B$ e $AB$ (tutti i nodi hanno uno stato, dunque $V_{A} \cup V_{B} \cup V_{C} = V$).
Il beneficio per un nodo $u$ di essere nello stato $AB$ è
$$
\sum_{v \in V_{A} \cap N(u)} a + \sum_{v \in V_{B} \cap N(u)} b + \sum_{v \in V_{AB} \cap N(u)} \max \{a,b\} - c 
$$
Il beneficio per essere negli stati $A$ e $B$, rispettivamente, è
$$
\sum_{v \in (V_{A} \cup V_{AB}) \cap N(u)} a \\
$$
$$
\sum_{v \in (V_{B} \cup V_{AB}) \cap N(u)} b \\
$$

Si studia ora la capacità di cascata di una rete in presenza di compatibilità, in particolare:
- si analizza un esempio nel caso particolare in cui $G$ è una catena infinita;
- descrivendo i risultati di uno studio qualitativo nel caso bidimensionale ($G$ griglia infinita);
- dimostrando che, nel caso unidimensionale, l'andamento del processo di diffusione conferma quanto osservato dall'analisi qualitativa.
Si indica con $p_A(u), p_B(u)$ e $p_{AB}(u)$ il beneficio di un nodo $u$ nell'adottare, rispettivamente, $A,B$ o $AB$.
### Esempio: G è una catena infinita
Siano $a = 5, b = 3, c = 1$.

Per simmetrica, è sufficiente considerare una catena infinita solo a destra, il cui primo nodo è nello stato $A$ (giallo) e tutti gli altri sono nello stato $B$ (blu).

![[AR/imgs/06-img15.png|center|500]]

Al primo passo, $u$ adotta $AB$ (verde): infatti $p_A(u) = 5, p_B(u) = 3$ e $p_{AB}(u) = 5+3-1 = 7$

![[AR/imgs/06-img16.png|center|500]]

Al secondo passo $v$ adotta $AB$: infatti $p_A(u) = 5, p_B(u) = 3+3 = 6$ e $p_{AB}(u) = 5+3-1 = 7$

![[AR/imgs/06-img17.png|center|500]]

Al terzo passo: $z$ adotta $AB$ per le stesse ragioni di $v$, ma ora a $u$ conviene abbandonare $AB$ e passare ad $A$ perché $p_A(u) = 10, p_B(u) = 0$ e $p_{AB}(u) = 5+5-1 = 9$

![[AR/imgs/06-img18.png|center|500]]

A questo punto, il fenomeno si ripete:

![[AR/imgs/06-img19.png|center|500]]

dopo un periodo transitorio durante il quale un nodo adotta lo stato misto, esso passerà ad adottare definitivamente il nuovo stato e quindi si genera una cascata completa nella quale il vecchio stato viene completamente soppiantato dal nuovo.

Si ricorda che, nel caso di esclusività fra $A$ e $B$, la soglia di adozione $q$ è definita come $q = \frac{a}{a+b}$. Essa rappresenta la frazione minima dei vicini di un nodo che deve essere nello stato $A$ per convincere quel nodo a passare anch'esso allo stato $A$, e tale frazione dipende dai benefici relativi che ha quel nodo nel trovarsi in uno dei due stati.
Riscrivendo $q$ come 
$$
q = \frac{1}{\frac{a}{b}+1} 
$$
si evidenzia il ruolo giocato dal rapporto fra il *valore* di $A$ e il *valore* di $B$ nella decisione di un nodo di rimanere nello stato $A$ o passare allo stato $B$.
Nel caso di esclusività fra $A$ e $B$, abbiamo dimostrato che **non si può generare una cascata completa (in un grafo infinito) se** $q > \frac{1}{2}$, ossia, se il *valore* di $A$ non è almeno pari al *valore* di $B$.

Si analizza ora nel caso di compatibilità fra $A$ e $B$.
Intanto si osserva che l'operazione di dividere per $b$ ha permesso di ridursi a studiare i processi di diffusione in funzione del solo parametro $a/b$ (ciò è equivalente a fissare $b=1$ e studiare i processi di diffusione in funzione di $a$).
Analogamente, nel caso di compatibilità fra $A$ e $B$, si fissa $b=1$ e si studiano i processi di diffusione in funzione di $a$ e $c$.

Uno studio qualitativo ha evidenziato uno strano comportamento:
- lo stato $A$ si impone quando $a$ è molto grande rispetto a $c$;
- invece, lo stato $A$ fa fatica a imporsi quando $c$ è molto grande rispetto ad $a$;
- infine, $A$ fa fatica a imporsi anche quando $c$ non è né troppo grande né troppo piccolo rispetto ad $a$ - questo è strano.
Per comprendere questa stranezza, consideriamo sempre la catena infinita $G$ con parametri $a$ e $c$.

![[AR/imgs/06-img20.png|center|500]]

Se $x$ è nello stato $A$ (giallo) e $v$ nello stato $B$ (blu), poiché $p_A(u) = a, p_B(u) = 1, p_{AB}(u) = a+1-c$ :
- se $p_{A}(u) \geqslant p_{B}(u)$ e $p_{A}(u) \geqslant p_{AB}(u)$, $u$ adotterà $A$, e questo accade quando $a \geqslant 1 \land a \geqslant a+1-c$, ossia quando $a \geqslant 1 \land c \geqslant 1$;
- se $p_{B}(u) > p_{A}(u)$ e $p_{B}(u) > p_{AB}(u)$, $u$ rimarrà in $B$, e questo accade quando $1 > a \land 1 > a+1-c$, ossia quando $a < 1 \land a < c$;
- se $p_{AB}(u) > p_{A}(u)$ e $p_{AB}(u) \geqslant p_{B}(u)$, $u$ adotterà $AB$, e questo accade quando $a+1-c > a \land a+1-c \geqslant 1$, ossia quando $c < 1 \land a \geqslant c$;

Si riassume tutto ciò nel seguente grafico nel piano $ac$
- $a \geqslant 1$ e $c \geqslant 1$, $u$ adotterà $A$: regione gialla;
- $a < 1$ e $a < c$, $u$ rimarrà in $B$: regione blu;
- $c < 1$ e $a \geqslant c$, $u$ adotterà $AB$: regione verde.

![[AR/imgs/06-img21.png|center|500]]

Si possono trarre delle conclusioni:
- se $u$ è rimasto nello stato $B$, allora la diffusione dello stato $A$ è stata bloccata sul nascere: il processo di diffusione non ha avuto nemmeno inizio;
- se $u$ è passato allo stato $A$, allora, al passo successivo il nodo $v$ (adiacente a $u$) passerà allo stato $A$, e così via: si è innescato il processo di diffusione di $A$, che genererà una diffusione completa senza mai passare per lo stato misto $AB$.
Resta da studiare cosa accade quando i parametri cadono nella regione verde, cioè quando siamo nella situazione nella figura seguente, dove $u$ ha adottato $AB$.

![[AR/imgs/06-img22.png|center|500]]

Si vede ora quale stato conviene adottare al nodo $v$.