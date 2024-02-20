# Richiami di probabilità e statistica
## Assiomi di probabilità
Uno ***spazio di campionamento*** $\Omega$ è un insieme i quali elementi descrivono gli esiti di un esperimento di interesse.

I sottoinsiemi di uno spazio di campionamento prendono il nome di ***eventi***. Si dice che un evento $A$ accade se l'esito di un esperimento è un elemento di $A$.

Gli eventi possono essere combinati secondo le operazioni insiemistiche.
Sia $\Omega$ uno spazio di campionamento e siano $E_1 \subseteq \Omega,E_2 \subseteq \Omega$ due eventi. 
L'evento $E_1 \cap E_2$ prende il nome di *intersezione* dei due eventi, ed accade se si verificano entrambi gli eventi $E_1$ ed $E_2$.
L'insieme $E_1 \cup E_2$  prende il nome di *unione* dei due eventi, ed accade se si verifica almeno uno dei due eventi $E_1$ ed $E_2$.
L'insieme $E^c=\{\omega \in \Omega \ : \ \omega \notin E\}$ è il *complemento* dell'evento $E$, ed accade se e solo se l'evento $E$ non si verifica. Si ha che $\Omega^c = \{ \emptyset\}$.

I due eventi $E_1$ ed $E_2$ si dicono *disgiunti* o *mutualmente esclusivi* se non hanno esiti in comune, ossia $E_1 \cap E_2  =  \emptyset$. 
Si dice che l'evento $E_1$ *implica* l'evento $E_2$ se l'esito di $E_1$ è contenuto in $E_2$, ossia $E_1 \subset E_2$.

Essendo insiemi, gli eventi sono soggetti alle proprietà insiemistiche.
```ad-Proprieta
title: Leggi di DeMorgan

Per ogni coppia di eventi $E_1$ ed $E_2$ si ha che
$$
(E_1 \cup E_2)^c = E_1^c \cap E_2^c \ \ \ \textnormal{e} \ \ \ (E_1 \cap E_2)^c = E_1^c \cup E_2^c 
$$
```
Si vuole esprimere quanto sia probabile il verificarsi di un evento. Per fare ciò, si definisce una ***funzione di probabilità***. In questo corso si farà sempre riferimento a spazi di probabilità *discreti*, ossia dove lo spazio di campionamento $\Omega$ risulta essere finito o numerabile.
```ad-Definizione
title: Definizione (Funzione di probabilità discreta):
Una *funzione di probabilità (discreta)* è una funzione $\mathbf{Pr}:\Omega \rightarrow [0,1]$ che soddisfa le seguenti condizioni:
1. $\mathbf{Pr}(\Omega)=1$,
2. Per ogni sequenza finita o numerabile di eventi mutualmente disgiunti due a due $E_1,E_2,E_3, . . .$ si ha che
$$
\mathbf{Pr}\Bigl( \bigcup_{i\geq 1} E_i \Bigl) = \sum_{i\geq 1}\mathbf{Pr}(E_i)
$$
```
```ad-Osservazione
Siano $E_1$ ed $E_2$ due eventi disgiunti, allora
$$
E_1 \cap E_2 = \emptyset\ \Rightarrow\ \mathbf{Pr}(E_1 \cap E_2) = 0
$$
```
Siano $E_1$ ed $E_2$ due eventi *non* disgiunti. 
Si ha che $$E_1 = (E_1 \cap E_2) \cup (E_1 \cap E_2^c)$$
A partire da ciò, si ottiene quindi la seguente uguaglianza:
$$
\mathbf{Pr}(E_1) \ = \ \mathbf{Pr}(E_1 \cap E_2)\ +\ \mathbf{Pr}(E_1 \cap E_2^c)
\tag{2}
$$
Applicando lo stesso ragionamento per l'evento $E_1 \cup E_2$ si ha che
$$
(E_1 \cup E_2) = ((E_1\cup E_2) \cap E_2)\ \cup\  ((E_1\cup E_2) \cap E_2^c)
$$

Osservando che  
$$
(E_1\cup E_2) \cap E_2\ = E_2
$$
$$
(E_1\cup E_2) \cap E_2^c\ =\ E_1 \cap E_2^c
$$
Vale quindi
$$
\mathbf{Pr}(E_1 \cup E_2) = \mathbf{Pr}(E_2) \ +\ \mathbf{Pr}(E_1 \cap E_2^c)
$$

E, ponendo $\mathbf{Pr}(E_1 \cap E_2^c)\ = \mathbf{Pr}(E_1) \ - \ \mathbf{Pr}(E_1 \cap E_2)$ dalla ($2$) si ottiene il seguente lemma
```ad-Lemma
Siano $E_1$ ed $E_2$ due eventi, allora
$$
\mathbf{Pr}(E_1 \cup E_2) = \mathbf{Pr}(E_1) +\mathbf{Pr}(E_2) \ - \ \mathbf{Pr}(E_1 \cap E_2)
$$
```
```ad-Osservazione
Siano $E_1$ ed $E_2$ due eventi disgiunti, allora
$$
\mathbf{Pr}(E_1 \cup E_2) = \mathbf{Pr}(E_1) +\mathbf{Pr}(E_2) 
$$
essendo $\mathbf{Pr}(E_1 \cap E_2) = 0$.
```
Il lemma appena enunciato può essere generalizzato a più eventi:
```ad-Lemma
title: Lemma (Principio di inclusione ed esclusione):
Sia $E_1,...,E_n$ una sequenza di $n$ eventi. allora
$$
\mathbf{Pr}\Bigl( \bigcup_{i \geq 1}^n E_i \Bigl) = \sum_{i=1}^{n} \mathbf{Pr}(E_i) - \sum_{i<j} \mathbf{Pr}(E_i \cap E_j) \ + \sum_{i<j<k} \mathbf{Pr}(E_i \cap E_j \cap E_k) \ - \cdots $$
 $$ \cdots + (-1)^{l+1} \sum_{i_1 <i_2 <...<i_l}\mathbf{Pr}\Bigl( \bigcap_{r=1}^{l} {E_i}_r \Bigl) + \cdots \ .
$$
```
Inoltre, essendo che, dato un evento $E \subseteq \Omega$ vale $E \cup E^c = \Omega$, si ha che
$$
\mathbf{Pr}(E) = 1\ -\ \mathbf{Pr}(E^c) 
$$
Una conseguenza della definizione di funzione di probabilità discreta prende il nome di ***union bound***:
```ad-Lemma
title: Lemma (Union bound):
Per ogni sequenza finita o numerabile di eventi $E_1,E_2,E_3,...$
$$
\mathbf{Pr}\Bigl( \bigcup_{i\geq 1} E_i \Bigl) \leq \sum_{i\geq 1}\mathbf{Pr}(E_i)
$$

```
Si osserva esplicitamente come questo lemma differisca dalla seconda condizione della definizione di funzione di probabilità discreta, la quale richiede che gli eventi siano disgiunti due a due.

A partire dai concetti appena enunciati, si può definire lo ***spazio di probabilità***:
```ad-Definizione
title: Definizione (Spazio di probabilità):
Uno spazio di probabilità ha tre componenti:
1. Uno spazio di campionamento $\Omega$, ossia l'insieme di tutti i possibili esiti del processo aleatorio modellato dallo spazio di probabilità.
2. Una famiglia di insiemi $\mathcal{F}$ che rappresenta tutti i possibili eventi, dove per ogni insieme $E\in \mathcal{F}$, detto *evento* si ha $E \subseteq \Omega$.
3. Una funzione di probabilità $\mathbf{Pr} : \mathcal{F} \rightarrow \mathbb{R}$. 

```
Un elemento di $\Omega$ è chiamato *evento semplice*. In uno spazio di probabilità discreto, la funzione di probabilità è definita unicamente dalle probabilità di eventi semplici, quindi $\mathbf{Pr} : \Omega \rightarrow [0,1]$.  
## Probabilità condizionata ed indipendenza
L'occorrenza di un evento può andare a modificare la probabilità che si verifichi un evento successivo correlato ad esso. Tale probabilità prende il nome di ***probabilità condizionata***.
```ad-Definizione
title: Definizione (Probabilità condizionata):
Siano $E_1$ ed $E_2$ due eventi.
La probabilità condizionata di $E_1$ dato $E_2$ è
$$
\mathbf{Pr}(E_1 | E_2) = \frac{\mathbf{Pr}(E_1 \cap E_2)}{\mathbf{Pr}(E_2)}
$$
assumendo che $\mathbf{Pr}(E_2) > 0$
```
Da questa definizione, moltiplicando entrambi i membri dell'equazione per $\mathbf{Pr}(E_2)$ si ottiene la ***regola della moltiplicazione***.
```ad-Proprieta
title: Regola della moltiplicazione
Per ogni coppia di eventi $E_1$ ed $E_2$ si ha
$$
\mathbf{Pr}(E_1 \cap E_2) = \mathbf{Pr}(E_1|E_2)\mathbf{Pr}(E_2)
$$
```
Da ciò segue nell'immediato che 
$$
\mathbf{Pr}(E_1|E_2) = \frac{\mathbf{Pr}(E_1 \cap E_2)}{\mathbf{Pr}(E_2)}
$$
Si fa presente come il calcolo di $\mathbf{Pr}(E_1 \cap E_2)$ sia eseguibile sia condizionando la probabilità di $E_1$ su $E_2$, avendo quindi 
$$
\mathbf{Pr}(E_1 \cap E_2) = \mathbf{Pr}(E_1|E_2)\mathbf{Pr}(E_2)
$$
oppure condizionando la probabilità $E_2$ rispetto a $E_1$
$$
\mathbf{Pr}(E_1 \cap E_2) = \mathbf{Pr}(E_2|E_1)\mathbf{Pr}(E_1)
$$
Nella maggior parte dei casi, si ha che una delle due tra $\mathbf{Pr}(E_1|E_2)$ e $\mathbf{Pr}(E_2|E_1)$ risulta facile da calcolare, al contrario dell'altra. 

E' possibile calcolare la probabilità di un evento mediante le probabilità condizionate di una serie di eventi disgiunti, i quali presi complessivamente vanno a formare lo spazio di campionamento. 
```ad-Teorema
title: Teorema (Teorema della probabilità totale)
Siano $E_1,E_2,...,E_m$ eventi mutualmente disgiunti tali per cui $E_1 \cup E_2 \cup \ ... \ E_m = \Omega$. La probabilità di un evento arbitrario $E \subseteq \Omega$ può essere espressa come
$$
\begin{align}
\mathbf{Pr}(E)\ \ =& \ \ \sum_{i=1}^m \mathbf{Pr}(E \cap E_i)\ \  \\ 
=&\ \ \ \mathbf{Pr}(E|E_1)\mathbf{Pr}(E_1) + \cdots + \mathbf{Pr}(E|E_m)\mathbf{Pr}(E_m)   \\
=&\ \sum_{i=1}^m\mathbf{Pr}(E|E_i)\mathbf{Pr}(E_i).
\end{align}
$$

```
```ad-Dimostrazione
title: Dimostrazione (Teorema della probabilità totale)
Essendo gli eventi $E_i$ per $i=1,...,m$ disgiunti e tali per cui $\bigcup_{i=1}^m E_i = \Omega$, si ha che
$$
\mathbf{Pr}(E) = \sum_{i=1}^m \mathbf{Pr}(E \cap E_i)
$$
Dalla definizione di probabilità condizionata risulta quindi
$$
\sum_{i=1}^m\mathbf{Pr}(E \cap E_i) = \sum_{i=1}^m\mathbf{Pr}(E|E_i)\mathbf{Pr}(E_i)
$$
```
```ad-Osservazione
Essendo $E_1,E_2,...,E_m$ eventi mutualmente disgiunti 
($\forall i,j=1,...,m$ tali per cui $i\neq j$ vale $E_i \cap E_j = \emptyset$) e $E_1 \cup E_2 \cup \ ... \ E_m = \Omega$, si ha che tale sequenza di eventi definisce una partizione dello spazio di campionamento.
```
Si vede ora un teorema che mette in relazione le probabilità condizionate, nota come **teorema di Bayes**.
```ad-Teorema
title: Teorema di Bayes:
Siano $E_1,E_2,...,E_m$ eventi disgiunti tali per cui $E_1 \cap E_2 \cap ... \cap E_m = \Omega$. La probabilità condizionata di $E_i$ dato un evento arbitrario $E$ può essere espressa come: 
$$
\mathbf{Pr}(E_i|E) = \frac{\mathbf{Pr}(E|E_i)\mathbf{Pr}(E_i)}{\mathbf{Pr}(E|E_1)\mathbf{Pr}(E_1)+\mathbf{Pr}(E|E_2)\mathbf{Pr}(E_2) + \cdots + \mathbf{Pr}(E|E_m)\mathbf{Pr}(E_m)}
$$
```
Tale teorema segue dalla relazione:

$$
\mathbf{Pr}(E_i|E) = \frac{\mathbf{Pr}(E|E_i)\mathbf{Pr}(E_i)}{\mathbf{Pr}(E)}
$$

Combinata con il Teorema della probabilità totale su $\mathbf{Pr}(E)$.

Eventi di uno stesso spazio di campionamento possono non essere correlati tra loro. In questo caso si parla di *indipendenza* tra eventi.
```ad-Definizione
title: Definizione (Eventi indipendenti):
Un evento $E_1$ si dice indipendente da $E_2$ se
$$
\mathbf{Pr}(E_1|E_2) = \mathbf{Pr}(E_1).
$$
```
A partire dalla definizione di eventi indipendenti si possono effettuare diverse osservazioni.

Essendo $$\mathbf{Pr}(E_1^c|E_2)=1-\mathbf{Pr}(E_1|E_2)$$ $$\mathbf{Pr}(E_1^c) = 1-\mathbf{Pr}(E_1)$$  si ha che 
$$
E_1 \textnormal{ è indipendente da } E_2 \iff E_1^c \textnormal{ è indipendente da } E_2
$$
Applicando la regola della moltiplicazione, se $E_1$ è indipendente da $E_2$ allora
$$
\mathbf{Pr}(E_1 \cap E_2) = \mathbf{Pr}(E_1|E_2)\mathbf{Pr}(E_2) =\mathbf{Pr}(E_1)\mathbf{Pr}(E_2)
$$
che mostra la seguente relazione
$$
E_1 \textnormal{ è indipendente da } E_2 \iff \mathbf{Pr}(E_1 \cap E_2) = \mathbf{Pr}(E_1)\mathbf{Pr}(E_2)
$$
A partire da quest'ultima osservazione, si fornisce una definizione alternativa di indipendenza, estendendola ad un numero di eventi maggiore di $2$.
```ad-Definizione
title: Definizione (Eventi indipendenti):
Due eventi $E_1$ ed $E_2$ sono indipendenti se e solo se 
$$
\mathbf{Pr}(E_1 \cap E_2) = \mathbf{Pr}(E_1)\cdot \mathbf{Pr}(E_2)
$$
Più generalmente, gli eventi $E_1,E_2,...,E_k$ sono mutualmente indipendenti se e solo se, per un qualsiasi sottoinsieme $\mathbf{I} \subseteq [1,k]$,
$$
\mathbf{Pr}\Bigl( \bigcap_{i\in \mathbf{I}} E_i  \Bigl) = \prod_{i\in \mathbf{I}}\mathbf{Pr}(E_i)
$$
```
In conclusione, dalla definizione di probabilità condizionata, se $E_1$ è indipendente da $E_2$, allora
$$
\mathbf{Pr}(E_2|E_1) = \frac{\mathbf{Pr}(E_1 \cap E_2)}{\mathbf{Pr}(E_1)} = \frac{\mathbf{Pr}(E_1)\mathbf{Pr}(E_2)}{\mathbf{Pr}(E_1)} = \mathbf{Pr}(E_2)
$$
ossia 
$$
E_1 \textnormal{ è indipendente da } E_2 \iff E_2 \textnormal{ è indipendente da } E_1
$$
Alla luce di quanto appena enunciato, si ha il seguente lemma
```ad-Lemma
title: Lemma (Indipendenza):
Per mostrare che due eventi $E_1$ ed $E_2$ sono indipendenti, basta dimostrare una delle seguenti uguaglianze:
$$
\mathbf{Pr}(E_1|E_2) = \mathbf{Pr}(E_1)
$$
$$
\mathbf{Pr}(E_2|E_1) = \mathbf{Pr}(E_2)
$$
$$
\mathbf{Pr}(E_1 \cap E_2) = \mathbf{Pr}(E_1)\mathbf{Pr}(E_2)
$$
dove $E_1$ può essere sostituito con $E_1^c$ ed $E_2$ con $E_2^c$ (o entrambi). Se una di queste uguaglianze risulta essere vera, allora sono vere tutte le restanti. Se due eventi non sono indipendenti, allora sono chiamati *dipendenti*.
```
La definizione di eventi indipendenti può essere estesa a più eventi.
```ad-Teorema
title: Teorema (Indipendenza di due o più eventi):
Gli eventi $E_1,E_2,...,E_m$ sono detti indipendenti se
$$
\mathbf{Pr}(E_1\cap E_2\cap \cdots \cap E_m) = \mathbf{Pr}(E_1)\mathbf{Pr}(E_2)\cdots \mathbf{Pr}(E_m)
$$

Ciò risulta vero anche sostituendo un numero arbitrario di eventi $E_1,...,E_m$ con il rispettivo complemento all'intero della formula.
```
## Variabili aleatorie discrete
Quando si studia un evento probabilistico, si è spesso interessati a dei valori associati agli eventi, piuttosto che agli eventi stessi. Una qualsiasi funzione dallo spazio di campionamento ai numeri reali prende il nome di ***variabile aleatoria***.
```ad-Definizione
title: Definizione (Variabile aleatoria):
Una variabile aleatoria $X$ su uno spazio di campionamento $\Omega$ è una funzione su $\Omega$ a valori reali (e quindi misurabile), ossia $X:\Omega \rightarrow \mathbb{R}$. Una variabile aleatoria discreta è una variabile aleatoria che assume solo un numero finito o numerabile di valori.
```
Per una variabile aleatoria discreta $X$ e un valore reale $a$, l'evento "$X=a$" include tutti gli eventi di $\Omega$ in cui $X$ assume il valore $a$. "$X=a$" rappresenta quindi l'insieme
$$
\{\ s\in \Omega\ |\ X(s) = a\ \}
$$
Si definisce la probabilità di tale evento come:
$$
\mathbf{Pr}(X=a) = \sum_{s\in\Omega:X(s)=a}\mathbf{Pr}(s)
$$
E' possibile estendere la definizione di indipendenza tra eventi alle variabili aleatorie.
```ad-Definizione
title: Definizione (Variabili aleatorie indipendenti)
Due v.a. $X$ ed $Y$ sono indipendenti se e solo se 
$$
\mathbf{Pr}(\ (X=x)\cap(Y=y)\ ) = \mathbf{Pr}(X=x)\cdot\mathbf{Pr}(Y=y)
$$

Analogamente, le variabili aleatorie $X_1,X_2,...,X_k$ sono mutualmente indipendenti se e solo se, $\forall \mathbf{I}\subseteq [1,k]$ e $\forall x_i$ con $i\in \mathbf{I}$ 

$$
\mathbf{Pr}\Bigl( \bigcap_{i\in\mathbf{I}}(X_i = x_i) \Bigl)\ =\ \prod_{i\in \mathbf{I}}\mathbf{Pr}(X_i = x_i)
$$
```
```ad-Definizione
title: Definizione ($k$-wise indipendenza)
Sia $W=\{X_1,...,X_n\}$ un insieme di $n$ variabili aleatorie. Si dice che tale insieme è $k$-wise indipendente, per $k\leq n$, se e solo se 
$$
\mathbf{Pr}\Bigl( \bigwedge\limits_{j=1}^k X_{i_j} = x_{i_j} \Bigl)\ \ \ =\ \ \ \prod_{j=1}^k \mathbf{Pr} \Bigl( X_{i_j} = x_{i_j} \Bigl) 
$$
per ogni sottoinsieme $\{X_{i_1},...,X_{i_k}\}\subseteq W$ di $k$ variabili aleatorie. Per $k=n$, si dice che tale insieme $W$ contiene variabili aleatorie completamente indipendenti.
```
Una caratteristica di base di una variabile aleatoria è il suo ***valore atteso*** (o media). La media di una variabile aleatoria è la media pesata dei valori che assume, dove ogni valore è pesato dalla probabilità che la variabile aleatoria assuma tale valore.
```ad-Definizione
title: Definizione (Valore medio)
Il valore atteso di una variabile aleatoria discreta $X$, denotato $\mathbf{E}[X]$, è dato da
$$
\mathbf{E}[X] \ =\ \sum_{i}\ i\ \mathbf{Pr}(X=i)
$$
dove la sommatoria è definita su tutti i valori nel range di $X$, ossia i valori che questa v.a. può assumere. 
```
Una proprietà fondamentale del valore atteso, che può semplificare in maniera significativa il suo calcolo, è la ***linearità del valore atteso***. Da questa proprietà, si deriva che il valore atteso della somma di v.a. è uguale alla somma dei loro valori attesi
```ad-Teorema
title: Teorema (Linearità del valore atteso)
Per ogni sequenza finita di v.a. discrete $X_1,X_2,...,X_n$ con valore atteso finito, si ha che 
$$
\mathbf{E}\Bigr[ \sum_{i=1}^n X_i \Bigr] = \sum_{i=1}^n \mathbf{E}[X_i]
$$
```

Si mostrano ora delle proprietà del valore atteso 
```ad-Teorema
Siano $X$ ed $Y$ due variabili aleatorie indipendenti. Allora
$$
\mathbf{E}\left[X \cdot Y \right] = \mathbf{E}\left[X   \right] \cdot \mathbf{E}\left[Y \right]
$$
```
```ad-note
title: Nota
Se $X$ ed $Y$ non sono indipendenti, allora 
$$
\mathbf{E}\left[X \cdot Y \right] \neq \mathbf{E}\left[X   \right] \cdot \mathbf{E}\left[Y \right]
$$
```
Sia $X$ una variabile aleatoria e $\mathcal{E}$ un evento. Il valore atteso condizionale di $X$ condizionato su $\mathcal{E}$ è definito come 
$$
\mathbf{E}\left[X|\mathcal{E}\right] = \sum_{x} x \cdot \mathbf{Pr}\left[X=x | \mathcal{E}\right]
$$
dove $x$ assume valori nel range di $X$.
Il teorema della probabilità totale implica il seguente lemma
```ad-Lemma
title: Lemma (Legge delle aspettative iterate)
Se gli eventi $B_i$, per $i=1,\ldots,k$ sono una partizione dello spazio di campionamento, allora per ogni variabile aleatoria $X$:
$$
\mathbf{E}\left[X\right] = \sum_{i=1}^k \mathbf{Pr}\left[B_i\right]\mathbf{E}\left[X|B_i\right]
$$
```
La varianza misura di quanto il valore di una variabile aleatoria si discosta globalmente dal suo valore atteso.
```ad-Definizione
title: Definizione (Varianza e deviazione standard)
La varianza di una v.a. $X$ è definita come
$$
Var\left[X\right] = \mathbf{E}\left[\left(X-\mathbf{E}\left[X\right]\right)^2\right] = \mathbf{E}\left[X^2\right] - \left(\mathbf{E}\left[X\right]\right)^2
$$

Alternativamente, può essere definita come segue:
Per una variabile aleatoria discreta, la varianza è calcolata sommando, il prodotto del quadrato della differenza tra il valore assunto dalla variabile aleatoria e il suo valore atteso con la probabilità che tale variabile assuma quei valori, per tutti i valori che può assumere la variabile aleatoria. In formule, sia $X=1,\ldots,n$ e $\mu = \mathbf{E}\left[X\right]$, allora
$$
Var\left[X\right] = \sum_{k=1}^n \left(k-\mu\right)^2 \mathbf{Pr}\left[X=k\right]
$$
Inoltre, se $X=1,2,\ldots$, si ha che 

$$
Var\left[X\right] = \sum_{k=1}^{\infty} \left(k-\mu\right)^2 \mathbf{Pr}\left[X=k\right]
$$

La deviazione standard di una v.a. $X$ è 
$$
\sigma\left[X\right] = \sqrt{Var\left[X \right]}
$$
```
```ad-Corollario
Siano $X$ e $Y$ variabili aleatorie indipendenti. Allora
$$
Var\left[X + Y \right] = Var\left[X\right] + Var \left[Y\right]
$$
```
```ad-note
title: Nota
Se $X$ ed $Y$ non sono indipendenti, allora 
$$
Var\left[X + Y \right] \neq Var\left[X\right] + Var \left[Y\right]
$$
```
```ad-Teorema
Siano $X_1,X_2,\ldots,X_n$ variabili aleatorie mutualmente indipendenti. Allora
$$
Var\left[\sum_{i=1}^n X_i  \right] = \sum_{i=1}^n  Var\left[X_i\right]
$$
```
## Distribuzioni di probabilità
### Distribuzione binomiale
Si supponga di eseguire un esperimento avente successo con probabilità $p$ e fallimento con probabilità $1-p$. Sia $Y$ una variabile aleatoria tale che
$$
\begin{equation*}
Y= 
  \left\{ 
    \begin{aligned}
      &\ 1 \ \ \ \ \textnormal{se l'esperimento ha successo} \\
      &\ 0 \ \ \  \textnormal{ altrimenti}  
    \end{aligned}
  \right.
\end{equation*}
$$
La variabile $Y$ prende il nome di variabile aleatoria *binomiale binaria*, o variabile aleatoria *Bernoulliana*. Si osserva che, per una variabile aleatoria binaria si ha che 

$$
\mathbf{E}[Y] \ = p \cdot 1 + (1-p) \cdot 0\ =\ p\ =\ \mathbf{Pr}(Y=1)
$$
Si consideri ora una sequenza di $n$ esperimenti **indipendenti**, ciascuno con probabilità di successo $p$ ( $\implies$ probabilità di fallimento ($1-p$)). Tale sequenza di esperimenti prende il nome di *processo di Bernoulli,* mentre ogni esperimento prende il nome di *prova di Bernoulli*.
Sia $X$ la variabile aleatoria che conta il numero di successi negli $n$ esperimenti, allora $X$ ha una ***distribuzione binomiale***.
```ad-Definizione
title: Definizione (Distribuzione binomiale):
Una variabile aleatoria binomiale $X$ con parametri $n$ e $p$, denotata da $X$~Bin$(n,p)$, è definita dalla seguente distribuzione di probabilità su $j=0,1,...,n$:

$$
\mathbf{Pr}(X = j) = \binom{n}{j}\ p^j\ (1-p)^{n-j}
$$
```
Ossia, $X$ è uguale a $j$ quando vi sono esattamente $j$ successi e $n-j$ fallimenti in $n$ esperimenti indipendenti, ciascuno dei quali ha successo con probabilità $p$.
```ad-Osservazione
$$\sum_{j=0}^n \mathbf{Pr}(X=j) = 1$$
In accordo con la definizione di funzione di probabilità.
```
E' possibile calcolare, partendo dalla definizione, il valore atteso di una variabile aleatoria binaria $X$ con distribuzione binomiale:
$$
\begin{align}
\mathbf{E}[X] =&\ \sum_{j=0}^n\ j\ \binom{n}{j}\ p^j\ (1-p)^{n-j}\ \\
\\
=&\ \sum_{j=0}^n\ j\ \frac{n!}{j !\ (n-j)!}\ p^j\ (1-p)^{n-j} \\
\\
=&\ \sum_{j=1}^{n}\ \ \frac{n!}{(j-1)!\ (n-j)!}\ p^j\ (1-p)^{n-j} \\
\\
=&\ np \sum_{j=1}^n \frac{(n-1)!}{(j-1)!((n-1)-(j-1))!}\ p^{j-1}\ (1-p)^{((n-1)-(j-1))} \\
\\
\stackrel{{\text{(1)}}}{=}&\ np \sum_{k=0}^{n-1} \frac{(n-1)!}{k!((n-1)-k)!}\ p^k\ (1-p)^{((n-1)-k)} 
\\
\\
=&\ np \sum_{k=0}^{n-1}\ \binom{n-1}{k}\ p^k\ (1-p)^{(n-1)-k} \\
\\
\stackrel{{\text{(2)}}}{=}&\ np 
\end{align}
$$
Dove:
- in $(1)$ si è posto $k=j-1$ 
- in $(2)$ si è fatto riferimento al teorema binomiale: $$(x+y)^n = \sum_{k=0}^{n}\binom{n}{k}\ x^k\ y^{n-k}$$ con $x=p$ e $y=1-p$ .

Si fa presente ora come, sfruttando la linearità del valore atteso, quest'ultimo può essere calcolato in maniera molto più semplice.
Se $X$~Bin$(n,p)$, allora $X=\textnormal{" \# successi in } n \textnormal{ prove"}$, dove ogni prova ha probabilità $p$ di avere successo. Si definisce, per $i=1,...,n$
$$
\begin{equation*}
X= 
  \left\{ 
    \begin{aligned}
      &\ 1 \ \ \ \ \textnormal{se l'esperimento ha successo} \\
      &\ 0 \ \ \  \textnormal{ altrimenti}  
    \end{aligned}
  \right.
\end{equation*}
$$
Allora $\mathbf{E}[X_i] = p \cdot 1 + 0\cdot (1-p) = p  = \mathbf{Pr}(X_i = 1)$ .
Inoltre, si ha che $X= \sum_{i=1}^n X_i$, allora, per la linearità del valore atteso

$$
\mathbf{E}[X]\ =\ \mathbf{E} \Bigr[\sum_{i=1}^n X_i \Bigr]\ =\ \sum_{i=1}^n \mathbf{E}[X_i]\ =\ np
$$
La varianza di una variabile aleatoria binomiale $X$ con parametri $n$ e $p$ può essere determinata calcolando $\mathbf{E}\left[X^2\right]$ come segue
$$
\begin{aligned}
\mathbf{E}\left[X^2\right]= & \sum_{j=0}^n\left(\begin{array}{l}
n \\
j
\end{array}\right) p^j(1-p)^{n-j} j^2 \\
= & \sum_{j=0}^n \frac{n !}{(n-j) ! j !} p^j(1-p)^{n-j}\left(\left(j^2-j\right)+j\right) \\
= & \sum_{j=0}^n \frac{n !\left(j^2-j\right)}{(n-j) ! j !} p^j(1-p)^{n-j}+\sum_{j=0}^n \frac{n ! j}{(n-j) ! j !} p^j(1-p)^{n-j} \\
= & n(n-1) p^2 \sum_{j=2}^n \frac{(n-2) !}{(n-j) !(j-2) !} p^{j-2}(1-p)^{n-j} \\
& +n p \sum_{j=1}^n \frac{(n-1) !}{(n-j) !(j-1) !} p^{j-1}(1-p)^{n-j} \\
= & n(n-1) p^2+n p .
\end{aligned}
$$
Dove le sommatorie sono state semplificate facendo riferimento al teorema binomiale: $(x+y)^n = \sum_{k=0}^n \binom{n}{k}x^{n-k}y^k = \sum_{k=0}^n \binom{n}{k}x^{k}y^{n-k} .$  Si conclude che
$$
\begin{aligned}
\operatorname{Var}[X] & =\mathbf{E}\left[X^2\right]-(\mathbf{E}[X])^2 \\
& =n(n-1) p^2+n p-n^2 p^2 \\
& =n p-n p^2 \\
& =n p(1-p)
\end{aligned}
$$
La varianza può essere alternativamente derivata facendo riferimento all'indipendenza. Una variabile aleatoria binomiale $X$ può essere rappresentata come la somma di $n$ prove di Bernoulli indipendenti, ciascuna avente successo con probabilità $p$. Tale prova di Bernoulli $Y$ ha varianza
$$
\mathbf{E}\left[(Y-\mathbf{E}[Y])^2\right]=p(1-p)^2+(1-p)(-p)^2=p-p^2=p(1-p) .
$$
E per la linearità della varianza, si ha che la varianza di $X$ è quindi $n p(1-p)$.
### Distribuzione geometrica
Si supponga di lanciare una moneta sino a quando esce testa. La distribuzione del numero di lanci è un esempio di distribuzione geometrica. In generale, si ha una distribuzione geometrica quando si eseguono una sequenza di eventi indipendenti sino al primo successo, dove ogni prova ha successo con probabilità $p$.
```ad-Definizione
title: Definizione (Distribuzione geometrica)
Una variabile aleatoria geometrica $X$ con probabilità $p$ è data dalla seguente distribuzione di probabilità su $n=1,2,...$

$$
\mathbf{Pr}(X=n) = (1-p)^{n-1}p
$$
```
Per la v.a. geometrica $X$, per essere uguale ad $n$, devono esserci $n-1$ fallimenti seguiti da un successo. Ovviamente
```ad-Osservazione
$$\sum_{n \geq 1} \mathbf{Pr}(X=n) = 1$$
In accordo con la definizione di funzione di probabilità.
```
Le variabili geometriche sono "prive di memoria", nel senso che la probabilità di ottenere un successo dopo $n$ prove è indipendente dal numero di fallimenti ottenuti in precedenza. Informalmente, si possono ignorare gli insuccessi passati, perché non cambiano la distribuzione del numero di prove future sino al primo successo. Formalmente
```ad-Lemma
Per una variabile aleatoria geometrica $X$ con parametro $p$ e per $n>0$
$$
\mathbf{Pr}(X=n+k\ |\ X>k) = \mathbf{Pr}(X=n)
$$
```
Si vede ora come calcolare il valore atteso di una variabile aleatoria avente distribuzione geometrica. Quando una variabile aleatoria assume valori in $\mathbb{N}$, si ha una formula alternativa per calcolare il suo valore atteso
```ad-Lemma
Sia $X$ una variabile aleatoria discreta che assume solo valori interi non negativi. Allora
$$
\mathbf{E}[X] = \sum_{i=1}^{\infty} \mathbf{Pr}(X \geq i)
$$
```
Si ha che per una variabile aleatoria geometrica $X$ con parametro $p$

$$
\mathbf{Pr}(X\geq i)\ =\ \sum_{n=i}^{\infty}(1-p)^{n-1}\ p = (1-p)^{i-1}
$$
Allora

$$
\begin{align}
\mathbf{E}[X] =& \sum_{i=1}^{\infty} \mathbf{Pr}(X\geq i) = \sum_{i=1}^{\infty} (1-p)^i-1 \\
\\
=&\ \frac{1}{1-(1-p)} = \frac{1}{p}
\end{align}
$$
## Discostamento dal valore atteso
Si presentano delle tecniche per stimare il discostamento di una variabile aleatoria dal suo valore atteso, ossia la probabilità che una variabile aleatoria assuma valori lontani dal suo valore atteso. Questi bounds forniscono una stima della probabilità di insuccesso di un algoritmo, e permettono di stabilire bounds con alta probabilità sul running time.
### Disuguaglianza di Markov
Sia $X$ una variabile aleatoria che assume *solo* valori non negativi. Allora, $\forall a > 0$ vale
$$
\mathbf{Pr}\left[X \geq a\right] \leq \frac{\mathbf{E}\left[X\right]}{a}
$$
#### Dimostrazione
Per $a>0$, sia
$$
\begin{equation*}
I= 
  \left\{ 
    \begin{aligned}
      &\ 1 \ \ \ \textnormal{se }\ X \geq a \\
      &\ 0 \ \ \  \textnormal{altrimenti }\   
    \end{aligned}
  \right.
\end{equation*}
$$
Si osserva che, essendo $X \geq 0$, vale 
$$
I \leq \frac{X}{a}
$$
Essendo $I$ una variabile aleatoria binaria
$$
\mathbf{E}\left[I\right] = \mathbf{Pr}\left[I=1\right] = \mathbf{Pr}\left[X \geq a \right]
$$
Facendo riferimento alla disuguaglianza precedente, si ha 
$$
\mathbf{Pr}\left[X \geq a\right] = \mathbf{E}\left[I\right] \leq \mathbf{E}\left[\frac{X}{a}\right] = \frac{\mathbf{E}\left[X\right]}{a}
$$
### Disuguaglianza di Chebyshev
Per ogni $a>0$
$$
\mathbf{Pr}\left[|X-\mathbf{E}\left[X\right]| \geq a \right] \leq \frac{Var\left[X\right]}{a^2}
$$
#### Dimostrazione
#TODO 
