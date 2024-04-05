In molte situazioni di data mining, non si conosce preventivamente l'intero data set. Ossia, tutti i dati presi in esame non risultano essere disponibili ad ogni istante. 
Si vuole studiare lo scenario in cui i dati arrivano in uno o più flussi, tali per cui questi risultino perduti nel caso in cui non vengano processati o memorizzati immediatamente nell'istante di arrivo. Si assume che questi flussi arrivino così rapidamente che non risulti possibile memorizzarli tutti in una memoria attiva, ossia in un database convenzionale.
I dati possono essere visti come infiniti e non stazionari, tali per cui quindi la loro distribuzione cambia nel tempo.
Gli algoritmi per processare flussi di dati richiedono che il flusso in input venga, in qualche modo, "schematizzato". Si studia quindi come poter raccogliere un campione di dati all'interno di un flusso affinché questo risulti essere utile, e come filtrare un flusso per eliminare gli elementi indesiderati. A partire da ciò, si mostra come stimare il numero di elementi differenti all'interno di un flusso utilizzando molta meno memoria rispetto a quella richiesta nel caso in cui si volessero listare tutti gli elementi visti. 
Un altro approccio per "riassumere" un flusso è quello di guardare solo ad una "finestra" di dati di lunghezza fissata, consistente negli ultimi $n$ elementi di un flusso. Si interroga poi tale finestra come se fosse una relazione all'interno di un database. Se vi sono molti flussi e/o $n$ risulta essere troppo grande, può non essere possibile memorizzare l'intera finestra per ogni flusso. Risulta quindi necessario schematizzare anche la finestra. 
La gestione dei flussi risulta di fondamentale importanza quando l'*input rate*, ossia la velocità con cui l'input viene trasmesso, viene controllato esternamente.  
# Stream Model
Si studia il modello volto al processo di flussi di dati.
Facendo un analogia con un sistema per la gestione di database, si può vedere un processore di flusso come una sorta di sistema di gestione di dati.
Un numero arbitrario di flussi può entrare nel sistema. 
Gli elementi di un flusso possono essere visti come delle tuple.
Ogni flusso fornisce elementi al sistema seguendo la propria *schedule* personale, ossia, non devono necessariamente avere la stessa velocità di trasmissione o gli stessi tipi di dati degli altri flussi. Inoltre, l'intervallo di tempo che passa nella trasmissione di elementi contenuti all'interno di un flusso non deve necessariamente essere uniforme. La proprietà principale che distingue il processo di flussi di dati, rispetto al processo di dati contenuti all'interno di un database, è data dal fatto che la velocità di arrivo degli elementi del flusso non è sotto il controllo del sistema che li elabora.

I flussi possono venir memorizzati all'interno di un archivio di grande capacità, ma si assume che l'accesso a quest'unità sia abbastanza lento da non poter permettere di rispondere alle queries, sui dati in essa memorizzati, efficientemente. Tale memoria può essere esaminata solo in circostanze particolari, utilizzando processi di recupero di informazioni i quali richiedono un tempo eccessivo. Il sistema dispone anche di una memoria di lavoro avente dimensione limitata, nella quale parti del flusso possono venir conservati e utilizzati per rispondere alle queries di interesse. Ovviamente, tale memoria dispone di una capacità sufficientemente limitata affinché non risulti essere possibile memorizzare tutti i dati provenienti da tutti i flussi presi in analisi. 

Il sistema volto al processo di flussi non è in grado quindi di memorizzare l'intero stream in maniera tale che gli elementi che lo compongono risultino essere rapidamente accessibili. Possono quindi essere memorizzati e aggiornati solo brevi sketch di un flusso.

I tipici problemi riguardanti i flussi di dati sono:
- Campionamento di dati da un flusso, e costruzione di un campione casuale.
- Queries su una *sliding window*, come il problema del Pattern Matching, o l'identificazione del numero di oggetti di un certo tipo $x$ negli ultimi $k$ elementi del flusso.
- Filtrare un flusso di dati, ossia selezionare un elemento con una certa proprietà $x$ dal flusso.
- Contare il numero di elementi distinti negli ultimi $k$ elementi del flusso.
- Stimare la media o la deviazione standard degli ultimi $k$ elementi.
- Individuare elementi frequenti.
# Pattern Matching
Si studia ora il problema del **Pattern Matching**.
Sia $x_i$ un elemento appartenente ad un certo alfabeto $\Sigma$. Il flusso è una stringa di lunghezza $m$ su $\Sigma$, formalmente, sia $x=x_1x_2\ldots x_m$ tale flusso, allora $x\in \Sigma^m$. Si ricorda che $m$ è troppo grande affinché $x$ possa venir interamente rappresentato in memoria.
Si supponga che venga fornito un pattern $y = y_1y_2\ldots y_n \in \Sigma^n$. La lunghezza del pattern $n$ è tale che $n \ll m$, ma anch'essa può risultare abbastanza grande da non poter memorizzare totalmente il pattern all'interno di una memoria ad accesso veloce.  
Si vuole sapere quante volte $y$ appare in $x$ come sua sottostringa, avente quindi forma $y=x_ix_{i+1}\ldots x_{i+n-1}$.

Per risolvere il problema del pattern matching in maniera efficiente su un Data Stream, risulta necessario utilizzare un data sketch adeguato.
A tal fine, si definisce una misura di similarità: dati due elementi $\hat{x},\hat{y}\in \Sigma^*$, si vuole sapere se $\hat{x}=\hat{y}$. Tale misura prende il nome di *identità*.  Si vuole quindi uno sketch $f(\cdot)$ tale per cui, si ha con alta probabilità che $f(\hat{x})=f(\hat{y})$ se e solo se $\hat{x}=\hat{y}$. Ci si riferisce a tale proprietà, nella trattazione di questo problema, con il termine *string identity*.
```ad-Osservazione
Ricordando la definizione di data sketch, la proprietà 4 che esso deve avere per il problema *Pattern Matching* è la proprio la *String Identity*.
```
## Rabin Hash function
Si studia quindi la ***Rabin hash function***, che risulta essere un data sketch valido per il problema pattern matching.
Senza perdita di generalità, sia $x$ una stringa di lunghezza $n$ sull'alfabeto $\Sigma = [0,\sigma -1]$. Le stringhe possono essere viste come interi di $n$ cifre in base $\sigma >0$. Si osserva esplicitamente che:
- Questo scenario può essere utilizzato per rappresentare sottoinsiemi di $[1,n]$ ponendo $\Sigma = \{0,1\}$.
- Per ogni funzione $f$, se $bitsize(f(x))<bitsize(x)$, dove $bitsize(\cdot)$ è una funzione che restituisce il numero di bit che un oggetto occupa in memoria, allora devono necessariamente verificarsi collisioni, ossia deve esistere necessariamente una coppia $x\neq y$ tale per cui $f(x)=f(y)$.
Una prima idea per risolvere il problema dell'identità, può essere quella di utilizzare come sketch la funzione
$$
h_{a,b}(x) = ((a \cdot x + b) \mod{p}) \mod{h}
$$
vista in precedenza, dove $p$ è un numero primo e $h$ è la dimensione dell'hash table, con $h \ll n$, considerando la stringa $x$ come un numero con $n$ cifre in base $|\Sigma|$. 
Sfortunatamente, questo non risulta essere un buon approccio: essendo che, per definizione di tale funzione, risulti necessario che $p>x$  (dove si evidenzia che $x$ deve essere preso come valore e non come la sua lunghezza) per ogni input $x$ della funzione, bisognerebbe eseguire le operazioni di aritmetica modulare su interi con $n$ cifre per aggiornare lo sketch, e ciò richiederebbe uno spazio pari a $\Omega(n)$. Ciò non rispetterebbe quindi il punto $3$ nella definizione di data sketch.

Si definisce quindi il **Rabin's hashing**, uno schema di hashing di stringhe che risolve il problema appena descritto. Si fa presente che tale schema non risulti essere universale, ma nonostante ciò, garantisce una bassa probabilità di collisione.
```ad-Definizione
title: Definizione (Funzione hash di Rabin):
Si fissi $q > \sigma$ numero primo, e si scelga uniformly at random $z\in \mathbb{Z}_q = \{0,1,\ldots,q-1\}$. 
Sia $x=\langle x[1],x[2],\ldots,x[n] \rangle \in \Sigma^n$ una stringa di lunghezza $n$.
La funzione hash di Rabin $k_{q,z}(x)$ è definita come:
$$
k_{q,z}(x) = \left( \sum_{i=0}^{n-1}x[n-i] \cdot z^i\right) \mod q
$$
In forma espansa
$$
k_{q,z}(x) = ( x[n] + x[n-1] \cdot z + x[n-2] \cdot z^2 + \dots + x[1]\cdot z^{n-1}) \mod{q}
$$

```
In altre parole, $k_{q,z}(x)$ è un polinomio modulo $q$ valutato in $z$, e ha come coefficienti i caratteri di $x$.

Sia $|x|$ la lunghezza della stringa $x$. Si definisce lo sketch di Rabin $f(x)$ della stringa $x$ come la coppia
$$
f(x)=(k_{q,z}(x),z^{|x|} \mod q)
$$
Si osserva che $f(x)$ necessita di $\Theta(\log{q})$ bits per essere rappresentata, perché $k_{q,z}(x)$ e $z^{|x|} \mod{q}$ sono numeri che vanno da $0$ a $q-1$.

Si vuole ora mostrare che questo sketch può essere calcolato ed aggiornato in maniera efficiente.
Si supponga di voler concatenare un carattere $c$ alla stringa $x$, ottenendo quindi la stringa $x \cdot c$, dove tale notazione rappresenta $x$ *concatenato* con $c$. Il valore di $x$ può essere valutato come segue
```ad-Lemma
title: Lemma 
$$
k_{q,z}(x \cdot c) = (k_{q,z}(x) \cdot z +c) \mod q
$$
```
La lunghezza di $x \cdot c$ è pari a $|x|+1$ e $z^{|x|+1} \mod q = (z^{|x|} \mod q) \cdot z \mod q$. 

Questo lemma fornisce anche un algoritmo per calcolare $k_{q,z}(x)$ in maniera efficiente: si inizia da $k_{q,z}(\varepsilon)$, dove $\varepsilon$ è la stringa vuota, e si concatenano i caratteri di $x$ uno alla volta.
______
**Algoritmo $A$ per** $k_{q,z}(x)$ 
$\mathbf{input}:$ $q$ primo, $z$ u.a.r. in $[q]$, $x=<x[1],x[1],\ldots,x[n]> \in \Sigma^n$ 
1. $k_{q,z}(\varepsilon) = 0$
2. **for** $j=1$ to $n$ **do**
	1. $(k_{q,z}(<x[1],x[2],\ldots,x[j-1]>) \cdot z + x[j]) \mod q$
____
Si osserva che $\mathbf{space}(A(q,z,x)) = \mathcal{O}(\log q)$.

Usando un'idea simile, si possono concatenare gli sketches di due stringhe in tempo costante, come segue
```ad-Lemma
title: Lemma
$$k_{q,z}(x \cdot y) = \left( k_{q,z}(x) \cdot z^{|y|} + k_{q,z}(y)\right) \mod q
$$
```
Anche a partire da tale lemma, si definisce il seguente algoritmo
___________
**Algoritmo $A_1$ per $k_{q,z}(x \cdot y)$**
$\mathbf{input}$: $q$ primo, $z$ u.a.r. in $[q]$, $x=<x[0],x[1],\ldots,x[n-1]>,y=<y[0],y[1],\ldots,y[n-1]> \in \Sigma^n$ 
1. $k_{q,z}(x \cdot y) = \left( k_{q,z}(x) \cdot z^{|y|} + k_{q,z}(y)\right) \mod q$
________
Dove $z^{|y|}$ può venir calcolato usando un algoritmo per l'esponenziazione modulare.

La lunghezza della stringa $x \cdot y$ è pari a $|x|+|y|$ e il valore $z^{|x|+|y|} \mod q$ può essere calcolato in maniera efficiente come
$$
z^{|x|+|y|} \mod q = \left(\left(z^{|x|} \mod q\right) \cdot \left(z^{|y|} \mod q\right)\right) \mod q
$$
Si vuole mostrare ora che questo sia un buono sketch per il problema dell'identità, ossia che, se $x \neq y$, allora $k_{q,z}(x) \neq k_{q,z}(y)$ con alta probabilità. Ciò è implicato dal seguente lemma
```ad-Lemma
Sia $x \neq y$, con $|x|=|y|=n$. Allora
$$
\mathbf{Pr} \left[ k_{q,z}(x) = k_{q,z}(y) \right] \leq \frac{n}{q}
$$
```
```ad-Dimostrazione
Si osserva che $$\mathbf{Pr} \left[ k_{q,z}(x) = k_{q,z}(y) \right] = \mathbf{Pr}\left[ k_{q,z}(x)-k_{q,z}(y) \equiv_q 0\right]$$
Ora, la quantità $k_{q,z}(x) - k_{q,z}(y)$ è, a sua volta, un polinomio. 
Sia $x-y$ la stringa definita come $$x-y = <x[n]-y[n] ,x[n-1]-y[n-1], \ldots, x[1]-y[1]>$$ dove ogni ogni operazione $x[i]-y[i]$ è effettuata in $\mod q$.
Allora, si ha che 
$$
k_{q,z}(x) - k_{q,z}(y) \mod q = k_{q,z}(x-y)
$$
Da ciò segue che 
$$
\mathbf{Pr}\left[ k_{q,z}(x)-k_{q,z}(y) \equiv_q 0\right] = \mathbf{Pr}\left[ k_{q,z}(x-y) \equiv_q 0\right]
$$
Essendo $x \neq y$, allora $k_{q,z}(x,y)$ è un polinomio di grado al più $n$ in $\mathbb{Z}_q$, e non è il polinomio zero. 
Si ricorda che ogni polinomio ad una variabile diverso da zero di grado $n$ su un campo ha al più $n$ radici. Essendo $q$ primo, $\mathbb{Z}_q$ è un campo e quindi vi sono al più $n$ valori di $z$ tali per cui $k_{q,z}(x-y) \equiv_q 0$. Essendo che $z$ viene scelto uniformemente da $[q]$, la probabilità di prendere una radice è al più $n/q$.
```
```ad-Corollario
title: Corollario (Probabilità di errore)
Si scelga un primo $n^{c+1} \leq q \leq 2 \cdot n^{c+1}$ per una costante arbitrariamente grande $c$. Allora, per ogni $bitsize(k_{q,z}(x) \in \mathcal{O}(\log n))$ bits e, per ogni $x \neq y$
$$
\mathbf{Pr}\left[k_{q,z}(x)=k_{q,z}(y)\right] \leq n^{-c}
$$
Ossia, $x$ ed $y$ collidono con probabilità decrescente come l'inverso di un polinomio.
```
Si osserva che, essendo $\mathbf{space}(k_{q,z}(x)) = \mathcal{O}(\log q) = \mathcal{O}(\log n)$, essendo $q = \Theta{(n^c)}$, lo sketch comprime una stringa di size $n$ in una di size $\log n$.
## Algoritmo di Karp-Rabin
Per risolvere il problema del pattern matching, si fa quindi riferimento alla funzione hash di Rabin. Si osserva che tale tecnica porta ad una soluzione diretta che impiega spazio $\mathcal{O}(n)$.
Si supponga che sia stato già processato il flusso $x_1,\ldots,x_i$ con $i \geq n$, e che si siano già calcolati gli sketches $k_{q,z}(x_{i-n+1}x_{i-n+2}\ldots x_i)$ e $k_{q,z}(y)$.
Confrontando questi due valori, in tempo costante, si può venire a sapere se il pattern compare o meno negli ultimi $n$ caratteri del flusso di dati. Il passo cruciale è quello di aggiornare lo sketch dello stream non appena un nuovo elemento $x_{i+1}$ arriva in input al processore del flusso. 
Ciò può essere fatto come segue:
$$
k_{q,z}(x_{i-n+2}x_{i-n+3}\ldots x_{i+1}) = (k_{q,z}(x_{i-n+1}x_{i-n+2}\ldots x_{i})-x_{i-n+1} \cdot z^{n-1}) \cdot z + x_{i+1} \mod q
$$
Il valore $z^{n-1} \mod q$ può essere pre calcolato, quindi l'operazione appena descritta richiede tempo costante, ossia, il numero di operazioni per ciascun oggetto entrante è $\Theta(1)$.
Si osserva che, dovendo accedere al carattere $x_{i-n+1}$ per eseguire tale operazione, ad ogni istante l'algoritmo deve mantenere gli ultimi $n$ caratteri visti nel flusso, utilizzando quindi spazio $\mathcal{O}(n)$.

Si fa presente che, se non vi sono collisioni tra il pattern e le $m-n+1 \leq m$ sottostringhe del flusso di lunghezza $n$, allora l'algoritmo restituisce il risultato corretto, ossia il numero di occorrenze del pattern nello stream. La probabilità che il pattern collida con una qualsiasi di queste sottostringhe è al più $n/q$. Per l'union bound, la probabilità che il pattern collida con almeno una sottostringa è $mn/q \leq m^2/q$. Si vuole che ciò accada con bassa probabilità, ossia che decresce come l'inverso di un polinomio. Ciò può essere ottenuto scegliendo un primo $q$ nel range $[m^{c+2},2\cdot m^{c+2}]$, per ogni costante $c$. Tale numero primo, e quindi l'output della funzione hash di Rabin, può essere memorizzato in $\mathcal{O}(\log m)$ bits, ossia $\mathcal{O}(1)$ parole. Da ciò si ottiene quindi il seguente teorema
```ad-Teorema
L'algoritmo di Karp-Rabin risolve il problema del pattern matching nello streaming model utilizzando $\mathcal{O}(n)$ parole di memoria e con un delay di $\mathcal{O}(1)$. La soluzione corretta viene restituita con probabilità $1-m^{-c}$, per ogni costante $c \geq 1$ scelta al momento dell'inizializzazione.
```
# Campionamento da un Data Stream
Non potendo mantenere in memoria l'intero flusso, un approccio per lavorare con i dati ricevuti è quello di memorizzarne un campione. Si studia quindi il problema di estrarre campioni utili all'interno di uno stream di dati.
In particolar modo, si prendono in considerazione i due seguenti problemi:
1. Campionare una proporzione fissata di elementi nel flusso.
2. Mantenere un campione casuale di grandezza fissata su uno stream potenzialmente infinito.
## Campionare una proporzione fissata di elementi nel flusso
In questo problema, si vuole selezionare un sottoinsieme di elementi in un flusso, in maniera tale che si possano effettuare su di essi delle queries di interesse, e che le relative risposte risultino essere *statisticamente rappresentative* dell'intero flusso. 
### Esempio applicativo
Lo scenario applicativo per lo studio del problema è il seguente:
Un motore di ricerca riceve un flusso di queries, e vuole studiare il comportamento dell'utente tipico. Si assume che il flusso in input $U$ consiste di tuple della forma $(userID,query,time)$ eseguite dagli utenti. Informalmente, si vuole rispondere a queries come "Quanto spesso un utente esegue la stessa query nello stesso giorno?" o "Quale frazione delle tipiche query di un utente sono state ripetute nell'ultimo mese?". Si assume inoltre che si desideri memorizzare solo $1/10$ degli elementi dello stream.

Il problema può essere formalizzato come segue:
**Input:** Stream $U$ di tuple $(userID,query,time)$.
**Task algoritmico:** Trovare un campione $S\subseteq U$ tale per cui per il tipico utente $u$ e per ogni query $q$ approssimi bene, in valore atteso, la frazione delle $q-$*occorrenze* in $U$ effettuate da $u$.
```ad-Definizione
title: Definizione ($q-$occorrenza)
Dato un flusso $U$ di tuple aventi struttura $(userID,query,time)$, si definisce $q-$occorrenza una tupla in $U$ avente la componente $query = q$.
```
L'approccio banale per affrontare il problema è quello di generare un numero casuale, ad esempio un intero tra $0$ e $9$, come risposta ad ogni query di ricerca per ogni utente. Si memorizza la tupla se e solo se il numero casuale è $0$. Cosi facendo, ogni utente ha in media, $1/10$ delle sue query memorizzate nel campione.
___
**N-Algo:**
1. Inizializza $S=\emptyset$
2. **for each** tupla = $(userID,query,time) \in U$
	1. Scegli independently uniformly at random $z\in[10]$
	2. **if** $z=0$ **then** Memorizza $(userID,query,time)$ in $S$
	3. **else** scarta $(userID,query,time)$
3. **for each** query $q$:
	1. restituisci i valori (medi) delle frazioni delle $q$-occorrenze calcolate *solamente* su $S$.
___
Si osserva esplicitamente che l'associazione tra tuple e numeri casuali può essere effettuato mediante funzioni hash, in particolar modo da una funzione $h:U \longrightarrow [10]$, che mappa casualmente ogni elemento di $U$ in uno tra $10$ buckets. Gli elementi mappati nei buckets $1,2,\ldots,9$ vengono scartati, mentre gli elementi mappati in $0$ costituiranno il campione memorizzato per un utente.  

Questo schema, però, fornisce una risposta sbagliata alla richiesta del numero medio di queries duplicate effettuate da un utente.
Si prende a titolo di esempio il seguente scenario:
Si considerino $s+d$ queries distinte $q_1,q_2,\ldots,q_s$ e $q^{'}_1,q^{'}_2,\ldots,q^{'}_d$.
Ogni utente $u$, nell'intervallo di tempo di un mese, effettua $s+2d$ queries di ricerca  nella seguente maniera:
- $s$ queries vengono eseguite una singola volta.
- $d$ queries vengono eseguite, ciascuna di esse, due volte.
- Nessuna query di ricerca viene effettuata più di due volte.
Nel flusso $U$ sono presenti quindi $s+2d$ $q-$occorrenze per ogni utente.
Se si dispone di un campione $S$ pari ad $1/10$ delle queries totali, ottenuto mediante l'algoritmo appena descritto, dove quindi $\mathbf{E}[|S|]=(1/10)\cdot |U|$, ci si aspetta di trovare in esso $s/10$ delle queries eseguite una singola volta.
Delle $d$ queries effettuate due volte, ci si aspetta che solo $d/100$ appariranno due volte nel campione. Infatti, sia $X$ la variabile aleatoria che conta il numero di queries, ripetute due volte all'interno del flusso, che appaiono entrambe le volte nel campione. Per ogni query $q$ che appare due volte nello stream si definisce la variabile aleatoria $X_q$ come segue:
$$
X_q= 
  \left\{ 
    \begin{aligned}
      &\ 1 \ \ \ \ \textnormal{se } q \textnormal{ appare due volte nel campione } S\\
      &\ 0 \ \ \  \textnormal{ altrimenti}  
    \end{aligned}
  \right.
$$
Si ha quindi che 
$$
X = \sum_{q}X_q
$$
Ricordando che, la probabilità che una tupla $(u,q,t)$ venga selezionata nel campione è proprio
$$
\mathbf{Pr}\left[h((u,q,t))=0\right] = \frac{1}{10}
$$
e che tale selezione risulta essere indipendente per ogni tupla, si ha che
$$
\mathbf{E}[X_q] = 1 \cdot \frac{1}{10} \cdot \frac{1}{10} = \frac{1}{100}
$$
Per la linearità del valore atteso si ottiene quindi
$$
\mathbf{E}[X] = \mathbf{E}\left[\sum_qX_q\right]= \sum_q\mathbf{E}\left[X_q\right] = d \cdot \frac{1}{100} = \frac{d}{100}
$$
Inoltre, delle queries che appaiono due volte nell'intero stream, ci si aspetta che $19d/100$ appaiano almeno una volta. Infatti, siano $(u,q,t_1)$ e $(u,q,t_2)$ due query duplicate. Siano $\mathcal{E}_1$ ed $\mathcal{E}_2$ gli eventi *indipendenti*
$$
\mathcal{E}_1 = (u,q,t_1) \textnormal{ appare nel campione } S
$$
$$
\mathcal{E}_2 = (u,q,t_2) \textnormal{ appare nel campione }S
$$
La probabilità che $q$ appaia almeno una volta nel campione è:
$$
\mathbf{Pr}\left[(\mathcal{E}_1 \cap \mathcal{E}_2) \cup (\mathcal{E_1} \cap \mathcal{\bar{E}_2}) \cup (\mathcal{\bar{E}}_1 \cap \mathcal{E}_2)\right]
$$
Essendo $\mathbf{Pr}[\mathcal{E}_i] = 1/10$ e, di conseguenza,  $\mathbf{Pr}[\mathcal{\bar{E}}_i] = \mathbf{Pr}[\mathcal{E_i}] - 1/10 = 9/10$ per $i=1,2$, si ha che
$$
\begin{align*}
\mathbf{Pr}\left[(\mathcal{E}_1 \cap \mathcal{E}_2) \cup (\mathcal{E_1} \cap \mathcal{\bar{E}_2}) \cup (\mathcal{\bar{E}}_1 \cap \mathcal{E}_2)\right] 
&= \frac{1}{10} \cdot \frac{1}{10} + \frac{1}{10}\cdot \frac{9}{10} +  \frac{9}{10} \cdot \frac{1}{10}\\ 
\\
&= \frac{1}{100} + \frac{2\cdot9}{100} = \frac{19}{100}
\end{align*}
$$
Sia $Y$ la variabile aleatoria che conta il numero di queries, ripetute due volte all'interno del flusso di dati in ingresso, che appaiono *almeno* una volta nel campione. Per ogni query $q$ che appare due volte nello stream si definisce la variabile aleatoria $Y_q$ come segue:
$$
Y_q= 
  \left\{ 
    \begin{aligned}
      &\ 1 \ \ \ \ \textnormal{se } q \textnormal{ appare almeno una volta nel campione } S\\
      &\ 0 \ \ \  \textnormal{ altrimenti}  
    \end{aligned}
  \right.
$$
Si ha che $\mathbf{E}[Y_q] = \mathbf{Pr}[Y_q = 1] =  \mathbf{Pr}\left[(\mathcal{E}_1 \cap \mathcal{E}_2) \cup (\mathcal{E_1} \cap \mathcal{\bar{E}_2}) \cup (\mathcal{\bar{E}}_1 \cap \mathcal{E}_2)\right]$, e che $Y = \sum_q Y_q$.
Quindi
$$
\mathbf{E}[Y] = \sum_q\mathbf{E}[Y_q] = d \cdot \frac{19}{100}
$$
Per un'argomentazione analoga, ci si aspetta che $18d/100$ appariranno esattamente una volta. Questo perché $18/100$ è la probabilità che una delle due occorrenze, di una query effettuata due volte, appaia nell'$1/10$ del campione selezionato, mentre l'altra si trova nei $9/10$ non selezionati:
$$
\mathbf{Pr}\left[ (\mathcal{E_1} \cap \mathcal{\bar{E}_2}) \cup (\mathcal{\bar{E}}_1 \cap \mathcal{E}_2)\right] =\left(\left(\frac{1}{10} \cdot \frac{9}{10}\right) + \left(\frac{9}{10}\cdot \frac{1}{10} \right)\right) = \frac{18}{100}
$$
Sia $Z$ la variabile aleatoria che conta il numero di queries, ripetute due volte all'interno del flusso di dati in ingresso, che appaiono esattamente una volta nel campione, e sia $Z_q$ la variabile aleatoria definita per ogni $q$ che appare due volte nello stream come segue:
$$
Z_q= 
  \left\{ 
    \begin{aligned}
      &\ 1 \ \ \ \ \textnormal{se } q \textnormal{ appare esattamente una volta nel campione } S\\
      &\ 0 \ \ \  \textnormal{ altrimenti}  
    \end{aligned}
  \right.
$$
Si ha che $\mathbf{E}[Z_q] = \mathbf{Pr}\left[ (\mathcal{E_1} \cap \mathcal{\bar{E}_2}) \cup (\mathcal{\bar{E}}_1 \cap \mathcal{E}_2)\right]$ e che $Z = \sum_q Z_q$, e quindi 
$$
\mathbf{E}[Z] = \sum_q\mathbf{E}[Z_q] = d \cdot \frac{18}{100}
$$
La risposta corretta alla query riguardante la frazione di ricerche ripetute è $d/(s+d)$. Nonostante ciò, la risposta ottenuta dal campione prodotto dall'algoritmo banale è $d/(10s+19d)$. Per derivare tale formula, si osserva che, delle queries contenute nel campione, ci si aspetta che solo $d/100$ appariranno ripetute due volte in esso, mentre $s/10 + 18d/100$ appariranno una singola volta. Si ha quindi che il numero atteso di elementi *distinti* in $S$ è pari a 
$$
\frac{s}{10} + \frac{d}{100} + \frac{18d}{100}
$$
e

$$
\mathbf{E}\left[|S| \right] = \frac{s}{10} + \frac{2d}{100} + \frac{18d}{100}
		
$$
Quindi, la frazione di ricerche ripetute che compaiono due volte nel campione è data da dal numero di ricerche ripetute che ci si aspetta di avere nel campione, diviso il numero degli elementi distinti nel campione:
$$
\frac{\frac{d}{100}}{\frac{s}{10} + \frac{d}{100} + \frac{18d}{100}} = \frac{d}{10s+19d} 
$$
e per nessun valore positivo di $s$ e $d$, si può avere che $d/(s+d) = d/(10s+19d)$.
### Campionamento degli utenti
Nello scenario appena descritto, il campione viene selezionato su *tutte* le queries effettuate da ogni singolo utente.
La query studiata per il problema perso in analisi, come molte altre queries riguardanti le statistiche del tipico utente, non può essere risposta prendendo un campione delle ricerche effettuate da ciascun singolo utente. Risulta necessario infatti prendere $1/10$ degli utenti, e prendere *tutte* le loro ricerche come campione. Se risultasse possibile memorizzare una lista di tutti gli utenti e la loro eventuale partecipazione alla definizione di tale campione, si potrebbe operare come segue:
- Ogni volta che una query di ricerca arriva nel flusso, si verifica se l'utente sia stato selezionato o meno per la costruzione del campione.
- Se è stato scelto, si aggiunge la query al campione, altrimenti no.
- Se l'utente che richiede la query non è stato ancora registrato, ossia, la query che arriva nel flusso è la prima per tale utente, si genera un intero casuale tra $0$ e $9$. Se il numero è $0,$ si memorizza tale utente tra quelli selezionati per la definizione del campione, altrimenti, si memorizza tra quelli non selezionati, dovendo necessariamente tener traccia anche di quest'ultimi al fine di scartare le eventuali queries future.
Questo metodo funziona sino a quando risulta possibile memorizzare l'intera lista di tutti gli utenti e la loro appartenenza alla definizione del campione nella memoria principale, non essendo possibile accedere al disco per ogni ricerca in ingresso. 
Usando una funzione hash, si può evitare di mantenere la lista degli utenti. Si esegue l'hash per ogni $userID$, mappandoli in uno tra dieci buckets etichettati con gli interi che vanno da $0$ a $9$. Se l'utente è stato mappato nel bucket $0$, si accettano le sue query per la definizione del campione, altrimenti no. 
Si osserva esplicitamente che, in realtà, non si memorizzano gli utenti nei buckets. Infatti, si usa una funzione hash come generatore di numeri casuali, con l'importante proprietà che, se applicata allo stesso utente diverse volte, si ottiene sempre lo stesso numero "casuale".
Così facendo, senza memorizzare l'appartenenza o meno al campione per ciascun utente, si può ricostruire tale informazione in ogni istante in cui arriva una query mediante la funzione hash.

Tale procedura può essere descritta mediante il seguente pseudocodice, dove l'insieme di tutti gli utenti è conosciuto in anticipo.:
____
**User-Sample Algo:**
1. Scegli $1/10$ degli utenti e inserisci *tutte* le loro $q-$occorrenze nel campione $S$ nella seguente maniera:
	1. Usa una funzione hash che mappa gli utenti in $10$ buckets i.u.a.r.
	2. Selezione tutte le $q-$occorrenze degli utenti mappati nel bucket $0$ e memorizzali in $S.$
2. Esegui dei calcoli sulla media di $S$.
___
Si fa un'analisi dell'algoritmo descritto. Sia $Q$ l'insieme delle query, e sia $I$ l'insieme degli utenti. Si supponga che $I$ sia conosciuto in anticipo.
Sia $x(v,q)$ il numero di $q-$occorrenze dell'utente $v\in I$ nello stream in input. 
Sia $X_S(V,q)$ il numero di $q-$occorrenze dell'utente $V \in I$ selezionato casualmente per la definizione del campione $S$. Si osserva esplicitamente che $V$, e quindi $X_S(V,q)$, è una variabile aleatoria. Questo perché gli utenti da cui costruire il campione sono scelti u.a.r. indipendentemente.
Si ha quindi che la media del numero delle $q-$occorrenze per ogni utente $v \in I$, sia questa $\mathbf{AVG}_{v\in I}(x(v,q))$, è uguale a $\mathbf{E}_{V\in S}\left[X_S(V,q)\right]$ ,per ogni $q$ fissato:
$$
\mathbf{AVG}_{v\in I}(x(v,q)) = \mathbf{E}_{V\in S}\left[X_S(V,q)\right]
$$
e quindi (? investigare, penso sia il teorema che si usa anche per dimostrare che la similarità tra due firme tende alla similarità dei rispettivi insieme in finding similar items), si ha che:
$$
\mathbf{AVG}_{V\in S}\left(X_S(V,q)\right) \longrightarrow \mathbf{AVG}_{v\in I}(x(v,q)) \textnormal{ per } |\textnormal{\{utenti in }S \}| \longrightarrow |I|
$$
Ossia,  l'output dell'algoritmo $\mathbf{AVG}_{V\in S}\left(X_S(V,q)\right)$ tende al valore target $\mathbf{AVG}_{v\in I}(x(v,q))$ al tendere del numero di utenti scelti per il campione verso il numero complessivo di utenti, ossia, aumentando gli utenti selezionati, si ottiene un risultato sempre più vicino all'obiettivo.

Si evidenzia che la dimensione attesa di $S$ è $1/10$ dello stream in input $U$ di *tutti* gli oggetti.
Infatti, sia $C_j$ la variabile aleatoria definita come segue:
$$
C_j= 
  \left\{ 
    \begin{aligned}
      &\ 1 \ \ \ \ \textnormal{se }  h(j)=0 \\
      &\ 0 \ \ \  \textnormal{ altrimenti}  
    \end{aligned}
  \right.
$$
dove $h:I \longrightarrow [10]$ è la funzione hash che mappa ogni utente in uno tra $10$ buckets, e se  $h(j)=0$ allora l'utente $j$ viene selezionato per la creazione di $S$.
Sia $x_j$ il numero di elementi dell'utente $j$ nell'intero stream $U$. Allora
$$
\sum_{j\in I}x_j = |U|
$$
e
$$
\mathbf{E}\left[|S|\right] = \sum_{j \in I}x_j \cdot \mathbf{Pr}\left[C_j = 1\right] = \frac{1}{10} \cdot \sum_{j\in I}x_j = \frac{|U|}{10}
$$
Più generalmente, si può ottenere un campione consistente in una frazione razionale $a/b$ degli utenti, mappando gli $userID$ in $b$ buckets, che vanno da $0$ a $b-1$. Si aggiunge la query al campione se il valore hash dell'utente che la effettua è minore di $a$.
### Il problema generale del campionamento
L'esempio applicativo studiato può essere generalizzato mediante il seguente problema:
Si ha un flusso di dati, consistente in tuple con $n$ componenti $(x_1,x_2,\ldots,x_n)$.
Un sottoinsieme delle componenti delle tuple $(x_{i,1},x_{i,2},\ldots,x_{i,j})$ definisce le *chiavi*, sulle quali si basa la selezione del campione. La scelta delle chiavi dipende dall'applicazione, ad esempio, per lo scenario applicativo visto, si hanno tuple a $3$ componenti della forma $(userID,query,time)$ e la chiave risulta essere la componente $userID$. 

Per ottenere un campione di size $a/b$, si mappa il valore della chiave, per ciascuna tupla, in $b$ buckets, e si inserisce una tupla nel campione se e solo se la valutazione della funzione hash nel valore della chiave è al più $a$. Se la chiave è composta da più di una componente, la funzione hash deve combinare i valori di tali componenti per creare un singolo valore hash. Il risultato di tale operazione sarà un campione consistente da tutte le tuple con determinati valori chiave. I valori chiave selezionati saranno approssimativamente una frazione pari ad $a/b$ di tutti i valori chiave che appaiono nello stream.

Ad esempio, si supponga di trovarsi nello scenario descritto in precedenza, dove i valori chiave sono dati dagli $userID$ di ciascun utente. Si supponga di avere un'hash table con $b$ buckets. Si vogliono selezionare le tuple se l'hash del valore chiave è al più $a$. Per generare un campione del $30\%$ degli utenti, si mappano le chiavi in $b=10$ buckets, e si inseriscono le tuple nel campione se queste vengono mappate in uno dei primi $3$ buckets.
### Variare dinamicamente la dimensione del campione
La dimensione del campione può aumentare man mano che il flusso entrante in input nel sistema aumenta. Può quindi non risultare possibile memorizzare la frazione di flusso desiderata al fine di creare un campione da analizzare. Se si ha un limite sul numero di tuple provenienti dal flusso che si possono memorizzare, allora la frazione dei valori chiave che lo costituiscono deve variare, diminuendo con l'avanzare del tempo. Per far si che, in ogni istante, il campione consista in *tutte* le tuple appartenenti ad un sottoinsieme dei valori chiave, si sceglie una funzione hash $h$ che mappa i valori chiave in un grande numero $B$ di buckets. Si mantiene una *variabile di soglia* $t \leq B-1$, che, inizialmente, può essere anche il più grande numero di bucket $B-1$. In ogni istante, il campione consiste in quelle tuple tali per cui la chiave $K$ soddisfa $h(K) \leq t$. Le nuove tuple che entrano nel sistema dal flusso vengono aggiunte al campione se e solo se soddisfano tale condizione.
Se il numero di tuple memorizzate nel campione eccede lo spazio ad esse dedicato, si riduce $t$ a $t'$, dove $t' << t$, e tutte le tuple mappate nei buckets avente numero $b>t'$ memorizzate in precedenza nel campione vengono rimosse da esso. Ulteriore efficienza può essere ottenuta mantenendo un indice sui valori hash, in maniera tale che si possano individuare velocemente tutte le tuple le quali la chiave viene mappata in un particolare valore.

Tale procedura può essere descritta schematicamente come segue:
1. Mappa ogni chiave in un grande numero $B$ di buckets.
2. Imposta un valore di soglia iniziale $t \leq B-1$. Tutte le chiavi mappate nei buckets aventi etichetta $b \leq t$ verranno inclusi in $S$.
3. Se $|S|$ diventa troppo grande, riduci $t$ a $t'$, con $t' << t$ e rimuovi tutte le tuple aventi chiave mappata nei buckets aventi etichetta $b>t'$.
4. Ripeti il passo $3$ se necessario.
## Mantenere un campione casuale di grandezza fissata su uno stream infinito.
Si studia il problema di mantenere un campione casuale $S$ di dimensione fissata $s$ nello scenario in cui la lunghezza del flusso che il sistema deve elaborare cresca nel tempo. In particolar modo, non si conosce la dimensione di tale flusso in anticipo, e questa può potenzialmente crescere all'infinito.
Si vuole costruire il campione $S$, in maniera tale che, supponendo che all'istante $n$ il sistema abbia visto i primi $n$ elementi del flusso, ogni elemento visto si trovi nel campione $S$ corrente con probabilità $s/n$, dove con campione corrente si fa riferimento a quello elaborato sino all'ultimo istante in cui ci si trova.

Una prima idea banale può essere quella di memorizzare tutte le $n$ tuple viste sino all'ultimo istante, e da esse estrarne $s$ i.u.a.r. Ovviamente, questo approccio risulta fallimentare, necessitando di una quantità di memoria pari a $\Theta(n)$ per conservare gli $n$ elementi.

La soluzione per il problema è data dal seguente algoritmo
____
**Reservoir Sampling RS**
**Input:** $s$ 
1. Memorizza i primi $s$ elementi del flusso in $S$
2. Si supponga di aver visto i primi $n$ elementi, e che all'istante $n+1$, l'$(n+1)$-esimo elemento arrivi al sistema ($n+1 > s$):
	1. Con probabilità $s/(n+1)$, inserisci l'$(n+1)-$esimo elemento in $S$, altrimenti scartalo.
	2. Se è stato scelto l'$(n+1)-$esimo elemento per l'inserimento in $S$, scegli u.a.r. uno dei precedenti $s$ elementi nel campione $S$ ed eliminalo.
___
Si vuole mostrare che, dopo aver visto $(n+1)$ elementi, $S$ mantenga la proprietà che ogni elemento visto sino a quell'istante si trovi in esso con probabilità $s/(n+1)$.
Lo si fa per induzione.
Il caso base è data dalla parte deterministica iniziale dell'algoritmo, ossia, per $n=s$ elementi visti, si ha che il campione $S$ mantiene la proprietà desiderata. Infatti, ciascuno degli $n=s$ elementi si trova in $S$ con probabilità $s/s=1$.
Induttivamente, si supponga di aver visto $n$ elementi, e che la probabilità che ciascuno di essi sia in $S$ è $s/n$.
Quando l'$(n+1)-$esimo elemento arriva, lo si inserisce in $S$ con probabilità $s/(n+1)$. Si vuole mostrare che, la probabilità che ogni altro elemento *del flusso*, all'istante di arrivo dell'elemento $n+1$,  si trovi in $S$ è proprio $s/(n+1)$.
Per fare ciò definiscono i tre seguenti eventi:
$$
\mathcal{E}_1 = \textnormal{L'elemento }n+1 \textnormal{ viene scartato}
$$
$$
\mathcal{E}_{2,1} = \textnormal{L'elemento } n+1 \textnormal{ non viene scartato}
$$
$$
\mathcal{E}_{2,2} = \textnormal{L'elemento }x \textnormal{ nel campione non viene eliminato}
$$
Si studiano le probabilità di questi eventi. 
L'elemento $n+1$ non viene scartato, ossia viene inserito in $S$ con probabilità
$$
\mathbf{Pr}\left[\mathcal{E}_{2,1}\right] = \frac{s}{n+1} $$
Si osserva che $\mathbf{Pr}[\mathcal{E}_{1}] = \mathbf{Pr}[\mathcal{\bar{E}}_{2,1}]$, quindi
$$
\mathbf{Pr}[\mathcal{E}_{1}] = 1 - \frac{s}{n+1}
$$
Essendo che, nel caso in cui l'$(n+1)-$esimo venisse scelto, ogni altro elemento nel campione in quell'istante ha probabilità $1/s$ di venir scartato, si ha che
$$
\mathbf{Pr}\left[\mathcal{E}_{2,2} \right] = 1 - \frac{1}{s} = \frac{s-1}{s}
$$
Per ogni vecchio elemento $x\in S$, la probabilità che l'algoritmo lo mantenga in $S$ è data da
$$
\mathbf{Pr}[\mathcal{E}_{1}] + \mathbf{Pr}\left[\mathcal{E}_{2,1}\right]\mathbf{Pr}\left[\mathcal{E}_{2,2} \right]  = \left(1 - \frac{s}{n+1} \right) + \left(\frac{s}{n+1}\right)\left(\frac{s-1}{s}\right) = \frac{n}{n+1}
$$
Infatti $x$ rimane in $S$ o se il nuovo elemento viene scartato, o se il nuovo elemento viene aggiunto e tale $x$ non viene rimosso. 
Quindi, all'istante $n+1$, ciascuno dei vecchi elementi $x\in S$ rimane in esso con probabilità $n/(n+1)$.
Per un generico elemento nel flusso processato in precedenza, si ha che la probabilità che questo si trovi in $S$ all'istante $n+1$ è data dalla probabilità che questo si trovi in $S$ all'istante $n,$ moltiplicata per la probabilità che tale elemento non venga rimosso per fare posto al nuovo, ossia 
$$
\frac{s}{n} \cdot \frac{n}{n+1} = \frac{s}{n+1}
$$
E ciò dimostra, per induzione sulla lunghezza del flusso $n$, che tutti gli elementi hanno la stessa probabilità $s/n$ di far parte del campione.
# Queries su una finestra scorrevole
Si supponga di avere una finestra di lunghezza $N$ su un flusso binario, che consiste negli $N$ elementi ricevuti più recentemente dal flusso. 
In un dato istante, gli elementi nel flusso alla sinistra della finestra sono stati già visitati da essa, mentre gli elementi alla sua destra devono ancora essere visitati.
Si vuole sapere, ad ogni istante, quanti $1$ sono presenti negli ultimi $k$ bits per ogni $k\leq N$.
Anche in questo scenario, per via dei limiti sulla memoria, non è possibile memorizzare l'intera finestra, ossia, gli ultimi $N$ bits che il sistema riceve in un certo istante.
## Formalizzazione del problema
**Input:** Un flusso binario on-line infinito $I$, $N$ lunghezza della finestra.
**Obiettivo:** Calcolare la funzione $\#1(I,N,k)$, che restituisce il numero di cifre $1$ negli ultimi $k$ bits del flusso, per ogni $k\leq N$.
## Calcolo della soluzione esatta
Si vogliono contare *esattamente* il numero di $1$ negli ultimi $k$ bits dello stream, per ogni $k \leqslant N$. Per fare ciò, bisogna necessariamente memorizzare tutti gli $N$ bits della finestra, essendo che una qualsiasi altra rappresentazione che utilizza meno di $N$ bits non può funzionare.
Infatti, sia questo $R(N,k)$ uno schema di rappresentazione che utilizza meno di $N$ bits per rappresentare gli $N$ bits nella finestra, ossia $|R(N,k)| \leqslant N-1$. Essendovi in tutto $2^N$ possibili stringhe binarie di lunghezza $N$, ma meno di $2^N$ possibili rappresentazioni per esse, devono necessariamente esistere due stringhe di bits $x$ ed $y$ tali che cui $x \neq y$, le quali hanno la stessa rappresentazione $R$. Allora $x$ e $y$ devono differire in almeno un bit, dunque si supponga che i primi $k-1$ bits di $x$ ed $y$ siano uguali, mentre differiscano solamente per il $k-$esimo bit. Se $k<N$, i restanti bits che costituiscono le due stringhe non sono di interesse alla risoluzione della query.
Essendo le rappresentazioni per tali stringhe identiche, un qualsiasi algoritmo che calcola la funzione $\#1(I,N,k)$ facendo riferimento allo schema $R(N,k)$ non è in grado di distinguere le due stringhe $x$ e $y$, producendo lo stesso output per entrambe e, quindi, fallendo, essendo che una di esse ha necessariamente un bit in più asserito rispetto all'altra.
Tale dimostrazione quindi mostra che:
- Non si può fare riferimento ad un *data sketch* per ottenere una soluzione esatta al problema.
- Per contare *esattamente* il numero di $1$ negli ultimi $k$ bits dello stream, per una qualsiasi distribuzione del flusso in input arbitraria, lo spazio di memoria impiegato da un algoritmo che risolve tale problema è almeno $N$. Bisogna quindi usare almeno $N$ bits per rispondere ad una qualsiasi query in merito agli ultimi $k$ bits per ogni $k\leq N$.
Si vuole quindi trovare una soluzione approssimata al problema.
## Algoritmo DGIM
Si mostra un algoritmo che utilizza $\mathcal{O}(\log^2 N)$ bits per rappresentare una finestra di $N$ bits, che permette di stimare il numero di $1$ nella finestra con un errore, nel caso peggiore, non più grande del $50\%$. Tale fattore di errore può essere ridotto ad ogni valore costante $c>0$, che utilizza sempre $\mathcal{O}(\log^2 N)$ bits, ma con un fattore costante che cresce man mano che $c$ diminuisce.

Per iniziare, si associa un *timestamp* ad ogni bit, ossia la posizione nella quale questo arriva nel flusso. Il primo bit ha timestamp $1$, il secondo ha timestamp $2$ e così via. Dovendo distinguere le posizioni degli elementi solamente all'interno della finestra di lunghezza $N$, si possono rappresentare i timestamps in modulo $N$, in maniera tale che questi possano venir rappresentati mediante $\log_2 N$ bits.
Così facendo, memorizzando il numero *totale* dei bits visti nel flusso in modulo $N$, si può determinare, da un timestamp in modulo $N$, la posizione del bit con tale timestamp all'interno della finestra.
### Divisione in buckets
Si divide la finestra in **buckets**. Un bucket è un record $\langle A,B \rangle$ consistente in:
- $A:$ Il time-stamp della fine di tale bucket (ossia il bit più a destra contenuto in esso), che richiede quindi $\mathcal{O}(\log N)$ bits di memoria.
- $B:$ Il numero di $1$ all'interno del bucket. Tale numero deve essere necessariamente una potenza di $2$, e ci si riferisce a tale quantità come la *size* del bucket. Per rappresentare tale valore sono necessari $\mathcal{O}(\log\log N)$ bits.
#### Dimensione di un bucket
Per rappresentare un bucket, sono necessari $\log_2N$ bits per rappresentare il timestamp (in modulo $N$) del suo estremo destro. Per rappresentare il numero di $1$ al suo interno sono necessari $\log_2\log_2N$ bits. Questo è vero perché tale numero, sia questo $i$, è una potenza di $2,$ sia questa $2^j = i$. Si può quindi rappresentare $i$ codificando $j$ in binario. 
$$
2^j = i \iff \log_22^j = \log_2 i \iff j = \log_2i \iff \log_2j = \log_2\log_2i
$$
Essendo $i$ al più $N$, per fare ciò sono necessari $\log_2\log_2 N$ bits. Quindi, $\mathcal{O}(\log N + \log \log N)= \mathcal{O}(\log N)$ bits risultano sufficienti per rappresentare un bucket.
#### Regole dei buckets
Ci sono sei regole che devono essere eseguite quando si rappresenta uno stream mediante buckets:
1. Ogni posizione all'interno della finestra contenente un $1$ deve trovarsi in un bucket;
2. I buckets **non si sovrappongono** nei timestamps, ossia, una posizione all'interno della finestra non può trovarsi in più di un bucket;
3. Possono esistere solo **uno** o **due** buckets con la stessa dimensione;
4. La size di ogni bucket *deve essere sempre* una **potenza di due**;
5. I buckets sono ordinati secondo la loro dimensione, decrescente rispetto al tempo (più il bucket contiene elementi recenti, più è piccolo in dimensione);
6. Un bucket viene eliminato quando il suo estremo destro si trova in una posizione maggiore di $N$ unità di tempo nel passato.
In particolar modo, le proprietà $(2),(3)$ e $(5)$ sono mantenute dinamicamente.
### Numero di buckets per la rappresentazione di una finestra
Si è osservato che ogni bucket può essere rappresentato mediante $\mathcal{O}(\log N)$ bits. Se la finestra ha lunghezza $N$, allora non possono sicuramente esserci più di $N$ bits aventi valore $1.$ Si supponga ora che il più grande bucket abbia size $2^j$. Allora $j$ non può eccedere $\log_2 N$, perché se così fosse, ci sarebbero più $1$ in questo bucket rispetto al numero degli $1$ nell'intera finestra. Quindi, ci sono al più due buckets di tutte le $\log_2{n}$ dimensioni possibili, che vanno da $2^{\log_2{N}}$ sino ad $1$, e nessun bucket di size maggiore.
Si conclude da ciò che, per una finestra di dimensione $N$, esistono $\mathcal{O}(\log{N})$ buckets.
Essendo che ogni bucket è rappresentato mediante $\mathcal{O}(\log N)$ bits, lo spazio totale necessario per memorizzare tutti i buckets rappresentanti una finestra di dimensione $N$ è $\mathcal{O}(\log^2N)$.
### Come mantenere le condizioni espresse all'ingresso di nuovi bits nel flusso
Si supponga di avere una finestra di lunghezza $N$, rappresentata dai buckets in maniera tale che vengano rispettate le condizioni definite in precedenza. Quando un nuovo bit entra nella finestra, può essere necessario modificare i buckets, in maniera tale che questi continuino a rappresentare la finestra e a soddisfare le condizioni dell'algoritmo DGIM.
Quando un nuovo bit entra nella finestra:
- Si controlla il bucket più a sinistra, ossia quello presente da più tempo. Se il suo timestamp, ossia il timestamp del bit più a destra contenuto in esso, è uguale al timestamp del bit corrente meno $N$, allora tale bucket non possiede più al suo interno nessuno degli $1$ nella finestra. Quindi, lo si elimina dalla lista dei buckets.
Bisogna ora considerare se il nuovo bit in ingresso è $0$ o $1$. Se è $0$, allora non è necessaria nessuna modifica all'interno dei buckets. Se è $1$, bisogna effettuare dei cambiamenti al loro interno:
- Si crea un nuovo bucket avente dimensione $1$, contenente solo tale bit. Tale bucket avrà quindi come timestamp il tempo attuale.
- Se era presente un solo bucket di dimensione $1$, allora non risulta necessario effettuare ulteriori cambiamenti. Altrimenti, se si hanno tre buckets di dimensione $1$, si combinano i due buckets più a sinistra aventi dimensione $1$.

Per combinare due buckets adiacenti della stessa dimensione, li si sostituiscono con un unico bucket avente dimensione doppia. Il timestamp del nuovo bucket così creato è il timestamp del bucket più a destra tra i due coinvolti in tale operazione.

In generale, combinare due buckets di una certa dimensione $2^i$ può portare alla creazione di un terzo bucket avente dimensione $2^{i+1}$, supponendo che gli altri due esistessero già in precedenza. Dunque si continuano a combinare i buckets sino a quando non risulti essere più necessario.  Essendovi al più $\log_2 N$ dimensioni differenti, ed essendo che la combinazione di due buckets adiacenti della stessa dimensione richiede tempo costante, si ha come risultato che ogni nuovo bit viene processato in tempo $\mathcal{O}(\log N)$.
### Come stimare il numero di $1$ negli ultimi $k$ bits per ogni $k$, con un errore non più grande del $50\%$.
Si supponga che si voglia sapere quanti $1$ si trovino negli ultimi $k$ bits della finestra, per $1 \leq k \leq N$. Per fare ciò, si prende il bucket $b$ con il timestamp minore, ossia meno recente, che contiene almeno alcuni dei $k$ bits più recenti. Si stima il numero degli $1$ contenuti negli ultimi $k$ bits come la somma delle sizes di tutti i buckets alla destra di $b$ (ossia più recenti), più metà della dimensione di $b$ stesso. Questo perché non si sa quanti $1$ del bucket più vecchio $b$ si trovino nel range della sotto-finestra di dimensione $k$. 
Si supponga che la stima, riguardante la risposta alla query, coinvolga un bucket $b$ di dimensione $2^j$, che si trova parzialmente nel range della query $k$.
L'output dell'algoritmo $Y$ è quindi il seguente
$$
Y = \sum_{i=0}^{j-1} a_i 2^i\ + \ \frac{1}{2}\cdot 2^j   \geq \sum_{i=0}^{j-1} 2^i\ + \ 2^{j-1} \geq 2^j-1 + \ 2^{j-1}
$$
dove $a_i \in \{1,2\}$ è il numero di buckets aventi size $2^i$.
Si vuole studiare quanto la risposta corretta $c$ sia lontana dalla stima effettuata. 
### Ridurre l'errore di approssimazione
Si tratta questo argomento informalmente.
L'idea chiave è quella di mantenere, invece di $1$ o $2$ buckets per ciascuna size, $b-1$ o $b$ buckets con $b>0$, eccetto per il bucket avente size maggiore, per il quale si può avere un qualsiasi numero tra $1$ ed $r$ di questi. Cosi facendo, l'errore è al più $\mathcal{O}(1/r)$.
Scegliendo $r$ in maniera appropriata, si può avere un tradeoff tra complessità spaziale ed errore di approssimazione.
### Contare interi
Si vuole vedere come estendere l'algoritmo per sommare gli ultimi $k$ elementi di uno stream, nel caso in cui tale flusso non fosse composto unicamente da cifre binarie ma da interi.
L'idea è quella che, partendo dall'ipotesi di avere all'interno del flusso interi positivi nel range che va da $1$ a $2^m$, per i quali sono quindi necessari $m$ bit per rappresentare ciascuno di questi valori univocamente,  si considerino gli $m$ bits di ciascun intero come un flusso separato. Così facendo, si può usare l'algoritmo DGIM per stimare il numero di $1$ che costituiscono un intero. Si supponga che il conto dell'$i-$esimo bit sia $c^i$. Si ha allora che la somma degli interi è 
$$
\sum_{i=0}^{m-1} c_i 2^i
$$
# Filtrare elementi nei flussi
Un'altro processo comune che viene applicato su flussi è la selezione, o **filtraggio**, di elementi. Si vogliono selezionare quelle tuple all'interno di un flusso che rispettano un criterio.
Se il criterio di selezione è una proprietà della tupla che può essere calcolata facilmente, ad esempio, se la prima componente della tupla è minore di $10$, allora la selezione è facile da effettuare. Diventa più difficile quando il criterio di selezione comprende la verifica dell'appartenenza di una componente ad un dato insieme.
## Il problema
Il problema può essere formulato quindi come segue
___
**Modello di flusso:** Ogni elemento del data stream $X$ preso in analisi è una tupla $x=\langle k_1,k_2,\ldots,k_l \rangle.$ 
**Input**: Una lista di filtri, ossia un sottoinsieme $S$ di valori che rispettano un determinato criterio per $k_1$.
**Output:** Determinare quali, e quindi filtrare, le tuple $x\in X$ tali per cui $k_1(x) \in S$.
___
Questo problema diventa particolarmente difficile da trattare quando l'insieme $S$ è troppo grande per essere immagazzinato nella memoria principale.
Infatti, l'approccio banale di utilizzare una funzione hash $h$ che mappa tutti gli elementi di $S$ in una hash table $T$, e verificare per ogni tupla $x$ del flusso se $h(k_1(x))$ venga mappata correttamente in $T$, non risulta possibile vista la quantità di memoria necessaria per memorizzare tutti gli elementi in $S$ in una hash table.
## Esempio applicativo: filtro mail
Si vogliono filtrare le email, contenute all'interno di un flusso, provenienti da specifici indirizzi. In particolar modo, il flusso è costituito da coppie $\langle k_1,k_2 \rangle$, dove $k_1$ è l'indirizzo del mittente e $k_2$ il contenuto della mail.
Si supponga di avere un insieme $S$ di un miliardo di indirizzi email ammissibili, ossia per i quali si è sicuri che, se una mail arrivi da uno di essi, allora questa sicuramente non è spam.
Si vogliono quindi selezionare tali mail all'interno del flusso, scartando quelle che non rispettano il vincolo definito.

Un approccio per la risoluzione del problema è dato dall'algoritmo first cut, caratterizzato da una prima fase di pre-processing, e da una seconda fase effettuata online, ossia, durante l'arrivo del flusso.
### Algoritmo First-cut
Sia $U$ l'insieme di *tutti* i valori assumibili da $k_1$, ossia di tutti gli indirizzi email, e sia $S\subseteq U$ l'insieme dei valori per $k_1$ reputati validi. L'algoritmo opera come segue.
___
**Fase 1: Pre-processing**
- Si crea un array $B[0 \ldots n-1]$ di $n$ bits, impostati inizialmente tutti a $0$.
- Si sceglie una funzione hash $h:U \longrightarrow [n]$.
- Per ogni componente $s\in S$ si calcola $h(s)$.
- Si imposta $B[h(s)]=1$ per ogni componente. I restanti bit in $B$ rimangono quindi impostati a 0.
___
**Fase 2: Online**
Sia $x=\langle x_1,\ldots,x_k \rangle$ il nuovo elemento entrante nel flusso.
- Calcola $h(x_1)$
- Se $B[h(x_1)] = 1$ allora accetta l'elemento $x$ dello stream, scartalo altrimenti.
___
Si fa presente che:
- se un elemento $x=\langle x_1,\ldots,x_k \rangle$ in ingresso è tale per cui $B[h(x_1)]=0$, allora tale indirizzo non si trova in $S$, essendo che $h$ mappa ogni elemento di $S$ all'interno dell'array $B$. Quindi si può scartare $x$ con sicurezza.
- per ogni elemento $x$ nello stream tale per cui $x_{1} \in S$, si ha che questa verrà accettata. L'algoritmo non restituisce quindi falsi negativi, cioè non scarta elementi $x$ che devono essere accettati.
Il problema è che $h$ potrebbe mappare un valore $v \in U \backslash S$, ossia non ammissibile secondo il vincolo definito, in un'entrata di $B$ impostata ad $1$, per via di una collisione $h(v)=h(s)$ con un elemento $s \in S.$ Può quindi generare falsi positivi, ossia elementi da scartare verranno immessi nel flusso filtrato da prendere in analisi.
#### Risultati
Sia $|S|=m$ il numero di indirizzi email ammessi, ad esempio $1$ miliardo, e sia $|B| = n$ la dimensione dell'array, ad esempio $1$ GigaByte, ossia $8$ miliardi di bits.
Si hanno quindi, per l'applicazione studiata, i seguenti risultati:
```ad-Teorema
title: Teorema (No falsi negativi)
Se l'indirizzo email è in $S$, ossia, per una mail $x=\langle x_1,x_2 \rangle$, $x_1 \in S$, allora sicuramente $x_1$ viene mappato da $h$ in un bucket avente bit impostato ad $1$, e quindi supera sicuramente la fase di filtraggio.
```
```ad-Teorema
title: Teorema (Falsi positivi)
Approsimativamente $m/n=1/8$ dei bits sono impostati ad $1$, quindi circa $1/8$ (ossia la probabilità che avvenga una collisione), degli indirizzi *non* in $S$ superano la fase di filtraggio.

```
In realtà, meno di $1/8$ indirizzi non in $S$ superano la fase di filtraggio, essendo che più di un'indirizzo può essere mappato nello stesso bit.
#### Analisi accurata
Si fa quindi un analisi più accurata per il numero di falsi positivi, facendo riferimento al modello *balls into bins*. In questo problema si hanno $m$ sfere che vengono lanciate in $n$ cesti, dove il cesto per ogni sfera viene scelto indipendentemente e con distribuzione di probabilità uniforme.
Sia quindi $|S|=m$ e $|B|=n$. La probabilità per un elemento $u\in U$ di venir mappato in un bucket pieno, ossia tale per cui $B[h(u)]=1$, è esattamente la frazione $\mathbf{Pr}[R]$ dei buckets già pieni in $B$. Per stimare $\mathbf{Pr}[R]$ si studia quindi un processo *balls into bins*.
Per fare ciò, ci si chiede, nel caso in cui si lanciassero $m$ sfere in $n$ cesti equiprobabili, quale sia la *probabilità che un cesto ottenga almeno una sfera*.
Per il nostro problema, il modello può essere formulato come segue:
- Ai cesti corrispondo i buckets che vanno da $0$ ad $n-1$.
- Alle sfere corrispondono i valori hash casuali $h(k_1(x))$ degli $m$ elementi in $S$. Si assume quindi che $h$ mappi u.a.r. tali elementi. 
- La probabilità che quindi una sfera entri in un cesto è $\mathbf{Pr}[\text{ball-into-bin}] = 1/n$ per la definizione di $h$.
Allora la probabilità che un cesto ottenga almeno una sfera è 
$$
\mathbf{Pr}[R] = 1 - \left(1- \frac{1}{n}\right)^m = 1 - \left(\left(1- \frac{1}{n}\right)^n\right)^{\frac{n}{m}} \approx 1-e^{-m/n}
$$
usando l'approssimazione $\left( 1-\frac{1}{n} \right)^n \approx e^{-1}$.
Si osserva infine che per $m \ll n$, $\mathbf{Pr}[R] \approx m/n$.
Quindi la frazione di $1$ (ossia di cesti pieni) in $B$ è uguale alla probabilità di falsi positivi $1-e^{-m/n}$. 

Per risolvere il problema, si fa quindi riferimento ad una tecnica chiamata *Bloom filtering*
## Bloom filter
Un **bloom filter** è una struttura dati che consiste in:
1. Un array $B$ di $n$ bits, impostati inizialmente tutti a $0$.
2. Una collezione di funzioni hash $h_1,h_2,\ldots,h_k$. Ciascuna funzione hash mappa i valori *chiave* in $n$ buckets, corrispondenti agli $n$ bit dell'array.
3. Un insieme $S$ di $m$ valori chiave.
Lo scopo di un Bloom filter è quello di filtrare e quindi accettare, tra tutti gli elementi dello stream, quelli aventi chiavi in $S$, scartando la maggior parte di quelli tali per cui la chiave non risulti essere in $S$.
Il suo utilizzo avviene come segue:
- Si inizializza l'array di bit impostando tute le sue entrate a $0$.
- Si prende ogni valore chiave $s \in S$, e se ne effettua l'hash utilizzando ciascuna delle $k$ funzioni.
- Si pone $B[h_i(s)]=1$ per ogni $i=1,\ldots,k$.
Per testare un elemento con chiave $x$ al suo arrivo nel flusso, si verifica se $B[h_i(x)]=1$ per ogni $i=1,\ldots,k$. Nel caso in cui questo risulti essere vero, ossia che $x$ viene mappato in un bucket impostato ad $1$ per ogni funzione hash $h_i$, allora si dichiara che $x$ appartiene ad $S$, altrimenti lo si scarta.
### Analisi
Si osserva che
- se un valore chiave $x$ è in $S$, allora un elemento con valore $x$ nel flusso passerà sicuramente attraverso il Bloom filter.
- se un valore chiave $y$ non si trova in $S$, l'elemento avente valore $y$ *potrebbe* comunque venir selezionato.
Si vuole quindi studiare la probabilità di falsi positivi in funzione di $n$, $m$ e $k$.
Anche per questa analisi, si fa riferimento al modello *balls into bins*, e si supponga di avere $k\cdot m$ sfere ed $n$ cesti. Ogni sfera ha la stessa probabilità di finire in uno degli $n$ cesti, ossia $1/n$. Analogamente a come fatto in precedenza, ci si chiede quale sia la frazione degli elementi in $B$ impostati ad $1$. 
La probabilità che una sfera *non* venga lanciata in un cesto fissato è
$$ 1-\frac{1}{n}$$
La probabilità che nessuna delle $k\cdot m$ sfere entri in un dato cesto è dunque 
$$
\left(1- \frac{1}{n}\right)^{k\cdot m} = \left(\left(1- \frac{1}{n}\right)^n\right)^{\frac{m\cdot k}{n}} \approx e^{-\frac{m\cdot k}{n}}
$$
Usando l'approssimazione $\left( 1-\frac{1}{n} \right)^n \approx e^{-n/n} = e^{-1}$.
Questa è proprio la probabilità che un bit in $B$ rimanga a $0$.
Allora la frazione di entrate aventi valore $1$ in $B$ è:
$$
\left(1 - e^{-k\cdot m/n}\right) \approx \frac{k\cdot m}{n}
$$
Ma si hanno esattamente $k$ funzioni hash mutualmente indipendenti, e si accetta l'elemento $x$ se tutte le $k$ funzioni hash mappano $x$ in un bucket avente valore $1$.
Allora, la probabilità di errore, ossia di avere un falso positivo è proprio
$$
\left(1 - e^{-k\cdot m/n}\right)^k
$$
### Sul numero di funzioni hash $k$
Si vuole studiare ora cosa succede all'aumentare del numero di funzioni hash per questa procedura, ossia al crescere di $k$.
Il valore ottimale per $k$, cioè quello che minimizza la probabilità di falsi positivi, è
$$k = (n/m) \ln(2)$$
#### Esempio filtro mail
Sia $m=1$ miliardo e $n=8$ miliardi.
Si ha che l'errore è:
- Per $k=1$, $(1-e^{-1/8})=0.1175$.
- Per $k=2$, $(1- e^{-1/4})^2 = 0.0493$.
In generale, l'andamento della funzione che descrive la probabilità di falsi positivi all'aumentare delle funzioni hash può essere raffigurato come segue:
![center|700](BloomFilterAnalysis.png)
Allora il valore che minimizza la probabilità di falsi negativi per questo esempio è $k= 8 \ln(2) = 5.54 \approx 6$, e l'errore per $k=6$ è $(1- e^{-1/6})^2 = 0.0235$.
# Contare elementi distinti
Si studia ora il problema di contare elementi distinti all'interno di un flusso. Come nei problemi precedenti, si fa riferimento a tecniche di hashing e algoritmi probabilistici per ottenere una soluzione approssimata che rispetti i vincoli sulla quantità di memoria utilizzata, fornendo quindi un buon trade-off tra spazio utilizzato e qualità della soluzione.
## Il problema
Si supponga che gli elementi del flusso vengano da un insieme universale $U$. Si vuole sapere quanti elementi differenti sono comparsi nello stream, contando dall'inizio del flusso.
Formalmente:
- Il flusso di dati consiste in una sequenza di elementi scelti da un'insieme $U$ tale che $|U|=N$.
- Si vuole mantenere un conto $d$ del numero di elementi distinti visti all'interno del flusso, partendo dall'inizio del flusso sino all'ultimo istante, ossia sino all'ultimo elemento processato.
## Soluzione Naive
L'approccio banale alla risoluzione del problema è quello di mantenere in memoria principale un insieme $S\subseteq U$ contenente tutti gli elementi visti nel flusso fino all'istante della sua analisi.
Tale insieme può essere implementato in una struttura dati di ricerca efficiente, comenegativi una hash table o un albero di ricerca, in maniera tale che risulti possibile aggiungere velocemente nuovi elementi, e controllare se l'elemento appena arrivato nello stream sia stato già visto o meno. Fintantoché il numero di elementi distinti non risulta essere troppo grande, ossia $|S| \ll |U|$, allora questa soluzione richiede una quantità di spazio che rispetta la capacità della memoria principale. Ma, come per gli altri problemi, se il numero di elementi distinti è troppo grande, o se vi è un numero troppo elevato di flussi da processare simultaneamente, allora non risulta possibile memorizzare i dati necessari nella memoria principale.
Si osserva che l'obiettivo del problema è contare il numero $d$ di elementi distinti visti nel flusso, dunque la memorizzazione di $S$ non è propriamente l'obiettivo del problema, nonostante permetta di rispondere correttamente al quesito posto dal problema originale.

Si fa riferimento ad un algoritmo probabilistico che permette di non memorizzare l'intero insieme $S$ e che garantisce una soluzione approssimata con bassa probabilità di errore.
## Algoritmo di Flajolet-Martin
Sia $d$ il numero di elementi distinti. Si osserva che, essendo $d \leq N$, è possibile memorizzare $d$ utilizzando $O(\log N)$ bits.
Mediante **l'algoritmo di Flajolet-Martin** è possibile ridurre la complessità spaziale a $O(\log \log N)$ bits. L'idea dell'algoritmo è quella di stimare il valore $\log{d}$ invece che $d$. Algoritmi che adottano una tale strategia sono detti *algoritmi di conteggio approssimato*.
### Fase di pre-processing
L'algoritmo è preceduto da una fase di *pre-processing*, nella quale si sceglie una *funzione hash* $h$ che mappa ciascuno degli $N$ elementi di $U$ in almeno $\log_2 N$ bits.
Senza perdita di generalità, si assuma che $U = [N]$. Si definisce la funzione $h$ come segue
$$
h:[N] \longrightarrow \{0,1\}^s \ \ \textnormal{dove }s \geq \log_2 N \textnormal{ bits}
$$
Si assume che $h$ sia veramente casuale, ossia, per ogni $i \in [N]$, si ha che $h(i)$ è scelto *uniformly at random* da $\{0,1\}^s$.
Per ogni elemento $i \in U$, si definisce $r(i)$ come il numero di bit pari a $0$, partendo da destra, che precedono il primo bit impostato ad $1$ della stringa $h(i)$.
Ad esempio sia, per un dato elemento $a \in U$, $h(a) = 01100$, allora $r(a)=2$.
Si osserva inoltre che, per la quantità $r(i)$, essendo $h(i)$ scelta *uniformly at random* da $\{0,1\}^s$, si ha che
$$
\mathbf{Pr}\left[r(i) \geq l\right] = \frac{2^{s-l}}{2^s}=\frac{1}{2^l}
$$
Perché tra tutte le possibili stringhe binarie di lunghezza $s$, ve ne sono esattamente $2^{s-l}$ su un totale di $2^s$ tali per cui $r(i)\geq l$, tutte aventi la stessa possibilità di essere scelte.
### L'algoritmo
Si può ora descrivere l'algoritmo di conteggio probabilistico. Tale algoritmo mantiene un data sketch del flusso ad ogni passo, definito come segue
$$
R = \textnormal{max} \{r(i), \textnormal{per ogni elemento } i \in U \textnormal{ visti fino all'ultimo istante}\}
$$
E viene descritto mediante il seguente pseudocodice:
_____
**Algoritmo di Flajolet-Martin FM**
**Input:** Flusso di elementi $i \in U$
1. $R = 0$
2. **for** $i$ nel flusso $do$
	1. Calcola $r(i)$
	2. $R = \textnormal{max}(R,r(i))$
3. **return** $2^R$
_____
Dove il valore in output $2^R$ è la **stima del numero degli elementi distinti** all'interno dello stream già visti dal sistema.
L'algoritmo ha diverse proprietà interessanti:
- Le occorrenze ripetute dello stesso elemento non influiscono sul valore di $R$, essendo che lo stesso elemento $i$ viene mappato sempre nello stesso valore $h(i)$, ed avrà quindi sempre lo stesso $r(i)$.
- Lo sketch $R$ può essere facilmente combinato assieme ad altri: se si dispone di una collezione di sketches $R_1,R_2,\ldots,R_k$ provenienti da stream differenti, definiti tutti sullo stesso alfabeto universale, e si desidera calcolare il numero $d$ di elementi distinti del flusso definito dalla combinazione di questi $k$ flussi, si può prendere, tra questi $k$ sketches, quello avente valore massimo
$$
\textnormal{max}\{R_1,R_2,\ldots,R_k\}
$$
### Analisi dell'algoritmo
Si vuole dimostrare ora la correttezza dell'algoritmo. Si ricorda che $\mathbf{Pr}\left[r(i) \geq l\right] = 1/2^l$. Sia $X_l$ la variabile aleatoria definita come segue:
$$
X_l= 
  \left\{ 
    \begin{aligned}
      &\ 1 \ \ \ \ \textnormal{se }\exists i \textnormal{ nello stream tale per cui }r(i)\geq l\\
      &\ 0 \ \ \  \textnormal{ altrimenti}  
    \end{aligned}
  \right.
$$

Allora si ha che 
$$
\begin{align*}
\mathbf{Pr}\left[X_l=1\right] &= \mathbf{Pr}\left[ \bigcup_{i}\left( r(i) \geq l\right) \right] \\
\\
&= 1 - \mathbf{Pr}\left[\bigcap_{i}\left(r(i)<l\right)\right] \\
\\
&= 1 - \left(1-2^{-l}\right)^{d}
\end{align*}
$$
Essendovi $d$ elementi distinti nel flusso, ed essendo $(r(i)<l)$ un evento indipendente per ogni $i\in U$, dato che $h(i)$ è scelta in modo indipendente per ogni $i$.
Allora si ha che 
$$
\mathbf{Pr}\left[X_t=0\right] = \left(1-2^{-l}\right)^{d}
$$
Sia $m=2^R$ l'output dell'algoritmo e siano $d$ gli elementi distinti nel flusso. Si vuole calcolare la probabilità di errore dell'algoritmo. 
Si calcola inizialmente la probabilità che $R > \lceil\log d\rceil+c$ per ogni $c>0$    
$$
\begin{align*}
\mathbf{Pr}\left[m > 2^c \cdot d\right] &= \mathbf{Pr}\left[R>\lceil\log d\rceil+c\right] = \mathbf{Pr}\left[X_{\lceil\log d\rceil+c} = 1\right] \\
\\
&= 1 - \left(1- \frac{1}{2^{\lceil\log d\rceil+c}}\right)^d \leq \frac{d}{2^{\lceil\log d\rceil+c}} \leq 2^{-c}
\end{align*}
$$
Dove, per effettuare la maggiorazione, si è fatto riferimento alla disuguaglianza $(1-x)^d \geq 1-xd$, vera per $|xd|<1$.

Si calcola ora la probabilità  che $R < \lceil\log d\rceil-c$ per ogni $c>0$
$$
\begin{align*}
\mathbf{Pr}\left[R<\lceil\log d\rceil-c\right] &= \mathbf{Pr}\left[X_{\lceil\log d\rceil-c} = 0\right] \\
\\
&= \left(1- \frac{1}{2^{\lceil\log d\rceil-c}}\right)^d \\
&= \left(\left(1- \frac{1}{2^{\lceil\log d\rceil-c}}\right)^{\frac{2^{\lceil \log{d} \rceil}}{2^c}}\right)^{\frac{d \cdot 2^c}{2^{\lceil \log{d} \rceil}}} \\
&\leq \textnormal{exp}\left( -\frac{d \cdot 2^c}{2^{\lceil\log d\rceil}}\right) \leq \textnormal{exp}(-2^{c-1})
\end{align*} 
$$
Dove, per effettuare la maggiorazione, si è fatto riferimento alla disuguaglianza $\left(1-\frac{1}{x}\right)^x \leq e^{-1} = \textnormal{exp}(-1)$.

Queste disuguaglianze forniscono forti bounds esponenziali sulla probabilità che si ottenga una stima che si trova entro un errore additivo $c$ dal risultato reale. In particolar modo, per $c=2$ si ha che 
$$
\mathbf{Pr}\left[X_{\lceil\log d\rceil+c} = 1\right] = \mathbf{Pr}\left[R > \lceil \log d\rceil +2\right] \leq 2^{-2} = \frac{1}{4}
$$
$$
\mathbf{Pr}\left[X_{\lceil\log d\rceil-c} = 0\right] = \mathbf{Pr}\left[R < \lceil \log d\rceil -2\right] \leq \textnormal{exp}(-2) \leq \frac{1}{8}
$$
per l'union bound, la probabilità che $-2 \leq R-\lceil \log{d} \rceil \leq 2$ vale
$$
\mathbf{Pr}\left[ |R - \lceil \log d\rceil\right| \leq 2] \geq 1 - \left( \frac{1}{4} + \frac{1}{8}\right) = \frac{5}{8} \approx \frac{2}{3}
$$
In altre parole, si ha che 
$$
\frac{1}{4} \cdot 2^{\lceil \log d\rceil} \leq 2^R \leq 4 \cdot 2^{\lceil \log d\rceil}
$$
Ossia una $8-$approssimazione a $d$ con probabilità almeno $\frac{5}{8} \approx \frac{2}{3}$. Si può poi usare l'algoritmo median per aumentare la probabilità di successo.
### Analisi spaziale
Lo spazio richiesto per l'algoritmo di Flajolet-Martin è semplicemente lo spazio necessario per memorizzare $R$, ed essendo che $R \leq \lceil \log d\rceil + c$ con alta probabilità, la memoria per $R$ sarà
$$
\lceil \log R\rceil = \log \log d + \mathcal{O}(1) \leq \log \log n
$$
e quindi $\textbf{Space}[\textnormal{FM algorithm}] = \mathcal{O}(\log \log d)$.
### Output atteso
Ci si chiede ora quale sia l'output atteso dell'algoritmo, ossia $\mathbf{E}[2^R]$. Flajolet e Martin hanno mostrato che $\mathbf{E}[2^R]$ tende rapidamente a $d$, scalato per una specifica costante:
$$
\mathbf{E}[2^R] \approx 0.77551d
$$
Questo rende l'algoritmo di conteggio probabilistico pratico: si calcola semplicemente $\mathbf{E}[2^R]$, e lo si divide per $0.77551$. 

Infine, è stata mostrata una rifinitura dell'algoritmo di Flajolet Martin che garantisce un algoritmo probabilistico $(1-\varepsilon)-$approssimante per $d$, che utilizza spazio par a $\mathcal{O}(\varepsilon^{-2} \cdot \log(1/\varepsilon)) + 2 \log \log n$.
# Calcolo dei momenti
Si studia una generalizzazione del problema del conteggio di elementi distinti all'interno di uno stream. Questo problema riguarda la distribuzione delle *frequenze* di elementi distinti all'interno di un flusso.
Si supponga che un flusso $I$ consista di elementi scelti da un'insieme universale $U=\{1,\dots,N\}.$ Si assuma che l'insieme universale sia totalmente ordinato. Sia $m_i$ la frequenza dell'elemento $i$, ossia il numero delle occorrenze di tale elemento nel flusso $I$, per ogni $i \in U$. Si definisce il $k-$momento del flusso come la somma su tutti gli $i$ di $(m_i)^k$:
$$
\sum_{i \in U} (m_i)^k
$$
Si fa presente per convenzione che, se $m_i=0$ per qualche elemento nell'insieme universale, allora si considera $(m_i)^0 = 0^0 = 0$.
Si guardano ora dei casi particolari di $k-$momento:
- $k=0:$ Il $0-$momento è dato dalla somma di $1$ per ogni $m_i >0$. Ogni elemento $i$ con $m_i = 0$ non contribuisce alla sommatoria, mentre ogni elemento $i$ con $m_i \geq 1$ contribuisce alla sommatoria con una quantità pari ad $1.$ Si osserva che questo caso particolare corrisponde al problema del conteggio di elementi distinti.
- $k=1:$ L' $1-$momento è la somma di tutti gli $m_i$, ossia, si conta ogni elemento nel flusso ogni volta che questo appare. Risulta quindi essere proprio la lunghezza del flusso. Tale caso è semplice da calcolare, basta contare la lunghezza del flusso visto sino all'ultimo elemento arrivato al sistema.
- $k=2:$ È la somma dei quadrati degli $m_i$ per ogni $i\in U$. Viene chiamato *surprise number*, essendo che misura quanto è irregolare la distribuzione degli elementi nel flusso. Maggiore è questo valore, più irregolare risulta essere la distribuzione degli elementi.

Come negli altri problemi visti, non ci sono difficoltà nel calcolo dei momenti di qualsiasi ordine nel caso in cui fosse possibile mantenere in memoria un contatore per ciascun elemento che appare nel flusso. Si prende in analisi il caso in cui non fosse possibile utilizzare tale quantità di memoria. Bisogna quindi stimare il $k-$momento mantenendo un limitato numero di valori in memoria, e calcolare una stima partendo da essi.
## Algoritmo di Alon-Matias-Szegedy
L'algoritmo **AMS** risolve il problema per tutti i momenti, dandone una stima *unbiased*. Il *bias* di un estimatore è dato dalla differenza tra il suo valore atteso, e il *vero* valore del parametro stimato. Un'estimatore si dice *unbiased* se il suo *bias* è uguale a $0$ per tutti i valori del parametro preso in analisi.
### Stima dei $2-$Momenti
Si assuma che un flusso abbia una certa lunghezza $n$ fissata. Si vedrà successivamente come affrontare il problema per flussi che crescono nel tempo.
Si supponga che non si abbia sufficiente spazio per contare tutti gli $m_i$ per tutti gli elementi del flusso. Si può stimare il $2-$momento del flusso usando una quantità di memoria limitata. Come per altri problemi studiati, maggiore è lo spazio usato, migliore sarà la stima di tale quantità. 
Per questo algoritmo, si sceglie e si aggiorna un campione di variabili aleatorie indipendenti ed identicamente distribuite $\mathcal{X} =\{X_j:j=1,\ldots,k\}$ tali per cui, per ogni $X \in \mathcal{X}$ si mantiene in memoria:
1. Un particolare elemento dell'insieme universale, al quale ci si riferisce come $X.el.$
2. Un intero $X.val$, corrispondente al valore della variabile. Per determinare il valore di una variabile $X$, si sceglie una posizione nel flusso tra $1$ ed $n$ u.a.r., e si imposta $X.el$ come l'elemento $i$ presente in tale posizione, e si inizializza il valore $X.val$ ad $1$. Man mano che il flusso viene letto, si aggiunge $1$ ad $X.val$ ogni volta che si incontra un'altra occorrenza di $X.el$. In altre parole, $X.val$ corrisponde al conteggio delle future occorrenze di $i=X.el.$
Si osserva che il conteggio avviene in memoria principale, e quindi il numero $k$ di variabili aleatorie deve essere limitato.
A partire da tale procedura, si può stimare il $2-$momento da ogni variabile $X$ come il valore 
$$
n(2X.val -1)
$$
Schematicamente, l'algoritmo si può rappresentare come segue, e viene applicato per un $X \in \mathcal{X}$:
___
**Algoritmo AMS**
**Input:** Flusso $I=I[1\ldots n]$ di lunghezza $n$.
- Si sceglie un istante di tempo $t$ (equivalentemente, una posizione) u.a.r. tale per cui $1 \leq t < n.$
- Si pone $X.el := i$, dove $i=I[t]$ è l'$i$-esimo elemento in $I$ 
- Si calcola il contatore $X.val=c$ come il numero di occorrenze di $i$ nel sotto-stream $I[t,\ldots,n].$ 
- **Output:** La stima per il $2-$momento $\sum_{i}m_i^2$ è $$f(X) = n(2c -1)$$
___
Tale algoritmo viene calcolato per ogni $X\in \mathcal{X}$, e la stima finale viene data dalla media dei relativi output 
$$
S = \frac{1}{k} \sum_{j=1}^k f(X_j)
$$
aumentando così la confidenza.
### Analisi
Per effettuare l'analisi dell'algoritmo, si mostra che il valore atteso di una qualsiasi variabile aleatoria costruita come descritto è il $2-$momento del flusso dal quale viene costruita.

Sia:
- $e(i)$: l'elemento dello stream che appare in posizione $i$ all'interno di tale flusso;
- $c(i)$: il numero di volte che l'elemento $e(i)$ appare nel flusso nelle posizioni che vanno da $i$ ad $n$ $(i,i+1,\ldots,n)$.
Il valore atteso di $n(2X.val - 1)$ è la media su tutte le posizioni $i$ tra $1$ ed $n$ di $n(2c(i)-1)$, ossia
$$
\mathbf{E}\left[n(2X.val-1)\right]\ =\ \frac{1}{n} \sum_{i=1}^{n} n(2c(i)-1)\ =\ \sum_{i=1}^{n} (2c(i)-1)
$$
Per dare un senso alla formula, bisogna cambiare l'ordine della sommatoria raggruppando tutte quelle posizioni aventi lo stesso elemento. Per ogni $a \in U$ fissato che appare $m_a$ volte nel flusso, si ha che il termine per l'ultima posizione nella quale $a$ appare deve essere $2 \times 1-1 = 1.$ Il termine per la penultima posizione nella quale $a$ appare è $2 \times 2 -1 =3$. Proseguendo in questa maniera, si ha che il termine per la prima posizione in cui $a$ appare è $2m_a -1$, perché in tale sotto-flusso, che va dalla prima posizione in cui $a$ appare sino all'ultima, $a$ appare esattamente $m_a$ volte. Si ha quindi che la formula per il valore atteso $2X.val -1$ può essere scritta come:
$$
\mathbf{E}\left[n(2X.val-1)\right]\ = \sum_{a}\sum_{i=1}^{m_a}(2i-1)
$$
ed essendo che $\sum_{i=1}^{m_a}(2i-1) = (m_a)^2$, si ha che
$$
\mathbf{E}\left[n(2X.val-1)\right] = \sum_{a}(m_a)^2
$$
che è proprio la definizione di $2-$momento. 
## Momenti di alto ordine
La stima per il $k-$momento, per $k >2$, si calcola eseguendo lo stesso algoritmo per il $2-$momento, cambiando però la maniera in cui si deriva la stima a partire da una variabile.
Per $k=2$ si è utilizzata la formula $n(2v-1)$ per rendere il valore $m_{a}$, dove tale valore è il conteggio del numero di occorrenze di un particolare elemento del flusso $a$, una stima del $2-$momento. Si è mostrato inoltre il motivo per cui questa formula funzione: la somma dei termini $2v-1$, per $v=1,\dots,m$ è uguale ad $m^2$, dove $m$ è il numero di volte in cui $a$ appare nel flusso. 
Si osserva che
$$2v-1 = v^2 - (v-1)^2$$
dunque se si vuole calcolare il $3-$momento, si sostituisce $2v-1$ con
$$
v^3-(v-1)^3=3v^2-3v+1
$$
e si ottiene $\sum_{v=1}^m3v^2-3v+1=m^3$.
Dunque si può usare come stima del $3-$momento il valore
$$
n(3v^2-3v+1)
$$
dove $v=X.val$ è il valore associato ad una variabile $X$.
In generale, si può stimare il $k-$momento per ogni $k \geq 2$ utilizzando un valore $v=X.val$ per la formula
$$
n(v^k-(v-1)^k)
$$
### Combinare i samples
Nella pratica, per aumentare la confidenza, si calcola $f(X)=n(v^k-(v-1)^k)$ per tante variabili aleatorie mutualmente indipendenti $X$ quante entrano in memoria. Si dividono poi le variabili aleatorie in sottogruppi, e si calcola la media di $f(X_j)$ su ogni $X_j$ appartenente al sottogruppo, per ogni sottogruppo. Successivamente, si prende l'elemento mediano delle medie di ogni sottogruppo.
## Flussi infiniti
Per il problema studiato, si è ottenuta una stima sotto l'ipotesi che la lunghezza del flusso $n$ sia costante. Nella pratica, $n$ cresce nel tempo, essendo la variabile che rappresenta il numero di input visti sino ad un certo istante. 
Questo fatto in se stesso non causa problemi, essendo che vengono memorizzati solo i valori di alcune variabili e si moltiplica una funzione definita su tali valori per $n$ nell'istante in cui si vuole stimare il momento.
Se si mantiene separatamente il conto del numero di elementi visti nel flusso, allora si può ottenere il valore $n$ in ogni istante in cui risulti necessario, utilizzando solamente $\log{n}$ di memoria in più.
Il problema risiede nella *selezione delle posizioni* per le variabili. Se tale selezione avviene una volta e si fissa, con la crescita del flusso, il conteggio effettuato sarà distorto (*biased*) a favore delle posizioni meno recenti, e la stima del momento risulterà essere troppo grande.
D'altra parte, se si aspetta troppo tempo per scegliere le posizioni, allora nei primi istanti di analisi del flusso non si avranno molte variabili, e si otterrebbe quindi una stima non affidabile.
La tecnica che si utilizza per ovviare a questi problemi consiste nel mantenere tante variabili quanto sia possibile memorizzarne in ogni istante, ed eliminarne alcune man mano che il flusso cresce. Le variabili scartate vengono sostituite da delle nuove, in maniera tale che, in ogni istante, la probabilità di prendere una qualsiasi posizione per una variabile è uguale alla probabilità, per una variabile già presente in memoria, di venir scartata. 
L'obiettivo è quindi che ogni istante iniziale $t$ venga selezionato con probabilità $k/n$.
Tale approccio consiste proprio nel *fixed-size sampling*, ossia il campionamento di un sottoinsieme di dimensione fissata di elementi di un flusso che cresce all'infinito.
Si utilizza quindi il **Reservoir-Algorithm**:
- Si scelgono i primi $k$ elementi per $k$ variabili.
- Quando l'$n-$esimo elemento arriva (con $n>k$), lo si inserisce nel campione con probabilità $k/n$.
- Se tale elemento è stato scelto, si elimina una delle variabili $X$ memorizzate in precedenza, scegliendola u.a.r.