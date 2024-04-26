In questa sezione, si studia una nuova classe di modelli distribuiti, detta **GOSSIP**.
# GOSSIP Model
Tali modelli operano in un contesto **totalmente sincrono**, in cui lo scambio di messaggi è scandito da un clock globale che identifica dei *round discreti*, ed un messaggio inviato ad un certo round $t$, verrà ricevuto dal destinatario entro la fine del round $t$. Si assume inoltre che la rete di comunicazione del sistema distribuito in tale modello è rappresentato da un grafo *non diretto* $G(V,E)$, con $|V|=n$ e $|E| = m$. Inoltre si denoterà con $N(v)$ il vicinato di un nodo $v \in V$ (inoltre, $v \in N(v)$) e con $d(v) = |N(v)|$.
I modelli GOSSIP si dividono in due sottoclassi: **PUSH** GOSSIP model e **PULL** GOSSIP model:
- **PUSH Model**: Ad ogni round $t=0,1,\dots$ ogni nodo $v\in V$ sceglie **u.a.r.** uno dei suoi vicini $u \in_U N(v)$ (notazione in [10 - Leader Election Unlabeled Ring](10%20-%20Leader%20Election%20Unlabeled%20Ring.md)) e $v$ può trasmettere ad $u$ un qualsiasi messaggio $M$. 
- **PULL Model**: Ad ogni round $t=0,1,\dots$ ogni nodo $v\in V$ sceglie **u.a.r.** uno dei suoi vicini $u \in_U N(v)$ (notazione in [10 - Leader Election Unlabeled Ring](10%20-%20Leader%20Election%20Unlabeled%20Ring.md)) e $v$ si può far trasmette da $u$ un qualsiasi messaggio $M$. 
## Proprietà
Si osserva che ad ogni round $t$ il numero totale di comunicazioni attive, o il numero totale di messaggio, è pari ad $n$ in entrambi i modelli. In particolare, queste comunicazioni generano un grafo diretto delle comunicazioni $G_t(V,E_t)$, dove $E_t$ rappresenta l'insieme dei **link attivi** per via delle comunicazioni tra i nodi: ad ogni round $t$, $E_t$ contiene $n$ archi e dunque $G_t$ risulta sempre un **grafo sparso**. Questo è vero perché ad ogni round, ognuno degli $n$ nodi attiva un link (il grafo delle comunicazioni $G$ è non diretto, ma se un link $(u,v)$ viene attivato sia da $u$ che da $v$ al round $t$, in $E_t$ di $G_t$ si inseriscono i due archi diretti $(u,v)$ e $(v,u)$).
In particolare, valgono le seguenti proprietà:
- Nel modello PUSH, ad ogni round, il numero atteso di operazioni di *push* **ricevute** da ogni nodo è $1$ ed è $O(\log{n})$ con alta probabilità.
- Nel modello PULL, ad ogni round, il numero atteso di operazioni di *push* **ricevute** da ogni nodo è $1$ ed è $O(\log{n})$ con alta probabilità.
### Dimostrazione
Dimostrazione nel modello push, pull è analoga.
Nel modello PUSH, ad ogni round, il numero atteso di operazioni di push ricevute da ogni nodo è 1.
Sia $v \in V$ un generico nodo fissato e si consideri la variabile aleatoria binaria $X_u^{(t)}$ che vale $1$ se il nodo $u$ esegue una operazione di push verso $v$ al round $t$ e vale $0$ altrimenti; allora
$$
\mathbf{Pr}\left(X_{u}^{(t)} = 1\right)  = \frac{1}{d(u)}
$$
Sia $X^{(t)}$ la variabile aleatoria che conta il numero di operazioni di push verso $v$ al round $t$. Dato che $X^{(t)} = \sum_{u \in V} X_{u}^{(t)}$ e $X_u^{(t)}$ sono tutte indipendenti, per la linearità del valore atteso vale che
$$
\mathbf{E}\left[X^{(t)}\right] = \sum_{u \in V} \mathbf{E}\left[X_{u}^{(t)}\right] = \sum_{u \in V} \mathbf{E}\left[X_{u}^{(t)}\right] = \sum_{()}
$$