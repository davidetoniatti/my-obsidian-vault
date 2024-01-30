# Verifica di identità polinomiali
Si hanno due polinomi di grado $d$, siano questi $F(x)$ e $G(x)$. 
Il primo polinomio $F(x)$ è rappresentato nella forma di prodotto di monomi
$$
F(x) = \prod_{i=1}^d(x-a_i)
$$
mentre $G(x)$ è rappresentato mediante la sua forma canonica 
$$
G(x)= \sum_{i=0}^d c_ix^i
$$
Dati questi due polinomi, si vuole verificare l'identità 
$$
F(x) \equiv G(x)
$$
Per risolvere tale problema, è possibile convertire $F(x)$ nella sua forma canonica, moltiplicando consecutivamente l'$i-$esimo monomio con il prodotto dei primi $i-1$ monomi precedenti. Così facendo, risulta sufficiente verificare che i coefficienti di ciascuna variabile siano uguali per i due polinomi, ovvero, siano $a_i$ e $c_i$ i coefficienti del letterale $x^i$ di grado $i$ per $F(x)$ e $G(x)$ rispettivamente, allora 
$$
F(x) \equiv G(x)\iff (\forall i=0,\ldots,d  \ [a_i = c_i]) \ 
$$
Tale operazione richiede $\Theta(d^2)$ moltiplicazioni di coefficienti, ciascuna eseguibile in tempo costante.
Si vuole progettare un algoritmo probabilistico che risolva il problema in maniera efficiente. 

Sia $d$ il grado massimo, ossia il più grande esponente di $x$, in $F(x)$ e $G(x)$. L'algoritmo in questione sceglie un intero $r$ uniformly at random nel range $\{1,\ldots,100d\}.$ Calcola poi i valori $F(r)$ e $G(r)$, e li confronta. Se $F(r)=G(r),$ allora l'algoritmo dichiara che i due polinomi sono equivalenti, altrimenti, se $F(r) \neq G(r),$ dichiara che non lo sono.
Si assuma che la generazione di un intero $r$ casuale in $\{1,\ldots,100d\}$ richieda tempo costante.
Il calcolo di $F(r)$ e di $G(r)$ può essere effettuato in tempo $\mathcal{O}(d),$ che risulta essere più veloce del calcolo della forma canonica di $F(x).$ Tale miglioramento in termini di efficienza, comporta però la possibilità che l'algoritmo restituisca una risposta incorretta.
Si studiano quindi i possibili scenari in cui può trovarsi l'algoritmo:
- Se $F(x) \equiv G(x)$ allora l'algoritmo restituisce *sempre* la risposta corretta, essendo che per ogni valore di $r$, $F(r)=G(r).$ 
- Se $F(x) \not\equiv G(x)$ e $F(r) \neq G(r),$ allora l'algoritmo restituisce la risposta corretta, essendo che ha trovato un $r$ tale per cui $F(x)$ e $G(x)$ non assumono lo stesso valore. In particolar modo, essendo che $F(x) \equiv G(x)$ se e solo se $F(r)=G(r)$ per ogni $r,$ ogni qual volta l'algoritmo trova un $r$ tale per cui $F(r) \not= G(r)$, allora sicuramente $F(x) \not\equiv G(x),$ ossia, i due polinomi non sono equivalenti.
- Se $F(x) \not\equiv G(x)$ e $F(r) = G(r)$ allora l'algoritmo restituisce la risposta sbagliata. E' possibile quindi che venga selezionato un $r$ tale per cui l'algoritmo decida che i due polinomi siano uguali, quando in realtà non lo sono. Affinché ciò avvenga, $r$ deve essere una radice dell'equazione $F(x)-G(x)=0.$ Il grado del polinomio $F(x)-G(x)=0$ è al più $d,$ e per il teorema fondamentale dell'algebra, un polinomio di grado $d$ non ha più di $d$ radici. Quindi, se $F(x) \not\equiv G(x),$ ci sono al più $d$ valori nel range $\{1,\ldots,100d\}$ tali per cui $F(r)=G(r)$. Essendovi $100d$ valori in tale range, la probabilità che l'algoritmo scelga un valore di questo tipo, e che quindi restituisca una risposta sbagliata è al più $1/100$.
Quindi, schematicamente, analizzando i casi in input:
- Se $F(x) \equiv G(x)$ l'algoritmo risponde correttamente.
- Se $F(x) \not\equiv G(x)$ l'algoritmo può rispondere in maniera incorretta.
Mentre valutando gli esiti in output si ha:
- Se l'algoritmo calcola $F(r) \neq G(r)$ allora sicuramente $F(x) \not\equiv G(x),$ e quindi risponde correttamente.
- Se l'algoritmo calcola $F(r) = G(r)$ non necessariamente $F(x) \equiv G(x)$, e quindi può aver restituito una risposta sbagliata.
Si formalizza  a probabilità di errore. Sia $\mathcal{E}$ l'evento
$$
\mathcal{E} = \textnormal{ L'algoritmo sbaglia}
$$
Gli elementi dell'insieme corrispondente all'evento $\mathcal{E}$ sono le radici del polinomio $F(x)-G(x)$ contenute in $\{1,\ldots,100d\}.$ Non essendo queste più di $d$, si ha che $\mathcal{E}$ include al più $d$ eventi semplici. Quindi
$$
\mathbf{Pr}\left[\mathcal{E}\right] \leq \frac{d}{100d} = \frac{1}{100}
$$

Un approccio per aumentare la probabilità che l'algoritmo risponda correttamente è quello di eseguirlo più volte, utilizzando valori casuali diversi per testare l'identità. Questo perché, essendo che l'algoritmo può effettuare un errore da un solo lato, ossia può aver sbagliato solamente se restituisce che i due polinomi sono equivalenti, se almeno uno degli $r$ scelti è tale per cui $F(r) \neq G(r)$, allora sicuramente i due polinomi sono diversi. Quindi, fissato un numero di round per i quali iterare l'algoritmo, se per ciascuno di essi risulta $F(r)=G(r)$ con $r$ selezionato ad ogni round *u.a.r.* da $\{1,\ldots,100d\},$ si può aver maggior confidenza che l'identità risulti essere verificata. 

Il campionamento di $r$ dall'intervallo può avvenire in due modalità: *con reinserimento* o *senza reinserimento.* Nel primo caso, non si tiene traccia dei valori di $r$ selezionati in precedenza. Di conseguenza, lo stesso valore per $r$ può essere assunto più volte. Nel secondo caso, una volta che un valore per $r$ è stato scelto, tale valore viene escluso dalla selezione casuale nei round successivi, dove questa avviene *u.a.r.* tra i valori dell'intervallo non ancora selezionati.

Sia quindi $k$ il numero rounds, ossia il numero di volte per cui viene iterato l'algoritmo, e siano i due polinomi *non* equivalenti. 
Si consideri il caso con reinserimento. Essendo che l'insieme da cui viene selezionato il valore di $r$ rimane invariato ad ogni round, si ha che la selezione di ciascun valore è indipendente dalle altre. Sia $\mathcal{E_i}$ l'evento 
$$
\mathcal{E_i} = \textnormal{Al round }i \textnormal{ viene scelto }r_i \textnormal{ tale che }F(r_i)-G(r_i)=0
$$
La probabilità che l'algoritmo restituisca la risposta sbagliata è data da 
$$
\mathbf{Pr}\left[\mathcal{E_1} \cap \mathcal{E_2} \cap \ldots \cap \mathcal{E_k}\right]
$$
Essendo che $\mathbf{Pr}\left[\mathcal{E_i}\right] \leq 1/100$ e gli eventi $\mathcal{E_1}, \mathcal{E_2}, \ldots,\mathcal{E_k}$ sono indipendenti, la probabilità che l'algoritmo restituisca la risposta sbagliata dopo $k$ iterazioni è
$$
\mathbf{Pr}\left[\mathcal{E_1} \cap \mathcal{E_2} \cap \ldots \cap \mathcal{E_k}\right] = \prod_{i=1}^k \mathbf{Pr}\left[\mathcal{E_i}\right] \leq \prod_{i=1}^k \frac{d}{100d} = \left( \frac{1}{100}\right)^k
$$

Si consideri il caso senza reinserimento. In questo scenario, la probabilità di scegliere un dato numero nell'intervallo è condizionata dagli eventi delle precedenti iterazioni.
Sia di nuovo $\mathcal{E_i}$ l'evento 
$$
\mathcal{E_i} = \textnormal{Al round }i \textnormal{ viene scelto }r_i \textnormal{ tale che }F(r_i)-G(r_i)=0
$$
La probabilità che l'algoritmo restituisca la risposta sbagliata è sempre data da 
$$
\mathbf{Pr}\left[\mathcal{E_1} \cap \mathcal{E_2} \cap \ldots \cap \mathcal{E_k}\right]
$$
In questo caso, gli eventi non risultano essere indipendenti tra loro. Applicando la definizione di probabilità condizionata, si ottiene
$$
\mathbf{Pr}\left[\mathcal{E_1} \cap \mathcal{E_2} \cap \ldots \cap \mathcal{E_k}\right] = \mathbf{Pr}\left[\mathcal{E_k}|\mathcal{E_1} \cap \mathcal{E_2} \cap \ldots \cap \mathcal{E_{k-1}}\right] \cdot \mathbf{Pr}\left[\mathcal{E_1} \cap \mathcal{E_2} \cap \ldots \cap \mathcal{E_{k-1}}\right]
$$
Ripetendo lo stesso procedimento si ha che
$$
\mathbf{Pr}\left[\mathcal{E_1} \cap \mathcal{E_2} \cap \ldots \cap \mathcal{E_k}\right] = \mathbf{Pr}[\mathcal{E_1}] \cdot \mathbf{Pr}[\mathcal{E_2}|\mathcal{E_1}]\cdot \mathbf{Pr}[\mathcal{E_3}|\mathcal{E_1}\cap\mathcal{E_2}] \cdots \mathbf{Pr}\left[\mathcal{E_k}|\mathcal{E_1} \cap \mathcal{E_2} \cap \ldots \cap \mathcal{E_{k-1}}\right]
$$

Si vuole dare un bound a 
$$
\mathbf{Pr}\left[\mathcal{E_j}|\mathcal{E_1} \cap \mathcal{E_2} \cap \ldots \cap \mathcal{E_{j-1}}\right]
$$
Si supponga che nelle prime $j-1$ iterazioni si abbia ottenuto sempre $F(r)-G(r)=0,$ ossia che siano state selezionate, essendo $F(x) \not\equiv G(x),$ sempre radici della differenza tra i due polinomi. 
Essendovi al più $d$ valori $r$ tali per cui $F(r)-G(r)=0,$ se le iterazioni che vanno da $1$ a $j-1<d$ ne hanno trovati $j-1,$ allora, essendo che questi non vengono reinseriti nell'intervallo per la selezione, si ha che alla $j-$esima iterazione si hanno al più $d-(j-1)$ radici rimanenti tra tutte le $100d-(j-1)$ possibili scelte.
Allora
$$
\mathbf{Pr}\left[\mathcal{E_j}|\mathcal{E_1} \cap \mathcal{E_2} \cap \ldots \cap \mathcal{E_{j-1}}\right] \leq \frac{d-(j-1)}{100d-(j-1)}
$$
e quindi è possibile dare il seguente bound sulla probabilità che l'algoritmo dia una risposta sbagliata dopo $k\leq d$ iterazioni
$$
\mathbf{Pr}\left[\mathcal{E_1} \cap \mathcal{E_2} \cap \ldots \cap \mathcal{E_k}\right]  \leq \prod_{j=1}^k\frac{d-(j-1)}{100d-(j-1)}  \leq \left( \frac{1}{100}\right)^k
$$
ed essendo che per $j>1$
$$
\frac{d-(j-1)}{100d-(j-1)} < \frac{d}{100d}
$$
si ha che la selezione senza reinserimento garantisce una probabilità minore che l'algoritmo sbagli.
Si osserva esplicitamente che, se si prendono $d+1$ campioni senza reinserimento, e i due polinomi non sono equivalenti, si ottiene sicuramente un $r$ tale per cui $F(r)-G(r) \neq 0.$ Quindi in $d+1$ iterazioni si può garantire in output la risposta corretta. Ma il calcolo del polinomio a $d+1$ punti richiede tempo $\Theta(d^2)$ utilizzando l'approccio standard. Tale opzione non risulta essere quindi più efficiente dell'approccio deterministico.

# Verifica del prodotto di matrici binarie
Si vede un esempio di algoritmo probabilistico che, a discapito della sicurezza sulla produzione di una risposta corretta in output, impiega minor tempo per l'elaborazione di tale risposta.

```ad-Problema
- **Input:** $A,B,C \in \{0,1\}^{n \times n}$
- **Output:** $\texttt{True}$ se $A \cdot B = C$ ($\mod 2$), $\texttt{False}$ altrimenti
```

Si ricorda il metodo deterministico (banale) per verificare il prodotto di due matrici:
- Si calcola $A \cdot B = C^{'}$ 
- Si verifica $C^{'} = C$

Il calcolo del prodotto di due matrici impiega tempo $\Theta(n^3)$. Questo perché, eseguendo il prodotto righe per colonne, per il calcolo di un'entrata di $C^{'}$ sono necessarie $n$ moltiplicazioni. Quindi, per calcolare le $n^2$ entrate di $C^{'}$ sono necessarie $\Theta(n^3)$ operazioni. 
Esistono anche algoritmi che impiegano tempo $\Theta(n^{2.37})$.
Si vede ora un algoritmo probabilistico che impiega tempo $\mathcal{O}(n^2 \log n)$.

```ad-Osservazione
Se $A B=C$, allora, dato un qualunque vettore $r = [r_1\ r_2\ \cdots r_n]\in \{0,1\}^n$, vale che $A B r = C r$ 
```
```ad-Osservazione
Il prodotto tra una matrice ed un vettore impiega tempo $\Theta(n^2)$
```

Se si prende un vettore $r = [r_1\ r_2\ \cdots\ r_n]^{\top} \in \{0,1\}^n$, si può calcolare $ABr$ nella seguente maniera:

- Si calcola prima $Br$

$$

B\cdot r = \begin{bmatrix} 
\ b_{11} & b_{12} & \cdots & b_{1n}\ \\
\ b_{21} & b_{22} & \cdots & b_{2n}\ \\
\vdots & \vdots & \ddots & \vdots \\
\ b_{n1} & b_{n2} & \cdots & b_{nn}
\end{bmatrix} 
\begin{bmatrix}
\ r_{11}\ \\
\ r_{21}\ \\
\ \vdots\ \\
\ r_{n1}\ \\
\end{bmatrix}\ =\ 
\begin{bmatrix}
\ r_{B1}\ \\
\ r_{B2}\ \\
\ \vdots\ \\
\ r_{Bn}\ \\
\end{bmatrix} \ =
\ r_B
$$

Calcolare $r_B$ richiede $n$ moltiplicazioni per $n$ entrate $\implies$ costo $\Theta(n^2)$.

- Si calcola poi $A(Br)=Ar_B$ 

$$

A\cdot r_B = \begin{bmatrix} 
\ a_{11} & a_{12} & \cdots & a_{1n}\ \\
\ a_{21} & a_{22} & \cdots & a_{2n}\ \\
\vdots & \vdots & \ddots & \vdots \\
\ a_{n1} & a_{n2} & \cdots & a_{nn}
\end{bmatrix} 
\begin{bmatrix}
\ r_{B1}\ \\
\ r_{B2}\ \\
\ \vdots\ \\
\ r_{Bn}\ \\
\end{bmatrix}\ =\ 
\begin{bmatrix}
\ r_{AB1}\ \\
\ r_{AB2}\ \\
\ \vdots\ \\
\ r_{ABn}\ \\
\end{bmatrix} \ =
\ r_{AB}
$$

Con costo $\Theta(n^2)$ per l'argomentazione precedente.

Quindi, l'algoritmo sceglie un vettore $r \in \{0,1\}^n$ *u.a.r* e calcola $A(B\cdot r)$ e $C\cdot r$.
Essendo che $\forall r\in \{0,1\}^n$, se $AB = C$ allora $ABr = Cr$, se $A(Br) \neq Cr$ allora sicuramente $AB \neq C$, portando l'algoritmo a restituire $\texttt{False}$. Restituisce $\texttt{True}$ altrimenti.


_______________________
**MatrixMult**($A,B,C$):
1. Scegli $r \in \{0,1\}^n$ *u.a.r*
2. $r_B = B\cdot r$
3. $r_{AB} = A \cdot r_B$
4. $r_C = C\cdot r$
5.	**if** $r_{AB} \neq r_C$:
	6. **return** $\texttt{False}$
7. **return** $\texttt{True}$ 	
_______________________

```ad-Osservazione
La scelta di $r\in\{0,1\}^n$ *u.a.r* ha costo $\Theta(n)$. In $\{0,1\}^n$ si hanno $2^n$ vettori, ciascuno avente la stessa probabilità di $1/2^n$ di essere scelto. Per generare $r$ basta chiamare la funzione
$$
\begin{equation*}
\texttt{urand}() = 
  \left\{ 
    \begin{aligned}
      &\ 1 \ \ \ \textnormal{w.p. }\ 1/2 \\
      &\ 0 \ \ \  \textnormal{w.p. }\ 1/2  
    \end{aligned}
  \right.
\end{equation*}
$$
per ciascuna delle $n$ entrate di $r$.
```

Si è osservato precedentemente che se $AB=C$, allora $r_{AB}=r_{C}$. Di conseguenza, se l'algoritmo restituisce $\texttt{False}$, allora sicuramente $AB \neq C$. 

Inoltre,
```ad-Osservazione
Se $AB=C$ allora l'algoritmo restituisce correttamente $\texttt{True}$.
```

Il fatto che se l'algoritmo restituisce $\texttt{False}$ implica che $AB \neq C$, non implica che se $AB\neq C$ allora l'algoritmo restituisce correttamente $\texttt{False}$.

```ad-Osservazione
Se $AB \neq C$, allora non è detto che l'algoritmo restituisca correttamente $\texttt{False}$.
```

Se $AB \neq C$ potrebbe verificarsi che 
$$
\begin{align*}
r_{AB} = r_{C}\ \ \ \textnormal{ oppure }\ \ \ r_{AB} \neq r_C
\end{align*}
$$
a seconda della scelta di $r$. Ad esempio, se $AB \neq C$ e $r = \textbf{0}$, allora vale sempre $ABr = Cr$, portando l'algoritmo a restituire incorrettamente $\texttt{True}$.
Ricapitolando:
- Se $AB=C$ allora $\forall r\in \{0,1\}^n$ vale $(ABr = ACr) \implies \textnormal{l'algoritmo restituisce } \texttt{True} \textnormal{ correttamente.}$
- Se $r_{AB} \neq r_{C}$ allora necessariamente $(AB \neq C) \implies \textnormal{l'algoritmo restituisce } \texttt{False} \textnormal{ correttamente}.$
- Se $AB \neq C$ non è detto che $r_{AB} \neq r_{C} \implies \textnormal{l'algoritmo può sbagliare restituendo } \texttt{True}$.
- Se $r_{AB} = r_{C}$ non è detto che $AB=C \implies \textnormal{l'algoritmo può sbagliare restituendo } \texttt{True}$.

Alla luce di queste considerazioni ci si chiede quindi, nel caso in cui $AB \neq C$,con quale probabilità l'algoritmo sbaglia?

```ad-Osservazione
Non ha senso chiedersi "Se l'algoritmo restituisce $\texttt{True}$, qual è la probabilità che $AB=C$?", perché questo evento non ha probabilità: o è vero o è falso.
```

Si vuole quindi dare una delimitazione alla probabilità che si verifichi questo evento, ossia dare un upper bound alla probabilità di errore.

```ad-Osservazione
Se $AB\neq C$, sia $D=(AB-C)$. Sicuramente $D \neq O$, perché almeno un elemento deve essere uguale ad $1$. Si può definire quindi $D$ come
$$
D = (d_{i,j})_{i,j:1,\ldots,n}, \exists i,j:d_{i,j}=1
$$
```

Si sfrutta l'entrata $d_{i,j}=1$ per studiare la probabilità che l'algoritmo sbagli.
L'algoritmo sbaglia nel caso in cui $AB\neq C$ e $r$ è stato scelto tale che $r_{AB} = r_{C}$. Essendo $D=AB-C$ si ha che $D\cdot r = ABr - Cr= r_{AB}-r_{C} = 0$.

Sia $d_{i,j}$ un elemento non nullo in $D$. Si considera l'$i$-esimo elemento del vettore $Dr$.
Tale elemento è ottenuto moltiplicando $r$ con l'$i-$esima riga di $D$. Quindi

$$
r_D[i] = \sum_{k=1}^n d_{i,k} \cdot r[k] \ \ \textnormal{ (mod 2)}
$$
Si tira fuori dalla sommatoria l'elemento uguale ad $1$.

$$
d_{i,j} \cdot r[j] + \sum_{k=1,k\neq j}^nd_{i,k}\cdot r[k]= r_D[i]
$$

Allora, ricordando che $r_{D}[i]=0$ e $d_{i,j}=1$, si ha 
$$
r[j] = - \sum_{k=1,k\neq j}^n d_{i,k} \cdot r[k]
$$
Se la sommatoria è uguale a $0$, allora $r[j]$ deve essere uguale a $0$ (lo è con probabilità $1/2$).
Se la sommatoria è uguale ad $1$, allora $r[j]$ deve essere uguale ad $1$ (anch'esso con probabilità $1/2$).
Quindi
$$
r[j] = - \sum_{k=1,k\neq j}^n d_{i,k} \cdot r[k] = 0\ \ \ \textnormal{ w.p. }\ \ \frac{1}{2}
$$

Dunque, la probabilità che $ABr=Cr$ quando $AB \neq C$ è al più $1/2$. 

Questa idea viene chiamata *principle of deferred decisions*: quando vi sono diverse variabili aleatorie, come le entrate $r[i]$ in $r,$ risulta spesso utile considerare alcune di esse come valori fissati, mentre le restanti rimangono aleatorie. Formalmente, questo corrisponde ad effettuare un condizionamento sui valori rivelati,ossia, quando è noto il valore assunto da alcune variabili aleatorie, bisogna calcolare le probabilità condizionate su tali valori per il resto dell'analisi.

Con questa argomentazione abbiamo dimostrato il seguente teorema

```ad-Teorema
Se $AB\neq C$, allora
$$
\mathbf{Pr}[ \texttt{MatrixMult}=\texttt{True}] \leq \frac{1}{2}
$$
```

Si osserva esplicitamente che, più entry in $D$ sono uguali ad $1$, più è bassa la probabilità errore. 

Dunque l'algoritmo:
- Se restituisce $\texttt{False}$, allora sicuramente $AB \neq C$.
- Se restituisce $\texttt{True}$, allora $AB=C$ con probabilità $1/2$.

Si vuole sviluppare ora un algoritmo che sbagli con probabilità minore.
In particolar modo, vogliamo un algoritmo che risponda correttamente con ***alta probabilità***.
```ad-Definizione
title: Definizione (Alta probabilità)
Sia $\{E_n\}$ una successione di eventi che dipende da un parametro $n$.
Si dice che $E_n$ avviene con alta probabilità se $\exists n_0 \in \mathbb{N}, \exists c > 0$ tale per cui, $\forall n \geq n_0$:
$$
\mathbf{Pr}[E_n] \geq 1 - \frac{1}{n^c}
$$

```

Informalmente, se la probabilità che l'evento non succede decresce come l'inverso di un polinomio in $n$.

Per progettare un algoritmo con probabilità inferiore di sbagliare, è sufficiente ripetere l'algoritmo più volte.
______________________________________________
**MatrixMult2**($A,B,C$):
1. **for** $t$ volte:
	2. Scegli $r \in \{0,1\}^n$ *u.a.r*
	3. $r_B = B\cdot r$
	4. $r_{AB} = A \cdot r_B$
	5. $r_C = C\cdot r$
	6. **if** $r_{AB} \neq r_C$:
		7. **return** $\texttt{False}$
8. **return** $\texttt{True}$ 	  
__________________________
```ad-Osservazione
La prima volta che si ottiene $r$ tale per cui $r_{AB} \neq r_C$ allora si può restituire sicuramente $\texttt{False}$, perché se $AB=C$, lo spazio $\{0,1\}^n$ sarebbe composto da **tutti** i vettori per cui l'algoritmo restituisce $\texttt{True}$.
```
```ad-Osservazione
Se $AB \neq C$
$$
\mathbf{Pr}[\texttt{MatrixMult2}=\texttt{True}] \leq \frac{1}{2^t}
$$
```
```ad-Osservazione
Il running time dell'algoritmo in funzione del parametro $t$ è $\Theta(tn^2)$.
In base alla scelta di $t$ si può quindi condizionare il running time dell'algoritmo, oltre alla probabilità di errore.
```

Si da ora una definizione di alta probabilità alternativa, legata maggiormente al concetto di algoritmo
```ad-Definizione
title: Definizione (Alta probabilità)
Diciamo che un algoritmo probabilistico è corretto con alta probabilità se
$$
\mathbf{Pr}[\textnormal{Algoritmo sbaglia}]\leq \frac{1}{n^c}
$$
dove $n$ è la size dell'input e $c>0$ costante.
```

Affinché $\texttt{MatrixMult2}$ sia corretto è sufficiente scegliere $t=\Theta(\log n)$. Da ciò derivano i due seguenti teoremi

```ad-Teorema
Se $AB\neq C$ e $t= \Theta(\log n)$
$$
\mathbf{Pr}[\texttt{MatrixMult2}=\texttt{True}] \leq \frac{1}{2^{\Theta(\log n)}}
$$
```
```ad-Teorema
Se $t=\Theta(\log n)$, $\texttt{MatrixMult2}$ ha running time $\Theta(n^2 \log n)$ ed è corretto con alta probabilità.
```
# Random Quick Sort
Si ricorda il funzionamento del QuickSort deterministico.
In input si ha una lista di $n$ elementi $x_1,\ldots,x_n$ presi da un universo totalmente ordinato. L'algoritmo inizia scegliendo un *pivot* dalla lista, sia quest'ultimo $x$. Confronta poi ogni altro elemento della lista in input con $x$, dividendola in due sottoliste, una contenente gli elementi minori o uguali di $x$, e l'altra contenente gli elementi strettamente maggiori di $x$. Si osserva che se i confronti vengono eseguiti in ordine naturale, ossia da sinistra verso destra della lista, allora l'ordine degli elementi in ciascuna sottolista è lo stesso della lista iniziale. L'algoritmo poi ordina ricorsivamente le sottoliste.

Nello scenario peggiore, il QuickSort impiega $\Omega(n^2)$ operazioni di confronto, e il running time del QuickSort è basato proprio sul numero di confronti che compie per ordinare una lista.
Ad esempio, si supponga che $$x_1=n, \ x_2=n-1, \ldots,x_{n-1}=2,\ x_{n}=1$$ e che la regola per la scelta del pivot sia quella di selezionare il primo elemento della lista. La prima chiamata a QuickSort sceglie quindi come pivot $x_1 = n$, esegue $n-1$ confronti e divide la lista in due sottoliste, una di size $0$ e l'altra di size $n-1$, dove gli elementi in quest'ultima hanno ordine dal più grande al più piccolo $(n-1,n-2,\ldots,2,1)$. Sceglie poi come pivot della sottolista $x_2 = n-1$, ed operando come visto in precedenza, esegue $n-2$ confronti.
Proseguendo in questa maniera, il numero di confronti impiegato dal QuickSort risulta essere 
$$
(n-1) + (n-2) + \ldots 2 + 1 = \sum_{i=0}^{n-1}i = \frac{n(n-1)}{2}
$$
Conseguendo nel running time di $\Omega(n^2)$ precedentemente espresso.

Essendo che il running time dell'algoritmo dipende dal numero di confronti effettuati, e che il numero di confronti effettuati dipende dalla scelta del pivot, una sua scelta ragionevole può conseguire in un numero minore di confronti.
Ad esempio, se il pivot dividesse le due sottoliste in maniera tale che la loro size risulti essere al più $\lceil n/2 \rceil,$ il numero di confronti risulterebbe essere descrivibile dalla seguente relazione di ricorrenza  
$$
C(n) \leq 2 \cdot C(\lceil n/2 \rceil) + \Theta(n)
$$
che se svolta porta a $C(n) = \mathcal{O}(n \log n).$
Intuitivamente, ad ogni iterazione dell'algoritmo, esiste una lista "buona" di pivot, ovvero quella contenente gli elementi che, se selezionati come pivot, dividono la lista di elementi da ordinare in due sottoliste, le quali dimensioni si differenziano al più per un fattore costante.
Si vuole far si che l'algoritmo selezioni pivot buoni sufficientemente spesso, per garantire che questo termini velocemente.
```ad-Claim
title: Claim
Se l'algoritmo seleziona i pivot uniformly at random, è estremamente difficile che vengano scelti ripetutamente pivot non buoni.
```

Si fornisce ora una variante probabilistica del QuickSort. Sia $I$ un vettore di numeri
______
**RandomQS**$(I)$:
1. **If** $|I| \leq 1:$
	1. **return** $I$
2. Scegli $a \in I$ u.a.r. e rimuovilo da $I$
3. $L=[\  ]$, $R = [ \ ]$ 
4. **for each** $u \in I:$
	1. **if** $u \leq a:$
		1. Aggiungi $u$ ad $L$
	2. **else**:
		1. Aggiungi $u$ ad $R$
5. **return** **RandomQS**$(L)$, $a$, **RandomQS**$(R)$
_____

Prima di effettuare l'analisi formale dell'algoritmo, si compiono le seguenti osservazioni
```ad-Osservazione
Due numeri vengono confrontati se e solo se uno di essi è scelto come pivot
```
```ad-Osservazione
Due numeri molto lontani vengono confrontati con probabilità bassa, mentre due numeri molto vicini vengono confrontati con probabilità alta.
```

Si analizza quest'ultima osservazione. Sia $X$ la variabile aleatoria che conta il numero di confronti eseguiti dall'algoritmo
$$
X = \# \textnormal{ numero di confronti eseguiti da RandomQS}
$$
Siano $y_1,y_2,\ldots,y_n$ gli $n$ elementi di $I$ ordinati, ossia 
$$
y_1 \leq y_2 \leq \ldots \leq y_n
$$
Per ogni $i,j=1,\ldots,n$ si definisce 
$$
\begin{equation*}
X_{i,j} = 
  \left\{ 
    \begin{aligned}
      &\ 1 \ \ \ \textnormal{se } y_i \textnormal{ e } y_j \textnormal{ vengono confrontati dall'algoritmo}\  \\
      &\ 0 \ \ \  \textnormal{altrimenti }\   
    \end{aligned}
  \right.
\end{equation*}
$$

Si vuole valutare quando $y_i$ ed $y_j$, con $i < j$ vengono confrontati. Si osserva inizialmente che  $y_i$ ed $y_{i+1}$ vengono confrontati con probabilità $1$, ossia $\mathbf{Pr}\left[X_{i,i+1}=1 \right] =1,$ questo perché finiranno necessariamente nella stessa lista quando avviene la separazione in due sottoliste, a meno che uno di essi non venga scelto come pivot, e anche in quel caso verranno comunque confrontati.
Siano $y_i,y_j$ con $i < j$ 
![center|500][Qs1.excalidraw]
$y_i$ ed $y_j$ vengono confrontati quando, la prima volta che il pivot viene selezionato da quella lista, o è $y_i$ o è $y_j.$ Se per la prima volta l'elemento scelto come pivot è un elemento intermedio $y_k$ con $i<k<j,$ allora $y_i$ ed $y_j$ non verranno mai confrontati, perché verranno messi in liste distinte. Siccome il pivot è scelto *u.a.r.*, ciascun elemento ha la stessa probabilità di essere un pivot. Siano quindi 
$$
\mathcal{E_1} = y_i\textnormal{ viene scelto come pivot}
$$
$$
\mathcal{E_2} = y_j\textnormal{ viene scelto come pivot}
$$
due eventi, dove si osserva che $\mathcal{E_1} \cap \mathcal{E_2} = \emptyset$, allora la probabilità che $y_i$ o $y_j$ sia un pivot è 
$$
\mathbf{Pr} \left[ \mathcal{E_1} \cup \mathcal{E_2} \right] = \mathbf{Pr}\left[\mathcal{E_1}\right] + \mathbf{Pr}\left[\mathcal{E_2}\right] = \frac{1}{j-i+1} + \frac{1}{j-i+1} = \frac{2}{j-i+1}
$$
dove $j-i+1$ è il numero di elementi nell'intervallo preso in analisi.
```ad-Osservazione
Se $j=i+1$, ossia $y_i$ ed $y_j$ sono elementi consecutivi, allora
$$
\frac{2}{j-i+1} = \frac{2}{2} = 1
$$
```
```ad-Osservazione
Essendo $X_{i,j}$ una v.a. che assume solo valori $0$ ed $1$,
$$
\mathbf{E}\left[X_{i,j}\right] = \mathbf{Pr}\left[X_{i,j}=1\right]
$$
```
```ad-Osservazione
Siccome $y_i$ ed $y_j$ vengono confrontati se e solo se uno dei due è un pivot, la probabilità che $X_{i,j}=1$ è proprio pari alla probabilità che uno dei due sia un pivot. Quindi
$$
\mathbf{E}\left[X_{i,j}\right] = \mathbf{Pr}\left[X_{i,j}=1\right] = \frac{2}{j-i+1}
$$
```
Date queste osservazioni, si fa ora un'analisi formale.
```ad-Teorema
Si supponga che, ogni volta che un pivot viene scelto per $\texttt{RandomQS}$, tale selezione avvenga indipendentemente e u.a.r. tra tutti i possibili pivot. Allora, per ogni input, il valore atteso di confronti fatto da $\texttt{RandomQS}$ è
$$
2 n \cdot \ln (n) + \mathcal{O}(n)
$$
```
```ad-Dimostrazione
Siano $y_1,y_2,\ldots,y_n$ gli stessi valori di input $x_1,x_2,\ldots,x_n$ ordinati in maniera non decrescente $(y_1\leq y_2\leq \ldots \leq y_n).$
Per $i<j$, sia 
$$
\begin{equation*}
X_{i,j} = 
  \left\{ 
    \begin{aligned}
      &\ 1 \ \ \ \textnormal{se } y_i \textnormal{ e } y_j \textnormal{ vengono confrontati dall'algoritmo}\  \\
      &\ 0 \ \ \  \textnormal{altrimenti }\   
    \end{aligned}
  \right.
\end{equation*}
$$
Allora, il mumero totale di confronti $X$ soddisfa 
$$
X = \sum_{i=1}^{n-1}\sum_{j=i+1}^n X_{i,j}
$$
Per la linearità del valore atteso si ha
$$
\mathbf{E}\left[X\right] = \mathbf{E}\left[\sum_{i=1}^{n-1}\sum_{j=i+1}^n X_{i,j}\right] = \sum_{i=1}^{n-1}\sum_{j=i+1}^n \mathbf{E}\left[X_{i,j}\right]
$$
Siccome $X_{i,j}$ assume solo valore $0$ ed $1$, $\mathbf{E}\left[X_{i,j}\right] = \mathbf{Pr}\left[X_{i,j}=1\right]$. Per le osservazioni fatte in precedenza, si ha che $\mathbf{Pr}\left[X_{i,j}=1\right] = 2/(j-i+1)$.
Usando la sostituzione $k=j-i+1$ si ha
$$
\begin{align*}
\mathbf{E}\left[X\right] &= \sum_{i=1}^{n-1}\sum_{j=i+1}^n \frac{2}{j-i+1}\ \ \overset{(1)}{=} \ \ \sum_{i=1}^{n-1}\sum_{k=2}^{n-i+1} \frac{2}{k} \\
\\
&\overset{(2)}{=} \sum_{k=2}^{n}\sum_{i=1}^{n+1-k} \frac{2}{k} = \sum_{k=2}^n (n+1-k) \frac{2}{k} \\
\\
&= \left(\left(n+1 \right)\sum_{k=2}^{n} \frac{2}{k} \right) -2(n-1) \\
\\
&= (2n+2) \sum_{k=1}^{n} \frac{1}{k} - 4n
\end{align*}
$$

Dove:
- (1): $$\sum_{j=i+1}^n \frac{1}{j-i+1} = \frac{1}{2}+ \frac{1}{3} + \ldots + \frac{1}{n-i+1}$$ 

- (2): Invece di far variare $k$ da $i$, si somma facendo variare $i$ da $k$. 
![center|500](RandomQs2.excalidraw)

Ricordando che $\mathcal{H}_n = \sum_{k=1}^n \frac{1}{k}$ soddisfa $\mathcal{H}_n = \ln(n) + \Theta(1)$ si ha che 
$$
\mathbf{E}\left[X\right] = 2n \cdot \ln(n) + \Theta(n) = \Theta(n \log n)
$$

```
# Calcolo del mediano
Si studia ora un algoritmo probabilistico per il calcolo del mediano di una variabile aleatoria.
Sia $X$ una variabile aleatoria. Il *mediano* di $X$ è un valore $m$ tale per cui 
$$
\mathbf{Pr}\left[X \leq m\right] \geq \frac{1}{2} \ \textnormal{ e }\ \mathbf{Pr}\left[X\geq m\right] \geq \frac{1}{2}
$$

Ad esempio, per una variabile aleatoria discreta distribuita uniformemente su un numero dispari di valori distinti e ordinati $x_1,x_2,\ldots,x_{2k+1},$ il mediano è il valore centrale $x_{k+1}.$ Per una variabile aleatoria discreta distribuita uniformemente su un numero pari di valori distinti e ordinati $x_1,x_2,\ldots,x_{2k},$ un qualsiasi valore nel range $(x_k,x_{k+1})$ è un mediano.

Il problema del calcolo del mediano può essere formalizzato come segue 

```ad-Problema
**Input:** Un insieme $S$ di $n=2k+1$ elementi presi da un universo totalmente ordinato.

**Output:** Il $k+1$-esimo elemento più grande in $S$.
```

Il mediano può venir calcolato facilmente ordinando in tempo $\mathcal{O}(n \log n),$ e selezionando l'elemento intermedio.  Si mostra una soluzione probabilistica più efficiente.

L'idea dell'algoritmo è quella di trovare due elementi che risultino essere vicini nella sequenza ordinata di elementi contenuti in $S,$ e tali per cui il mediano si trovi tra essi. Formalmente, si cercano due elementi $d,u \in S$ tali per cui
1. $d \leq m \leq u$ , ossia il mediano $m$ si trova tra $d$ e $u$,
2. per $C=\{s\in S: d \leq m \leq u\}$ si ha che $|C|=\mathcal{o}(n/\log n),$ ossia, il numero di elementi tra $d$ ed $u$ è piccolo.

Una volta che questi due elementi vengono identificati, il mediano può esser trovato facilmente in tempo lineare. L'algoritmo conta, in tempo lineare, il numero $l_d$ di elementi in $S$ più piccoli di $d$ ed ordina, in tempo sublineare o $\mathcal{o}(n),$ l'insieme $C.$ Essendo  $|C|=\mathcal{o}(n/\log n),$ l'insieme $C$ può venir ordinato in tempo $\mathcal{o}(n)$ utilizzando un qualsiasi algoritmo di ordinamento standard che impiega tempo $\mathcal{O}(k \log k)$ per ordinare $k$ elementi. Il $(\lfloor n/2 \rfloor - l_d +1)-$esimo elemento nell'insieme $C$ ordinato è il mediano $m,$ essendovi esattamente  $\lfloor n/2 \rfloor$ elementi in $S$ più piccoli di $m$: $\lfloor n/2 - l_d \rfloor$ nell'insieme $C$ ed $l_d$ nell'insieme $S \backslash C.$
Per trovare gli elementi $d$ ed $u$, si campiona *con reinserimento* un multi-insieme $R$ di $\lceil n^{3/4} \rceil$ elementi di $S.$ Essendo che il campionamento avviene con reinserimento, ogni elemento inserito in $R$ viene scelto i.u.a.r. dall'insieme $S$. Quindi, un elemento di $S$ può apparire più volte in $R$.
Essendo $R$ un campione casuale di $S$, ci si aspetta che $m$, l'elemento mediano di $S$, sia vicino all'elemento mediano di $R$. Si scelgono quindi $d$ ed $u$ come elementi di $R$ che circondano il mediano di $R$.
In particolar modo, per garantire che l'insieme $C$ contenga con alta probabilità $m$, si fissano $d$ ed $u$ in maniera tale che siano, rispettivamente, il $\lfloor n^{3/4}/2 - \sqrt{n} \rfloor-$esimo e il $\lceil n^{3/4}/2 + \sqrt{n} \rceil-$esimo elemento della sequenza degli elementi ordinati di $R$. 

Si fornisce il seguente algoritmo
__________
**MedianAlgorithm**
1. Scegli un multi-set $R$ di $\lceil n^{3/4} \rceil$ elementi di $S,$ scelti *i.u.a.r.* e con reinserimento.
2. Ordina l'insieme $R.$
3. Sia $d$ il $\lfloor n^{3/4}/2 - \sqrt{n} \rfloor-$esimo elemento più piccolo nell'insieme ordinato $R$. 
4. Sia $u$ il $\lceil n^{3/4}/2 + \sqrt{n} \rceil-$esimo elemento più piccolo nell'insieme ordinato $R.$
5. Confrontando ogni elemento in $S$ con $d$ ed $u$, calcola l'insieme $C=\{s\in S: d \leq m \leq u\}$ e i numeri $l_d = |\{x\in S: x <d\}|$ e $l_u = |\{x \in S: x>u\}|$ 
6. Se $l_d > n/2$ o $l_u > n/2$ allora **FAIL**
7. Se $|C| \leq 4n^{3/4}$ ordina l'insieme $C,$ altrimenti **FAIL**
8. Restituisci il $(\lfloor n/2 \rfloor - l_d +1)-$esimo elemento nell'insieme ordinato $C.$
_____

L'analisi dell'algoritmo mostra che la scelta della dimensione di $R$ e le scelte per $d$ ed $u$ sono adatte per garantire che:
- L'insieme $C$ risulta essere abbastanza grande da includere $m$ con alta probabilità
- L'insieme $C$ è sufficientemente piccolo per poter venir ordinato in tempo sublineare con alta probabilità

Si mostra ora che, indipendentemente dalle scelte casuali effettuate durante la procedura, l'algoritmo termina sempre in tempo lineare, e che questo o restituisce in output il risultato corretto o fallisce.
```ad-Teorema
title: Teorema
L'algoritmo termina in tempo lineare, e se non restituisce in output **FAIL**, allora restituisce correttamente l'elemento mediano dell'insieme $S$ in input.
```
```ad-Dimostrazione
La correttezza dell'algoritmo segue dal fatto che questo può restituire una risposta sbagliata solo se il mediano $m$ non si trovi nell'insieme $C$. Ma nel caso in cui questo sia vero, allora $l_d > n/2$ oppure $l_u > n/2,$ e quindi il passo $6$ dell'algoritmo garantisce che questo restituisca **FAIL.**
Similmente, sino a quando $C$ rimane sufficientemente piccolo, il lavoro totale effettuato è lineare in $S.$ Il passo $7$ garantisce che l'algoritmo non impieghi più di tempo lineare, infatti, se l'ordinamento di $C$ richiede troppo tempo vista la sua dimensione, l'algoritmo restituisce **FAIL** senza ordinare.
```

Si vuole ora delimitare la probabilità che l'algoritmo restituisca **FAIL.** Si identificano quindi tre eventi tali per cui, se nessuno di questi capita, allora l'algoritmo non fallisce.
Siano 
$$
\mathcal{E_1}: Y_1 = |\{r\in R| r \leq m\}| < \frac{1}{2}n^{3/4}-\sqrt{n}
$$
$$
\mathcal{E_2}: Y_2 = |\{r\in R| r \geq m\}| < \frac{1}{2}n^{3/4}-\sqrt{n}
$$
$$
\mathcal{E_3} : |C| > 4n^{3/4}
$$
```ad-Lemma
L'algoritmo fallisce se e solo se almeno uno degli eventi $\mathcal{E_1},\mathcal{E_2}$ o $\mathcal{E_3}$ si verifica.
```
```ad-Dimostrazione
Il fallimento nel passo $7$ dell'algoritmo è equivalente all'evento $\mathcal{E_3}$.
Il fallimento nel passo $6$ avviene se e solo se $l_d > n/2$ o $l_u > n/2$. Ma per $l_d > n/2$ il $(n^{3/4}/2 - \sqrt{n})-$esimo elemento più piccolo di $R$ deve essere più grande di $m,$ è quindi equivalente all'evento $\mathcal{E_1}.$ Similmente, $l_u > n/2$ è equivalente all'evento $\mathcal{E_2}.$
```
```ad-Lemma
$$
\mathbf{Pr}\left[\mathcal{E}_1\right] \leq \frac{1}{4} n^{-1 / 4}
$$
```
```ad-Dimostrazione
Sia $X_i$ una variabile aleatoria definita come segue
$$
X_i= \begin{cases}1 & \text { se l' } i \text {-esimo campione è minore o uguale al mediano } \\ 0 & \text { altrimenti }\end{cases}
$$

Le variabili $X_i$ per ogni $i$ sono indipendenti, essendo che il campionamento viene fatto con reinserimento. Essendovi $(n-1)/2 +1$ elementi in $S$ che sono minori o uguali del mediano, la probabilità che un elemento $S$ scelto casualmente sia minore o uguale del mediano può essere scritta come
$$
\mathbf{Pr}\left[X_i=1\right]=\frac{(n-1) / 2+1}{n}=\frac{1}{2}+\frac{1}{2 n} .
$$

L'evento $\mathcal{E}_1$ è equivalente a
$$
Y_1=\sum_{i=1}^{n^{3 / 4}} X_i<\frac{1}{2} n^{3 / 4}-\sqrt{n}
$$

Essendo $Y_1$ la somma di prove che costituiscono un processo di Bernoulli, è una variabile aleatoria binomiale con parametri $n^{3 / 4}$ e $1 / 2+1 / 2 n$. Quindi si ha che, facendo riferimento al calcolo della varianza di una v.a. binomiale:
$$
\begin{aligned}
Var\left[Y_1\right] & =n^{3 / 4}\left(\frac{1}{2}+\frac{1}{2 n}\right)\left(\frac{1}{2}-\frac{1}{2 n}\right) \\
& =\frac{1}{4} n^{3 / 4}-\frac{1}{4 n^{5 / 4}} \\
& <\frac{1}{4} n^{3 / 4}
\end{aligned}
$$

Applicando la disuguaglianza di Chebyshev si ottiene che
$$
\begin{aligned}
\mathbf{Pr}\left[\mathcal{E}_1\right] & =\mathbf{Pr}\left[Y_1<\frac{1}{2} n^{3 / 4}-\sqrt{n}\right] \\
\\
& \leq \mathbf{Pr}\left[\left|Y_1-\mathbf{E}\left[Y_1\right]\right|>\sqrt{n}\right] \\
\\
& \leq \frac{Var\left[Y_1\right]}{n} \\
\\
& <\frac{\frac{1}{4} n^{3 / 4}}{n}=\frac{1}{4} n^{-1 / 4}
\end{aligned}
$$

```

Similmente si può ottenere lo stesso bound sulla probabilità dell'evento $\mathcal{E}_2.$
Si da ora una delimitazione alla probabilità dell'evento $\mathcal{E_3}.$

```ad-Lemma
$$
\operatorname{Pr}\left[\mathcal{E}_3\right] \leq \frac{1}{2} n^{-1 / 4}
$$
```
```ad-Dimostrazione
Se si verifica $\mathcal{E}_3$, ossia $|C|>4 n^{3 / 4}$, allora si verifica almeno uno tra quesi due eventi:
$\mathcal{E}_{3,1}$ : almeno $2 n^{3 / 4}$ elementi di $C$ sono più grandi del mediano;
$\mathcal{E}_{3,2}$ : almeno $2 n^{3 / 4}$ elementi di $C$ sono più piccoli del mediano.
Si da una delimitazione alla probabilità che il primo evento si verifichi. Tale bound, per simmetria, vale anche per il secondo. Se vi sono almeno $2 n^{3 / 4}$ elementi di $C$ maggiori del mediano, allora la posizione di $u$ negli elementi di $S$ ordinati è almeno $\frac{1}{2} n+2 n^{3 / 4}$ e quindi $R$ ha almeno $\frac{1}{2} n^{3 / 4}-\sqrt{n}$ campioni tra gli $\frac{1}{2} n-2 n^{3 / 4}$ elementi più grandi in $S$.

Sia $X$ il numero di campioni tra gli $\frac{1}{2} n-2 n^{3 / 4}$ elementi più grandi in $S$. Sia $X=\sum_{i=1}^{n^{3 / 4}} X_i$, dove
$$
X_i= \begin{cases}1 & \text { se l' } i \text {-esimo campione è tra gli } \frac{1}{2} n-2 n^{3 / 4} \text { elementi più grandi in } S, \\ 0 & \text { altrimenti }\end{cases}
$$

$X$ è una variabile aleatoria binomiale, e quindi si ha che
$$
\mathbf{E}[X]=\frac{1}{2} n^{3 / 4}-2 \sqrt{n}
$$
e
$$
\operatorname{Var}[X]=n^{3 / 4}\left(\frac{1}{2}-2 n^{-1 / 4}\right)\left(\frac{1}{2}+2 n^{-1 / 4}\right)=\frac{1}{4} n^{3 / 4}-4 n^{1 / 4}<\frac{1}{4} n^{3 / 4}
$$

Applicando la disuguaglianza di Chebyshev si ottiene che
$$
\begin{aligned}
\mathbf{Pr}\left[\mathcal{E}_{3,1}\right] & =\mathbf{Pr}\left[X \geq \frac{1}{2} n^{3 / 4}-\sqrt{n}\right]  \leq \mathbf{Pr}[|X-\mathbf{E}[X]| \geq \sqrt{n}] \\
\\
&\leq \frac{Var[X]}{n}<\frac{\frac{1}{4} n^{3 / 4}}{n}=\frac{1}{4} n^{-1 / 4} .
\end{aligned}
$$

Similmente
$$
\mathbf{Pr}\left[\mathcal{E}_{3,2}\right] \leq \frac{1}{4} n^{-1 / 4}
$$
e quindi
$$
\mathbf{Pr}\left[\mathcal{E}_3\right] \leq \mathbf{Pr}\left[\mathcal{E}_{3,1}\right] + \mathbf{Pr}\left[\mathcal{E}_{3,2}\right] \leq \frac{1}{2} n^{-1 / 4}
$$
```

Combinando le delimitazioni ottenute per i tre eventi, tali per cui il verificarsi di almeno uno di questi comporta il fallimento dell'algoritmo, si ha che la probabilità che l'algoritmo fallisca è pari a 
$$
\mathbf{Pr}\left[\mathcal{E}_{1}\right] + \mathbf{Pr}\left[\mathcal{E}_{2}\right] + \mathbf{Pr}\left[\mathcal{E}_{3}\right] \leq n^{-1/4}
$$

Si può ripetere l'algoritmo mostrato sino a quando questo non trova il mediano con successo. Così facendo, si può ottenere un algoritmo iterativo che non fallisce mai, ma con un tempo di esecuzione casuale. I campioni presi nelle esecuzioni successive dell'algoritmo sono indipendenti, quindi il successo di ogni esecuzione è indipendente dalle altre. Da quest'ultima osservazione, si fa presente che il numero di esecuzioni necessarie affinché si ottenga il mediano con successo è una variabile aleatoria geometrica.
# Contention Resolution
Si hanno $n$ processi $P_1,P_2,...,P_n$, i quali competono per l'accesso ad un database condiviso. L'intervallo di tempo durante il quale avviene questa dinamica viene suddiviso in *rounds* discreti. Al database può accedere un singolo processo in ciascun round. 
Se due o più processi tentano di accedere al database simultaneamente, ossia all'inizio dello stesso round, allora questi vengono rigettati e, per la durata di quel round, nessun processo accede al database.
Nonostante ogni processo voglia accedere alla risorsa condivisa il più frequentemente possibile, è inutile che ciascuno di essi provi a farlo per ogni round perché, se così fosse, nessuno di essi accederebbe al database, rendendo la risorsa inutilizzata.
Si vogliono quindi suddividere i rounds facendo si che questi vengano assegnati equamente a ciascun processo, in maniera tale che ciascuno di essi possa accedere al database su base regolare.

Si fa presente inoltre come i processi non possano comunicare tra loro. Di conseguenza, un processo che tenta di accedere alla risorsa in un certo round, non può sapere se gli altri stiano compiendo la stessa operazione.

Si progetta ora un protocollo probabilistico per la risoluzione di questo problema:
In ciascun round, ogni processo decide se provare ad accedere al database con probabilità $p>0$, indipendentemente dalla decisione effettuata dagli altri processi. 
Se esattamente un processo decide di provare l'accesso, allora questo avrà successo. Se ci provano invece due o più processi, allora questi verranno tagliati fuori, come da definizione del problema. Se in un round, nessuno dei processi prova l'accesso, allora il round risulta "sprecato". 

Questa strategia si trova al principio del *symmetry-breaking paradigm*: se tutti i processi operassero in maniera prefissata, provando ripetutamente ad accedere al database durante lo stesso istante, allora non vi sarebbe progresso. 

Si vuole ora scegliere $p$ affinché la probabilità che un processo possa accedere con successo al database sia massimizzata.
Sia $P_i$ uno dei processi in input, e sia $t$ un round arbitrario.
Si definisce l'evento

$$
A[i,t] = \textnormal{Il processo } P_i \textnormal{ prova ad accedere al database al round } t
$$

Si ha che ogni processo prova ad accedere al database ad ogni round con probabilità $p$, quindi $\forall i = 1,...,n$ e $\forall t$ si ha

$$
\mathbf{Pr}\Bigr[A[i,t]\Bigr] = p
$$

Il complementare dell'evento $A[i,t]$ è definito come segue

$$
A[i,t]^c = \textnormal{Il processo } P_i \textnormal{ non prova ad accedere al database al round t}
$$

e, ovviamente 

$$
\mathbf{Pr}\Bigr[A[i,t]^c \Bigr] = 1-\mathbf{Pr}\Bigr[A[i,t]\Bigr]  = 1- p
$$

A partire da questi eventi, si definisce l'evento tale per cui un processo riesce ad accedere con successo al database in un dato round:

$$
S[i,t] = \textnormal{Il processo } P_i \textnormal{ accede con successo al database al round } t
$$

```ad-Osservazione
Il processo $P_i$ accede con successo al database al round $t$ se e solo se tale processo prova ad accedere al round $t$ e **tutti** gli altri processi non provano ad accedere al database al round $t$.
```

Quindi, si ha che 
$$
S[i,t] = A[i,t]\ \cap\ \Bigl( \bigcap_{j\neq i}A[j,t]^c \Bigl) 
$$

Dalla definizione del protocollo, si ha che tutti gli eventi sono indipendenti tra loro, ossia $\forall i,j = 1,...,n,\ j\neq i$ 
$$ \mathbf{Pr}\Bigr[A[i,t] \cap A[j,t]\Bigr] = \mathbf{Pr}\Bigr[A[i,t]\Bigr]\cdot\mathbf{Pr}\Bigr[A[j,t]\Bigr]$$

Da questa osservazione si ottiene la probabilità che si verifichi l'evento $S[i,t]$:

$$
\mathbf{Pr}\Bigr[S[i,t]\Bigr] = \mathbf{Pr}\Bigr[A[i,t]\Bigr]\cdot \prod_{j\neq i}\mathbf{Pr}\Bigr[A[j,t]^c\Bigr] = p(1-p)^{n-1}
$$

Si ha quindi la probabilità che $P_i$ acceda con successo al database al round $t$.

Rimane ora da scegliere $p$ per massimizzare tale probabilità. 

```ad-Osservazione
Per $p=0$ e $p=1$, si ha che $\forall i=1,...,n$ e $\forall t$ 

$$
\mathbf{Pr}\Bigr[S[i,t]\Bigr] = 0
$$
Infatti,
- per $p=0$, per ogni round nessun processo prova ad accedere al database
- per $p=1$, ad ogni round ogni processo prova ad accedere al database, rimanendo esclusi per definizione del problema
```

La funzione $f(p) = p(1-p)^{n-1}$ assume valori positivi per per $0<p<1$. Si vuole vedere per quale valore di $p$ tale funzione risulti essere massimizzata. Per fare ciò, si calcola la sua derivata prima 

$$
f^{'}(p) = (1-p)^{n-1} - (n-1)p(1-p)^{n-2}
$$

La quale assume valore $0$ per $p=1/n$, dove $f(p)$ risulta essere massimizzata. Quindi si ha che per tale valore di $p$, la probabilità che si verifichi $S[i,t]$ risulta essere massima.
Per $p=1/n$ si ha quindi

$$
\mathbf{Pr}\Bigr[S[i,t]\Bigr] = \frac{1}{n} \Bigl(1- \frac{1}{n}\Bigl)^{n-1}
$$

Si fanno le seguenti osservazioni sull'andamento asintotico di quest'ultima espressione:

```ad-Osservazione
title: Osservazione
1. La funzione $\Bigl(1 - \frac{1}{n}\Bigl)^n$ ha convergenza monotona da $\frac{1}{4}$ salendo fino a $\frac{1}{e}$ per $n\geq 2$
$\\$
1. La funzione $\Bigl(1 - \frac{1}{n}\Bigl)^{n-1}$ ha convergenza monotona da $\frac{1}{2}$ scendendo fino a $\frac{1}{e}$ per $n \geq 2$
```

Si ha quindi che per $n\geq 2$, 
$$\frac{1}{e} \leq \Bigl(1-\frac{1}{n}\Bigl)^{n-1} \leq \frac{1}{2}$$ 
ciò implica che

$$
\frac{1}{(en)}\ \leq\ \ \mathbf{Pr}\Bigr[S[i,t]\Bigr]\ \ \leq\ \ \frac{1}{(2n)} 
$$

ossia 
$$
\mathbf{Pr}\Bigr[S[i,t]\Bigr] = \Theta\Bigl(\frac{1}{n}\Bigl)
$$


Assodato che $p=1/n$ massimizzi la probabilità che un processo acceda con successo al database in un dato round, si vuole studiare quanto tempo sia necessario affinché un processo $P_i$ acceda con successo alla risorsa almeno una volta.

Si definisce l'evento

$$
F[i,t] = \textnormal{Il processo } P_i \textnormal{ non accede al database con successo in ciascun round da } 1 \textnormal{ a } t
$$

Si osserva che

$$
F[i,t]\ \ =\ \ \Bigr[ \ \ \bigcap_{r=1}^t S[i,r]^c \ \ \Bigr]
$$

Ed essendo tali eventi indipendenti, si ha 

$$
\mathbf{Pr}\Bigr[F[i,t]\Bigr] \ \ =\ \ \mathbf{Pr}\Bigr[ \ \bigcap_{r=1}^t S[i,r]^c \  \Bigr] \ =\ \prod_{r=1}^t \mathbf{Pr} \Bigr[\ S[i,r]^c \ \Bigr] \ = \ \Bigr[\ 1 - \frac{1}{n} \Bigl(1 - \frac{1}{n} \Bigl)^{n-1}\ \Bigr]^t
$$

Per quanto osservato in precedenza,

$$
  \frac{1}{n}\Bigl(1-\frac{1}{n}\Bigl)^{n-1}\ \geq \ \frac{1}{en}\ \ \ \ \ \ \implies \ \ \ \ \ \mathbf{Pr}\Bigr[F[i,t]\Bigr] \ =\ \prod_{r=1}^t \mathbf{Pr} \Bigr[\ S[i,r]^c \ \Bigr] \ \leq\ \Bigl( 1 - \frac{1}{en}\Bigl)^t
$$

Ponendo $t=en$, si può fare riferimento sempre all'osservazione fatta in precedenza. Essendo che $en$ non è un intero, si prende $t = \lceil en \rceil$ 

$$
\mathbf{Pr}\Bigr[F[i,t]\Bigr]  \ \leq\ \Bigl( 1 - \frac{1}{en}\Bigl)^{\lceil en \rceil} \ \leq\ \Bigl(1 - \frac{1}{en}\Bigl)^{en}\ \leq\ \frac{1}{e}
$$

Tale disuguaglianza mostra come la probabilità che il processo $P_i$ non abbia successo in ciascuno dei round che vanno da $1$ ad $\lceil en \rceil$ è delimitata superiormente dalla costante $e^{-1}$, indipendente da $n$. 
Si vuole ora ridurre la probabilità che ciò accada.
Ponendo $t=\lceil en \rceil \cdot (c \ln n)$ si ha che 

$$
\mathbf{Pr}\Bigr[F[i,t]\Bigr]  \ \leq\ \Bigl( 1 - \frac{1}{en}\Bigl)^t \leq \biggl(\Bigl( 1 - \frac{1}{en}\Bigl)^{\lceil en \rceil}\biggl)^{c \ln n} \ \leq\ e^{-c \ln n} = n^{-c}
$$

Quindi, asintoticamente, si ha che la probabilità che $P_i$ non abbia ancora avuto successo dopo $\Theta(n)$ rounds è delimitata da una costante, mentre per $\Theta(n \ln n)$ rounds, questa probabilità assume lo stesso comportamento dell'inverso di un polinomio in $n$. 

Ci si chiede ora quanti rounds siano necessari tali per cui vi sia un'alta probabilità affinché tutti i processi accedano con successo al database almeno una volta.

Per studiare tale quantità, si definisce l'evento 

$$
F_t = \textnormal{Il protocollo fallisce dopo } t  \text{ round } 
$$

Dove il protocollo *fallisce* dopo $t$ round se esiste almeno un processo che non è riuscito ad accedere con successo al database.
Si vuole trovare un valore ragionevole di $t$ affinché $\textbf{Pr}[F_t]$ sia sufficientemente piccolo.
Si osserva esplicitamente che si verifica $F_t$ se e solo se accade uno degli eventi $F[i,t]$. Allora
$$
F_t\ \ =\ \ \bigcup_{i=1}^n F[i,t]
$$
la quale risulta essere un'unione di evento non indipendenti tra loro, questo perché se un processo fallisce l'accesso alla risorsa condivisa, allora ve ne deve necessariamente essere un'altro che ha compiuto la stessa operazione. Si può fare quindi riferimento all'*union bound*:

$$
\mathbf{Pr}[F_t] \leq \sum_{i=1}^n \mathbf{Pr}\Bigr[F[i,t]\Bigr]
$$

L'espressione a destra della disuguaglianza è una somma di $n$ termini, ciascuno dei quali assume lo stesso valore. Per rendere la probabilità di $F_t$ piccola, bisogna rendere ciascuno dei termini a destra significativamente più piccoli di $1/n$. Per $t=\Theta(n)$ ciascuno di tali termini risulta essere delimitato solamente da una costante. Per $t= \lceil en \rceil \cdot (c \ln n)$, si ha che $\mathbf{Pr}\bigr[ F[i,t]\bigr] \leq n^{-c}$ per ogni $i$. In particolar modo, per $t=2\lceil en \rceil \ln n$ si ha 

$$
\mathbf{Pr}[F_t] \leq \sum_{i=1}^n \mathbf{Pr}\Bigr[F[i,t]\Bigr] \leq n \cdot n^{-2} = n^{-1}
$$
e ciò mostra il seguente teorema
```ad-Teorema
Con probabilità almeno $1-n^{-1}$, tutti i processi accedono al database con successo almeno una volta entro $t=2\lceil en \rceil \ln n$ rounds.
```