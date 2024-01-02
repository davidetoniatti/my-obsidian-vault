# Introduzione

```ad-info
Il materiale descritto per questo argomento si trova nel capitolo 3 del libro di testo e nella dispensa Communities [lucidi 12-29].
```

Fino ad ora si sono studiati grafi che modellano reti a livello statistico; infatti, si è usato pesantemente il calcolo delle probabilità.
In questo modo, si sono studiate proprietà che valevano *mediamente* per i nodi, e non si è posto interesse sulle *peculiarità* dei singoli nodi.

Si comincia ora una serie di lezioni in cui si cambia il punto di vista dell'analisi delle reti, dunque si analizzeranno le posizioni dei singoli nodi all'interno della rete per studiare:
- se è possibile evidenziare particolari strutture nella rete;
- se tutti i nodi si trovano, grosso modo, nella stessa situazione all'interno della rete, oppure vi sono delle differenze osservabili fra nodo e nodo;
- se i nodi possono trarre vantaggio da queste differenze.
## L'esperimento di Granovetter
Negli anni '60 il sociologo statunitense Mark Granovetter, nel preparare la sua tesi dottorato, intervistò un gruppo di individui che avevano recentemente cambiato lavoro.
Granovetter fece loro una serie di domande volte a capire in che modo erano venuti a conoscenza della possibilità di ottenere l'impiego che avevano ottenuto.
Il risultato fu che molti degli intervistati avevano avuto informazioni che li avevano condotti alla loro occupazione attuale attraverso una comunicazione da parte di qualcuno che conoscevano, una sorta di passa parola, e questo può ben essere prevedibile.
Il fatto abbastanza inaspettato che emerse dalle sue interviste fu che spesso l'informazione era arrivata da qualcuno che conoscevano superficialmente, cioè non da amici stretti.

```ad-important
Spesso l’informazione era arrivata da qualcuno che conoscevano superficialmente, non dagli amici più stretti
```

Allora, l'esperimento di Granovetter sembra suggerire che non è tanto la forza del legame quella che ha aiutato a trovare lavoro, **quanto il tipo di informazione che una relazione è capace di veicolare**.
In effetti gli amici stretti frequentano, grosso modo, gli stessi posti che frequentiamo noi, dunque abbiamo accesso alle stesse fonti di informazioni.
Invece, i conoscenti lontani frequentano posti diversi da quelli che si frequentano personalmente, dunque hanno accesso a informazioni differenti, ossia provenienti da zone diverse della rete, alle quali non si è in gradi di accedere senza di loro.
Si modella ora questo concetto tramite la definizione dei concetti di **Bridges e Local Bridges**.
# Reti come grafi dalla struttura disomogenea
## Bridges e Local Bridges
Si consideri il grafo $G=(V,E)$  che modella la rete, gli archi che *aumentano* le nostre informazioni sono quelli che collegano un soggetto a regioni della rete inaccessibili ai suoi amici.
Si definisce come **bridge** un arco la cui rimozione disconnette la rete. Dall'esperimento di Granovetter, segue che i bridge sono gli archi che hanno il maggior *valore informativo*.
D'atra parte, è stato dimostrato che una rete contiene con buona probabilità componenti giganti densamente connesse, dunque la presenza di un bridge è poco probabile.
Per questo motivo, si rilassa la definizione: un arco è un **local bridge** se i suoi vicini non hanno vicini in comune: $(u,v) \in E$ è un **local bridge** se $N(u) \cap N(v) = \emptyset$ , dove $N(u)$ è l'insieme dei nodi adiacenti (vicini) al nodo $u$.
## Legami forti e deboli
In una rete sociale, si possono riconoscere due tipi di legami tra gli individui:
- **relazioni forti**: che collegano un soggetto agli amici stretti;
- **relazioni deboli**: che collegano un soggetto a semplici conoscenti.
Dunque si modella una rete di questo tipo mediante un grafo $G$ in cui gli archi sono partizionati in due sottoinsiemi: **archi forti (strong ties)** $S$ e **archi deboli (weak ties)** $W$, ossia $G=(V,S \cup W)$ con $S \cap W = \emptyset$.
Come ha mostrato l'esperimento di Granovetter, gli archi deboli veicolano informazioni alle quali non si avrebbe accesso tramite gli archi forti: e questa è la **forza degli archi deboli**.
### Legami forti e deboli e chiusura triadica
Si osserva che, gli amici cari di un soggetto hanno buone probabilità di entrare in contatto fra loro, dato che probabilmente frequentano gli stessi posti che frequenta l'amico in comune.
Dunque se c'è una relazione di amicizia fra $a$ e $b$ e una relazione di amicizia fra $a$ e $c$, ossia gli archi $(a,b),(a,c)$ sono **archi forti**, allora è probabile che prima o poi si creerà anche l'arco $(b,c)$, generando una **chiusura triadica**.
Quindi per descrivere queste reti è necessario utilizzare grafi dinamici, che evolvono nel tempo.
Alla base della creazione di chiusure triadiche si possono individuare ragioni diverse:
- opportunità di incontrarsi;
- incentivo a stringere una relazione – se $c$ vuole andare al cinema con l'amico $a$ e lui va sempre con il suo amico $b$, a $c$ conviene diventare amico di $b$;
- fiducia – se $c$ si fida di $a$ che si fida di $b$, allora anche $c$ si può fidare di $b$.

Si supponga ora di lasciar evolvere la rete fintantoché non si raggiunga una configurazione stabile, cioè che non cambia più nel tempo. Allora probabilmente, tutti le possibili chiusure triadiche si sono formate.
Formalmente, sia $G=(V,S \cup W)$: un nodo $u$ soddisfa la proprietà della **chiusura triadica forte (STCP)** se, per ogni coppia di archi forti incidenti su $u$, i loro estremi sono collegati da un arco: $u \in V$ soddisfa la **STCP** se
$$ \forall (u,v) \in S,  \forall (u,z) \in S \left[ (v,z) \in S \cup W \right] $$
 Allora il grafo $G$ soddisfa la **STCP** se tutti i suoi nodi la soddisfano.
```ad-theorem
title: Teorema
Sia $G=(V,S \cup W)$; se un nodo $u \in V$ soddisfa la STCP e se esistono due nodi distinti $x$ e $z$ tali che $(u,x) \in S$ e $(u,z)$ è un local bridge, allora $(u,z) \in W$.
```

```ad-proof
title: Dimostrazione
Poiché $(u,z)$ è un local bridge, vale $N(u) \cap N(z) = \emptyset$; se fosse $(u,z) \in S$, allora, poiché $(u,x) \in S$ e $u$ soddisfa la STCP, dovrebbe essere $(x,z) \in S \cup W$, ossia sarebbe $x \in N(u) \cap N(z)$, ossia $N(u) \cap N(z) \neq \emptyset$, che è una contraddizione.
```
Questo teorema mostra come una proprietà globale (essere un local bridge) si riflette in una proprietà locale (essere un weak tie).
- Essere weak tie è proprietà locale: un nodo sa da solo se un suo vicino è un amico o un conoscente;
- Essere local bridge è una proprietà globale: un nodo deve chiedere a tutti i suoi vicini se qualcuno di loro conosce un suo conoscente per sapere se è un local bridge.
In effetti, i weak ties dell'esperimento di Granovetter, in quanto portatori di nuova informazione, erano anche local bridges.

L'evoluzione di una rete sociale, con un graduale aumento delle chiusure triadiche, porterà a formare nella rete un **gruppo di nodi fortemente coeso**, cioè con un elevato grado di interconnessione fra i nodi che lo compongono. A questo gruppo coeso si da il nome di **cluster** o **comunità**.
Per misurare il grado di coesione di un nodo $u$ all'interno di un gruppo di nodi è stato definito il **coefficiente di clustering** $c(u)$ come il rapporto fra il numero di relazioni fra vicini di $u$ rispetto a tutte le coppie possibili di vicini di $u$:

$$ c(u) = \frac{ | \{ (x,y) \in E: x \in N(u) \land y \in N(u) \} | }{ \frac{ |N(u)| \cdot (|N(u)| - 1) }{2} } $$
Informalmente, il coefficiente di clustering misura quanto un nodo è *ben inserito* all'interno della rete costituita dai suoi vicini:
- un nodo con coefficiente di clustering basso è *male* inserito nel gruppo dei suoi amici, cioè si trova, in quel gruppo, in una posizione periferica;
- un nodo con coefficiente di clustering elevato è *molto* inserito nel gruppo dei suoi amici, cioè è un elemento centrale nel gruppo dei suoi amici;
difatti, il coefficiente di clustering è un **indice di centralità** di un nodo.
Sia $C$ un sottoinsieme di nodi: si definisce per ogni nodo $u \in C$ il **coefficiente di clustering di** $u$ **relativo a** $C$ come
$$
c_{C}(u) = \frac{ | \{ (x,y) \in E: x \in N(u) \cap C \land y \in N(u) \cap C \} | }{ \frac{ |N(u) \cap C| \cdot (|N(u) \cap C| - 1) }{2} }
$$
Questa quantità definisce quanto il nodo $u$ è integrato all'interno del sottoinsieme di nodi $C$: più il coefficiente $c_C(u)$ è elevato più $u$ è *centrale* all'interno di $C$, contrariamente più è basse, meno $u$ è integrato.
Dunque si può concludere che se $C$ ha tutti nodi con coefficiente di clustering alto, allora $C$ è una **comunità** (o **cluster**).
Ma di preciso quanto deve essere elevato questo valore per poter stabilire che $C$ è una comunità?
Inoltre, il coefficiente di clustering è un buon fattore da considerare per identificare le comunità?
# Comunità
## Cut-communities
Dato un grafo $G=(V,E)$, una **cut-community** è un sottoinsieme proprio e non vuoto $C \subset V$ dei nodi di $G$ che **minimizza** gli **archi del taglio**, ossia gli archi che collegano nodi $C$ a nodi in $V\setminus C$. Formalmente, $C \subset V$ è una **cut-community** se $C \neq \emptyset$ e
$$
\mid \{ (u,v): u \in C \land v \in C \setminus V \}\mid = \min_{C' \subset V: C' \neq \emptyset} (\mid\{ (u,v): u \in C' \land v \in C' \setminus V \})
$$
Per calcolare una cut-community, si utilizza l'algoritmo di **Ford-Fulkerson**, che permette di calcolare il taglio minimo che separa due nodi $s$ e $t$. Dunque l'algoritmo calcola un sottoinsieme $C \subset V$ tale che $s \in C$, $t \in V \setminus C$ e
$$
\mid \{ (u,v): u \in C \land v \in C \setminus V \}\mid = \min_{C' \subset V: s \in C' \land t \in V \setminus C'} (\mid\{ (u,v): u \in C' \land v \in C' \setminus V \})
$$
Allora calcolare una cut-community è sufficiente calcolare i tagli minimi per ogni coppia di nodi $s,t$ e scegliere quello più piccolo tra essi.

Il problema di questo metodo è che non c'è modo di controllare la dimensione della comunità $C$ risultante, infatti potrebbe capitare che $C$ sia composta dal solo elemento $s$, e ciò è in disaccordo col concetto intuitivo di comunità (una sola persona non può costituire una comunità). Si consideri il grafo in figura
![](05-ar_img01.png)
in questo esempio, comunque si scelga un nodo $x$, l'<u>unico</u> taglio minimo è del tipo $(\lbrace x \rbrace, V \setminus \lbrace x \rbrace)$, di dimensione 7. Infatti, aggiungendo un qualsiasi nodo insieme ad $x$ si ottiene un taglio di dimensione strettamente maggiore di 7.
Per esempio $12 = \vert (\lbrace u_0, u_1 \rbrace, V \setminus \lbrace u_0, u_1 \rbrace ) \vert > \vert (\lbrace u_0 \rbrace, V \setminus \lbrace u_0 \rbrace ) \vert = 7$.

Dunque, la definizione di cut-community non è ragionevole per modellare le comunità.
## Web-communities
Una (**strong**) **web-community** è un sottoinsieme **proprio e non vuoto** $C \subset V$, tale che i nodi che lo compongono hanno più vicini all'interno di $C$ stesso anziché all'esterno, ovvero
$$
\forall u \in C \left[ \vert N(u) \cap C \vert > \vert N(u) \setminus C \vert = \vert N(u) \cap (V \setminus C) \vert  \right]
$$
Alternativamente, risulta comodo guardare alla precedente proprietà come il fatto che *almeno* la metà dei vicini di $u$ è all'interno di $C$
$$
\frac{\vert N(u) \cap C \vert}{\vert N(u) \vert} > \frac{1}{2}
$$
Rilassando la relazione di maggioranza stretta, si definisce in maniera analoga una **weak web-community** come un qualsiasi sottoinsieme $C$ non vuoto e proprio di $V$ tale che
$$
\frac{\vert N(u) \cap C \vert}{\vert N(u) \vert} \geq \frac{1}{2}
$$
## Relazione tra cut- e web-communities
Le definizioni di cut-community e web-community sono correlate, infatti vale il seguente teorema.
### Teorema
Sia $G=(V,E)$ un grafo; se $C \subset V$ è una cut-community per $G$ tale che $\mid C\mid > 1$ allora $C$ è una weak web-community per $G$.
Più formalmente, dato un sottoinsieme $C \subset V$ tale che
$$
   \vert (C, V \setminus C) \vert = \min_{A \subset V}{ \lbrace \vert (A, V \setminus A) \vert \rbrace }\\
   \land\\
   1 < \vert C \vert < \vert V \vert$$
 allora $$\forall u \in C \; \left[ \vert N(u) \cap C \vert \geq \vert N(u) \setminus C \vert \right]$$
#### Dimostrazione
Si supponga per assurdo che $C$ non sia una weak web-community. Allora, esiste un nodo $u \in C$ tale che
$$
\mid N(u) \cap C \mid < \mid N(u) - C \mid
$$
Poiché $C$ contiene almeno due nodi, allora esiste in $C$ un nodo $v$ distinto da $u$, ossia $C' = C-\{ u \} \neq \emptyset$. Si consideri il taglio indotto da $C'$
$$
(C-\{u\}, (V-C) \cup \{u\}) = (C',V \setminus C')
$$ 
La dimensione di tale taglio è pari a
$$
\begin{align}
\mid (C',V \setminus C') \mid &=  \mid (C-\{u\}, (V-C) \cup \{u\}) \mid \\
&= \mid  (C,V \setminus C) \mid - \mid \{ (u,z): z \in N(u) \setminus C \} \mid + \mid \{ (u,z): z \in N(u) \cap C \} \mid \\
&= \mid  (C,V \setminus C) \mid - \mid N(u) \setminus C  \mid + \mid N(u) \cap C \mid \\
&< \mid  (C,V \setminus C) \mid
\end{align}
$$
dunque, $C'$ è un sottoinsieme proprio e non vuoto di $V$ e il numero di archi del taglio indotto da $C'$ minore del numero di archi del taglio indotto da $C$. Questo è assurdo dato in quanto essendo $C$ per ipotesi una cut-community deve essere vero che $(C, V \setminus C)$ è un taglio minimo di $G$.
### SWCP è NP-Completo
Anche se esiste un algoritmo polinomiale che decide se un grafo è partizionabile in cut-community, non si conosce un algoritmo che **decida** in tempo polinomiale se un grafo è partizionabile in web-communities.

Infatti, si possono trovare due cut-communities $C$, $V \setminus C$ che generino un taglio minimo, ma non si garantisce che $\vert C \vert > 1$ e $\vert V \setminus C \vert > 1$.

In realtà, è possibile dimostrare che il problema di decidere se un grafo è partizionabile in web-communities è **NP-completo**.

Dato un grafo $G=(V,E)$, decidere se esiste un sottoinsieme proprio e non vuoto $C$ di $V$ tale che $C$ e $C-V$ sono due strong web-communities.
```ad-theorem
title: Teorema
Il problema SWCP è NP-completo
```
Prima di dimostrare il teorema, si dimostra il seguente lemma.
#### Lemma
Se $G=(V,E)$ è partizionabile in due strong web-communities e esistono $x,y,z \in V$ tali che $N(x) = \{y,z\}$ ($x$ ha grado 2 in $G$), <u>allora</u> per ogni $C \subset V$ tale che $C$ e $C-V$ sono due strong web-communities $x,y,z \in C$ oppure $x,y,z \in V-C$.
##### Dimostrazione
Sia $C \subset V$ tale che $C$ e $C-V$ sono due strong web-communities. Senza perdita di generalità, sia $x \in C$:
- se fosse $y \in V-C$ e $z \in V-C$, allora $$\mid N(x) \cap C \mid = 0 < \mid N(x) \cap (V-C) \mid = 2$$
- se fosse $y \in C$ e $z \in V-C$, allora $$\mid N(x) \cap C \mid = 1 = \mid N(x) \cap (V-C) \mid $$
- se fosse $y \in V-C$ e $z \in C$, allora $$\mid N(x) \cap C \mid = 1 = \mid N(x) \cap (V-C) \mid $$
In tutti e tre i casi verrebbe contraddetta l'ipotesi che $C$ e $V-C$ sono due strong web-communities.
#### Dimostrazione NP-completezza
Il problema SWCP è in NP. Infatti, un certificato è un sottoinsieme di nodi $C \subset V$ e verificare che $C$ e $V-C$ sono strong web-communities richiede tempo polinomiale in $\mid G \mid$.

Per dimostrare la completezza, è necessario mostrare una **riduzione** da un problema già noto essere **NP-completo** al problema in questione.
In questo caso, si mostra una riduzione a partire dal problema 3-SAT.
$$\mbox{3-SAT} \preccurlyeq_{P} SWCP$$
Siano $X = \{x_1,\dots,x_n\}$ insieme di variabili booleane e $f = c_1 \land c_{2} \land \dots \land c_{m}$ una formula booleana in forma normale congiuntiva dove ogni clausola $c_j = l_{j_{1}},l_{j_{2}},l_{j_{3}}$ contiene tre letterali tali che $l_{jh} \in X$ oppure $\lnot l_{jh} \in X$ per $j = 1,\dots,m$ e $h = 1,2,3$.
Si costruisce un grafo $G$ nel modo seguente.

L'insieme dei nodi $V$ è composto dai nodi $T$ e $F$ che rappresentano le assegnazioni vero e falso. Questi due nodi sono posizionati nel grafo in modo tale che essi dovranno necessariamente appartenere a due comunità differenti affinché $G$ sia partizionabile in due strong web-community. In altre parole, $T$ e $F$ appartengono alla stessa comunità se e soltanto se $C=V$, dunque quando $C$ non è una comunità, poiché non è contenuta propriamente in $V$.

Per ogni variabile $x_i \in X$, si costruisce il seguente **gadget variabile**:
1. Si inseriscono in $V$ i nodi $x_{i},w_{i},y_{i},z_{i},t_{i},f_{i}$;
2. Si inseriscono in $E$ gli archi $(x_{i},y_{i}),(x_{i},z_{i}),(w_{i},z_{i}),(w_{i},y_{i}),(y_{i},t_{i}),(z_{i},f_{i})$;
3. Per il nodo $x_i$ si aggiungono dei vicini senza nome, tanti quante sono le clausole che contengono la variabile $x_i$ più uno;
4. Per il nodo $w_i$ si aggiungono dei vicini senza nome, tanti quante sono le clausole che contengono la variabile $\lnot x_i$ più uno;
![Esempio Gadget Clausola|center|400](05-ar_img02.png)
Si osserva che se $T$ e $F$ sono nella stessa comunità, allora tutti i nodi in figura sono in quella comunità. Invece, se $T$ e $F$ sono in due comunità distinte, allora anche $x_i$ e $w_i$ appartengono a due comunità distinte. Infatti, poiché $t_i$ e $f_i$ hanno entrambi grado 2, per il [lemma precedente](#Lemma) deve necessariamente essere vero che  $t_i, y_i $ fanno parte della stessa comunità di $T$ e $f_i, z_i$ fanno parte della stessa comunità di $F$.
Poiché $y_i$ ha grado 3, allora almeno 1 tra $x_i$ e $w_i$ deve appartene alla comunità di $T$. Similmente per $z_i$, poiché ha grado 3 allora almeno uno tra $x_i$ e $w_i$ deve appartene alla comunità di $F$.
Poiché $y_i$ e $z_i$ non sono collegati, allora o $x_i$ appartiene e $C$ e $w_i$ a $V-C$, oppure viceversa.
Infine, collocati i nodi $x_i$ e $w_i$ nelle rispettive comunità, essi si porteranno appresso i rispettivi vicini senza nome.

Per ogni clausola $c_j$ di $f$, si costruisce il seguente **gadget clausola**:
1. Si inseriscono in $V$ i nodi $c_{j},l_{j_{1}},l_{j_{2}},l_{j_{3}}$;
2. Si inseriscono in $E$ gli archi dal nodo $c_j$ verso i nodi $l_{j_{1}},l_{j_{2}},l_{j_{3}}$ e dai nodi $l_{j_{1}},l_{j_{2}},l_{j_{3}}$ al nodo $T$;
3. Si inseriscono in $E$ gli archi per collegare il nodo $c_j$ ai letterali che compongono la clausola corrispondente: si inserisce l'arco $(c_j,x_i)$ se $c_j$ contiene il letterale $x_i$ e si inserisce l'arco $(c_j,w_i)$ se $c_j$ contiene il letterale $\lnot x_i$.
Un esempio di gadget clausola con $c_j = x_i \lor \lnot x_{2} \lor x_{3}$.
![|center|400](05-ar_img03.png)
Si osserva che, sempre per il [lemma precedente](#Lemma), dato che $l_{j1},l_{j2},l_{j3}$ hanno tutti grado 2, allora se $G$ è partizionabile in due strong web-community certamente sia $l_{j1},l_{j2},l_{j3}$ che $c_j$ devono essere nella stessa comunità a cui appartiene $T$.
Inoltre poiché $c_j$ ha grado esattamente $6$, e poiché 3 dei suoi vicini sono nella comunità a cui appartiene $T$, allora basta che **almeno uno** tra i suoi nodi gadget variabile (i suoi letterali) (nell'esempio $x_1, w_{2}, x_3$) appartenga anch'esso alla comunità in cui si trova $T$.
Dalla costruzione appena fatta emerge che quindi $c_j$ deve appartenere alla stessa comunità a cui appartiene $T$, e per rendere ciò possibile **almeno uno** dei letterali che lo compongono deve appartenere alla stessa comunità; altrimenti, $c_j$ avrebbe tanti vicini nella comunità di $T$ quanti nella comunità di $F$.

Un esempio di grafo $G$ è il seguente, con
$$ f(x_{1},x_{2},x_{3}) = (x_{i}\lor \lnot x_{2} \lor x_{3}) \land (\lnot x_{1} \lor \lnot x_{2} \lor \lnot x_{3}) $$
![|center|700](05-ar_img04.png)

Si mostra ora che affinché $G$ sia partizionabile in due strong web-communities è necessario che $T$ e $F$ appartengano a due comunità differenti, mostrando che se $T$ e $F$ fanno parte della stessa comunità $C$, allora tutti i nodi $V$ sono in $C$.
Poiché $T$ e $F$ sono in $C$ e poiché per ogni $1 \leq j \leq m$ i nodi $l_{j1},l_{j2},l_{j3}$ hanno grado 2, allora $l_{j1},l_{j2},l_{j3}, c_{j}$ devono esser contenuti in $C$.
Inoltre, poiché $T$ e $F$ sono in $C$ e poiché per ogni $1 \leq i \leq n$ i nodi $t_{i},f_{i}$ hanno grado 2, allora $t_{i},f_{i},y_{i}, z_{i}$ devono esser contenuti in $C$.
Allora, per ogni $1 \leq i \leq n$, se il letterale $x_i$ è contenuto in $k$ clausole allora il nodo $x_i$ ha $k+2$ vicini <u>con nome</u> in $C$. Perciò, per poter essere inserito in $V-C$ il nodo $x_i$ dovrebbe avere almeno $k+2$ vicini in $V-C$, ma gli altri vicini di $x_i$ sono i $k+1$ nodi senza nome, che sono $k+1: non abbastanza per permettere a $x_i$ di far parte di $C$. Perciò, $x_i$ e i $k+1$ nodi senza nome ad esso collegati devono essere in $C$.
Analogamente vale per il letterale $\lnot x_i$ e il corrispettivo nodo $w_i$.

Riassumendo, affinché $G$ sia partizionabile in due strong web-communities è necessario che $T$ e $F$ appartengano a due comunità differenti.
Si ricorda che se $T$ e $F$ sono in due comunità differenti, allora per ogni variabile $x_i \in X$ anche i nodi $x_i$ e $w_i$ associati appartengono a due comunità distinte (uno nella stessa comunità di $T$, l'altro nella stessa comunità di $F$). Questo significa che ogni partizione di $G$ in due strong web-communities corrisponde ad una assegnazione di verità $a$ per $X$: si assegna vero a tutti i letterali corrispondenti a nodi che sono nella stessa comunità di $T$ e falso a tutti gli altri, o viceversa.
Dunque, per ogni $i \in [n]$:
- se $x_i$ è nella stessa comunità di $T$ e $w_i$ nella stessa comunità di $F$, allora $a(x_i) = \texttt{true}$, mentre se $x_i$ è nella stessa comunità di $F$ e $w_i$ nella stessa comunità di $T$, allora $a(x_i)= \texttt{false}$;
- oppure, viceversa, se $x_i$ è nella stessa comunità di $T$ e $w_i$ nella stessa comunità di $F$, allora $a(x_i) = \texttt{false}$, mentre se $x_i$ è nella stessa comunità di $F$ e $w_i$ nella stessa comunità di $T$, allora $a(x_i)= \texttt{true}$;
Si ricorda inoltre che se $T$ e $F$ sono in due comunità differenti, allora per ogni clausola $c_j$ il nodo $c_j$ **deve** appartenere alla comunità che contiene $T$, e questo è possibile solo se almeno uno dei nodi nei gadget variabile collegato a $c_j$ è contenuto nella comunità di $T$.

Si conclude la prova mostrando che $G$ è partizionabile in due strong web-communities se e soltanto se $f$ è soddisfacibile.
- $(\implies)$. Se $G$ è partizionabile in due strong web-communities, allora $T$ e $F$ **non** sono nella stessa comunità. Allora, per ogni variabile $x_i \in X$ i due nodi $x_i$ e $w_i$ devono essere contenuti in due comunità distinte. Dunque, si pongono a `true`  tutti gli $x_i$ che corrispondono a nodi contenuti nella comunità di $T$ e a `false` tutti gli altri. Inoltre, per ogni clausola $c_j$ il nodo $c_j$ deve appartenere alla comunità di $T$, e questo è possibile se almeno uno dei nodi gadget variabile collegato a $c_j$ è contenuto nella comunità di $T$: se tale letterale è $x_i$, allora $x_i$ è nella comunità di $T$ e $a(x_i) = \texttt{true}$, se tale letterale è $\lnot x_i$, allora $w_i$ è nella comunità di $T$ e $a(x_i) = \texttt{false}$. Quindi, $a$ è una assegnazione di verità che soddisfa ogni clausola in $f$, ossia, $f$ è soddisfacibile.
- $(\impliedby)$. Se $f$ è soddisfacibile, sia $a$ una assegnazione di verità per $X$ che soddisfa ogni clausola $c_j$ in $f$. Si inseriscono nella stessa comunità di $T$:
	- tutti i nodi $c_j, l_{j1},l_{j2},l_{j3}$ per ogni $j \in [m]$;
	- tutti i nodi $x_i,y_i,t_i$ e i senza nome adiacenti ad $x_i$ tali che $a(x_i) = \texttt{true}$;
	- tutti i nodi $w_i,y_i,t_i$ e i senza nome adiacenti ad $w_i$ tali che $a(x_i) = \texttt{false}$;
	Poiché $a$ soddisfa tutte le clausole, 