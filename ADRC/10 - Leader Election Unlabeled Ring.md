In [07 - Leader Election](07%20-%20Leader%20Election.md) è stato mostrato che non è possibile risolvere *deterministicamente* il problema della Leader Election senza l'assunzione che tutti i nodi abbiano un identificativo univoco. In questa sezione, si studia un protocollo **probabilistico** che risolve tale problema in un sistema nel quale i nodi sono completamente **anonimi**.
# Leader election unlabeled ring
Sia $G=(V,E)$ una rete con $n$ nodi **anonimi**, dunque non identificati in modo univoco, dove $n$ è noto globalmente a tutti i nodi. Inoltre, ogni nodo sa di far parte di una rete con topologia ad anello. Infine, ogni nodo $v \in V$ ha accesso ad una fonte *privata* e *indipendente* di casualità, denotata con $s(v)$.
Sotto tali assunzioni e sotto le assunzioni standard $R$, è possibile definire un protocollo probabilistico che risolve il problema della leader election.
## Idea
L'idea dietro al protocollo è la seguente: ogni nodo sceglie uniformemente a caso e in modo indipendente un numero in $[m]$; tale numero diventa l'identificativo del nodo. A questo punto, per risolvere il problema è sufficiente eseguire uno dei protocolli deterministici conosciuti per la leader election su anello con l'assunzione $UI$ (Unique Identifier).
Certamente si ammette una certa probabilità di errore, infatti gli identificatori dei nodi potrebbero non essere univoci. Per rendere questa probabilità di errore molto piccola, si deve scegliere $m$ tale che con alta probabilità non ci saranno due nodi $v,w \in V$ tali che $id(v) = id(w)$. Dunque con questo protocollo probabilistico non si raggiunge con certezza una configurazione finale accettabile, ma tale configurazione desiderata verrà ottenuta con alta probabilità. Si ricorda che la configurazione finale accettabile è quella in cui c'è un nodo nello stato leader e tutti gli altri nodi nello stato follower.
## Il protocollo
Formalmente, il protocollo $\text{RL}$ è diviso in due fasi:
- **Fase 1.** Ogni nodo iniziatore $v \in V$ che si sveglia sceglie un valore $j_v$ *uniformly at random* dall'insieme $[m]$ e invia un messaggio di wake-up ai suoi vicini, che ripetono tale procedura. Si osserva che ogni nodo $v \in V$, può simulare la scelta dell'$id$ generando $\lfloor\log_{2}n\rfloor$ random bits in modo uniforme;
- **Fase 2.** Dopo che tutti i nodi sono svegli e hanno generato un loro $id$, esegui uno degli algoritmi di leader election deterministica studiati.

```ad-note
title: Notazione
Per denotare la scelta in modo uniforme di $j_v$ dall'insieme $[m]$, si utilizza la seguente notazione
$$
j_{v} \in_{U} [m] \iff \forall j \in [m] : \mathbf{Pr}(j_{v} = j) = \frac{1}{m}
$$
```

## Analisi
Si osserva che se al termine della fase 1, tutti i nodi hanno $id$ tra loro distinti, allora la fase 2 del protocollo termina sempre in modo corretto.
Si calcola ora la probabilità di errore del protocollo $RL$. Siano $\mathcal{E}$ e $\mathcal{B}$ i seguenti eventi
$$
\begin{align}
\mathcal{E} &:= \text{il protocollo RL fallisce} \\
\mathcal{B} &:= \exists v,w \in V: v \neq w \ \land \ j_{v} = j_{w}
\end{align}
$$
naturalmente vale che l'evento $\mathcal{E}$ implica l'evento $\mathcal{B}$ (o $\mathcal{E}\subseteq\mathcal{B}$) in quanto l'unico caso in cui il protocollo fallisce è quando gli $id$ vengono generati con almeno un doppione. In termine di probabilità vale quindi $\mathbf{Pr}(\mathcal{E}) \leqslant\mathbf{Pr}(\mathcal{B})$.
Per studiare l'evento $\mathcal{B}$, si analizza un processo di *balls-into-bins*. In particolare, ogni esecuzione del protocollo corrisponde ad un processo *balls-into-bins*: i nodi sono le **palle**, mentre tutti i possibili valori che gli $id$ dei nodi possono assumere sono i **secchi**. Secondo tale modello, l'evento $\mathcal{B}$ accade se e solo se 2 o più **palle** (nodi) cadono nello stesso **secchio** (possibile $id$). Al fine di calcolare la probabilità di tale evento si calcola la probabilità che $k$ palle, con $k>1$, finiscono nello stesso secchio, che è la seguente
$$
\binom{n}{k} \cdot \left( \frac{1}{m} \right)^k \cdot \left( 1 - \frac{1}{m} \right)^{n-k}
$$
sommando tale espressione al variare di $k$ per $k>1$ si ottiene la probabilità dell'evento
$$
\mathcal{B}_i = \text{almeno due palle (nodi) finiscono (assumono) nel secchio (l'$id$) $i$}
$$
cioè l'evento in cui il secchio (l'$id$) $i$ porta al fallimento del protocollo. Si limita ora tale probabilità
$$
\begin{align}
\mathbf{Pr}(\mathcal{B}_{i}) &= \mathbf{Pr}\left(\bigcup_{k=2}^n \{ \text{esattamente $k$ palle finiscono nel secchio $i$} \}\right)   \\
&= \sum_{k=2}^n \binom{n}{k} \cdot \left( \frac{1}{m} \right)^k \cdot \underbrace{\left( 1 - \frac{1}{m} \right)^{n-k}}_{\leqslant 1} \quad \text{(eventi disgiunti)}\\
&\leqslant \sum_{k=2}^n \binom{n}{k} \cdot \left( \frac{1}{m} \right)^k \\
&\leqslant \sum_{k=2}^n \left( \frac{e\cdot n}{k} \right)^k \left(\frac{1}{m} \right)^k \quad \text{(stirling su binomio)}\\
&\leqslant \left( \frac{e\cdot n}{2m} \right)^2 + \sum_{k=3}^n \left( \frac{e\cdot n}{k} \right)^k \left(\frac{1}{m} \right)^k \quad\text{(caccio fuori termine $k=2$)}\\
&\leqslant \left( \frac{e\cdot n}{2m} \right)^2 + n \left( \frac{e\cdot n}{3m}\right)^3 \quad (\star)\\
&= O\left( \frac{n^2}{m^2} + \frac{n^4}{m^3} \right)
\end{align}
$$
Dove il passaggio $(\star)$ è dato dal fatto che i termini della sommatoria diventano sempre più piccoli al crescere di $k$, in quanto $m\geqslant n$, dunque la sommatoria è sicuramente minore o uguale di $n$ volte il termine con $k=3$ (termine dominante).
Dato che il fallimento del protocollo può avvenire per colpa di un qualsiasi secchio, l'evento $\mathcal{B}$ è uguale all'unione degli eventi $\mathcal{B}_{i}$ al variare di $i \in [m]$. Dunque, applicando lo Union Bound, vale che
$$
\begin{align}
\mathbf{Pr}(\mathcal{E}) &\leqslant  \mathbf{Pr}\left(\bigcup_{i = 1}^m \mathcal{B}_{i}\right)  \\
&\leqslant \sum_{i=1}^m \mathbf{Pr}(\mathcal{B}_{i}) \\
&\leqslant m \cdot \mathbf{Pr}(\mathcal{B}_{1}) \\
&=  m \cdot O\left( \frac{n^2}{m^2} + \frac{n^4}{m^3} \right) \\
&= O\left( \frac{n^2}{m} + \frac{n^4}{m^2} \right)
\end{align}
$$
In conclusione, per far funzionare il protocollo con alta probabilità, è sufficiente scegliere $m\geqslant n^3$. In questo modo si ottiene
$$
\mathbf{Pr}(\mathcal{E}) \leqslant O\left( \frac{1}{n} +\frac{1}{n^2}\right)  = O\left( \frac{1}{n} \right)
$$
Si osserva che il termine $k=2$ della sommatoria è stato cacciato fuori per poter ottenere l'alta probabilità con $m\geqslant n^3$.