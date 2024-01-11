In questa serie di lezioni si affrontano problemi su un particolare modello di dati detto **Data Stream model** (o *Stream model*) nel quale l'input è costituito da uno o più flussi di dati che entrano nei canali di input e se questi dati non vengono elaborati immediatamente o archiviati, vengono persi per sempre.
Inoltre, questi dati arrivano in modo estremamente *rapido*, dunque non è fattibile archiviarli tutti, ad esempio, in un database e poi interagire con essi al momento che scegliamo.
In generale, il sistema riceve gli elementi dello stream uno alla volta rispetto ad un'unità di tempo (un elemento ad ogni unità di tempo) estremamente corta (ad esempio, un dato ogni millisecondo).
L'obiettivo degli algoritmi che operano in queste circostanze (detti anche algoritmi di *streaming*) è quello di calcolare statistiche interessante sui dati dello stream utilizzando la minor quantità di memoria possibile: idealmente, una quantità polilogaritmica rispetto alla dimensione dell'intero stream. Dunque il sistema può memorizzare e aggiornare solo degli *sketches* relativamente corti dello stream, e tramite questi sketches gli algoritmi di streaming devono essere in grado di fornire risposte a query statistiche di interesse.
![](Pasted%20image%2020240103110920.png)
# Pattern matching problem
Il primo problema statistico che si considera nello stream model è il problema del **pattern matching**. Il problema è il seguente: data una stringa $x$, verificare se la stringa appare nello stream di dati $S$.
Formalmente, sia $\Sigma$ un alfabeto e siano $y_i$ gli elementi di tale alfabeto, e sia lo stream $S$ una stringa di lunghezza $m$ (con $m$ *molto* grande) su $\Sigma$. Si supponga di avere un pattern $x = x_1 x_2 \dots x_n \in \Sigma^n$ con $n$ *molto* più piccolo di $m$.
Il problema chiede quante volte appare $x$ in $S$ come sottostringa $x = y_i,y_{i+1},\dots,y_{i+n-1}$.

Dato che si lavora nell'ambito dello stream di dati, non è possibile mantenere lo stream $S$ in memoria: si deve utilizzare un buon *data sketch* in grado di preservare la verifica della identità tra stringhe.
### Definizione: data sketch
Sia $x$ un qualche tipo di dato, ad esempio una stringa $x \in \Sigma^*$. Un **data sketch** è una funzione randomizzata $f$ che mappa $x \in \Sigma^*$ in una sequenza di $k$ bit <u>corta</u> che offre le seguenti proprietà:
1. $f$ è semplice da calcolare;
2. La dimensione $k$ dello sketch $f(x)$ è molto più <u>corta</u> rispetto alla dimensione di $x$;
3. $f(x)$ è semplice da aggiornare se $x$ viene aggiornato. Inoltre, è semplice ottenere lo sketch dato dalla combinazione di più sketches.
4. $f(x)$ può essere utilizzato per calcolare efficientemente alcune proprietà di $x$, cioè $f(x)$ preserva (con alta probabilità) delle *proprietà chiavi* di $x$.
### Preservare l'identita - Rabin's hash function
La **proprietà chiave** da preservare per affrontare il pattern matching problem è l'**identità tra stringhe**: date due stringhe $x,y \in \Sigma^*$, si vuole costruire uno sketch $f(x)$ tale che $f(x) = f(y)$ se e soltanto se $x=y$ con alta probabilità.

Senza perdita di generalità, sia $x$ una stringa di lunghezza $n$ su un'alfabeto $\Sigma$. Senza perdita di generalità, sia $\Sigma = [0,\sigma-1]$ e siano le stringhe viste come interi in base $\sigma > 0$.
Si osserva immediatamente che, per ogni funzione $f$, se $\mid f(x) \mid < \mid x \mid$, dove con $\mid \cdot \mid$ si indica il numero di bit, allora devono avvenire delle collisioni: infatti, deve esistere una coppia $x \neq y$ tale che $f(x) = f(y)$ (pigeonhole principle).

Una prima idea per risolvere il problema è quella di considerare la stringa $x$ come un numero di $\mid x \mid = n$  cifre in base $\mid \Sigma \mid$ ed utilizzare la funzione
$$ h(x) = ((a \cdot x + b) \mod{M}) \mod{d} $$
Dato che però è richiesto $M>x$ per ogni input $x$ della funzione, si devono fare operazioni in aritmetica modulare su interi a $n$ cifre.

Per risolvere il problema, si utilizza la funzione hash di Rabin.
#### Definizione: Rabin's hash function
Sia $q$ un numero primo fissato, e sia $z \in [0,q)$ un numero scelto uniformemente a caso (si osserva che $z$ è un numero in $Z_q$). Sia $x[1,n] \in \Sigma^n$ una stringa di lunghezza $n$. La funzione hash di Rabin $k_{q,z}(x)$ è definita come
$$
k_{q,z}(x) = \left( \sum_{i=0}^{n-1} x[n-i] \cdot z^i \right) \mod{q}
$$
$$
k_{q,z}(x) = ( x[n] + x[n-1] \cdot z + x[n-2] \cdot z^2 + \dots + x[1]\cdot z^{n-1}) \mod{q}
$$
In altre parole, $k_{q,z}(x)$ è un polinomio modulo $q$ calcolato nel valore $z$ (un punto random in $[0,q)$) che ha come coefficienti i caratteri della stringa $x$.
Si osserva che il valore $k_{q,z}(x)$ è un numero modulo $q$, dunque lo spazio occupato da esso è logaritmico in $q$: $O(\log{q})$.

Per prima cosa, si osserva che $k_{q,z}(x)$ è semplice da calcolare e aggiornare. Si supponga di dover appendere il carattere $c$ in fondo alla stringa $x$, ottenendo dunque la stringa $x \cdot c$. Allora per il metodo di Horner per la valutazione di un polinomio vale:
$$
k_{q,z}(x) =  x[n] + z (x[n-1]+z(x[n-2]+x[n-3] \cdot z))   \mod{q}
$$
$$ k_{q,z}(x \cdot c) = (k_{q,z}(x) \cdot z + c) \mod{q} $$
Questa formulazione ricorsiva fornisce un algoritmo efficiente per calcolare $k_{q,z}(x)$: si applica la formula precedente a partire da $k_{q,z}(\epsilon) = 0$ concatenando un carattere di $x$ per volta.
Inoltre, per mezzo di un idea simile è possibile concatenare gli sketches di due stringhe in tempo logaritmico. Sia $x\cdot y$ la concatenazione di due stringhe $x$ e $y$. Sia inoltre lo sketch di $x$ composto da una coppia $(k_{q,z}(x), \mid x \mid)$, dove $\mid x \mid$ indica il numero di caratteri di $x$. Allora si calcola la coppia $(k_{q,z}(x \cdot y), \mid x+y \mid)$ come:
$$
k_{q,z}(x \cdot y) = (k_{q,z}(x) \cdot z^{\mid y \mid} + k_{q,z}(y)) \mod{q}
$$
$$
\mid x+y \mid = \mid x \mid + \mid y \mid
$$
In questo caso, lo spazio occupato aumenta di una quantità $O(\log{\mid y \mid}\log{q})$.

### Lemma: good sketch for identity
Sia $x \neq y$ con $\mid x \mid = \mid y \mid = n$. Allora
$$
\mathcal{P}(k_{q,z}(x) = k_{q,z}(y)) \leq \frac{n}{q}
$$
#### Dimostrazione
Si osserva che
$$
\mathcal{P}(k_{q,z}(x) = k_{q,z}(y)) = \mathcal{P}(k_{q,z}(x) - k_{q,z}(y) \equiv_{q} 0)
$$
dove $k_{q,z}(x) - k_{q,z}(y)$ è un polinomio.
Sia $x-y$ la stringa
$$
(x-y)[i] = x[i] - y[i] \mod{q} = x[n-1]-y[n-1],\dots, x[0]-y[0]
$$
Allora vale che
$$
k_{q,z}(x) - k_{q,z}(y) \mod{q} = k_{q,z}(x-y) \mod{q}
$$
dunque
$$
\mathcal{P}(k_{q,z}(x) = k_{q,z}(y)) = \mathcal{P}(k_{q,z}(x) - k_{q,z}(y) \equiv_{q} 0) = \mathcal{P}(k_{q,z}(x-y) \equiv_{q} 0)
$$
Siccome $x \neq y$, allora $k_{q,z}(x-y)$ è un polinomio di grado al più $n$ su $Z_q$ e non è il polinomio pari a zero.
Dato che ogni polinomio univariato (?) non nullo di grado $n$ su un campo ha al più $n$ radici, e dato che $q$ è primo e quindi $Z_q$ è un campo, allora ci sono al più $n$ valori $z$ per cui vale $k_{q,z}(x-y) \equiv_{q} 0$. Dato che $z$ è preso uniformemente a caso in $Z_q$, allora la probabilità di prendere una radice è pari al numero di casi favorevoli $n$ fratto il numero di casi possibili $q$j.