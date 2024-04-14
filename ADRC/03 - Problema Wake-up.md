In un sistema distribuito costituito dall'insieme delle entità $V$, il problema del **wake-up** consiste nel raggiungere una configurazione finale del sistema in cui tutti i nodi sono nello stato AWAKE, a partire da una situazione in cui un sottoinsieme di nodi $W \subseteq V$ iniziatori si trovano nello stato AWAKE e i restanti nodi in $V\setminus W$ si trovano nello stato ASLEEP.
# Il problema
Formalmente, il problema è descritto dalla tripla $P = \langle P_{init}, P_{final}, R \rangle$ in cui:
- $P_{init} = \left\{ W \subseteq V : W \neq \emptyset \land \forall x \in V, s(x) = \begin{cases} \text{awake} \quad&x \in W \\ \text{asleep} &x \in V \setminus W \end{cases} \right\}$
- $P_{final} = \forall x \in V, s(x) = \text{awake}$
- $R = \begin{cases} \text{Total Reliability (TR)} \\ \text{Bidirectional Link (BL)} \\ \text{Connectivity (CN)} \end{cases}$
# Protocollo WFLOOD
Per risolvere il task del Wake-Up, si progetta il seguente protocollo, che prende il nome di **WFLOOD**.
Durante l'esecuzione del protocollo, le entità assumono i seguenti stati:
- $S = \{ \text{awake, asleep} \}$
- $S_{init} = \{ \text{awake,asleep} \}$
- $S_{term} = \{ \text{awake} \}$
il protocollo è descritto dal seguente pseudocodice
```python
if state == "AWAKE":
    spontaneously:
		send("wake-up!") to N()
	receiving(msg):
	    None

if state == "ASLEEP":
    receiving(msg):
		send("wake-up!") to N() - sender
		state = "AWAKE"
```
Il protocollo WFLOOD è molto simile al protocollo FLOODING studiato nella lezione precedente. In particolare si osserva che il problema del broadcast è un caso speciale del problema del wake-up, nel quale è presente un solo nodo svegli. In altri termini, il problema del wake-up è come il problema broadcast in cui però possono esserci più nodi che posseggono l'informazione da condividere con gli altri nodi.
## Correttezza
La correttezza del WFLOOD segue direttamente dalla correttezza del FLOODING per il broadcast.
## Complessità
Si analizza ora la message e la ideal time complexity. Sia $k$ il numero di nodi inizialmente nello stato AWAKE, cioè $|W|=k$.
### Message Complexity
Ogni nodo in $W$ invia un messaggio a tutti i suoi vicini, mentre ogni nodo in $W\setminus V$ invia un messaggio a tutti i suoi vicini meno il vicino che lo ha fatto svegliare. Dunque il numero di messaggi totali inviati è pari a
$$
\begin{align}
M(\text{WFLOOD}) &= \sum_{u \in W} |N(u)| + \sum_{u \in V \setminus W} \left(|N(u)| - 1\right) \\
&= \sum_{u \in W} |N(u)| + \sum_{u \in V \setminus W} |N(u)| - \sum_{u \in V \setminus W} 1 \\
&= \sum_{u \in V} |N(u)| - \sum_{u \in V \setminus W} 1 \\ \\
&= 2m - (n-k) = 2m -n +k
\end{align}
$$
dalla formula si evince che il caso peggiore è quando $W \equiv V$, ovvero $n = k$: dato che ogni nodo non conosce lo stato degli altri, tutti i nodi mandano il messaggio a tutti i vicini, dunque la message complexity è proprio $2m$. Viceversa, il caso migliore è quando $k=1$, dove il protocollo WFLOOD si riduce all'esecuzione del protocollo FLOOD a singola sorgente, con message complexity $2m-n+1$.
# WFLOOD in un albero
Si supponga di eseguire il protocollo WFLOOD su una rete ad albero $T$. Essendo $T$ un albero, vale che $m=n-1$. Sostituendo alla formula calcolata in precedenza, si ottiene che la message complexity di WFLOOD su $T$ è pari a
$$
M(\text{WFLOOD/Tree}) = 2m - n +k = 2(n-1) -n +k = n+k-2
$$
___
#TODO Lower bound clique
___