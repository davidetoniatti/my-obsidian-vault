In questa sezione, si presenta un nuovo modello distribuito detto **GOSSIP**. In particolare, verrà studiato un protocollo che, su tale modello, risolve il task del broadcasting in un numero di step logaritmico rispetto al numero di nodi nella rete.
# GOSSIP Model
Il modello **GOSSIP** opera in un contesto **totalmente sincrono**, in cui lo scambio di messaggi è scandito da un clock globale che identifica dei *round discreti*. In particolare, un messaggio inviato ad un certo round $t$, viene ricevuto dal destinatario entro la fine del round $t$. Si assume inoltre che la rete di comunicazione del sistema distribuito in tale modello è rappresentato da un grafo *non diretto* $G(V,E)$, con $|V|=n$ e $|E| = m$. Infine, si denota con $N(v)$ il vicinato di un nodo $v \in V$, dove $v \in N(v)$, e con $d(v) = |N(v)|$.
Il modello **GOSSIP** si divide in due sottoclassi: **PUSH GOSSIP model** e **PULL GOSSIP model**.
- **PUSH GOSSIP Model**: Ad ogni round $t=0,1,\dots$ ogni nodo $v\in V$ sceglie **u.a.r.** uno dei suoi vicini $u \in_U N(v)$ (notazione in [10 - Leader Election Unlabeled Ring](10%20-%20Leader%20Election%20Unlabeled%20Ring.md)) e $v$ può trasmettere ad $u$ un qualsiasi messaggio $M$. 
- **PULL GOSSIP Model**: Ad ogni round $t=0,1,\dots$ ogni nodo $v\in V$ sceglie **u.a.r.** uno dei suoi vicini $u \in_U N(v)$ e $v$ si può far trasmettere da $u$ un qualsiasi messaggio $M$. 
## Proprietà
Si osserva che ad ogni round $t$ il numero totale di comunicazioni attive, o il numero totale di messaggio, è al più $n$ in entrambi i modelli. In particolare, queste comunicazioni generano un grafo diretto delle comunicazioni al round $t$ $G_t(V,E_t)$, dove $E_t$ rappresenta l'insieme dei **link attivi** per via delle comunicazioni tra i nodi: ad ogni round $t$, $E_t$ contiene al più $n$ archi e dunque $G_t$ risulta sempre un **grafo sparso**. Questo è vero perché ad ogni round, ognuno degli $n$ nodi attiva al più un link (il grafo delle comunicazioni $G$ è non diretto, ma se un link $(u,v)$ viene attivato sia da $u$ che da $v$ al round $t$, in $E_t$ di $G_t$ si inseriscono i due archi diretti $(u,v)$ e $(v,u)$).
In particolare, valgono le seguenti proprietà.
- Nel modello **PUSH**, ad ogni round, il numero atteso di operazioni di *push* **ricevute** da ogni nodo è $1$ ed è $O(\log{n})$ con alta probabilità.
- Nel modello **PULL**, ad ogni round, il numero atteso di operazioni di *pull* **ricevute** da ogni nodo è $1$ ed è $O(\log{n})$ con alta probabilità.
### Dimostrazione
Dimostrazione nel modello push, nel modello pull è analoga. In particolare, si considera il caso in cui la rete è un grafo completo $K_{n}$. Si osserva che questo è il grafo più denso, dunque i risultati dimostrati valgono anche per grafi che non sono completi.
#### Ad ogni round, il numero atteso di operazioni di push ricevute da ogni nodo è 1
Sia $v \in V$ un generico nodo fissato e si consideri la variabile aleatoria binaria $X_u^{(t)}$ che vale $1$ se il nodo $u$ esegue una operazione di push verso $v$ al round $t$ e vale $0$ altrimenti. Dato che il grafo è completo, ogni nodo $u \in V$ ha come vicino il nodo $v$; inoltre per definizione vale che $\forall u \in V, d(u)=n$, dunque la probabilità per ogni nodo $u$ di fare push verso $v$ è pari a
$$
\mathbf{Pr}\left(X_{u}^{(t)} = 1\right)  = \frac{1}{n}
$$
Sia $X^{(t)}$ la variabile aleatoria che conta il numero di operazioni di push verso $v$ al round $t$. Dato che $X^{(t)} = \sum_{u \in V} X_{u}^{(t)}$, per la linearità del valore atteso vale che
$$
\mathbf{E}\left[X^{(t)}\right] = \sum_{u \in V} \mathbf{E}\left[X_{u}^{(t)}\right] = \sum_{u \in V} \mathbf{Pr}\left(X_{u}^{(t)} = 1\right) = \sum_{u \in V} \frac{1}{n} = 1
$$
#### Ad ogni round, il numero atteso di operazioni di push ricevute da ogni nodo $O(\log{n})$ con alta probabilità
Si utilizza la seguente disuguaglianza di concentrazione.
```ad-Proposizione
title: Chernoff bound moltiplicativo
Siano $X_1,\dots,X_n$ variabili aleatorie binarie tutte indipendenti tra loro. Sia $X = \sum_{i=1}^n X_i$ e sia $\mathbf{E}[X] \leqslant \mu$. Allora, per ogni $\delta > 0$ vale che
$$
\mathbf{Pr}(X \geqslant (1+\delta)\mu) \leqslant \left(\frac{e^{\delta}}{(1+\delta)^{(1+\delta)}}\right)^{\mu}
$$
```
Si applica con
- $\mu = 1$;
- $\delta = c\log{n}$ con $c>0$ e tale che $1+c\log{n} \geqslant e^2$;
- $X=X^{(t)}$, dove $X^{(t)}$ è la variabile aleatoria che conta il numero di operazioni di push verso $v$ al round $t$; si ricorda che essa è una somma di variabili aleatorie indipendenti tra loro. 
Dunque si ottiene che
$$
\begin{align}
\mathbf{Pr}(X^{(t)} \geqslant 1+c \log{n}) &\leqslant \frac{e^{c \log{n}}}{(1+c \log{n})^{(1+c \log{n})}} \\
& \leqslant \frac{e^{c \log{n}}}{(1+c \log{n})^{c \log{n}}} \quad \text{(1)} \\
& \leqslant \left( \frac{1}{e} \right)^{c\log{n}}  \quad \text{(2)}\\
&= \frac{1}{n^c}
\end{align}
$$
dove
- $\text{(1)}$ perché $(1+c\log{n})^{(1+c\log{n})} > (1+c\log{n})^{c\log{n}}$;
- $\text{(2)}$ perché $\delta$ è tale che $1+c\log{n} \geqslant e^2$.
# PULL Broadcast protocol (BP) su clique
Si definisce un protocollo probabilistico sul modello **GOSSIP PULL** che risolve il task del broadcast a singola sorgente su una clique $K_n$: il ***PULL Broadcast Protocol (BP)***
Sia $M$ l'informazione che ogni nodo deve possedere per completare il task. Si assuma che ogni entità $x \in V$ disponga di un registro privato $c_x$ tale per cui 
$$
c_x = \begin{cases} \text{informed}\ \ \ \ \   \ \ \ \text{ se }x \text{ ha ricevuto }M \\ \text{notInformed} \ \ \text{ se }x \text{ non ha ricevuto }M\\ \end{cases}
$$
Formalmente, il problema è descritto dalla seguente tripla $P = \langle P_{init}, P_{final}, G_{pull} \rangle,$ dove:
- $P_{init} = \exists! x \in V, c_x=\textnormal{informed}\ \wedge \forall y \neq x, c_x= \textnormal{notInformed}$;
- $P_{final} = \forall x \in V, c_x = \text{informed}$;
- $G_{pull} = \text{Restrizioni modello GOSSIP PULL (primo paragrafo)} \cup \text{KT}$.
dove $\text{KT}$ sta per conoscenza della topologia: la rete di comunicazione è una *clique*. Per conoscere le restrizioni del modello GOSSIP PULL, studiare bene il primo paragrafo.
## Il protocollo
I nodi, durante l'esecuzione del protocollo, possono trovarsi in due stati: $\text{INFORMED}$ e $\text{NOT-INFORMED}$ in accordo con il valore del registro $c_x$. Si descrive dunque ad altissimo livello il protocollo:
- Inizialmente, la sorgente $s \in V$ è nello stato $\text{INFORMED}$ ed è l'unico nodo informato nella rete. Il resto dei nodi non è informato e sono nello stato $\text{NOT-INFORMED}$.
- Ad ogni round $t\geqslant 1$, ogni nodo $v$ $\text{NOT-INFORMED}$ esegue una operazione di *pull* su un altro nodo $u \in_{U} N(v)$:
	- se $u$ è un nodo $\text{INFORMED}$, allora $v$ si fa inviare una copia del messaggio $M$ e passa nello stato $\text{INFORMED}$;
- Il protocollo termina globalmente quando tutti i nodi si trovano nello stato $\text{INFORMED}$.
## Analisi time complexity
Il protocollo **BP** termina globalmente in $\Theta(\log{n})$ round con alta probabilità.
### Dimostrazione
Sia $t\geqslant 1$ un round fissato. Sia $Y_v$, per ogni nodo $v \in V \setminus I_{t}$, la variabile aleatoria binaria
$$
Y_{v}= 
  \left\{ 
    \begin{aligned}
      &\ 1 \ \ \ \ \text{se } v \text{ diventa informato al round } t+1\\
      &\ 0 \ \ \  \textnormal{ altrimenti}  
    \end{aligned}
  \right.
$$
Inoltre, si definisce l'insieme
$$
\begin{align}
I_{0} &\equiv \{ s \} \\
I_{t} &\equiv \{ v \in V: v \text{ è informato al round } t\}
\end{align}
$$
dove si denota $|I_t|$ con $m_t$.
Dato che, in un round, ogni nodo effettua una operazione di pull da un altro nodo della rete scelto uniformemente a caso e i nodi informati al round $t$ sono in totale $m_t$, vale che per ogni nodo $v \in V \setminus I_t$
$$
\mathbf{E}[Y_{v}] = \mathbf{Pr}(Y_{v} = 1) = \frac{m_{t}}{n} 
$$
A questo punto, la dimostrazione si divide in due fasi:
- Nella **fase 1** si dimostrerà che con alta probabilità il numero di nodi informati al round $t$ $|I_{t}|$ cresce in modo **esponenziale** fino ad $\frac{n}{2}$;
- Nella **fase 2** si dimostrerà che con alta probabilità il numero di nodi restanti non informati decresce in modo esponenziale fino ad arrivare a zero.

Cosi facendo, si dimostra che in tempo logaritmico il protocollo termina il task con alta probabilità.
#### Fase 1: fino a $m_t \leqslant \frac{n}{2}$
Si definisce con $I^*$ l'insieme dei soli <u>nuovi</u> nodi informati al round $t+1$, cioè tale che $I_{t+1} = I_t \cup I^*$. In particolare, $I_t$ e $I^*$ sono disgiunti, dunque vale che $|I_{t+1}| = |I_t| + |I^*|$. Si osserva $|I^*|$ è la seguente variabile aleatoria
$$
|I^*| = \sum_{v \in V \setminus I_{t}} Y_{v}
$$
Allora, per la linearità del valore atteso, vale che
$$
\mathbf{E}[|I^*| \mid |I_{t}| = m_{t}] = \sum_{v \in V \setminus I_{t}} \mathbf{E}[Y_{v} \mid |I_{t}| = m_{t}] = (n-m_{t}) \frac{m_{t}}{n}
$$
essendo $m_t \leqslant \frac{n}{2}$, vale che
$$
\mathbf{E}[|I^*| \mid |I_{t}| = m_{t}] = (n-m_{t}) \frac{m_{t}}{n} \geqslant \left( n-\frac{n}{2} \right)\frac{m_{t}}{n} = \frac{m_{t}}{2}
$$
dunque l'insieme dei nodi informati al round $t+1$ è in valore atteso pari a
$$
\begin{align}
\mathbf{E}[|I_{t+1}| \mid |I_{t}| = m_{t}] &= \mathbf{E}[|I_t| + |I^*| \mid |I_{t}| = m_{t}]  \\
&= m_{t} + \mathbf{E}[|I^*| \mid |I_{t}| = m_{t}] \geqslant m_{t}+ \frac{m_{t}}{2} = \frac{3}{2}m_{t}
\end{align}
$$
Questa espressione è una relazione ricorsiva che dice che, ad ogni round, il numero di nodi informati cresce, in media, di almeno un fattore costante fintantoché $m_t < \frac{n}{2}$. In particolare, srotolando l'espressione si ottiene
$$
m_{t}\geqslant \frac{3}{2}m_{t-1} \geqslant \left( \frac{3}{2} \right)^2 m_{t-2} \geqslant \dots \geqslant \left( \frac{3}{2} \right)^t
$$
Se $\tau = \min\left\{ t\geqslant 1 : m_{t} > \frac{n}{2} \right\}$ è il primo round in cui il numero di nodi informati supera la metà del numero di nodi totali, secondo la relazione di ricorrenza si ottiene
$$
\tau \approx \log_{\frac{3}{2}}\left( \frac{n}{2} \right) \in \Theta(\log{n})
$$
ovvero in un numero logaritmico di round, almeno la metà dei nodi viene informato, **in media**. L'analisi appena fatta indica ciò che accade in media; per dimostrare questo risultato in alta probabilità, si divide questa fase in due sottofasi.
##### Fase 1.1: $1 \leqslant m_{t} \leqslant \alpha \log{n}$
In questa prima sottofase, si considera l'intervallo $1 \leqslant m_{t} \leqslant \alpha \log{n}$ e si dimostra il seguente claim.

```ad-Claim
Per ogni $\alpha > 0$, esiste un $\gamma = \gamma(\alpha)$ sufficientemente grande tale che dopo i primi $\tau_{1} = \gamma\log{n}$ round si hanno almeno $m_{\tau_{1}} \geqslant \alpha\log{n}$ nodi informati con alta probabilità.
```

Sia, per ogni $u \in V \setminus \{ s \}$, la variabile aleatoria binaria
$$
Y_{u}^{(\tau_{1})}= 
  \left\{ 
    \begin{aligned}
      &\ 1 \ \ \ \ \text{se } u \text{ è informato al round } \tau_{1}+1\\
      &\ 0 \ \ \  \textnormal{ altrimenti}
    \end{aligned}
  \right.
$$
cioè vale zero se entro i primi $\tau_{1}$ passi, $v$ non viene informato.
L'operazione di pull di un nodo ad un round $t$ è **indipendente** rispetto all'operazione di pull fatta al round $t+1$, per ogni nodo nella rete. Dunque vale che
$$
\begin{align}
\mathbf{Pr}\left(Y_{u}^{(\tau_{1})} = 0\right) &= \mathbf{Pr}\left(\bigcap_{t = 1}^{\tau_{1}} \{ u \text{ sceglie nodo senza informazione al round }t \}\right) \\
&= \prod_{t=1}^{\tau_{1}}\mathbf{Pr}\left(\{ u \text{ sceglie nodo senza informazione al round }t \}\right) \\
&= \prod_{t=1}^{\tau_{1}} \left( 1- \frac{m_{t}}{n} \right) \\
&\leqslant \left( 1 - \frac{1}{n} \right)^{\tau_{1}} \quad m_{t} \geqslant 1:\text{ almeno la sorgente informata} \\
&\leqslant \left( e^{-\frac{1}{n}} \right)^{\tau_{1}} \quad \left( 1-\frac{1}{n} \leqslant e^{-1/n} \right)\\
&= e^{-\gamma \frac{\log{n}}{n}}
\end{align}
$$
Sia $Y^{(\tau_{1})}$ la variabile aleatoria che conta il numero di nodi informati al round $\tau_1+1$, ossia $m_{\tau_{1}+1} = Y^{(\tau_{1})}$. Si osserva che
$$
Y^{(\tau_{1})} = \sum_{u \in V} Y_{u}^{(\tau_{1})}
$$
dunque per la linearità del valore atteso vale che 
$$
\begin{align}
\mathbf{E}\left[Y^{(\tau_{1})}\right] &= \sum_{u \in V } \mathbf{E}\left[Y_{u}^{(\tau_{1})}\right] \\
&= \sum_{u \in V}1-\mathbf{Pr}\left(Y_{u}^{(\tau_{1})} = 0\right) \\
&\geqslant \sum_{u \in V} \left(1- e^{-\gamma \frac{\log{n}}{n}} \right) \\
&\geqslant\sum_{u \in V} \frac{\gamma\log{n}}{2n} \quad (\star)\\
&= n \cdot \frac{\gamma\log{n}}{2n} = \frac{\gamma\log{n}}{2}
\end{align}
$$
dove la disuguaglianza $(\star)$ è data dal fatto che per un $n$ sufficientemente grande vale
$$
\left(1- e^{-\gamma \frac{\log{n}}{n}} \right) \geqslant \frac{\gamma \log{n}}{2n}
$$
Dati che $Y^{(\tau_{1})}$ è la somma di variabili aleatorie *indipendenti* tra di loro, si può applicare il seguente Chernoff Bound moltiplicativo
```ad-Teorema
title: Chernoff bound moltiplicativo
Siano $X_1,\dots,X_n$ delle variabile aleatorie indipendenti e a valori in $\{0,1\}$. Sia $X:=\sum_{i=1}^n X_i$ e sia $\mathbf{E}[X]\leqslant \mu$. Allora per ogni $0 < \delta <1$ vale che
$$
\mathbf{Pr}(X \leqslant (1-\delta)\mu) \leqslant e^{\frac{-\mu\delta^2}{2}}
$$
```
In particolare, con $\delta = 1/2$ e $\mu = \gamma \log n$, si ottiene che la probabilità di esserci meno di  $\frac{\gamma \log{n}}{2}$ nodi informati entro i primi $\tau_{1} = \gamma \log n$ round è al più
$$
\mathbf{Pr}\left(Y^{(\tau_{1})} \leqslant \left( 1- \frac{1}{2} \right) \gamma \log n\right)\leqslant e^{-\frac{\gamma \log n}{8}} = n^{-\frac{\gamma}{8}}
$$
Considerando un $\gamma = \gamma(\alpha)$ sufficientemente grande, cioè tale che $\frac{\gamma\log{n}}{2}\geqslant\alpha \log n$, si ottiene che $m_{\tau_{1}}\geqslant \alpha \log n$ con alta probabilità.
$$
\begin{align}
\mathbf{Pr}\left(Y^{(\tau_{1})} \leqslant \alpha \log n\right) &\leqslant \mathbf{Pr}\left(Y^{(\tau_{1})} \leqslant \frac{\gamma\log{n}}{2} \right) \leqslant n^{-\frac{\gamma}{8}} \\
&\implies \mathbf{Pr}(Y^{(\tau_{1})}= m_{\tau_{1}} \geqslant \alpha \log n) \geqslant 1-n^{-\frac{\gamma}{8}}
\end{align}
$$
##### Fase 1.2 $\alpha \log n \leqslant m_{t} \leqslant \frac{n}{2}$
Questa sotto-fase viene immediatamente dopo la fase di *Bootstrap* e riguarda l'intervallo $\alpha \log n \leqslant m_{t} \leqslant \frac{n}{2}$. Si dimostra il seguente claim.

```ad-Claim
Esiste un certo $\beta$ sufficientemente grande tale che dopo $\tau_{2} = \beta \log n$ round vale che $m_{\tau_{2}} \geqslant \frac{n}{2}$ con alta probabilità.
```

Sia $\tau_1 \leqslant t \leqslant \tau_{2}$ un round fissato, e sia $m_t = |I_t|$. Sopra si è dimostrato che vale
$$
\mathbf{E}[|I_{t+1}|] \geqslant \frac{3}{2}m_{t}
$$
Dato che nella fase di bootstrap si è dimostrato che dopo i primi $\tau_{1} = \gamma\log{n}$ round si hanno almeno $m_{\tau_{1}} \geqslant \alpha\log{n}$ nodi informati con alta probabilità e $t \geqslant \tau_{1}$ per ipotesi, vale che
$$
\mathbf{E}[m_{t+1}] = \mathbf{E}[|I_{t+1}|] \geqslant \frac{3}{2}m_{t} \geqslant \frac{3}{2}\alpha \log n \geqslant \alpha \log n 
$$
Dato che $m_{t+1}$ è una somma di variabili aleatorie indipendenti (le variabili $Y_u^{(t)}$), si può applicare un Chernoff Bound, ottenendo che
$$
\mathbf{Pr}\left( m_{t+1} \leqslant (1-\delta) \frac{3}{2}m_{t} \right) \leqslant e^{-\frac{\delta^2}{2}\frac{3}{2}m_{t}} \leqslant e^{-\frac{\delta^2}{2}\alpha \log n} =n^{-\frac{\delta^2}{2}\alpha} = n^{-c}
$$
Ponendo $\delta \geqslant \frac{1}{3}$, si ottiene
$$
\mathbf{Pr}(m_{t+1}\leqslant m_{t})  \leqslant n^{-c}
$$
Questo ultimo bound implica l'alta probabilità della crescita esponenziale di $m_t$, in quanto 
$$
\mathbf{Pr}(m_{t+1} > m_{t})  \geqslant 1- n^{-c}
$$
cioè, con alta probabilità, il numero di nodi informati al round $t+1$ aumenta di un fattore costante; inoltre la probabilità che esiste un round $\tau_{1} \leqslant t \leqslant \tau_{2}$ tale che $m_{t+1} \leqslant m_{t}$ (cioè non aumenta di un fattore costante), per lo union bound, è al più
$$
\sum_{t = \tau_{1}}^{\tau_{n}} n^{-c} \leqslant \sum_{t = \tau_{1}}^{\tau_{n}} n^{-2} = (\tau_{2}-\tau_{1}) n^{-2} < \frac{n}{n^{2}} = \frac{1}{n}
$$
dunque, con alta probabilità, ad ogni round il numero di nodi informati aumenta di un fattore costante.
#### Fase 2: da $n/2$ ad $n$
La fase 2 è del tutto analoga alla prima, con la differenza che si dimostra che la decrescita dei nodi non informati è esponenziale nel tempo.

Si fissi un round $t \geq \tau_2 = \beta \log n.$ Essendo che dalla prima fase si ha che $m_{\tau_2} \geq \frac{n}{2},$ si ha che un nodo $\text{NOT-INFORMED}$ al tempo $t$ fa pull del messaggio e diventa $\text{INFORMED}$ al tempo $t+1$ con probabilità almeno $\frac{1}{2}.$

Sia $z_t = n- m_t$ il numero di nodi ancora non informati al tempo $t,$ e sia per ogni nodo $v \in V \setminus I_{t}$ non informato $X_v^{(t)}$ la variabile aleatoria che vale $1$ se $v$ rimane non informato al round $t+1$ e $0$ altrimenti. Allora, dato che $z_{t} \leqslant \frac{n}{2}$, vale
$$
\mathbf{E}[X_{v}^{(t)} | |V \setminus I_{t}| = z_{t}]=\mathbf{Pr}\left(X_{v}^{(t)} = 1\right) = \frac{z_{t}}{n} \leqslant \frac{1}{2}
$$
Sia $z_{t+1}$ il numero di nodi non informati al round $t+1$. Vale che
$$
z_{t+1} = \sum_{v \in V \setminus I_{t}} X_{v}^{(t)}
$$
Allora, mediamente, si ha che il numero di nodi ancora non informati al round $t+1$ è pari a
$$
\mathbf{E}[z_{t+1} | z_{t}] \leq \frac{z_t}{2} \leq \frac{z_\tau}{2} \leq \frac{n}{4}
$$
Si usa ora il seguente Chernoff Bound.
```ad-Teorema
title: Chernoff bound moltiplicativo
Siano $X_1,\dots,X_n$ delle variabile aleatorie indipendenti e a valori in $\{0,1\}$. Sia $X:=\sum_{i=1}^n X_i$ e sia $\mathbf{E}[X]\leqslant \mu$. Allora per ogni $0 < \delta <1$ vale che
$$
\mathbf{Pr}(X \geqslant (1+\delta)\mu) \leqslant e^{\frac{-\mu\delta^2}{3}}
$$
```
In particolare, si applica ponendo $\mu =\frac{1}{2}z_{t}$ e $\delta = \frac{1}{3}$, ottenendo che la probabilità che il numero di nodi non informati al round $t$ sia maggiore o uguale di $\frac{2}{3}z_{t} > \frac{1}{2}z_{t}$ è
$$
\mathbf{Pr}\left( z_{t+1} \geqslant \frac{2}{3}z_{t} | z_{t} \right) \leqslant e^{-\beta z_{t}}
$$
con $\beta=\frac{1}{54}$.
Dunque finché $z_{t} \in \Omega(\log n)$ vale che
$$
\mathbf{Pr}\left( z_{t+1} \geqslant \frac{2}{3}z_{t} | z_{t}\right) \leqslant e^{-\beta c\log n} = n^{-\beta c}
$$
il che implica, inglobando la costante $c$ in $\beta$,
$$
\mathbf{Pr}\left( z_{t+1} \leqslant \frac{2}{3}z_{t} | z_{t}\right) \geqslant 1 - n^{-\beta}
$$
cioè il numero di nodi non informati decresce in modo esponenziale.
Questa catena di disuguaglianze può essere generalizzata come segue
$$
z_t \leq \frac{z_{t-1}}{2} \leq \frac{z_{t-2}}{4} \leq \ldots \leq \frac{z_{t-i}}{2^i} \leq \ldots \leq \frac{z_{\tau_2}}{2^{t-\tau_2}} \leq \frac{n}{2^{t-\tau_2+1}} 
$$
Ci si chiede ora per quali valori di $t$ il numero di nodi sopravvissuti diventa molto piccolo, diciamo $z_t \leq c \log n.$ 
Certamente, se $\frac{n}{2^{t-\tau_2+1}} \leq c \log n$ allora anche $z_t \leq c \log n,$ allora:
$$
\begin{align*}
\frac{n}{2^{t-\tau_2+1}} &= c \log n \\
2^{t-\tau_2} &= \frac{n}{2c \log n} \\
2^t2^{-\tau_2} &= \frac{n}{2c \log n} \\
2^t2^{-\beta \log n} &= \frac{n}{2c \log n} \\
2^t 2^{-\beta'} &= \frac{n}{2c \log n} \\
2^t &= \frac{n^{\beta'+1}}{2c \log n} \\
t &= \log \left(\frac{n^{\beta'+1}}{2c \log n} \right) \\
&= \log(n^{\beta'+1})- \log(2c\log n) \\
&= (\beta^{'} +1 )\log n - \Theta(\log(\log n)) \in \Theta(\log n)
\end{align*}
$$

Si calcola ora la probabilità che una volta rimasti $O(\log n)$ nodi NOT-INFORMED, è sufficiente un solo time slot affinché tutti i nodi, con alta probabilità, vengano informati.

Dato che $z_t \leqslant c \log n$, la probabilità che un nodo NOT-INFORMED faccia pull su un nodo INFORMED in questa fase del protocollo è dell'ordine di $1 - \frac{\log n}{n},$ perciò si ha che in media il numero di nodi che non faranno pull su nodi informati sarà
$$
\mathbf{E} \left[z_{t+1} | z_t = c \log n\right] \leqslant c \frac{\log^2n}{n}
$$
Per la disuguaglianza di Markov, si ha che la probabilità di avere almeno un nodo non informato è molto bassa:
$$
\mathbf{P}\left(z_{t+1} \geq 1\right) \leq c \frac{\log^2 n}{n}
$$
ciò implica che con alta probabilità si passerà da $O(\log n)$ nodi non informati a nessuno in un solo step, cioè nella fase finale in un singolo round tutti i nodi hanno ricevuto l'informazione.