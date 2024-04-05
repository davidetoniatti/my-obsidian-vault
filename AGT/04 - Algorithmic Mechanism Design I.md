Il **Mechanism Design**, o *progettazione di Meccanismi* è una sottoarea della Teoria dei giochi il cui tipo di studio ha un aspetto più algoritmico e progettuale rispetto a scenari non cooperativi.
Informalmente, il mechanism design si occupa di definire un gioco in maniera tale che l'outcome desiderato venga necessariamente raggiunto, e che quindi il relativo profilo di strategia risulti essere un equilibrio.

Bisogna far presente che i giochi indotti da meccanismi presentano delle differenze rispetto ai giochi in forma standard:
- Ciascun giocatore possiede delle informazioni, ossia dei valori, i quali risultano essere *privati*, ossia tali per cui solo il giocatore in questione risulti esserne in possesso, ed *indipendenti* da quelli degli altri partecipanti. Tali valori prendono il nome di *tipi,* e si evidenzia come, essendo questi privati, possano venir utilizzati per manipolare il sistema da progettare.
- La matrice dei payoff è una funzione di questi tipi.

In generale, è possibile interpretare gli obiettivi dei meccanismi progettati nei termini astratti di una *scelta sociale*. Una scelta sociale è semplicemente un'aggregazione di preferenze dei diversi partecipanti verso una singola decisione collettiva. Il mechanism design si occupa quindi di implementare le scelte sociali desiderate in uno scenario strategico, assumendo che i diversi agenti coinvolti agiscano in maniera egoistica e razionale.

L'idea del Mechanism design è quella di ottenere delle situazioni ottimali per un dato scenario,  incentivando gli agenti a comportarsi *bene*, in modo socialmente utile. 
Dunque alla base del Mechanism Design vi è la progettazione di un sistema che, caratterizzato da un ambiente popolato da agenti egoistici, deve raggiungere un certo obiettivo. A tale scopo, il sistema incentiva gli agenti a cooperare col sistema tramite un sistema di pagamento che premia gli agenti che collaborano al suo raggiungimento: deve generare una situazione di equilibrio, per il quale agli agenti non conviene fare una scelta diversa rispetto a quella *giusta* per il sistema.

Un aspetto fondamentale nella progettazione di meccanismi è che si applica in situazioni in cui gli agenti hanno determinate preferenze sull'outcome del gioco: si vuole, date queste preferenze, decidere un esito che soddisfi tutti gli agenti. Il problema in queste aggregazioni è, come detto in precedenza, che le informazioni effettivamente dichiarate dagli agenti al sistema possono non rispettare le loro preferenze reali, cioè i giocatori possono mentire al sistema per manipolarlo ed ottenere dei vantaggi. Dunque, idealmente, il sistema deve incentivare i giocatori a esprimere la preferenza reale, cioè a dire la *verità*.
Per esempio, in un sistema di voto che ha l'obiettivo di aggregare le reali preferenza dei giocatori per restituire un risultato che sia concorde all'opinione dei votanti, potrebbero esserci dei soggetti che votano un altro partito rispetto alla loro preferenza (ad esempio perché ha più probabilità di vincere, o perché ne fa parte qualche parente, o perché è stato corrotto). Questi votanti "falsi" alterano il risultato restituito dal sistema, che non rappresenterà le reali preferenze dei cittadini.
# Il problema dell'asta: single item auction
Si considerino $n$ agenti, o compratori, ciascuno dei quali partecipa ad un asta per la vendita di un singolo oggetto, ad esempio un quadro.
Ogni agente $a_i$ ha in testa un valore $t_i$, che rappresenta il massimo valore che $a_i$ è disposto a spendere per aggiudicarsi il quadro; si osserva che per ogni $a_i$ il valore $t_i$ è *privato*, cioè non è conosciuto da nessun'altra entità presente all'asta.
Al fine di ottenere il quadro, ciascun agente $a_i$ dichiara in modo *indipendente dagli altri* un'offerta $r_i$ per il quadro. Si osserva che potenzialmente $r_i \neq t_i$, ossia i compratori possono dichiarare un valore diverso da quello privato.
L'agente $a_i$ che vince il quadro dovrà pagare $p$ per ottenere un utile di $u_i = t_i - p$; dunque i compratori puntano a vincere il quadro massimizzando il proprio utile.
L'obiettivo del venditore del quadro è quello di definire un *meccanismo d'asta* che venda il quadro al compratore con il valore privato $t_i$ più alto.
Il meccanismo non conosce i valori privati e gli agenti potrebbero dichiarare dei valori inferiori ai valori privati, dato che essi vogliono massimizzare il profitto (da qui la natura *egoistica* del gioco), dunque il meccanismo deve essere in grado di far dichiarare ad ogni agente il valore reale.
In particolare, il meccanismo d'asta deve:
- Decidere l'agente che vince l'asta in funzione dei valori dichiarati $r_1,\dots,r_n$;
- Decidere quanto dovrà pagare il vincitore per prendersi il quadro.
Inoltre, il meccanismo d'asta è pubblico, dunque tutti gli agenti che partecipano sanno come viene scelto il vincitore e quanto dovrà pagare nel caso di vittoria.
Dunque il meccanismo dovrà regolare queste due decisioni in modo che per ogni agente è conveniente dichiarare come offerta il valore reale che attribuisce al quadro. In questo modo il meccanismo raggiunge il suo scopo, ossia assegnare il quadro all'agente con il valore privato più alto.
Si vedono tre possibili meccanismi d'asta.
## No payment
In questo meccanismo il quadro viene attribuito all'agente $a_i$ che ha dichiarato l'offerta più alta $r_i = \max_{j=1,\dots,k}\{ r_{j} \}$ e il vincitore non paga niente per il quadro.
Si nota facilmente che tale meccanismo non incentiva gli agenti a dire la verità. Infatti, a tutti gli agenti conviene offrire $r_i = +\infty$ per massimizzare il guadagno.
Allora il sistema non è in grado di assegnare il quadro all'agente con valore privato più alto.
## Pay your bid
In questo meccanismo, come nel precedente, il quadro viene attribuito all'agente $a_i$ che ha dichiarato l'offerta più alta $r_i = \max_{j=1,\dots,k}\{ r_{j} \}$. Questa volta, il vincitore paga il quadro tanto quanto il valore da lui dichiarato $r_i$.
Si nota che anche tale meccanismo non incentiva gli agenti a dire la verità. Infatti ogni agente dichiara $r_i < t_i$ in modo da avere utilità positiva in caso di vittoria del quadro. Dunque non sempre il quadro viene assegnato al giocatore il valore privato $t_i$ più alto.
## Vickrey's Second Price Auction
In questo meccanismo il quadro viene sempre attribuito all'agente $a_i$ che ha dichiarato l'offerta più alta $r_i = \max_{j=1,\dots,k}\{ r_{j} \}$. Il vincitore del quadro paga il quadro tanto quanto il secondo valore dichiarato più alto $R = \max_{j \neq i} \{r_j\}$, cioè quanto la seconda miglior offerta.
Questo meccanismo funziona, cioè incentiva gli agenti a dire la verità. Infatti vale il seguente teorema.
```ad-Teorema
title: Teorema
Nell'asta di Vickrey, $r_i = t_i$ è una strategia dominante  per ogni giocatore $i$.
```
Dunque nell'asta di Vickrey ogni giocatore massimizza la sua utilità quando dichiara la verità, indipendentemente dal tipo riportato dagli altri giocatori.
### Dimostrazione
Siano $a_{i}$, $t_i$ e il vettore di strategie degli altri giocatori $r_{-i}$ fissati.
Sia $R=\max_{j\neq i} \{r_j\}$, cioè $R$ è la miglior offerta escludendo quella del giocatore $i$. Si osserva che $R$ non è noto all'agente $a_{i}$, dato che le offerte sono fatte a busta chiusa.
#### Caso $t_i \geqslant R$ 
In questo caso, $a_{i}$ è l'agente che da valore più alto al quadro.
- Dichiarando $r_i = t_i$, l'agente $a_i$ riceve un utilità pari a $u_i = t_i - R \geq 0$; infatti l'agente vince se $t_i > R$, mentre se $t_i = R$ l'agente può sia vincere che perdere (in base alle regole per lo spareggio), ma l'utilità è sempre zero.
- Dichiarando un qualsiasi $r_i > R$ con $r_i \neq t_i$, l'agente $a_{i}$ vince e continua ad avere utilità $u_i = t_i - R \geq 0$; non si ottiene nessun vantaggio a dichiarare più di $t_i$.
- Dichiarando un qualsiasi $r_i < R$, l'agente $a_{i}$ non vince e riceve utilità pari a zero $u_i = 0$; questo outcome è peggiore per l'agente $a_{i}$.
Non è conveniente per l'agente $a_{i}$ dichiarare un valore diverso da $t_i$.
#### Caso $t_i < R$ 
In questo caso, esiste un'agente $a_{j}$ che da un valore più alto del quadro rispetto all'agente $a_{i}$.
- Dichiarando $r_i = t_i$, l'agente $a_{i}$ non vince e riceve un utilità pari a $u_i = 0$; infatti vince l'agente che ha tipo riportato pari a $R$;
- Dichiarando un qualsiasi $r_i < R$ con $r_i \neq t_i$, l'agente $a_{i}$ non vince e continua ad avere utilità pari a zero $u_i = 0$; non si ottiene nessun vantaggio a riportare meno di $t_i$.
- Dichiarando un qualsiasi $r_i > R$, l'agente $a_{i}$ vince ma ottiene utilità  $u_i = t_{i}-R < 0$;  dunque l'outcome è peggiore per l'agente $a_{i}$.
Non è conveniente per l'agente $a_{i}$ dichiarare un valore diverso da $t_i$.

In tutti i casi, riportare un tipo *falso* (cioè un tipo riportato diverso dal tipo privato) non produce una utilità migliore, dunque riportare $r_i = t_i$ , o dichiarare la verità, è una *strategia dominante*.
# Ingredienti per definire un problema di mechanism design
Volendo generalizzare il problema specifico appena affrontato, l'idea dietro alla **algorithmic mechanism design** è quella di incentivare gli agenti a comportarsi in modo tale che le scelte individuali ed egoistiche degli agenti riescono ad ottenere dei buoni livelli di welfare.

Un modo per pensare ad un problema di mechanism design è il seguente: il sistema deve risolvere un problema di ottimizzazione, ma non è a conoscenza di alcune parti dell'istanza in input. Queste informazioni sconosciute sono mantenute da agenti razionali ed egoistici. Tali agenti dovranno dichiarare le informazioni mancanti al sistema e riceveranno un pagamento in base all'informazione rivelata al sistema. In particolare, gli agenti possono **mentire** al sistema per ottenere un maggiore guadagno.
Allora il sistema deve avere un meccanismo composto da:
- un'algoritmo che calcola una soluzione ottima (o buona);
- uno schema di pagamento che ricompensa gli agenti in modo da far dichiarare le parti di input *reali*, cioè in grado di far rivelare la verità da ogni agente.
Grazie a questo meccanismo, il sistema può calcolare la soluzione ottima rispetto all'istanza reale in input.

Gli ingredienti da considerare nell'ambito del mechanism design sono i seguenti.
- $N$ **agenti** o giocatori: ogni agente ha una sola informazione privata $t_i \in T_i$ detta **tipo** (type);
- Un insieme di outcome ammissibili $F$: il sistema dovrà decidere uno degli outcome in $F$. Nell'asta di Vickery, l'insieme $F$ è dato da tutti gli agenti che partecipano all'asta: un'outcome è un giocatore che vince l'asta.
- Per ogni vettore dei tipi $t=(t_1,\dots,t_N)$, una *funzione di scelta sociale* $f(t) \in F$ specifica l'outcome che si vuole ottenere dato $t$; in altre parole, se il sistema conoscesse il vettore $t$ dei tipi di ogni giocatore, allora $f(t)$ è l'outcome da implementare. Inoltre il sistema, vorrebbe calcolare esattamente un outcome di $F$ che dipende dai tipi reali, dunque deve implementare la funzione di scelta sociale $f(t) \in F$. Nell'asta di Vickery, la funzione di scelta sociale è l'assegnazione dell'oggetto all'agente con il **costo reale** più basso $$f(t) = \arg\min_{i}(t_{1},\dots,t_{n})$$
- Ogni agente ha uno *spazio di strategie* $S_i$. Questa trattazione si restringe ai meccanismi detti *direct revelation mechanism*, in cui l'azione che un agente può eseguire è quella di dichiarare un tipo $r_i \in T_i = S_i$. Si osserva che il tipo riportato può essere diverso dal tipo reale di un'agente, ovvero $r_i \neq t_i$.
- Per ogni possibile outcome $x \in \mathcal{F}$, ciascun agente effettua una **valutazione** $v_i(t_i,x)$ rispetto alla sua preferenza per quel particolare outcome.
- Per ogni possibile vettore $r$ dei tipi riportati, ciascun agente riceve un **pagamento** $p_i(r)$. I pagamento vengono utilizzati dal meccanismo per incentivare gli agenti a collaborare.
- L'**utilità**  dell'agente $a_i$ se l'outcome per $r$ è $x(r)$ è pari a $$u_{i}(t_i,x(r)) = p_{i}(r) - v_{i}(t_{i},x(r))$$
A questo punto, l'obiettivo del mechanism design è quello di implementare un **meccanismo** $M = \langle g(r),p(r) \rangle$, dove
- $g(r)$ è un **algoritmo** che calcola l'outcome del gioco $x = g(r)$ in funzione dei tipi riportati dagli agenti $r = (r_1,\dots,r_n)$;
- $p(r)$ è uno **schema di pagamento** che specifica il pagamento di ogni agente rispetto ai tipi riportati dagli agenti $r = (r_1,\dots,r_n)$.
Il meccanismo deve essere implementato in modo tale da garantire che nelle situazioni di equilibrio rispetto alle utilità dei vari agenti si ottiene
$$ x = g(r) = f(t) $$
Se questo è vero in equilibrio di strategie dominanti, allora ogni agente $a_i$ riporterà il tipo $r_i$ che permette di calcolare l'outcome $x = f(t)$.
Il gioco indotto da un problema di mechanism design è il gioco in cui gli $n$ agenti sono i giocatori e la matrice dei payoff è implicitamente definita nelle funzioni di utilità.
## Meccanismi truthful
Si può formalizzare la proprietà principale dell'asta di Vickrey, cioè che a tutti gli agenti conviene dire la verità, con le seguenti definizioni.
```ad-Definizione
title: Definizione
Un meccanismo $M = \langle g(r),p(r) \rangle$ è un'**implementazione con strategie dominanti** se esiste un vettore di tipi riportati $r^* = (r_1^*,\dots,r_n^*)$ tale che $f(t) = g(r^*)$ in equilibrio di strategie dominanti, cioè per ogni agente $a_i$  e per ogni vettore dei tipi riportati $r = (r_1,\dots,r_n)$ vale
$$ u_{i}(t_{i},g(r_{-i},r_{i}^*)) \geqslant u_{i}(t_{i},g(r_{-i},r_{i})) $$
Inoltre, se dire la verità è la strategia dominante, cioè se $r^* = t$, allora il meccanismo è detto **truthful**, o **strategy-proof** o **incentive compatible**.
```
In questo caso, gli agenti riportano i reali tipi privati e non manipolano il valore dichiarato per fini strategici. Dunque l'algoritmo $g(\cdot)$ del meccanismo viene eseguito sulla reale istanza in input.

A questo punto, la questione principale da affrontare è come progettare dei meccanismi truthful, in altre parole:
1. come progettare l'algoritmo $g(r)$;
2. come definire uno schema di pagamenti
in modo che la social choice function sottostante sia implementata in maniera truthful?
# Come progettare meccanismi truthful?
Come già descritto nelle righe precedenti, i problemi di mechanism design possono essere pensati come problemi di **ottimizzazione** in cui parte dell'istanza è nascosta dagli agenti. L'obiettivo del meccanismo è quello di incentivare gli agenti a rivelare la porzione di input **reale** che tengono nascosta.
## Problemi di minimizzazione
I risultati riportati nel corso della trattazione saranno per problemi di *minimizzazione* (si possono definire anche per problemi di massimizzazione in modo analogo). Dunque si considera la seguente impostazione.
- per ogni outcome, o soluzione, $x \in \mathcal{F}$, la **funzione di valutazione** $v_i(t_i,x)$ rappresenta il **costo** pagato dall'agente $a_i$ nel particolare outcome $x$;
- la **funzione di scelta sociale** $f(t)$ associa al vettore dei tipi $t$ viene mappata ad una soluzione $x$ che **minimizza** una qualche misura su $x$;
- I **pagamenti** vanno sempre dal meccanismo verso gli agenti. Gli agenti negli outcome sostenere un costo, e vengono pagati per sostenerlo. L'utile è dato per ogni agente è dato dal pagamento meno il costo.
### Vickrey's Second Price Auction: versione di minimizzazione
Il meccanismo d'asta del secondo prezzo di Vickrey nel contesto della vendita del quadro ha lo scopo di assegnare l'opera all'agente che massimizza la sua valutazione reale.
Si può definire una versione di minimizzazione di tale meccanismo, in cui l'obiettivo è assegnare una risorsa all'agente che incorre nel minor costo. In questa versione del problema si ha che:
- Il tipo $t_i$ rappresenta il costo che l'agente $a_i$ incorre se dovesse vincere l'asta;
- Il valore $p_i$ rappresenta il pagamento che il sistema offre all'agente che vince l'asta;
- L'utilità dell'agente $a_i$ è data da $u_i = p_i - t_i$: più alto è il pagamento rispetto al costo del lavoro, e più all'agente $a_i$ conviene vincere l'asta.
In questa versione il lavoro (l'oggetto dell'asta), va all'agente che richiede meno costo, che verrà pagato una quantità pari al costo della seconda del secondo agente più economico.
## Problemi utilitari
Un problema è **utilitario** se l'outcome desiderato è quello che minimizza la somma delle valutazioni degli agenti. Formalmente, un problema è utilitario se la funzione obiettivo (o funzione di scelta sociale) ha la forma
$$ f(t) := \arg\min_{x \in \mathcal{F}} \sum_{i} v_{i}(t_{i},x) $$
Per questa classe di problemi, esiste una classe di meccanismi truthful: il **meccanismo VCG**.
# Il meccanismo VCG
Il meccanismo **VCG** (Vickrey, Clarke, Groves) è l'unico meccanismo **truthful** per i problemi utilitari, ed è descritto come segue:
- L'algoritmo $g(r)$ calcola l'outcome $x$ che minimizza la somma delle valutazioni rispetto ai tipi riportati dagli agenti $$x = g(r) = \arg\min_{y \in \mathcal{F}} \sum_{i} v_{i}(r_{i},y)$$
- Il pagamento per l'agente $i$ è pari a $$p_{i}(r) = h_{i}(r_{-i}) - \sum_{j\neq i} v_{j}(r_{j},g(r))$$
dove $h_i(r_{-i})$ è una funzione arbitraria qualsiasi che dipende dai valori riportati da tutti gli agenti escluso $a_i$.

Dunque si può dimostrare il seguente teorema.
## Truthfulness VCG
I meccanismi VCG sono truthful per problemi utilitari.
### Dimostrazione
Fissato il tipo $t_i$ di un'agente $a_i$ e fissato il vettore dei tipi riportati dagli altri agenti $r_{-i}$, si applica l'algoritmo $g(\cdot)$ per calcolare due outcomes: uno in cui $a_i$ dichiara $r_i = t_i$, cioè dice la verità, e uno in cui $a_i$ dichiara $r_i \neq t_i$, cioè non dice la verità.
- Quando $r_i = t_i$ ($a_i$ **dice la verità**), vale $$ x = g(t_{i},r_{-i}) = g(\hat{r})$$ In questo caso, l'utilità per $a_i$ è pari a $$ \begin{align}
u_{i}(t_{i},x) &= p_{i}(t_{i},r_{-i}) - v_{i}(t_{i},x)\\
&= \left(h_{i}(r_{-i}) - \sum_{j\neq i} v_{j}(r_{j},x) \right) - v_{i}(t_{i},x) \\
&= h_{i}(r_{-i}) - \sum_{j} v_{j}(\hat{r}_{j},x)
\end{align}  $$
- Quando $r_i \neq t_i$ ($a_i$ **mente**), vale $$ x' = g(r_{i},r_{-i}) $$ In questo caso, l'utilità per $a_i$ è pari a $$ \begin{align}
u_{i}(t_{i},x') &= p_{i}(r_{i},r_{-i}) - v_{i}(t_{i},x')\\
&= \left(h_{i}(r_{-i}) - \sum_{j\neq i} v_{j}(r_{j},x') \right) - v_{i}(t_{i},x') \\
&= h_{i}(r_{-i}) - \sum_{j} v_{j}(\hat{r}_{j},x')
\end{align}  $$
A questo punto si osserva che, per come è definito, l'algoritmo $g(\cdot)$ calcola l'outcome $x$ che calcola la soluzione ottima rispetto al vettore dei tipi riportati $\hat{r}=(r_{-i},t_{i})$. Dunque $x$ è l'outcome per cui la quantità  $\sum_{i}v_{i}(\hat{r},x)$ è minima.
Allora vale che
$$\sum_{j}v_{j}(\hat{r}_{j},x) \leqslant \sum_{j}v_{j}(\hat{r}_{j},x')$$
ma dato che
$$
\begin{align}
\sum_{j}v_{j}(\hat{r}_{j},x) &= h_{i}(r_{-i}) - u_{i}(t_{i},x) \\
\sum_{j}v_{j}(\hat{r}_{j},x') &= h_{i}(r_{-i}) - u_{i}(t_{i},x')
\end{align}
$$
si ottiene
$$
h_{i}(r_{-i}) - u_{i}(t_{i},x) \leqslant h_{i}(r_{-i}) - u_{i}(t_{i},x') \implies
u_{i}(t_{i},x) \geqslant u_{i}(t_{i},x')
$$
cioè l'utilità dell'agente $a_i$ è migliore quando dichiara la verità $t_i$.
## Come definire $h_i(r_{-i})$
Si osserva che se tutti gli agenti sono *costretti* a partecipare ad un gioco, allora la funzione $h_i(r_{-i})$ può essere definita in modo arbitrario. In generale però questa non è una buona assunzione, dunque è necessario definire una funzione $h_i(r_{-i})$ che sia *ragionevole*: non tutte le funzioni hanno senso.

Ad esempio, si consideri l'asta di Vickrey in cui $h_i(r_{-i})=0$ per ogni $i$.
Sia $a_i$ l'agente che ha vinto l'asta, allora il suo utile vale
$$
\begin{align}
u_{i}(t_{i},x) &= p_{i}(r)-v_{i}(t_{i},x) \\
&= h_{i}(r_{-i}) - \sum_{j \neq i} v_{j}(r_{j},x) -v_{i}(t_{i},x)\\
&= \underbrace{-\sum_{j \neq i} v_{j}(r_{j},x)}_{0 \text{ perché $a_{i}$ ha vinto}} -v_{i}(t_{i},x) \\
&= - v_{i}(t_{i},x) = - t_{i}
\end{align}
$$
dato che le valutazioni degli agenti sono quantità maggiori o uguali a zero, allora $a_i$, nonostante vince l'asta, ha utilità negativa dato che non viene pagato dal meccanismo.
Similmente, sia $a_i$ un'agente che non ha vinto l'asta e sia $a_h$ il vincitore.
L'utile di $a_i$ è pari a
$$
\begin{align}
u_{i}(t_{i},x) &= p_{i}(r)-v_{i}(t_{i},x) \\
&= h_{i}(r_{-i}) - \sum_{j \neq i} v_{j}(r_{j},x) -v_{i}(t_{i},x)\\
&= -\sum_{j \neq i} v_{j}(r_{j},x) - \underbrace{v_{i}(t_{i},x)}_{0 \text{ perché $a_{i}$ non ha vinto}} \\
&= -\sum_{j \neq i} v_{j}(r_{j},x) \\
&= -v_{h}(r_{h},x)
\end{align}
$$
dato che le valutazioni degli agenti sono quantità maggiori o uguali a zero, allora $a_i$, nonostante perde l'asta e dunque non deve niente al meccanismo, ha utilità negativa perché la funzione di pagamento del meccanismo *vuole* i soldi dagli agenti perdenti!
```ad-tip
Il meccanismo è **truthful** anche con $h_i(r_{-i})=0$.
```
#### Pagamento di clarke
Uno schema di pagamento coerente per l'asta di Vickrey deve garantire una utilità maggiore di zero per il vincitore e uguale a zero per tutti gli altri agenti partecipanti. Per ottenere tali garanzie, si utilizza un particolare schema di pagamento detto **clarke payment**, dove
$$ h_{i}(r_{-i}) = \sum_{j \neq i}v_{j}(r_{j},g(r_{-i})) $$
dove $g(r_{-i})$ è un outcome in cui l'agente $a_i$ non partecipa. Allora il pagamento per ogni agente è pari a
$$ p_{i}(r) = \underbrace{\sum_{j \neq i}v_{j}(r_{j}g(r_{-i}))}_{{(1)}} - \underbrace{\sum_{j \neq i}v_{j}(r_{j},g(r))}_{(2)} $$
si può dimostrare che con questo schema di pagamento, tutte le utilità degli agenti sono non negative. Dunque, tutti gli agenti sono interessati a partecipare all'asta.

In particolare, usando questo schema di pagamento nell'asta di Vickrey, il vincitore $a_i$ viene pagato di una quantità pari alla seconda migliore offerta, dato che:
- $(1)$ è proprio il valore della seconda migliore offerta;
-  $(2)$ è pari a zero perché ogni agente $a_j$ con $j \neq i$ perde l'asta, quindi ha valutazione zero.
mentre ogni agente $a_j$ con $j \neq i$ viene pagato zero, dato che:
- $(1)$ è il valore della migliore offerta, cioè del vincitore $a_i$;
- $(2)$ è sempre il valore della migliore offerta.
