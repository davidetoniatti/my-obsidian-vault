Nella lezione precedente, è stato introdotto un modello di grafi aleatori detto modello di [Erdős-Rényi](01%20-%20Introduzione%20e%20grafi%20di%20Erdős-Rényi.md#Il%20modello%20di%20Erdős-Rényi) . In un grafo generato con tale modello, al crescere di $k$, si è visto che il numero di nodi che hanno grado $k$ decresce esponenzialmente in $k$.
Tale decrescita è data dal fatto che nel grafo $G_{n,p}$ gli archi vengono aggiunti in sequenza, indipendentemente gli uni dagli altri. Dunque, il grado di un nodo è dato dalla somma degli eventi indipendenti
$$
\delta_{i} = \sum_{j \in [n]-i} e_{ij}
$$
dove $e_{ij}$ è la variabile aleatoria tale che
$$
e_{ij} = \begin{cases}
1, \quad &\text{se esiste l'arco }(i,j) \\
0, \quad &\text{altrimenti}
\end{cases}
$$
Per il **Teorema del limite centrale**, vale che la somma di *molti* valori aleatori indipendenti si distribuisce approssimativamente come una distribuzione normale (o gaussiana) intorno al suo valore atteso, dunque decresce molto velocemente quando ci si allontana dal valore atteso.

Il punto cruciale sta nel fatto che la porzione di grafo che è già stata costruita non ha alcuna influenza sul prossimo arco che verrà creato.

Questo risultato però non rispecchia pienamente la realtà; infatti, sono stati osservati fenomeni *sostanzialmente* differenti.

Si consideri il grafo dato dalle informazioni nel *web*: si modella come un *grafo diretto* in cui ogni nodo è una pagina e un arco $(a,b)$ diretto nel grafo indica che la pagina $a$ contiene un hyperlink alla pagine $b$.
In uno studio in cui si analizza la distribuzione degli archi entranti, o *gradi*, dei nodi nel grafo del web, è stato osservato che la frazione di pagine web che ha grado entrante $k$ è proporzionale a $k^{-c}$ per qualche costante $c$, invece che a $k^{-k}$.
Una funzione che decresce in tale modo, cioè come l'inverso di un polinomio, è chiamata **power law**.
```ad-Definizione
title: Definizione
Siano $C>0$ e $\alpha >1$ due costanti. Una **funzione potenza** (o **power law**) è una funzione proporzionale a
$$
C \cdot x^{-\alpha}
$$
```
Il fatto che in una rete reale (come il web) il numero di nodi di grado alto decresce **molto** più lentamente rispetto al modello *Erdős-Rény*, implica che non è del tutto appropriato considerare l'esistenza di un arco *indipendente* rispetto agli altri.
## Riconoscere una power law
Spesso, riconoscere una power law rispetto ad una funzione che decresce come l'inverso di una esponenziale non è scontato.
Un modo per verificare se una funzione è una power law è quello di considerare il grafico **log-log**, in cui i *tick* del grafico crescono esponenzialmente.
```ad-info
Per *tick* si intende gli indici del grafico che vengono segnati sugli assi. Generalmente si è abituati a vedere dei tick che crescono in maniera lineare $[0, 1, 2, ...]$, però spesso è necessario visualizzare l'asse secondo una scala differente, per esempio che cresce in maniera quadratica $[0, 1, 4, 9, 16, ...]$.
```
In pratica, i due assi del grafico rappresentano $\ln{x}$ e $\ln{y}$ , e invece di considerare $y=f(k)$, si considera $\ln{y}=\ln{f(k)}$. In questo modo, se $f(k) = k^{-c}$, il grafico sarà grosso modo una retta, dato che
$$
\ln{y} = \ln{k^{-c}} \approx -c \ln{k}
$$
![](Pasted%20image%2020240717154507.png)
# Fenomeno Rich get richer
Uno dei fenomeni più noti nelle reti è il cosiddetto fenomeno **rich get richer**. Si descrive il fenomeno con un esempio.

> Si sta costruendo una pagina web, nella quale bisogna inserire un riferimento ad un certo modello generativo di grafi aleatori. Fra tutte le pagine disponibili riguardo tale argomento, verrà scelta quella più *autorevole*: ad esempio, la pagina in prima posizione su un motore di ricerca, cioè una pagina che viene già puntata da molte altre pagine.

Dunque, considerando il grado entrante di una pagina web come il numero di hyper-link che la puntano, si ha che sulla base del precedente ragionamento, il grado della pagina in prima posizione nel motore di ricerca *tenderà sempre di più a crescere* rispetto, ad esempio, all'ultima pagina indicizzata.
Tutto questo implica che nel grafo del web l'esistenza di un arco **dipende** dall'esistenza di altri archi.

Un altro esempio concreto del fenomeno *rich-get-richer* avviene nelle reti sociali di un social network: se una persona è famosa ed ha molti follower tenderà a diventare sempre più popolare.
## Un modello per il fenomeno
Si vuole definire un modello generativo di grafi in cui l'aggiunta di un nuovo arco dipende dagli archi già presenti nel grafo, basato sull'assunzione che gli individui *tendono a copiare il comportamento* di altri individui. Inoltre, questo modello dovrà esibire una power law.
Si considera un processo di creazione di un grafo con le seguenti proprietà, tipiche del meccanismo di creazione delle pagine web:
1. I nodi vengono inseriti uno alla volta;
2. Ogni volta che viene inserito un nuovo nodo, si decide quali archi diretti inserire sulla base di quelli già presenti.
### Procedura di generazione
Fissato una probabilità $p \in [0,1]$, si eseguono tali operazioni in modo sequenziale.
1. Al tempo $t=1$ viene inserito il nodo $1$;
2. Al tempo $t=2$ viene inserito il nodo $2$ e l'arco diretto $(2,1)$;
3. Al generico passo $t = i > 2$ vengono creati il nodo $i$ e, scelto u.a.r. un nodo $j < i$:
	- Viene creato l'arco $(i,j)$ con probabilità $p$;
	- Viene creato l'arco $(i,k)$ dove $k$ è il nodo puntato dal nodo $j$, cioè l'estremo dell'arco $(j,k)$.
	
Intuitivamente: man mano che il numero di archi entranti in un nodo aumenta, cresce la probabilità che quel nodo venga selezionato come estremo di un arco uscente da un nodo di nuova creazione.
Allora il grafo generato da tale modello descrive bene il fenomeno *rich get richer*: è molto più probabile che un nuovo arco inserito punti ad un nodo con grado entrante elevato.
Infine, si osservi che ogni nodo $i>1$ ha esattamente un arco uscente.
![|center](Pasted%20image%2020240717155259.png)
### Una descrizione alternativa
Per rendere più semplice la formalizzazione del modello, si mostra una descrizione alternativa e equivalente del processo di costruzione del grafo appena introdotto.
Fissato una probabilità $p \in [0,1]$, si eseguono tali operazioni in modo sequenziale:
1. Al tempo $t=1$ viene inserito il nodo $1$;
2. Al tempo $t=2$ viene inserito il nodo $2$ e l'arco diretto $(2,1)$;
3. Al generico passo $t = i > 2$ vengono creati il nodo $i$ e un arco uscente da $i$
	- Con probabilità $p$, viene scelto u.a.r. un nodo $j < i$ e viene creato l'arco $(i,j)$;
	- Con probabilità $1-p$, viene scelto u.a.r. un nodo $j < i$ e viene creato l'arco $(i,k)$, dove $k$ è l'estremo dell'arco uscente $(j,k)$ dal nodo $j$.
## Verifica power law
Definito il modello, si verifica che questo rispetta una *power law*, ossia che nei grafi generati in accordo con questo modello, il numero atteso di nodi di grado $k$ decresce come l'inverso di un polinomio in $k$.
### Un po' di formalizzazione
Si definisce per prima cosa la variabile aleatoria binaria $\delta_{i,j}$ che vale 1 se esiste l'arco $(i,j)$, 0 altrimenti
$$
\delta_{i,j} = \begin{cases}
1, &\mbox{se } (i,j) \in E\\
0, &\mbox{altrimenti}
\end{cases}
\;\;\; \forall i > j
$$
La probabilità che esista un arco diretto $(i,j)$ nel grafo generato da tale modello è pari a
$$
\begin{align*}
      \mathbf{Pr}(\delta_{i,j} = 1)
      &= p \cdot \mathbf{Pr}\left( \mbox{viene scelto } j \right) + (1-p) \cdot \mathbf{Pr}\left( \mbox{viene scelto un nodo } h : \delta_{h,j} = 1 \right)\\
      &= \frac{p}{i-1} + (1-p)\cdot\mathbf{Pr} \Big( \bigcup_{h < i : (h,j) \in E} \mbox{viene scelto } h \Big)\\
      \textbf{(1)}&= \frac{p}{i-1} + (1-p)\cdot\sum_{h < i : (h,j) \in E} \mathbf{Pr} \left( \mbox{viene scelto } h \right)\\
      \textbf{(2)}&= \frac{p}{i-1} + \frac{(1-p)}{i-1}\cdot \sum_{h < i : (h,j) \in E} 1\\
      &= \frac{p}{i-1} + \frac{(1-p)}{i-1} \cdot\sum_{1 \leq h < i} \delta_{h,j} 
\end{align*}
$$
dove:
-   **(1)** la probabilità dell\'unione di eventi *disgiunti* è pari alla somma delle loro probabilità.
-   **(2)** la probabilità di scegliere un nodo $h < i$ è equamente distribuita come $\frac{1}{i-1}$.
Ora un paio di osservazioni.
#### Osservazione 1
Ha senso calcolare $P((i,j)\in E)$ per $i > 1$, dato che il nodo $1$ non ha archi uscenti.
#### Osservazione 2
Dato che ogni nodo $i>2$ ha esattamente un arco uscente, deve valere necessariamente che
$$
    \mathbf{Pr}(\exists j < i : (i,j) \in E ) = 1
$$
Infatti
$$
\mathbf{Pr}(\exists j < i : (i,j) \in E ) = \sum_{1 \leq j < i} \mathbf{Pr}( (i,j) \in E )
$$
Questo si può dimostrare per induzione su $i$:
-   per $i = 2$ vale $$
     \sum_{1 \leq j < 2} \mathbf{Pr}( (2,j) \in E ) = \mathbf{Pr}( (2,1) \in E ) = 1
     $$ e questo è vero **per costruzione**.
-   sia vero per $k \leq i - 1$, $\sum_{1 \leq j < k} \mathbf{Pr}( (k,j) \in E )=1$ per ipotesi induttiva, allora per $k = i$ si ottiene
$$
\begin{align*}
	\sum_{1 \leq j < i} \mathbf{Pr}( (i,j) \in E )
	&= \sum_{1 \leq j < i} \left[ \frac{p}{i-1} + \frac{(1-p)}{i-1} \sum_{1 \leq h < i} \delta_{h,j}  \right]\\ 
	&= \sum_{1 \leq j < i} \frac{p}{i-1} + \sum_{1 \leq j < i} \Big( \frac{(1-p)}{i-1} \sum_{1 \leq h < i} \delta_{h,j} \Big) \;\;\; \mbox{spezzo la serie}\\
	&= \frac{(i-1)p}{i-1} + \frac{(1-p)}{i-1} \sum_{1 \leq j < i} \sum_{1 \leq h < i} \delta_{h,j}\\
	&= p + \frac{(1-p)}{i-1} \sum_{1 \leq h < i} \underbrace{ \sum_{1 \leq j < i} \delta_{h,j} }_{h \scriptsize{\mbox{ ha 1 solo arco uscente}}} \;\;\; \mbox{inverto le due serie}\\
	&= p + \frac{(1-p)}{i-1} \sum_{1 \leq h < i} 1\\
	&= p + \frac{(1-p)}{i-1}(i-1)\\
	&= p + (1 - p) = 1
\end{align*}
$$
#### Osservazione 3
La regola per costruire il grafo ha la seguente interpretazione, che esprime chiaramente il fenomeno *rich get richer*.
Il nodo $j$ a cui connettere il nodo $i$ è scelto:
- uniformemente a caso con probabilità $p$;
- con probabilità proporzionale al grado di $j$ con probabilità $1-p$.
### Dimostrazione Power Law
La dimostrazione della presenza di una Power Law nel modello definito procede in quattro passi intermedi:
1.  Definizione di una legge aleatoria che esprima la variazione del grado (entrante) di un nodo nel tempo.
2.  Approssimazione **deterministica** e **continua** della legge.
3.  Risoluzione di una *equazione differenziale* che calcola il valore di tale approssimazione.
4.  Individuazione di una *power law*.
#### 1 - Legge aleatoria
Sia $D_j(t)$ la variabile aleatoria che esprime il numero di archi entranti nel nodo $j$ al passo $t$ del processo di generazione del grafo.
Si osserva che, per costruzione, la variabile $D_j(t)$ è definita per $t \geqslant j$ e che $D_j(j) = 0$  per ogni $j$: esprime la condizione iniziale della variabile.
Si osserva che il grado entrante di un nodo $j$ al passo $t+1$ può aumentare di **al più** una unità rispetto al passo $t$. In particolare, $D_j(t+1) = D_j(t) +1$ se e solo se l'arco $(t+1,j)$ viene creato.
Dunque sia la variazione nel tempo del grado entrante del nodo $j$ espressa come la differenza $D_j(t+1) - D_j(t)$; la probabilità che il grado entrante di $j$ aumenti di uno vale
$$
\begin{align*}
   \mathbf{Pr}(D_j(t+1) - D_j(t) = 1) &= \mathbf{Pr}(\delta_{t+1, j} = 1)\\
   &= \frac{p}{(t+1)-1} + \frac{(1-p)}{(t+1)-1} \sum_{1 \leq h < (t+1)} \delta_{h,j}\\
   &= \frac{p}{t} + \frac{(1-p)}{t}D_j(t)
\end{align*}
$$

#### 2 - Approssimazione deterministica e continua
Stabilita la legge $\mathbf{Pr}(D_j(t+1) - D_j(t) = 1) = \frac{p}{t} + \frac{(1-p)}{t}D_j(t)$, si definisce una sua approssimazione. Questo perché generalmente le leggi *discrete* e *probabilistiche* sono troppo complesse da analizzare.

Per prima cosa si rimuove il fattore aleatorio e viene resa deterministica con la seguente approssimazione
$$
\begin{cases}
   X_j(j) = 0\\
   \\
   X_j(t+1) - X_j(t) = \frac{p}{t} + \frac{(1-p)}{t}X_j(t)
\end{cases}
$$    
$\forall t \geq j$.
A questo punto si approssima il comportamento di tale funzione *discreta* con una funzione definita su un dominio *continuo*
$$
\begin{cases}
	   x_j(j) = 0\\
	   \\
	   \frac{d}{dt}x_j(t) = \frac{p}{t} + \frac{(1-p)}{t}x_j(t)
\end{cases} \quad \forall t \geqslant j
$$    
Non è detto che questa approssimazione si avvicini alla legge reale.
Si osserva infine che la seconda equazione del sistema è una *equazione differenziale*, cioè una equazione che lega una funzione alla sua derivata, del tipo $f'(x) = a \cdot f(x) + b$.
#### 3 - Risoluzione equazione differenziale
Si risolve ora la seguente equazione differenziale
$$\frac{d}{dt}x_j(t) = \frac{p}{t} + \frac{(1-p)}{t}x_j(t)$$

Per prima cosa si raccoglie il fattore $1/t$
$$ \frac{d}{dt}x_j(t) = \frac{1}{t} \left[ p + (1-p)x_j(t) \right] $$
dunque si dividono entrambi i membri per $\left[ p + (1-p)x_j(t) \right]$
$$ \frac{1}{p + (1-p)x_j(t)}  \frac{d}{dt}x_j(t) = \frac{1}{t} $$

A questo punto si procede integrando in $dt$
$$
\begin{align}
\int \frac{1}{p + (1-p)x_j(t)}  \frac{d x_j(t)}{dt} \, dt &= \int \frac{dt}{t} \\
\int \frac{d x_j(t)}{p + (1-p)x_j(t)} &= \int \frac{dt}{t}
\end{align}
$$
Si moltiplicano entrambi i membri per $(1-p)$
$$ \int \frac{(1-p) d x_j(t)}{p + (1-p)x_j(t)} = (1-p) \int \frac{dt}{t} $$
Si pone dunque $y = p + (1-p)x_j(t)$. La derivata di $y$ vale
$$ y' = \frac{dy}{dx_j(t)} = (1-p)$$
ovvero il numeratore della frazione nel primo integrale.
$$\int \frac{y'}{y} \, dx_j(t) = (1-p) \int \frac{dt}{t}$$
Il quale si può semplificare come
$$\int \frac{y'}{y} \, dx_j(t) =  \int \frac{\frac{dy}{dx_j(t)}}{y} \, dx_j(t) = \int \frac{dy}{y}$$

Adesso che è rimasto solo il fattore $y$ al primo 
membro, si sostituisce nuovamente col valore originale
$$\int \frac{dy}{y} = \int \frac{d \left[p + (1-p) x_j(t) \right]}{p + (1-p)x_j(t)} = (1-p) \int \frac{dt}{t}$$ A questo punto si svolge l'integrale, ottenendo che
$$\ln{\left( p + (1-p) x_j(t) \right)} = (1-p) \ln{(t)} + c$$
Da cui, elevando a potenza, si ottiene
$$p + (1-p) x_j(t) = t^{1-p} \cdot e^c = C \cdot t^{1-p}$$

Per calcolare la costante $C$ è sufficiente  considerare la condizione iniziale per cui $x_j(j) = 0$, ottenendo che
$$p = C \cdot j^{1-p} \implies C = \frac{p}{ j^{(1-p)} }$$
Infine, si conclude ottenendo
$$
\begin{align*}
p + (1-p) x_j(t) &= C \cdot t^{1-p}\\
p + (1-p) x_j(t) &= t^{1-p} \cdot e^c = \frac{p}{ j^{(1-p)} } \cdot t^{1-p} = p \left( \frac{t}{j}  \right)^{1-p}\\
(1-p) x_j(t) &= p \left[ \left( \frac{t}{j} \right)^{1-p} - 1 \right]\\
x_j(t) &= \frac{p}{1-p} \left[ \left( \frac{t}{j}  \right)^{1-p} - 1 \right]
\end{align*}
$$
Dunque si ottiene
$$
x_j(t) = \frac{p}{1-p} \left[ \left( \frac{t}{j}  \right)^{1-p} - 1 \right]
$$
#### 4 - Individuazione Power Law
In conclusione si calcola, dati $k$ e $t$, quanto vale la frazione dei nodi che al tempo $t$ ha esattamente grado entrante $k$.
Si definisce l'insieme $A_t(k) = \lbrace j \leq t : x_j(t) \geq k \rbrace$, ovvero l'insieme dei nodi che hanno grado entrante **almeno** $k$ al tempo $t$ (secondo l'approssimazione continua e deterministica).
Allora la frazione dei nodi con grado $k$ sarà la differenza tra il numero di nodi con grado <u>almeno</u> $k$ e il numero di nodi con grado <u>almeno</u> $k+1$, fratto il numero di nodi inseriti fino al tempo $t$ (che equivale a $t$, dato che al passo $t$ l'insieme dei nodi è $[t]$).
In formule, si deve calcolare
$$
\frac{1}{t} | A_t(k) - A_t(k+1) | = \frac{1}{t} \left( |A_t(k)| - |A_t(k+1)| \right)
$$
Per definizione, un nodo $j$ appartiene all'insieme $A_t(k)$ se e solo se $j \leq t$ e $x_j(t) \geqslant k$.
Ma dalla precedente equazione differenziale vale che $x_j(t) = \frac{p}{1-p} \left[ \left( \frac{t}{j}  \right)^{1-p} - 1 \right]$, perciò $x_j(t) \geq k$ se e solo se $\frac{p}{1-p} \left[ \left( \frac{t}{j} \right)^{1-p} - 1 \right] \geq k$.
A questo punto, si risolve la disequazione per trovare i valori di $j$ che rispettano la disuguaglianza
$$
\begin{align*}
 \frac{p}{1-p} \left[ \left( \frac{t}{j}  \right)^{1-p} - 1 \right] &\geq k\\
 \left( \frac{t}{j}  \right)^{1-p} - 1 &\geq k\frac{1-p}{p}\\
 \left( \frac{t}{j}  \right)^{1-p} &\geq k\frac{1-p}{p} + 1\\
 \frac{t}{j} &\geq \left[ k\frac{1-p}{p} + 1 \right]^{ \frac{1}{1-p} }\\
 j &\leq t \left[ k \frac{1-p}{p} + 1 \right]^{ - \frac{1}{1-p} }
\end{align*}
$$
Si osservi che, poiché $k \frac{1-p}{p} + 1$ è maggiore di 1 e l'esponente $- \frac{1}{1-p}$ è minore di 0, è vero che
$$\left[ k \frac{1-p}{p} + 1 \right]^{ - \frac{1}{1-p} } \leq 1 \implies t \left[ k \frac{1-p}{p} + 1 \right]^{ - \frac{1}{1-p} } \leq t$$
Riformulando l'affermazione precedente, il nodo $j$ appartiene all'insieme $A_t(k)$ se e soltanto se $j \leq t \left[ k \frac{1-p}{p} + 1 \right]^{ - \frac{1}{1-p} }$.

Dunque si riformula anche la definizione dell'insieme $A_t(k)$ come
$$A_t(k) := \bigg\{ j \leq t \left[ k \frac{1-p}{p} + 1 \right]^{ - \frac{1}{1-p} }  \bigg\} $$
tale insieme ha cardinalità esattamente
$$|A_t(k)| =  t \left[ k \frac{1-p}{p} + 1 \right]^{ - \frac{1}{1-p} }$$

Per comodità, si definisce la funzione
$$
F(k) = \left[ k \frac{1-p}{p} + 1 \right]^{ - \frac{1}{1-p} }
$$
Perciò la quantità da calcolare si può riscrivere come
$$\frac{1}{t} \left( |A_t(k)| - |A_t(k+1)| \right) = F(k) - F(k+1) = f(k)$$
A questo punto si *approssima* $f(k)$ come la derivata $-\frac{d}{dk}F(k)$.
Se il segno "`-`" confonde, con la seguente equazione apparirà più chiaro
$$
\begin{align}
 F(k) - F(k+1)
 &= \frac{(-1)}{(-1)} \left( F(k) - F(k+1) \right)\\
 &= - \left( - \frac{ F(k) - F(k+1) }{1} \right)\\
 &= - \left( \frac{ F(k+1) - F(k) }{1} \right)
 \approx -\frac{d}{dk}F(k)
\end{align}
$$
Derivando in $k$ si ottiene
$$
\begin{align*}
	 f(k) &\approx -\frac{d}{dk} F(k)\\
	 &= - \left( \frac{1}{1 - p} \right)
	 \cdot \left( \frac{1-p}{p} \right)
	 \cdot \left[ k \frac{1-p}{p} + 1 \right]^{ - \frac{1}{1-p} - 1 }\\
	 &= \frac{1}{p} \cdot
	 \left[ k \frac{1-p}{p} + 1 \right]^{ - \left( 1 + \frac{1}{1-p} \right) }
\end{align*}
$$
Tale funzione è l'inversa di un polinomio in $k$ con esponente massimo $\left( 1 + \frac{1}{1-p} \right)$, e in quanto tale è una *power-law*.
### Osservazioni sull'analisi
In realtà l'approssimazione continua e deterministica fatta per analizzare la correttezza della procedura di generazione del modello potrebbe discostarsi molto dal valore effettivo, ovvero quello descritto in maniera discreta e probabilistica.
In realtà è stato dimostrato che *con alta probabilità* nel modello di rete per *rich-get-richer* proposto, la frazione di nodi con grado $k$ è proporzionale a $k^{- \left(1 + \frac{1}{1-p} \right)}$.
# Impredicibilità della popolarità
Da come è definito il modello *rich-get-richer*, è ragionevole pensare che la distribuzione dei gradi dei nodi dipenda *fortemente* dalla fase iniziale del processo di generazione.
A tale ipotesi vi è a supporto un esperimento, di [Salganik-Dodds-Watts](https://www.princeton.edu/~mjs3/salganik_dodds_watts06_full.pdf) del 2006 che mostra quanto sia importante e instabile la fase iniziale, e di come i feedback dei partecipanti possono influenzare le scelte della utenza futura.

> Viene creato un sito di download musicale e vengono caricati 48 brani sconosciuti; in particolare, vengono create 8 playlist diverse e in ciascuna di essa sono presenti gli stessi 48 brani.
> Quando qualcuno visita il sito, viene indirizzato ad una delle 8 liste in modo casuale. Inoltre, per ogni playlist, viene mostrato un *contatore dei download* delle canzoni internamente alla playlist: se la canzone `X` viene scaricata dalla playlist `A`, quel download viene contato e riportato solo per `A`, e non per le altre playlist.
> Intuitivamente, il contatore di una playlist rappresenta un indice di popolarità di un brano fra i visitatori indirizzati a quella playlist. Dunque il contatore può incentivare i visitatori di una playlist a scaricare i brani più popolari di tale lista, in quanto la gente è influenzata dal parere della massa.
> Le conclusioni più interessanti si ottengono però rispetto all'evoluzione dei contatori delle diverse playlist nel tempo.
> Dato che inizialmente i contatori sono tutti posti a 0, e dato che le persone vengono indirizzate alle playlist <u>totalmente a caso</u>, si potrebbe pensare che col tempo più o meno tutte le playlist abbiano una stessa classifica dei download.
> In realtà si è osservato che ogni playlist costruisce un classifica totalmente differente dalle altre.
> Questo risultato, è dovuto al fatto che la popolarità di una canzone in una playlist piuttosto che in un altra dipende da come si evolvono le fasi iniziali.

Questo esperimento mostra che **il processo che conduce alla popolarità è fortemente sensibile alle condizioni iniziali**.
Perciò si potrebbe inizialmente sfruttare il **feedback** per cercare di influenzare la popolarità di una entità della rete.
# La lunga coda
La distribuzione della popolarità dei nodi di una rete può avere importanti effetti su attività commerciali. Si consideri un sito di vendita di libri online, dove ciascun libro viene modellato mediante un nodo.
Il gestore di questo sito potrebbe chiedersi:

> *è più conveniente vendere <u>tante copie di pochi libri molto popolari</u>, oppure è meglio vendere <u>poche copie ma di tantissimi libri poco popolari</u>?*

Anche in questo caso si è interessati a questioni di popolarità, però invece di sapere quanti nodi hanno grado $k$ (ovvero sono popolari) si è interessati a sapere se il volume di affare di tanti nodi poco popolari è paragonabile a quello di pochi nodi molto popolari.

Per affrontare tale questione, si considerino i nodi in ordine di popolarità <u>non crescente</u>.  
Ovvero, siano i nodi $[n]$ e sia $\forall j \in [n], \delta_{j}$ la popolarità, o grado entrante, di $j$. Si assume che i nodi siano ordinati in maniera tale che:
$$
\delta(1) \geq \delta(2) \geq ... \geq \delta(n)
$$
Si assume inoltre che il numero di nodi avente una certa popolarità <u>aumenti</u> al diminuire della popolarità.
Ovvero il numero di nodi con popolarità 100 sarà <u>minore</u> di quelli con popolarità 99, che a sua volta sarà più piccolo di quelli con popolarità 98, ecc...
![|center](Pasted%20image%2020240723145557.png)

A questo punto, si costruisce un grafico dove lungo l'ascisse si elencano tutti i nodi che hanno la stessa popolarità con un solo punto: l'ascissa di tale punto è il numero di nodi che hanno quella popolarità; lungo l'asse delle ordinate invece si elenca la popolarità dei gruppi di nodi: in corrispondenza al punto che indica un gruppo di nodi con popolarità $i$, si indica il grado $i$ di questi nodi. Si osservi che non esiste nessun grado di popolarità con numero di nodi uguali, dato che il numero di nodi con popolarità $p_1 > p_2$ è minore del numero di nodi con popolarità $p_2$.
![|center](Pasted%20image%2020240723145932.png)
Si osserva che, scambiando gli assi coordinati, si ottiene il grafico che indica il numero $n$ di nodi che hanno grado entrante $k$, cioè il grafico analizzato nella sezione precedente.
Sia $d(k)=n$ la funzione che, data la popolarità $k$, restituisce il **numero** di nodi $n$ aventi grado entrante $k$. Allora, assumendo che $d$ sia invertibile, il grafico in figura rappresenta grosso modo la funzione $k=d^{-1}(n)$, ossia la funzione che, dato un certo numero di nodi aventi la stessa popolarità, restituisce proprio tale popolarità.
È possibile dimostrare che se la funzione $f(k)$ che esprime **la frazione** del numero di nodi che hanno popolarità $k$ è una **PowerLaw**, allora anche la funzione $d(k)$ (che invece esprime il numero esatto, e non una frazione) è una **PowerLaw**, ovvero
$$n = d(k) \approx k^{-c}$$
per qualche $c > 0$.
Da essa è poi facile ricavarne l'inversa
$$k = d^{-1}(n) \approx n^{-1/c}$$
Se quindi $k \approx n^{-1/c}$, dove $k$ è un livello di popolarità ed $n$ è il numero di nodi con tale popolarità, si può osservare graficamente che $k$ decresce **molto** lentamente.
![|center](Pasted%20image%2020240723150510.png)
Dalla figura, si vede che la popolarità $k$ al crescere del numero dei libri decresce molto lentamente: si crea appunto una **lunga coda**.
Dato che in genere il **volume di affari** ricavabile dalla vendita di un gruppo di prodotti è proporzionale alla loro popolarità, l'area di quelli poco popolari, ovvero l'area sotto la **lunga coda**, è <u>tutt'altro che trascurabile</u>.
# Gli effetti della web-search
Ci si domanda se l'utilizzo dei motori di ricerca su internet hanno un impatto su fenomeno del *rich-get-richer*.
Da una parte il fenomeno è **amplificato**, in quanto i motori di ricerca tendono a proporre come primi risultati le pagine più "famose" (inerentemente ad una ricerca).
D'altra parte l'effetto è **mitigato**, in quanto la ricerca è soggetta a una "scrematura" delle pagine in base alle parole usate per la ricerca.
Infatti l'utilizzo di parole inusuali possono portare come risultato della ricerca pagine che non sono "famose".
Per quello visto prima, un venditore è interessato a promuovere i suoi prodotti "di nicchia" (quelli meno popolari), appunto per cercare di ricavare il guadagno dalla lunga coda.
Per questo motivo i venditori online cercano sempre di reindirizzare gli utenti verso i prodotti "in coda", per esempio tramite l'utilizzo del cosiddetto **recommendation system** i quali invitano gli utenti a visitare le pagine meno popolari.
In poche parole gli strumenti di web-search sono un esempio di utilizzo degli **effetti feedback**, i quali a volte amplificano l'effetto *rich-get-richer*, mentre altre volte lo mitigano.