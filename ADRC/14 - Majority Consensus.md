Il problema di trovare un _agreement_ tra i nodi di una rete distribuita è uno dei problemi più importanti nei sistemi distribuiti moderni. In questa sezione si studia tale problema, noto come **Majority Consensus Problem** e si analizza un protocollo per tale problema, detto $k$-Majority Protocol ($k\text{-MAJ}$).
# Majority Consensus Problem
Si considera un grafo di $n$ nodi in cui ogni nodo può trovarsi in due stati, o colori: $0$ o $1$. Informalmente, il problema del **Majority Consensus** consiste nel progettare un protocollo con l'obiettivo di far *convergere* tutti i nodi verso uno dei due colori possibili. In particolare, si assume che inizialmente ci sia un certo sbilanciamento $s$ (*bias*) nella rete, ossia la maggioranza dei nodi è colorato di uno dei due colori, e il sistema deve convergere verso tale colore di maggioranza.
Il problema viene affrontato in un sistema sincrono a tempo discreto, dunque per ogni istante di tempo $t$ si può definire esplicitamente la configurazione del sistema al tempo $t$, cioè il numero di nodi che sono colorati di $0$ e che sono colorati di $1$ al tempo $t$.

Formalmente, sia $G=([n],E)$ un grafo di $n$ nodi, sia $C=\{ 0,1 \}$ un insieme di colori e sia $\mathbf{x}_{t}:[n]\to C$ una colorazione (o configurazione) della rete $G$ al tempo $t$. Partendo dall'assunzione che la colorazione iniziale $\mathbf{x}_{0}$ abbia un certo *bias* $s = s_{0}$, l'obiettivo è quello di far convergere il sistema ad un certo tempo $\tau$ finito in una colorazione $x_{\tau}$ **monocromatica**, cioè dove ogni nodo è colorato col colore più popolare in $\mathbf{x}_{0}$. In altri termini, si raggiunge una colorazione finale nella quale tutti i nodi sono colorati col colore che era di maggioranza inizialmente.

Si definiscono le seguenti quantità:
- $a(\mathbf{x}_t) = a_{t}$ come il numero di nodi nella configurazione $\mathbf{x}_{t}$ che hanno colore $0$;
- $b(\mathbf{x}_t) = b_{t}$ come il numero di nodi nella configurazione $\mathbf{x}_{t}$ che hanno colore $1$;
- Assumendo che nella generica configurazione $\mathbf{x}_{t}$ la maggioranza ha colore $1$, si definisce il *bias* $s_t$ come $$b(\mathbf{x}_{t}) = b_{t} = \frac{n}{2} +s_{t} \implies s_{t} = b_{t} - \frac{n}{2}$$
D'ora in poi si farà riferimento semplicemente ad $s$ come al *bias* che dipende dal contesto corrente $\mathbf{x}_{t}$ (se ben chiaro). Si osserva che il *bias* $s$ assume valori tra $0$ e $\frac{n}{2}$. In particolare, se $s=0$ allora la metà dei nodi assume un colore e l'altra metà assume l'altro colore, mentre se $s=n/2$ allora tutti i nodi hanno lo stesso colore.

Si definisce formalmente il problema del $l$-Majority-Consensus Problem
```ad-Problema
title: $l$-Majority-Consensus Problem
Dato un sistema distribuito $G=([n],E)$, l'$l$-Majority-Consensus Problem è definito dalle seguenti proprietà. A partire da una qualsiasi colorazione iniziale $\mathbf{x}_{0}: [n] \to C$ con un bias arbitrario $s=s_0 \geqslant l$, il sistema deve convergere ad una colorazione stabile in cui tutti i nodi hanno addotato il colore di maggioranza iniziale, ovvero il sistema deve raggiungere un tempo $\tau$ tale che $\mathbf{x}_{\tau}$, cioè il vettore che rappresenta la colorazione al tempo $\tau$, abbia tutte le $n$ entrate uguali al colore di maggioranza iniziale.
```
# Obiettivi
Si risolve il problema del Majority Consensus su **grafi completi**, ossia per il resto della trattazione il grafo $G=([n],E)$ è una **clique** $K_n$ di $n$ nodi.
Si osserva che per questa topologia di grafi è immediato progettare un protocollo che risolve il problema: ogni nodo contatta e chiede il colore a tutti gli altri nodi della rete nello stesso istante. Dunque sfruttando la maggioranza già presente nel sistema, ogni nodo può calcolare in un singolo round un *agreement* sul colore da assumere. Questa soluzione però ha i seguenti costi:
- Una **Message  complexity** di $O(n^2)$;
- Una **Memoria locale per nodo** di $O(n)$, dato che ogni nodo deve salvare il colore di tutti gli altri nodi;
- Una congestione della rete molto importante, dato che tutti gli archi della rete sono attivi in quel singolo round.
Allora da un protocollo che risolve il problema del Majority consensus si vogliono le seguenti proprietà:
- Una **Message complexity** $O(n)$ ad ogni round;
- Una **memoria locale per nodo** costante ad ogni round;
- Una **congestione limitata**, cioè $O(n)$ archi attivi durante ogni round;
- Un **Tempo di convergenza**, cioè un numero di round, logaritmico in $n$: $O(\log {n})$.
# $k$-Majority Protocol ($k\text{-MAJ}$)
Il protocollo $k\text{-MAJ}$ è diviso in round. Ad ogni round, ogni nodo sceglie *u.a.r.* $k$ tra i suoi vicini (compreso se stesso) e cambia il proprio colore a seconda della maggioranza calcolata tra i nodi scelti. Eventuali *ties* vengono gestite in modo arbitrario. Inoltre, la scelta dei $k$ vicini è effettuata con ripetizione, cioè un nodo $v$ potrebbe scegliere un nodo $w$ più volte. Il protocollo termina quando tutti i nodi sono dello stesso colore.
___
Si analizza il protocollo in casi distinti, a seconda del valore di $k \in \{ 1,2,3 \}$. Si fanno prima alcune considerazioni generali.
## Strumenti per l'analisi
Il protocollo considerato può essere modellato per mezzo di un processo stocastico, in particolare con una **catena di Markov**: la colorazione del sistema al tempo $t+1$ dipende solamente dalla colorazione al tempo $t$. Sia $t$ un round fissato. La colorazione al round $t$ (e in generale, qualsiasi colorazione), per i fini delle analisi, può essere rappresentata dalla seguente coppia
$$
(a_{t} = a, b_{t} = b)
$$
L'obiettivo delle successive analisi sarà calcolare il valore atteso della variabile aleatoria $X_{t+1} = a_{t+1}$ che indica il numero di nodi che sono colorati col colore $0$ al termine del round $t$ (cioè all'inizio del round $t+1$), sapendo che al round $t$ ci sono $X_{t} = a$ nodi colorati col colore $0$.
A tale scopo, si definiscono le seguenti variabili aleatorie
$$
\forall i \in [n], \forall t = 0,1,2,\dots\quad Y_{t}^i := \begin{cases}
1, \quad \text{se il nodo $i$ è $0$ al round $t$} \\
0, \quad \text{altrimenti}
\end{cases}
$$
allora si può esprimere $a_{t+1}$ come somma di variabili aleatorie
$$
X_{t+1} = \sum_{i=1}^n Y_{t+1}^i
$$
e dunque per la linearità del valore atteso
$$
\mathbf{E}[X_{t+1}| X_{t} = a] = \sum_{i=1}^n \mathbf{E}[Y_{t+1}^i|X_{t}=a] = \sum_{i=1}^n \mathbf{Pr}(Y_{t+1}^i = 1 | X_{t} = a) 
$$
L'unica differenza nei casi che verranno analizzata è la probabilità che un certo nodo si colori di $0$ data la colorazione al tempo $t$. Per calcolare tale probabilità è fondamentale sottolineare il fatto che il grafo è completo e le scelte da ogni nodo vengono effettuate u.a.r., includendo se stessi e con ripetizione. Allora è evidente che per l'analisi non è necessario studiare *quali* sono i nodi che colorati in un certo modo, ma *quanti* sono.
## Caso $k=1$
Sia $k=1$, sia per ogni round $t$ il bias $s_t\geqslant 0$ arbitrario e sia $1$ il colore di maggioranza iniziale. Allora un generico nodo $i$ ad ogni round $t$ per scegliere il suo nuovo colore si basa solamente sul colore di un altro nodo pescato in modo uniforme. Allora
$$
\mathbf{Pr}(Y_{t+1}^i = 1 | X_{t} = a) = \frac{a}{n} 
$$
dunque risulta che
$$
\mathbf{E}[X_{t+1}| X_{t} = a] = \sum_{i=1}^n \frac{a}{n} = a
$$
cioè nel caso $k=1$ il protocollo, in media, **non** cambia il *bias* del sistema. Si osserva che lo stato locale dei nodi può certamente cambiare, ma il numero di nodi colorati di $0$ rimane, in media, lo stesso. Allora per $k=1$, il protocollo $k-\text{MAJ}$ converge molto lentamente nello stato finale desiderato.

Un'analisi alternativa è la seguente: ricordando che la maggioranza è colorata di $1$, sia $s_{t} = b_{t} - \frac{n}{2}$ il valore del *bias* al round $t$ e sia $s_{t+1}$ una variabile aleatoria che indica il *bias* al round $t+1$. Sia $\forall i \in [n], \forall t = 0,1,2,\dots, \ Z_t^i$ la variabile aleatoria che vale $1$ se il nodo $i$ sceglie un nodo colorato di $1$ al round $t$ e $0$ altrimenti. Sapendo che $b_{t} = s_{t}+\frac{n}{2}$, vale che
$$
\mathbf{Pr}(Z_{t+1}^i = 1 | s_{t}) = \frac{b_{t}}{n} = \frac{\left( \frac{n}{2} + s_{t} \right)}{n} = \frac{1}{2} + \frac{s_{t}}{n}
$$
dunque
$$
\mathbf{E}[b_{t+1}|s_{t}] = \sum_{i=1}^n \mathbf{E}[Z_{t+1}^i|s_{t}] = \frac{n}{2} + s_{t}
$$
e ciò implica che $s_{t+1}$ in media vale
$$
\mathbf{E}[s_{t+1}|s_{t}] = \mathbf{E}[b_{t+1}|s_{t}]-\frac{n}{2} = s_{t}
$$
## Caso $k=2$
Sia $k=2$, sia per ogni round $t$ il bias $s_t\geqslant 0$ arbitrario e sia $1$ il colore di maggioranza iniziale. Inoltre, si supponga che eventuali *ties* nella scelta di un colore siano risolti tramite il lancio di una moneta equa.
Allora un certo nodo $i$ ha due modi diversi per colorarsi di $0$: pescando in modo uniforme due nodi di colore $0$, oppure pescare in modo uniforme un nodo di colore $0$ e un nodo di colore $1$, e ottenere testa nel lancio della moneta (si assume che testa favorisca il colore $0$). Allora vale che
$$
\mathbf{Pr}(Y_{t+1}^i = 1 | X_{t} = a) = \left( \frac{a}{n} \right)^2 + 2\left( \frac{a}{n} \cdot\frac{b}{n} \cdot\frac{1}{2} \right)
$$
dato che $b=n-a$, si ottiene
$$
\begin{align}
\mathbf{Pr}(Y_{t+1}^i = 1 | X_{t} = a) &= \left( \frac{a}{n} \right)^2 + 2\left( \frac{a}{n} \cdot\frac{n-a}{n} \cdot\frac{1}{2} \right) \\
&=\left( \frac{a}{n} \right)^2 + \frac{a}{n}\left( 1-\frac{a}{n} \right) \\
&= \left( \frac{a}{n} \right)^2 + \frac{a}{n} - \left( \frac{a}{n} \right)^2  \\
&= \frac{a}{n}
\end{align}
$$
Anche in questo caso il protocollo convergerà molto lentamente nello stato finale desiderato, infatti vale come prima
$$
\mathbf{E}[X_{t+1}| X_{t} = a] = \sum_{i=1}^n \frac{a}{n} = a
$$
Un'analisi alternativa è la seguente: ricordando che la maggioranza è colorata di $1$, sia $s_{t} = b_{t} - \frac{n}{2}$ il valore del *bias* al round $t$ e sia $s_{t+1}$ una variabile aleatoria che indica il *bias* al round $t+1$. Sia $\forall i \in [n], \forall t = 0,1,2,\dots, \ Z_t^i$ la variabile aleatoria che vale $1$ se il nodo $i$ si colora di $1$ al round $t$ e $0$ altrimenti. Sapendo che $b_{t} = s_{t}+\frac{n}{2}$, vale che
$$
\begin{align}
\mathbf{Pr}(Z_{t+1}^i = 1 | s_{t}) &= \left( \frac{b_{t}}{n} \right)^2 + 2\left( \frac{b_{t}}{n} \cdot\frac{n-b_{t}}{n} \cdot\frac{1}{2} \right) \\
&=\left( \frac{\left( \frac{n}{2} + s_{t} \right)}{n} \right)^2 + \frac{\left( \frac{n}{2} + s_{t} \right)}{n}\left( 1-\frac{\left( \frac{n}{2} + s_{t} \right)}{n} \right) \\
&= \left( \frac{1}{2}+\frac{s_{t}}{n} \right)^2 + \left( \frac{1}{2}+\frac{s_{t}}{n} \right) - \left( \frac{1}{2}+\frac{s_{t}}{n} \right)^2  \\
&= \frac{1}{2}+\frac{s_{t}}{n}
\end{align}
$$
dunque
$$
\mathbf{E}[b_{t+1}|s_{t}] = \sum_{i=1}^n \mathbf{E}[Z_{t+1}^i|s_{t}] = \frac{n}{2} + s_{t}
$$
e ciò implica che $s_{t+1}$ in media vale
$$
\mathbf{E}[s_{t+1}|s_{t}] = \mathbf{E}[b_{t+1}|s_{t}]-\frac{n}{2} = s_{t}
$$
# Unbalanced $2$-coloring with $3\text{-MAJ}$
Si analizza ora il protocollo $k-\text{MAJ}$ con $k=3$. In particolare, un nodo ad ogni round sceglie $3$ altri nodi in modo uniforme e indipendente, con reinserimento e includendo se stesso; il nodo cambia il proprio colore a seconda della maggioranza presente tra i nodi scelti.
```ad-Definizione
title: Definizione
Sia $\mathbf{x}: [n] \to C \equiv \{0,1\}$ una qualsiasi colorazione. Allora $\mathbf{x}$ è $\omega$-unbalanced se il suo bias $s$ è tale che $s \geqslant \omega$.
```
Si dimostra che se la configurazione iniziale è sufficientemente sbilanciata, cioè se il *bias* è sufficientemente grande, allora il protocollo $3\text{-MAJ}$ risolve il problema del Majority Consensus su clique in $O(\log{n})$ round con alta probabilità.
## Teorema
Se $G\equiv K_n$ e la colorazione iniziale è $\omega$-unbalanced con $\omega = \Omega\left(\sqrt{ n\log{n} }\right)$, cioè ha bias $s \in \Omega\left(\sqrt{ n\log{n} }\right)$, allora il protocollo $3\text{-MAJ}$ converge al colore di maggioranza in $O(\log{n})$ passi con alta probabilità.
### Dimostrazione
Si assuma senza perdita di generalità che il colore di maggioranza iniziale sia il colore $1$, cioè $b_0 > a_0$. Inoltre, fissato un generico round $t\geqslant 1$, sia $a = a_t$ e $b= b_t$.
Sia $X_t$ la variabile aleatoria che indica il numero di nodi che sono colorati col colore $0$ al termine del round $t$ (cioè all'inizio del round $t+1$) e sia $\forall i \in [n]$, $Y_t^i$ la variabile aleatoria tale che
$$
\quad Y_{t}^i := \begin{cases}
1, \quad \text{se il nodo $i$ è $0$ al round $t$} \\
0, \quad \text{altrimenti}
\end{cases}
$$
Fissata la configurazione al tempo $t$, $X_{t}=a$, la probabilità che il nodo $i$ al tempo $t+1$ si colori di $0$ è pari a
$$
\begin{align}
\mathbf{Pr}(Y_{t+1}^i = 1 | X_{t} = a)  &= \left( \frac{a}{n} \right)^3 + 3 \frac{a^2(n-a)}{n^3} \\
&= \frac{a^3}{n^3} + \frac{3a^2}{n^2} - \frac{3a^3}{n^3} \\
&= \frac{a^2}{n^3} (a+3n-3a) \\
&= \frac{a^2}{n^3} (3n-2a)
\end{align}
$$
quindi il numero atteso di nodi colorati di $0$ al round $t+1$ è dato da
$$
\begin{align}
\mathbf{E}[X_{t+1}| X_{t} = a] &= \sum_{i=1}^n \mathbf{E}[Y_{t+1}^i| X_{t} = a] \\
&= \sum_{i=1}^n \mathbf{Pr}(Y_{t+1}^i=1| X_{t} = a) \\
&= \sum_{i=1}^n \frac{a^2}{n^3} (3n-2a) \\
&= \left( \frac{a}{n} \right)^2(3n-2a)
\end{align}
$$
Arrivati a questo punto dell'analisi, per continuare si assuma che il valore iniziale del *bias* sia $\Omega \left(\sqrt{ n\log{n} }\right)$, dunque
$$
s = s_{0} \geqslant c \cdot \sqrt{ n\log{n} }
$$
Si divide l'analisi in tre fasi, a seconda del range in cui la minoranza $a$ si trova.
#### Fase 1
In questa prima fase, si considera $X_{t} = a \in \left[ \frac{n}{4}, \frac{n}{2}-c\sqrt{ n\log{n} } \right]$.
Si esprime $a$ in funzione del bias $s$ nel seguente modo
$$
a = \frac{n}{2}-s, \quad c\sqrt{ n\log{n} } \leqslant s \leqslant \frac{n}{4}
$$
In questa fase, si dimostrano le seguenti cose:
1. Fintantoché $c\sqrt{ n\log{n} } \leqslant s \leqslant n/4$, allora con alta probabilità il bias **incrementa esponenzialmente** ad ogni round. In particolare vale, con alta probabilità, che $$X_{t+1} \leqslant \frac{n}{2}-\frac{9}{8}s$$
2. Dal risultato precedente, segue che con alta probabilità in $T= O(\log{n})$ round il numero di nodi di colore $0$ diventa $< n/4$ e dunque la prima fase termina.
##### Incremento esponenziale del bias
Si osserva che la funzione
$$
f(a) = a^2(3n-2a)= n^2 \mathbf{E}[X_{t+1}|X_{t}=a]
$$
è sempre crescente per ogni $0 < a < n$ ($f'(a) = 6a(n-a) >0$, in quanto è prodotto di due quantità maggiori di zero $\forall 0<a<n$). Da ciò segue che, per ogni $a\leqslant \frac{n}{2}-s$, si ottiene
$$
\begin{align}
\mathbf{E}[X_{t+1}|X_{t}=a] &= \left( \frac{a}{n} \right)^2(3n-2a) \\
&= \left( \frac{1}{n^2} \right) a^2(3n-2a) \\
&\leqslant \left( \frac{1}{n^2} \right)\left( \frac{n}{2}-s \right)^2 \left( 3n-2\left( \frac{n}{2}-s \right) \right) \\
&= \left( \frac{1}{n^2} \right)\left( \frac{n}{2}-s \right)^2 \left(2n+2s\right) \\
&= \left( \frac{1}{n^2} \right) \left( \frac{n^2}{4}+s^2-ns \right)(2n+2s) \\
&= \frac{n}{2}-\frac{3}{2}s +\frac{2s^3}{n^2} \\
&= \frac{n}{2}-\frac{3}{2}s +s\left( \frac{2s^2}{n^2} \right) \\
&\leqslant \frac{n}{2} - \frac{5}{4}s
\end{align}
$$
dove l'ultimo passaggio segue dal fatto che
$$
s \leqslant \frac{n}{4} \iff 2s^2 \leqslant \frac{n^2}{8} \iff  \frac{2s^2}{n^2} \leqslant \frac{1}{8}
$$
e dato che $\frac{11}{8} > \frac{5}{4}$, si ottiene che
$$
\begin{align}
\frac{n}{2}-\frac{3}{2}s +s\left( \frac{2s^2}{n^2} \right) &\leqslant \frac{n}{2}-\frac{3}{2}s + \frac{1}{8}s \\
&\leqslant \frac{n}{2}- \frac{11}{8}s \\
&\leqslant \frac{n}{2}- \frac{5}{4}s
\end{align}
$$
Si osserva che $X_{t+1}$ è una somma di variabili aleatorie **indipendenti** e condizionate a $X_{t}$. Dunque si può applicare il seguente *Chernoff Bound*:
```ad-Teorema
title: Chernoff bound additivo
Siano $X_1,\dots,X_n$ delle variabile aleatorie indipendenti e a valori in $\{0,1\}$. Sia $X:=\sum_{i=1}^n X_i$ e sia $\mu = \mathbf{E}[X]$. Allora per ogni $0 \leqslant \lambda \leqslant \mu$ vale che
$$
\mathbf{Pr}(X \geqslant \mu+\lambda) \leqslant e^{\frac{-2\lambda^2}{n}}
$$
```
In particolare, si applica con parametri
$$
\mu = \mathbf{E}[X_{t+1}|X_{t}] = \frac{n}{2}-\frac{5}{4}s, \quad \lambda = \frac{1}{8}s 
$$
dunque si ottiene che, per ogni $a\leqslant s \leqslant \frac{n}{4}$,
$$
\begin{align}
&\mathbf{Pr}\left( X_{t+1} \geqslant \left( \frac{n}{2}- \frac{5}{4}s \right) + \frac{1}{8}s | X_{t} = a \right)  = \\
&\mathbf{Pr}\left( X_{t+1} \geqslant\frac{n}{2}-\frac{9}{8}s | X_{t} = a \right) \leqslant e^{\frac{-2s^2}{64n}} = e^{\frac{-s^2}{32n}}
\end{align}
$$
Dato che si assume che $s \geqslant c \sqrt{ n\log{n} }$ per qualche costante $c >0$ sufficientemente grande, ne segue che
$$
\mathbf{Pr}\left( X_{t+1} \geqslant\frac{n}{2}-\frac{9}{8}s | X_{t} = a \right) \leqslant \frac{1}{n^{\Theta(1)}}
$$
e quindi si ottiene che $X_{t+1} \leq \frac{n}{2}-\frac{9}{8}s$ con alta probabilità.
Si osserva che si è appena dimostrato che finché il bias è contenuto nell'intervallo $c\sqrt{ n\log{n} } \leqslant s \leqslant \frac{n}{4}$, il bias incrementa in modo **esponenziale** con alta probabilità. Infatti, se $X_{t+1} = a_{t+1} \leqslant \frac{n}{2}-\frac{9}{8}s_{t}$, allora
$$
s_{t+1} = \frac{n}{2} - a_{t+1} \geqslant \frac{n}{2}-\frac{n}{2}+\frac{9}{8}s_{t} = \frac{9}{8}s_{t}
$$
e quindi, srotolando la relazione ricorsiva
$$
s_{t} \geqslant \left( \frac{9}{8} \right)^{t} s_{0} = \left( \frac{9}{8} \right)^{t} c\sqrt{ n\log n }
$$
Si dimostra ora che questo fatto implica che in $O(\log n)$ round la prima fase termina.
##### Terminazione in $O(\log n)$
Si definisce l'evento seguente
$$
\mathcal{E}_t:=X_t \leqslant \max \left\{\frac{n}{4}, \frac{n}{2}-\left(\frac{9}{8}\right)^t\right\}
$$
si osserva che
$$
\mathbf{Pr} \left[\mathcal{E}_{t+1} \mid \bigcap_{i=1}^{t} \mathcal{E}_i\right]=1-n^{-\alpha}
$$
per qualche costante $\alpha$. Infatti, assumendo che $n / 2-(9 / 8)^t \geq n / 4$, se vale $\mathcal{E}_{t}$ allora si ottiene con alta probabilità
$$
\begin{aligned}
X_{t+1} & \leqslant \frac{n}{2}-\frac{9}{8} s \\
& =\frac{n}{2}-\frac{9}{8} \cdot\left(\frac{n}{2}-X_{t}\right) \\
& \leqslant \frac{n}{2}-\frac{9}{8} \cdot \frac{n}{2}+\frac{9}{8} X_{t} \\
& \leqslant \frac{n}{2}-\frac{9}{8} \cdot \frac{n}{2}+\frac{9}{8} \cdot\left(\frac{n}{2}-\left(\frac{9}{8}\right)^{t}\right) \\
& =\frac{n}{2}-\left(\frac{9}{8}\right)^{t+1}
\end{aligned}
$$
che è proprio l'evento $\mathcal{E}_{t+1}$.
Allora si può calcolare la probabilità che in $T=\frac{\log (n / 4)}{\log (9 / 8)}$ passi si ha $X_t \leq n / 4$  in modo da passare alla fase successiva. Tale probabilità è cosi maggiorata
$$
\begin{aligned}
\operatorname{Pr}\left[\exists t \in[0, T]: X_t \leqslant n / 4\right] & \geqslant \operatorname{Pr}\left[\bigcap_{i=1}^T \mathcal{E}_i\right] \\
& =\prod_{i=1}^T \operatorname{Pr}\left[\mathcal{E}_i \mid \bigcap_{j=1}^{i-1} \mathcal{E}_{j}\right] \\
& \geqslant\left(1-n^{-\alpha}\right)^T \\
& \geqslant 1-2 T n^{-\alpha} \\
& \geqslant 1-n^{-\alpha / 2}
\end{aligned}
$$
quindi con alta probabilità in $O(\log n)$ passi si supera la prima fase.
#### Fase 2
In questa fase, si considera $X_{t} = a \in \left[ O(\log n), \frac{n}{4}\right]$, dunque $X_{t} = a \leqslant \frac{n}{4}.$ In questa fase si dimostrano le due seguenti cose:
1. Fintantoché $a \in \Omega(\log n)$, allora con alta probabilità si ha una decrescita esponenziale del numero di nodi colorati di $0$ ad ogni round. In particolare vale, con alta probabilità, che $$X_{t+1} = a_{t+1} \leqslant \frac{4}{5}a_{t}$$
2. Dal risultato precedente, segue che con alta probabilità $T= O(\log{n})$ round il numero di nodi di colore $0$ diventa $O(\log{n})$ e dunque la seconda fase termina.
##### Decrescita esponenziale
Vale che
$$
\mathbf{E}[X_{t+1}|X_{t}=a] = \left( \frac{a}{n} \right)^2(3n-2a) \leqslant a \left( \frac{\frac{n}{4}}{n^2} \right)(3n) = \frac{3}{4}a
$$
Allora si applica il seguente Chernoff Bound
```ad-Teorema
title: Chernoff bound moltiplicativo
Siano $X_1,\dots,X_n$ delle variabile aleatorie indipendenti e a valori in $\{0,1\}$. Sia $X:=\sum_{i=1}^n X_i$ e sia $\mathbf{E}[X]\leqslant \mu$. Allora per ogni $0 < \delta <1$ vale che
$$
\mathbf{Pr}(X \geqslant (1+\delta)\mu) \leqslant e^{\frac{-\mu\delta^2}{3}}
$$
```
In particolare, si applica ponendo $\mu =\frac{3}{4}a$ e $\delta = \frac{1}{15}$, ottenendo che la probabilità che il numero di nodi colorati di $0$ al round $t$ sia maggiore o uguale di $\frac{4}{5}a > \frac{3}{4}a$ è
$$
\mathbf{Pr}\left( X_{t+1} \geqslant \frac{4}{5}a | X_{t} = a \right) \leqslant e^{-\beta a}
$$
con $\beta=\frac{1}{900}$.
Dunque finché $a \in \Omega(\log n)$ vale che
$$
\mathbf{Pr}\left( X_{t+1} \geqslant \frac{4}{5}a | X_{t} = a \right) \leqslant e^{-\beta c\log n} = n^{-\beta c}
$$
il che implica, inglobando la costante $c$ in $\beta$,
$$
\mathbf{Pr}\left( X_{t+1} \leqslant \frac{4}{5}a | X_{t} = a \right) \geqslant 1 - n^{-\beta}
$$
cioè il numero di nodi colorati di $0$ decresce in modo esponenziale.
##### Terminazione in $O(\log n)$
Si dimostra ora che anche questa seconda fase ha una durata logaritmica in termini di round.
Si osserva che se fino al round $t$ della seconda fase c'è stata una decrescita esponenziale, allora questa decrescita continua con alta probabilità anche al round $t+1$ in quanto
$$
X_{t+1} \leqslant \frac{4}{5}X_{t} \leqslant \frac{4}{5} \left( \frac{4}{5} \right)^t \frac{n}{4} = \left( \frac{4}{5} \right)^{t+1} \frac{n}{4}
$$
Ma allora la probabilità di terminare la seconda fase è maggiorata dalla probabilità di avere $T$ round continui di decrescita esponenziale, con $T$ tale che
$$
X_{t} \leqslant \left( \frac{3}{4} \right)^T \frac{n}{4} \leqslant c\log n \implies T=\frac{\log \left( \frac{4c\log{n}}{n} \right)}{\log \frac{3}{4}}
$$
Dato che questo avviene con alta probabilità, si può concludere che con alta probabilità in $O(\log n)$ passi la seconda fase termina. (Il motivo è lo stesso del punto 2 della fase 1, prova su carta a fare lo stesso conto e portarlo qui se corretto, se no sti cazzi porcodio BASTA).
#### Fase 3
In questa terza e ultima fase il valore di $a$ decrementa da $O(\log n)$ fino a $0$, permettendo quindi al sistema di raggiungere la maggioranza sul colore $1$.
Dato che $a \leqslant c\log n$, dal comportamento in media del processo si ottiene
$$
\mathbf{E}[X_{t+1}|X_{t}=a] = \left( \frac{a}{n} \right)^2(3n-2a) \leqslant \left( \frac{c\log^2n}{n^2} \right)(3n) = 3c^2 \frac{\log^2n}{n}
$$
Applicando la **disuguaglianza di Markov** si ottiene che la probabilità che ci sia almeno un nodo colorato di $0$ al round $t+1$ sapendo che ci sono $a \leqslant c\log n$ nodi colorati di $0$ al round $t$ è
$$
\mathbf{Pr}(X_{t+1}\geqslant 1|X_{t} = a) \leqslant 3c^2 \frac{\log^2n}{n}
$$
ciò implica che con alta probabilità si passerà da $O(\log n)$ nodi colorati di $0$ a nessuno in un solo step, cioè la terza fase termina in un singolo round e il sistema raggiunge l'agreement ricercato.