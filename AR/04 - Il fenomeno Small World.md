# L'esperimento di Milgram
Lo psicologo sociale [Stanley Milgram](https://en.wikipedia.org/wiki/Stanley_Milgram) nel 1967 condusse il seguente esperimento:
- Scelse un destinatario, cioè una persona alla quale doveva essere recapitata una lettera della quale lo stesso Milgram era il mittente, che risiedeva molto lontano rispetto a lui.
- Scelse a caso un insieme di persone, dette iniziatori, e ha consegnato ad ognuno di essi una copia della lettera insieme ad una serie di informazioni sul destinatario: nome, indirizzo, occupazione e informazioni personali.
- Chiese ad ogni iniziatore di fare in modo di far giungere la copia della lettera al destinatario, senza inviargliela direttamente: ogni iniziatore doveva, con l'obiettivo di far giungere la lettera al destinatario nel minor numero di passi possibile, inviarla a un suo <u>diretto</u> conoscente chiedendogli di ripetere le medesime azioni.

Al termine dell'esperimento Milgram osservò che circa un terzo delle lettere riuscirono ad arrivare al target, ma ancor più interessante che tutte arrivarono mediamente in *sei passi*.

Questo esperimento suggerì due proprietà importanti delle reti sociali:
1.  un'abbondanza di **cammini molto brevi** tra gli individui della rete, noto come **fenomeno Small World** (o *"Six Degrees of Separation"*).
2. tali cammini possono essere facilmente trovati dagli individui, che altro non conoscono della struttura della rete se non il loro vicinato ed una *euristica* che gli consenta di intuire i vicini più plausibili per i cammini. Questo è noto come **fenomeno della navigabilità**.
## Una moltitudine di percorsi brevi
Il risultato dell'esperimento di Milgram suggerisce che in una rete sociale esistono **tanti cammini minimi** fra coppie di nodi. Una motivazione intuitiva di tale risultato è la seguente

> Si supponga di conoscere in maniera diretta 100 persone, e che tali persone conoscono direttamente altre 100 persone.
> Allora con due "hop" si possono raggiungere $100 \cdot 100 = 10000$ altre persone.
> E se a loro volta gli amici dei miei amici conoscono 100 persone, in tre "hop" si possono raggiungere altri $100 \cdot 100 \cdot 100 = 1000000$ persone, e così via...

Questo ragionamento però non è del tutto consistente: infatti si assume che ogni persona conosca altre $100$ persone *distinte*. Questa assunzione è poco plausibile nella realtà: se una persona è amica stretta di due persone distinte, è *molto probabile* che queste due persone si conosceranno e diventeranno amiche, formando un *triangolo* tra questi individui.
La caratteristica di una rete di avere molti di questi *triangoli* è detta **chiusura triadica** (**triadic closure**), la quale diminuisce di molto il numero di individui raggiungibili con percorsi brevi rispetto al modello con crescita esponenziale.
![La chiusura triadica diminuisce di molto il numero di persone raggiungibili con percorsi brevi.|center|600](04-ar_img01.png)
# Il modello Watts-Strogatz
Nel 1998 **Watts** e **Strogatz** proposero un modello generativo di grafi aleatori che genera grafi che soddisfano le due proprietà viste nell'esperimento di Milgram: la presenza di numerose chiusure triadiche e la presenza di molti cammini minimi tra coppie di nodi (Small World).
Il grafo generato in accordo con questo modello è composto da:
- una componente **deterministica** che consiste in una **griglia bidimensionale**;
- una componente **aletoria** al di sopra della griglia.
In particolare, la griglia bidimensionale è un sottoinsieme di $\mathbb{N}^2$, i cui *punti* sono i nodi del grafo. Nella griglia ogni nodo ha un arco con i suoi vicini posti a destra, sinistra, sopra, sotto e in diagonale.
Più formalmente, fissato un $n \in \mathbb{N}$:
$$
V \equiv \lbrace (i,j) \in \mathbb{N}^2 | 0 \leqslant i < n \land 0 \leqslant j < n \rbrace
$$
$$
E_1 \equiv \lbrace \lbrace (i,j), (a,b) \rbrace \in V^2 | \left[ a \equiv_n i+\alpha \land b \equiv_n j+\beta \right] \; \forall \alpha,\beta = -1, 0, 1 \ \land \ (i,j) \neq (a,b) \rbrace
$$
Se si fa attenzione alla definizione di $E_1$ (ovvero l'insieme degli archi della componente deterministica), si può vedere che la griglia ha una sorta di *periodicità*, ovvero i nodi a un bordo della griglia sono collegati ai nodi sul bordo opposto.
In questa modo si ottiene un primo grafo con un'alta quantità di *triangoli* (prima proprietà desiderata).
![center|500](04-ar_img02.png)
Per costruire la componente aleatoria si fissa un $k>1$, e per ogni nodo $v \in V$ si aggiungono come ulteriori vicini altri $k$ nodi scelti <u>uniformemente a caso</u>.

Osservando un grafo generato in accordo a questo modello, si individua una certa dicotomia tra gli archi deterministici e quelli aleatori:
- Gli archi della griglia, cioè quelli deterministici, rappresentano le relazioni fra nodi *fisicamente* vicini: questi si frequentano di più rispetto a nodi *fisicamente* lontani. Allora gli archi della griglia rappresentano **legami forti** tra i nodi (**strong ties**). Dato che facilmente due individui che hanno un amico comune vicino si incontrano, i triangoli fra gli archi deterministici sono *sempre presenti* (o quasi sempre);
- Gli archi aleatori invece rappresentano le relazioni fra nodi *fisicamente* lontani: questi si frequentano di meno rispetto a nodi *fisicamente* vicini. Allora gli archi aleatori rappresentano **legami deboli*** tra i nodi (**weak ties**). Dato che difficilmente due individui che hanno un amico in comune lontano si incontrano, triangoli fra gli archi random sono *poco probabili*.

Dunque un grafo generato dal modello Watts-Strogatz contiene molti triangoli; ora è doveroso chiedersi se esistono numerosi percorsi brevi tra coppie di nodi.
Intuitivamente parlando, se si considerano cammini composti dai soli archi aleatori, è poco probabile incontrare due volte uno stesso nodo in brevi distanze, dato che gli archi sono distribuiti uniformemente nel grafo. Dunque, molto probabilmente, in $h$ passi è possibile raggiungere $k^h$ nodi. Successivamente *Bollobàs & Ching* diedero una dimostrazione formale a questo fenomeno. Più precisamente dimostrarono la presenza di cammini brevi in un modello con molta meno *randomness*.
Si considera un nuovo modello simile a quello Watts-Strogatz, dove però solo un nodo su $k$ ha archi random, e il numero di archi random è pari a 1.
Si partiziona poi la rete in quadrati di dimensione $k\times k$, detti "**città**", contenenti $k^2$ individui, e si considera il fenomeno small-world a livello di città. Essendo che soltanto un nodo su $k$ ha un arco random, ci si aspetta che circa $k$ individui in una città posseggano un arco random ciascuno: ogni città ha, collettivamente e mediamente, $k$ archi random verso altre città selezionate u.a.r. Tale formulazione è la stessa del modello Watts-Strogatz, con la differenza che le città svolgono il ruolo dei singoli nodi: si possono trovare cammini brevi tra ogni coppia di città.
Allora per trovare un cammino breve tra due individui, si trova prima un cammino breve tra le due città a cui questi appartengono e si usano tali archi, assieme agli archi deterministici contenuti all'interno delle città, per far si che venga ottenuto un cammino breve tra i due nodi. Questo è il punto cruciale del modello Watts-Strogatz: introdurre una piccola fonte di randomness, nella forma di weak ties, è sufficiente a rendere il mondo *piccolo*, con cammini brevi per ogni coppia di nodi.
## Navigabilità del modello Watts-Strogatz
Assodato che nel modello di Watts-Strogatz esistono numerosi percorsi brevi tra coppie di nodi, ci si chiede se questi cammini sono facilmente rintracciabili dai nodi che conoscono solo il loro vicinato e non l'intera struttura della rete.
Si consideri un nodo $u$ che vuole inviare un messaggio ad un destinatario $v$ di cui si conoscono solamente le coordinate sulla griglia geometrica. Il nodo $u$ vuole far arrivare il messaggio il più velocemente possibile, cioè vuole consegnarlo per mezzo di un cammino minimo (che probabilmente sarà breve) che lo collega a $v$. Non conoscendo la struttura della rete, $u$ non conosce un percorso che lo collega a $v$: conosce solo la posizione, che si assume essere molto distante rispetto la posizione di $u$.
Allora il nodo $u$ potrebbe mandare in *broadcast* il messaggio ai suoi vicini (che sono 8/9) e chiedere a loro di inoltrare il messaggio nuovamente ai loro vicini. Questa tecnica di spedizione del messaggio è conosciuta come **flooding** ed è il modo più veloce per raggiungere il destinatario $v$, dato che equivale ad effettuare una visita in ampiezza del grafo. Lo svantaggio di tale approccio è che vengono generati una quantità di messaggi esponenziale, determinando un sovraccarico della rete.

Si cerca dunque un modo per trovare un cammino non troppo lungo tra $u$ e $v$ in modo da trasmettere una sola copia del messaggio, e non un numero esponenziale, un po' come fecero le persone che parteciparono all'esperimento di Milgram.
Questo tipo di ricerca, detta **decentralizzata** o **miope**, in cui i nodi possono solamente inviare il messaggio da consegnare (non fare delle copie), non è applicabile nel modello di Watts-Strogatz.
![|center|500](04-ar_img03.png)
Si consideri la figura: il nodo $s$ vuole mandare un messaggio al nodo $d$. Seguendo la regola "invia il messaggio al vicino che si avvicina di più alla destinazione", il nodo $s$ invia il messaggio al nodo $a$, che poi dovrà seguire i nodi della griglia (tanti) per raggiungere $d$. Se invece $s$ avesse inoltrato al nodo $b$, allontanandosi da $d$, avrebbe raggiunto $d$ in tre passi. Purtroppo però $s$ non può in alcun modo sapere che conviene inviare il messaggio a $b$, in quanto conosce <u>solo il suo vicinato</u> e non quello di $b$.
In effetti, è stato dimostrato che nel modello di Watts-Strogatz la ricerca decentralizzata di un percorso da $s$ a $t$ non individua un percorso breve, e *mediamente individua un percorso molto più lungo di un cammino minimo*.
Questo è vero perché nel modello di Watts-Strogatz l'estremo di un arco random uscente da un nodo è scelto uniformemente a caso fra tutti i nodi: non si tiene conto di quanto sono *vicini* i nodi che congiunge l'arco random. Detto altrimenti, *gli archi random sono **troppo** casuali*.
# Un modello per la ricerca decentralizzata
Si vuole definire un modello di generazione di reti sociali che, rispetto al modello di Watts-Strogatz, consenta in più una ricerca decentralizzata di cammini che non siano troppo più lunghi dei cammini minimi.
Intuitivamente, per garantire questa proprietà gli archi random devono essere scelti in modo da tener conto di quanto sono *vicini* i nodi che tali archi congiungono.

Il nuovo modello è basato su un'ossatura deterministica, ovvero la stessa griglia bidimensionale e *periodica* del modello di Watts-Strogatz, e da una componente aleatoria, ovvero da ogni nodo esce un arco random.
Nella componente aleatoria, la probabilità che l'arco random uscente dal nodo $u$ sia $(u,v)$ è inversalmente proporzionale alla distanza <u>sulla griglia</u> dei nodi $u$ e $v$; formalmente
$$
\mathbf{Pr}((u,v) \in E) = \frac{1}{Z_{u}} \cdot \frac{1}{d(u,v)^q}
$$
dove
- $d(u,v)$ indica la lunghezza del cammino minimo fra $u$ e $v$ **nella griglia**, ossia il cammino non deve contenere archi random;
- $Z_u$ è un **fattore di normalizzazione** definito come $$Z_u = \sum_{v \in V \setminus \lbrace u \rbrace} \frac{1}{d(u,v)^q}$$ perché vale $$\sum_{v \in V \setminus \lbrace u \rbrace} \mathbf{Pr}( (u,v) \in E ) = 1$$ Poiché la griglia è *simmetrica e periodica*, il fattore di normalizzazione è uguale per tutti i nodi e verrà indicato con $Z$.
- $q$ è un parametro che prende il nome di **esponente di clustering**, che definisce la *randomness* degli archi random.

Il parametro $q$ permette di generare diversi modelli. Ad esempio, per $q=0$ gli archi sono totalmente casuali, in quanto la probabilità che tale arco esista è uguale per tutti (uniforme); per valori di $q$ molto grandi invece, si ottiene che la probabilità che esistano archi molto lunghi è piccola, perciò la componente aleatoria risulta molto simile alla componente deterministica.

Il funzionamento della ricerca decentralizzata dipende dalla scelta di $q$: per $q=0$ questa non funziona bene. In realtà è possibile dimostrare che se la componente deterministica è una griglia *simmetrica e periodica* $d$-dimensionale (nodi nello spazio $\mathbb{R}^d$) allora l'esponente di clustering ottimale è proprio $q = d$.
## Analisi intuitiva sulle prestazioni della ricerca miope nel modello
Si vede ora qualche intuizione in supporto all'affermazione

> Se la componente deterministica del grafo è una griglia bidimensionale, allora l’esponente di clustering ottimale è $q = 2$.

Si riconsideri l'esperimento di Milgram. In esso, le distanze percorse dal messaggio possono essere organizzate in *scale di risoluzione*, infatti la lettera poteva giungere:
- Ad un individuo dall'altra parte del mondo;
- Ad un individuo dall'altra parte del continente;
- Ad un individuo dall'altra parte dello stato;
- Ad un individuo dall'altra parte della città;
Dunque, anche se per pochi salti la lettera percorre relativamente poca distanza, presto farà un salto che riduce drasticamente la distanza con la destinazione.
Una maniera ragionevole per pensare a queste scale di risoluzione in un modello di rete, dalla prospettiva di un certo nodo $v$, è quella di considerare i gruppi di tutti i nodi entro una certa distanza da $v$, in maniera crescente, ossia i nodi a distanza comprese tra 2 e 4, 4 e 8 e cosi via.

Si formalizza quanto osservato considerando una semplice griglia <u>bidimensionale</u>. Fissato un nodo $u$, si partizionano i nodi rimanenti in gruppi in base alla distanza da $u$: in pa rticolare, i nodi nell'$h$-esimo gruppo si trovano a distanza compresa tra $2^h$ e $2^{h+1}$ da $u$.
Dato che i punti in una circonferenza crescono come il quadrato del raggio, avremo che i nodi nell'intervallo tra $2^h$ e $2^{h+1}$ sarà proporzionale a $2^{2h}$. Infatti
$$\vert \lbrace x \in V \vert 2^h \leqslant d(u,x) < 2^{h+1} \rbrace \vert \approx (2^{h+1})^2\pi - (2^h)^2\pi = 4\pi 2^{2h} - \pi 2^{2h} = 3\pi 2^{2h} $$
Scelto un nodo  $v$ nel blocco $2^{h+1}-2^h$, la probabilità che l'arco random uscente da $u$ sia $(u,v)$ è **proporzionale a** $2^{-2h}$, ovvero $1$ diviso la distanza tra $u$ e $v$ che è compresa tra $2^{h}$ e $2^{h+1}$, tutto elevato a $q=2$.

Si calcola ora quanto vale la probabilità che l'arco random uscente da $u$ cada nel blocco $2^{h+1}-2^h$.
Sia l'evento $\mathcal{E}_x$ che occorre se esiste l'arco random tra $u$ e il nodo $x$ a distanza $2^h \leqslant d(u,x) < 2^{h+1}$.
Sappiamo che tale evento occorre con probabilità
$$\mathbf{Pr}(\mathcal{E}_x) \propto d(u,x)^{-2} \propto 2^{-2h}$$
Sia quindi l'evento $\mathcal{E}$ che occorre quando esiste un arco random che collega $u$ all'intervallo desiderato. Tale pro b abilità sarà pari a
$$
\begin{align*}
	\mathbf{Pr}(\mathcal{E})
	&= \mathbf{Pr}(\bigcup_{x : 2^h \leqslant d(u,x) < 2^{h+1}} \mathcal{E}_x) \overbrace{=}^{\text{eventi disgiunti}} \sum_{x : 2^h \leqslant d(u,x) < 2^{h+1}} \mathbf{Pr}(\mathcal{E}_x)\\
	&\approx \mid\{\text{nodi a distanza $2^{h+1}$ da $u$}\} -  \{\text{nodi a distanza $2^{h}$ da $u$}\}\mid \cdot \mathbf{Pr}(\mathcal{E}_x)\\
	&= \alpha 2^{2h} \cdot \beta 2^{-2h}\\
	&= \alpha \cdot \beta 
\end{align*}
$$
per qualche costante $\alpha, \beta$.
Dunque la probabilità che un arco random che parta da $u$ consenta un salto di risoluzione $h$ è **indipendente** dalla risoluzione $h$, che sia $2^4$ o $2^{100}$.
Ciò implica che i weak ties sono distribuiti in maniera **uniforme** tra le risoluzioni, ammesso che $q$ sia pari al valore ottimale $d$. Intuitivamente, non occorrono molti passi sulla griglia prima di arrivare ad un nodo il cui arco random diminuisce **drasticamente** (di un ordine di grandezza) la distanza dall'obiettivo.
## Analisi formale sulle prestazioni della ricerca miope nel modello (caso $d=1$)
Si analizzano formalmente le prestazioni dell‘algoritmo di ricerca decentralizzata applicata al modello generativo introdotto nel solo caso $d =1$, ossia quando i nodi sono disposti su un anello al quale sono aggiunti gli archi random con coefficiente di clustering $q=1$.
Formalmente, si considera un grafo $G$ tale che i nodi sono disposti su un anello, ossia $V = [n]$ e l'insieme $E$ prima di inserire archi random è $\{ (i,i+1): 1 \leqslant i \leqslant n \} \cup \{ (n,1) \}$.
![|center|500](04-ar_img06.png)
All'insieme $E$ vengono aggiungi gli archi random, dove l'arco $(u,v) \in E$ con probabilità
$$
\mathbf{Pr}((u,v) \in E) = \frac{1}{Z}\cdot \frac{1}{d(u,v)}
$$
con $Z =\sum_{v \in V \setminus \{ u \}} \frac{1}{d(u,v)}$.
Si osserva che in un anello ogni nodo $u \in V$ ha due vicini a distanza due, due vicini a distanza tre, ..., due vicini a distanza $\frac{n}{2}$. Dunque per ogni $u \in V$ vale 2
$$
\begin{align}
Z &=\sum_{v \in V \setminus \{ u \}} \frac{1}{d(u,v)} = 2\sum_{h=1}^{n/2} \frac{1}{h} \\
&\leqslant 2 \sum_{h=1}^{\infty} \frac{1}{h} = 2\ln{n}
\end{align}
$$
### Lunghezza media del percorso
Scelti due nodi $s$ e $t$ in $G$, si applica l'algoritmo di ricerca decentralizzata per calcolare un percorso da $s$ a $t$. Sia $X$ la variabile aleatoria che denota la lunghezza di tale percorso. Allora si può dimostrare
$$
\mathbf{E}[X] \in O(\ln^2{n})
$$
Prima di procedere con la dimostrazione, si fanno alcune considerazioni.
Si ricorda che la **ricerca decentralizzata** consiste essenzialmente nel passare il messaggio al nodo vicino che più si avvicina al destinatario. Dunque l'algoritmo costruisce un percorso da $s$ a $t$ trasmettendo al passo uno il messaggio ad un suo vicino (più prossimo alla destinazione), che, a sua volta, al passo 2, trasmette al suo vicino più prossimo alla destinazione, e cosi via fino a quando il messaggio raggiunge $t$. Allora si suddivida in fasi il processo di trasmissione del messaggio da un nodo all'altro in base a distanze dalla sorgente $t$ che si <u>dimezzano</u>: durante la **fase** $j$ il messaggio è in possesso di un nodo $u$ tale che
$$
\frac{d(s,t)}{2^{j+1}} < d(u,t) \leqslant \frac{d(s,t)}{2^j}
$$
Poiché $d(s,t) \leqslant \frac{n}{2}$ e ad ogni fase si dimezza la distanza fra il nodo che possiede il messaggio e $t$, allora il numero di fasi è $\leqslant \log_2{n}$.
Sia $X_j$ la durata della fase $j$, ossia il numero di nodi che entrano in possesso del messaggio durante la fase $j$. Allora
$$
X = \sum_{j=1}^{\log_{2}{n}} X_{j}
$$
dunque, per la linearità del valore atteso
$$
\mathbf{E}[X] = \sum_{j=1}^{\log_{2}{n}} \mathbf{E}[X_{j}]
$$
Allora si deve dimostrare che per ogni $j$ vale
$$
\mathbf{E}[X_{j}] \in O(\ln{n})
$$
#### Dimostrazione
Si supponga che il messaggio sia in possesso del nodo $v$ durante la fase $j$: allora vale
$$
\frac{d(s,t)}{2^{j+1}} < d(v,t) \leqslant \frac{d(s,t)}{2^j}
$$
e la fase $j$ termina sicuramente se esiste un arco $(v,z)$ tale che
$$
d(z,t) \leqslant \frac{d(v,t)}{2} \leqslant \frac{1}{2}\frac{d(s,t)}{2^j} = \frac{d(s,t)}{2^{j+1}}
$$
quindi si ottiene un primo lower bound alla probabilità che la fase $j$ termini
$$
\begin{align}
\mathbf{Pr}(\text{la fase $j$ temina}) &= \mathbf{Pr}\left( \exists z \in V : (v,z)\in E \land d(z,t) \leqslant \frac{d(s,t)}{2^{j+1}} \right) \\
&\geqslant \mathbf{Pr}\left( \exists z \in V : (v,z)\in E \land d(z,t) \leqslant \frac{d(v,t)}{2} \right) \\
\end{align}
$$
Si osserva che il maggiore uguale è dato dal fatto che potrebbe essere $d(v,t)=\frac{d(s,t)}{2^{j+1}}+1$; in tal caso, esisterebbe un arco $(v,z)$ dell'anello, ossia tale che $d(z,t)=d(v,t)-1 = \frac{d(s,t)}{2^{j+1}}$, e tale arco farebbe certamente terminare la fase $j$ e tuttavia sarebbe $d(z,t)>\frac{d(v,t)}{2}$.
Sia $I$ l'insieme di tutti i nodi a distanza da $t$ non più di $\frac{d(v,t)}{2}$, ovvero
$$
I \equiv \left\{ u \in V : d(u,t) \leqslant \frac{d(v,t)}{2} \right\}
$$
Sia $A \subseteq E$ l'insieme degli archi che costituiscono l'anello e sia $R \subseteq E$ l'insieme degli archi random. Si ricorda che $A \cup R = E$ e $A \cap R = \emptyset$.
Allora
$$
\begin{align}
\mathbf{Pr}&\left( \exists z \in V : (v,z) \in E \land d(z,t) \leqslant \frac{d(v,t)}{2} \right)  \\
&= \mathbf{Pr}(\exists z \in I : (v,z)\in E) \\
&= \mathbf{Pr}( \exists z \in I : (v,z) \in A \lor (v,z) \in R) \\
&= \sum_{z \in I} \mathbf{Pr}((v,z) \in A \lor (v,z) \in R) \\
&\geqslant \sum_{z \in I} \mathbf{Pr}((v,z) \in R)
\end{align}
$$
Si stima ora la distanza $d(v,z)$ con $z \in I$.
Certamente $v$ può raggiungere $z$ tramite un cammino del tipo $v \leadsto t \leadsto z$, e questo cammino sarà lungo **almeno** $d(v,z)$
$$
d(v,z) \leqslant d(v,t) + d(t,z) = d(v,t) + d(z,t) \leqslant d(v,t) + \frac{d(v,t)}{2} = \frac{3d(v,t)}{2}
$$
allora, per ogni $z \in I$ vale
$$
\begin{align}
\mathbf{Pr}((v,z) \in R) &= \frac{1}{Z} \frac{1}{d(v,z)} \geqslant \frac{1}{Z} \frac{2}{3d(v,t)} \\
&\geqslant \frac{1}{2\ln{n}} \frac{2}{3d(v,t)} \ \ \ (Z \leqslant 2\ln n)\\
&= \frac{1}{3d(v,t)\ln{n}}
\end{align}
$$
Allora vale
$$
\begin{align}
\mathbf{Pr}\left( \exists z \in V : (v,z) \in E \land d(z,t) \leqslant \frac{d(v,t)}{2} \right) &\geqslant \sum_{z \in I} \mathbf{Pr}((v,z) \in R) \\
&\geqslant \sum_{z \in I} \frac{1}{3d(v,t)\ln{n}} \\
&= \frac{|I|}{3d(v,t)\ln{n}}
\end{align}
$$
Resta da valutare $| I |$, e infine calcolare $\mathbf{E}[X_{j}]$.
Sia
$$
|I| = \left|\left\{ u \in V : d(u,t) \leqslant \frac{d(v,t)}{2} \right\}\right|
$$
e siano $v_s,v_d$ i due nodi in $I$ a distanza massima da $t$, allora
$$
d(v_{s},t) = d(v_{d},t) = \left\lfloor\frac{d(v,t)}{2}\right\rfloor
$$
siano $v_{s_{1}},v_{d_{1}}$ i due nodi adiacenti a $t$, allora $I$ contiene il nodo $t$ e i $\left\lfloor\frac{d(v,t)}{2}\right\rfloor$ a sinistra e a destra di $v_{s_{1}},v_{d_{1}}$ rispettivamente. Allora
$$
|I| = 1 + 2\left\lfloor\frac{d(v,t)}{2}\right\rfloor \geqslant 1 + 2\frac{d(v,t)-1}{2} = d(v,t)
$$
![|center](04-ar_img04.png)
Dunque
$$
\mathbf{Pr}(\text{la fase $j$ temina}) \geqslant \frac{d(v,t)}{3d(v,t)\ln{n}} 
$$
da cui si conclude che
$$
\mathbf{Pr}(\text{la fase $j$ NON temina}) \leqslant 1-\frac{1}{3\ln{n}} 
$$
Si calcola ora $E[X_j]$.
La probabilità che la fase $j$ non termina se siamo nel nodo $v$ è pari a $1 -  \frac{1}{3\ln{n}}$. Allora, la probabilità che la fase $j$ non termina entro $h$ passi è pari a
$$
\mathbf{Pr}(X_{j}\geqslant h) \leqslant \left[ 1 - \frac{1}{3\ln{n}} \right]^h
$$
Per definizione di valore atteso, ricordando che $d(s,t) \leqslant \frac{n}{2}$, vale
$$
\begin{align}
\mathbf{E}[X_{j}] &= \sum_{h=1}^{\frac{n}{2}} \mathbf{Pr}(X_{j} \geqslant h) \\
&\leqslant \sum_{h=1}^{\frac{n}{2}} \left[ 1 - \frac{1}{3\ln{n}} \right]^h \\
&\leqslant \sum_{h \geqslant 0} \left[ 1 - \frac{1}{3\ln{n}} \right]^h = \frac{1}{1-\left[ 1 - \frac{1}{3\ln{n}} \right]} = 3\ln{n}
\end{align}
$$
Dunque per ogni $j$, vale $\mathbf{\mathbf{E}}[X_j] \in O(\ln{n})$. Si conclude ora la dimostrazione per $\mathbf{\mathbf{E}}[X]$:
$$
\begin{align} \mathbf{E}[X] &= \sum_{j=1}^{\log_{2}{n}} \mathbf{E}[X_{j}] = \sum_{j=1}^{\log_{2}{n}} 3\ln{n} = \log_{2}{n} \cdot 3\ln{n} \\
&= \log_{2}{e}\cdot \ln{n}\cdot 3\ln{n} \in O(\ln^2{n})
\end{align}
$$
## Perché $q=d$ funziona bene in $\mathbb{R}^d$
Gli elementi che permettono di dimostrare che la ricerca decentralizzata si comporta bene nell'anello sono:
1. Fissato $d$, il numero di nodi a distanza al più $d$ dalla destinazione $t$ è, approssimativamente, pari a $d$;
2. Il fattore di normalizzazione è $Z\leqslant 2\ln n$
Ciò permette di mostrare che, sia $v$ il nodo che detiene il messaggio tale che $d(v,t) = d$, la probabilità che $v$ abbia un vicino a distanza minore o uguale a $\frac{d}{2}$ da $t$ è proporzionale a $\frac{1}{\ln n}$ indipendentemente da $d.$ Infatti, sia $I =\left\{  u \in V: d(u,t) \leqslant \frac{d(v,t)}{2}  \right\}$ con $|I|=d(v,t)$, si è mostrato che
$$
\mathbf{Pr}\left( \exists z \in V: (v,z)\in E \land d(z,t) \leqslant \frac{d(v,t)}{2} \right) \geqslant \frac{1}{3\ln n}
$$
Si può estendere tale ragionamento per $q=d=2$. In tal caso:
1. $|I|=\alpha d(v,t)^2$ per $\alpha$ costante (area di un quadrato di lato circa $d(v,t)$);
2. $Z = \sum_{v\in V \setminus \{ u \}} \frac{1}{d(u,v)^2}$ e si dimostra che $Z\leqslant \beta \ln n$, per qualche costante $\beta$;
ciò implica che
$$
\begin{align}
\mathbf{Pr}\left( \exists z \in V: (v,z) \in E \land d(z,t) \leqslant \frac{d(v,t)}{2} \right) &\geqslant \sum_{z\in I} \frac{1}{Zd(v,z)^2} \\
&\geqslant \frac{4|I|}{Z9d(v,t)^2} \\
&\geqslant \frac{4\alpha}{9\beta \ln n}
\end{align}
$$
Allora, anche nel caso $q=d=2$, la probabilità che $v$ abbia un vicino a distanza minore uguale a $\frac{d}{2}$ da $t$ è proporzionale a $\frac{1}{\ln n}$ indipendentemente da $d$.
Allo stesso modo, si possono fare considerazioni analoghe per $d>2$.
## Ricerca miope con $q \neq d$
Sia $d=1$ e $q=0$.
La probabilità che l'arco random uscente dal nodo $u$ sia $(u,v)$ è
$$
\mathbf{Pr}((u,v) \in E) = \frac{1}{Z} \frac{1}{d(u,v)^0} = \frac{1}{Z} = \frac{1}{n-1}
$$
con $Z = n-1$ poiché deve essere
$$
\sum_{v \in V \setminus \{ u \}} \mathbf{Pr}((u,v) \in E) = \sum_{v \in V \setminus \{ u \}} \frac{1}{Z} = 1
$$
Nel caso $q=d=1$ è "facile" entrare in regioni del grafo contenenti nodi sempre più vicini a $t$ (gli insiemi $I$): più precisamente, la probabilità di *dimezzare* la distanza del possessore del messaggio da $t$ è la stessa indipendentemente da dove tale nodo con il messaggio si trova
Si mostra ora, informalmente, che nel caso $q=0,d=1$ è *difficile* entrare in regioni di grafo vicine a $t$, in particolare entrare nell'insieme
$$
R=\{ u \in [n]: d(u,t) \leqslant \sqrt{ n } \}
$$
Sia $t$ un nodo e sia $s$ un nodo non nell'insieme $R$. Allora per ogni $u \in R$ e per ogni $v \in [n]-R$, vale
$$
\mathbf{Pr}((v,u) \in E) > \frac{1}{n}
$$
$>$ perché $\frac{1}{n-1} > \frac{1}{n}$ e perché $\frac{1}{n-1}$ è la probabilità di esistenza di un arco random, ma $u$ e $v$ potrebbero anche essere vicini lungo l'anello.
Allora, per ogni $u \not\in R$ vale
$$
\mathbf{Pr}(\exists u \in R: (u,v) \in E) > \frac{\mid R \mid}{n} = \frac{2\sqrt{ n }}{n} = \frac{2}{\sqrt{ n }}
$$
allora, detta $Y$ la variabile aleatoria che rappresenta il numero di passi per raggiungere da $s$ un nodo in $R$, vale
$$
\mathbf{Pr}(Y \geqslant h) < \left( 1 - \frac{2}{\sqrt{ n }} \right)^h
$$
e dunque
$$
\mathbf{E}[Y] = \sum_{h \geqslant 0} \mathbf{Pr}(Y \geqslant h) \leqslant \sum_{h \geqslant 0}\left( 1 - \frac{2}{\sqrt{ n }} \right)^h = \frac{1}{1-\left( 1 - \frac{2}{\sqrt{ n }} \right)} = \frac{\sqrt{ n }}{2}
$$
che è **esponenzialmente** più grande di $E[X] \in O(\ln^2{n})$ nel caso $q=1$.
Dunque entrare nella regione $R$ richiede mediamente $\frac{\sqrt{ n }}{2}$ passi; analogamente, si può calcolare in quanti passi mediamente si entra nella regione di nodi
$$
R_{2}=\left\{  u \in [n]: d(u,t) \leqslant \frac{\sqrt{ n }}{2}  \right\}
$$
Per ogni $v \in R-R_2$, vale
$$
\mathbf{Pr}(\exists u \in R_{2}: (v,u) \in E) > \frac{\mid R_{2} \mid}{n} = \frac{\sqrt{ n }}{n} = \frac{1}{\sqrt{ n }}
$$
allora, detta $Y_{2}$ la variabile aleatoria che rappresenta il numero di passi per raggiungere da $s$ un nodo in $R_{2}$, vale
$$
\mathbf{Pr}(Y_{2} \geqslant h) < \left( 1 - \frac{1}{\sqrt{ n }} \right)^h
$$
e dunque
$$
\mathbf{E}[Y] = \sum_{h \geqslant 0} \mathbf{Pr}(Y_{2} \geqslant h) \leqslant \sum_{h \geqslant 0}\left( 1 - \frac{1}{\sqrt{ n }} \right)^h = \frac{1}{1-\left( 1 - \frac{1}{\sqrt{ n }} \right)} = \sqrt{ n }
$$
Allora una volta entrati nella regione $R$, entrare in $R_2$ utilizzando gli archi random richiede un numero di passi mediamente equivalente a muoversi lungo gli archi dell'anello: una volta raggiunta una distanza da $t$ dell'ordine di $\sqrt{ n }$, gli archi random non sembrano giocare più alcun ruolo, perché essi sono ***troppo random***. Questo fatto, può essere generalizzato per tutti i casi $0 < q < 1$.

Nel caso $q>1$ invece, gli archi random sono troppo corti, cioè è *difficile* usare archi random che coprano grandi distanze. Di conseguenza, la ricerca miope in questo caso riesce a fare poco meglio rispetto ad utilizzare solo archi dell'anello per raggiungere $t$.

In conclusione, si osserva che in effetti è possibile dimostrare il seguente teorema
```ad-Teorema
title: Teorema
Sia $X$ la variabile aleatoria che indica la lunghezza del cammino trovato dalla ricerca miope su un anello di $n$ nodi.
Comunque si sceglie un $q \neq 1$ esistono sempre due costanti $\alpha_q$ e $c_q$ tali che
$$\mathbf{E}\left[ X \right] \geqslant a_q \cdot n^{c_q}$$
```
