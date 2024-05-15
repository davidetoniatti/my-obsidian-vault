# Introduzione
Si consideri una rete modellata mediante un grafo connesso $G=(V,E).$ Ogni nodo $x \in V$ possiede un valore $value(x) \in \mathbb{R}.$ Si assuma di trovarsi nel modello Local sincrono sotto le classiche restrizioni $R,$ e che nella rete sia presente solamente un etichettamento dei nodi locale e non globale. Si vuole definire un protocollo nel quale ogni agente calcoli la media su tutti i valori posseduti da ciascuno di essi. In particolar modo, si osserva che in questo problema non è semplicemente richiesto di definire un protocollo che faccia si che ogni agente, al termine di esso, conosca la media tra tutti i valori, ma si vuole proprio che *ogni* agente esegua il calcolo della media: nel primo caso, si potrebbe sfruttare una combinazione dei protocolli più elementari visti in precedenza, ad esempio un protocollo per lo Spanning Tree Costruction seguito poi da un protocollo per la saturation su alberi e di average finding. Nel problema adesso affrontato si vuole proprio che ciascun agente calcoli e mandi in output la media dei valori. I motivi possono essere diversi, un esempio può essere la network resilience: nel caso in cui si potessero verificare faults, il mancato funzionamento dei nodi saturi porterebbe al fallimento del task distribuito.
Un altro motivo è l'efficienza: la combinazione dell'esecuzione di questi protocolli, su una rete di grandi dimensioni, potrebbe costare tempo eccessivo.

Si vogliono quindi trovare delle regole locali di aggiornamento dei dati di ciascun nodo, in maniera tale che il processo dinamico da esse definito porti in una configurazione finale dove ogni nodo è in grado di calcolare la media globale.

```ad-note
Una dinamica è quindi un insieme di regole locali che ciascun agente della rete esegue in maniera omogenea, ossia, non dipendono dallo stato in cui si trova il nodo: tutti i nodi, ad ogni passo del processo, eseguono le stesse regole.
```

Questa dinamica di averaging, combinata con un criterio di clusterizzazione opportuno, fornisce un protocollo distribuito in grado di riconoscere, se esistono, delle comunità all’interno di un grafo.
# Averaging Dynamics
La dinamica di averaging opera come segue:
- Al round $t=0$, ogni nodo $v \in V$ possiede un valore iniziale $x^{(0)}(v) \in \mathbb{R}.$
-  Ad ogni round successivo $t \geqslant 1,$ ogni nodo $v \in V$ aggiorna il suo valore $x^{(t)}(v)$ con la media dei valori posseduti dai suoi vicini al termine del round precedente $t-1.$

Si vuole studiare se questa dinamica converge, e nel caso in cui ciò fosse vero, quando e a cosa converge.

Si osserva che è possibile effettuare l'analisi dell'averaging dynamics su un grafo $G$ facendo riferimento al comportamento di una passeggiata aleatoria su $G.$
Sia quindi $G=(V,E)$ un grafo non diretto, connesso e non-bipartito, con $|V|=2n$, $A$ la sua matrice di adiacenza (quindi $A_{i,j}=1 \iff (i,j)\in E$ ) e $d_i$ il grado del nodo $i.$ La matrice di transizione del random walk su $G$ è la matrice $P=D^{-1}A,$ dove $D$ è la matrice diagonale tale per cui $D_{i,i} = d_i.$
L'entrata $P_{i,j} = (1/d_i)A_{i,j}$ corrisponde quindi alla probabilità di passare dal nodo $i$ al nodo $j$ in un passo della passeggiata aleatoria su $G.$


Si vuole mostrare che, sia $\bar{x}^{(0)}$ un qualsiasi vettore iniziale di valori, dopo $t$ rounds della dinamica di averaging, il vettore dei valori al tempo $t$ può essere scritto come 
$$
\bar{x}^{(t)} = P^t \bar{x}^{(0)}
$$
## Caso grafi non regolari
Si studia inizialmente il caso di grafi non regolari, dove $P$ deve esser presa trasposta:
$$
\bar{x}^{(t)} = (P^T)^t \bar{x}^{(0)} 
$$
Nota: Si osserva esplicitamente che, in caso di grafi regolari, $P=P^T$
Si consideri un qualsiasi vettore colonna iniziale $\bar{x}^{(0)}=<x^{(0)}(1),x^{(0)}(2),\ldots,x^{(0)}(2n)>^{T} \in [\mathbb{R}^+]^{2n}$ e sia $M=<\bar{x}^{(0)},\bar{1}>= \sum x^{(0)}(i) = \lVert\bar{x}^{(0)}\rVert_1.$
Si ha che 
$$
\bar{x}^{(1)} = P^T \bar{x}^{(0)} = (AD^{-1})\bar{x}^{(0)} = A(D^{-1}\bar{x}^{(0)})
$$
Si ha che

$$
D^{-1}\bar{x}^{(0)} = \begin{bmatrix} 
\ \frac{1}{d_1} & 0 & \cdots & 0\ \\
\ 0 & \frac{1}{d_2} & \cdots & 0\ \\
\vdots & \vdots & \ddots & \vdots \\
\ 0 & 0 & \cdots & \frac{1}{d_{2n}}
\end{bmatrix} 
\begin{bmatrix}
\ x^{(0)}(1)\ \\
\ x^{(0)}(2)\ \\
\ \vdots\ \\
\ x^{(0)}(2n)\ \\
\end{bmatrix}\ =\ 
\begin{bmatrix}
\ \frac{x^{(0)}(1)}{d_1}\ \\
\ \frac{x^{(0)}(2)}{d_2}\ \\
\ \vdots\ \\
\ \frac{x^{(0)}(2n)}{d_{2n}} \\
\end{bmatrix} \ 
$$
ed essendo che 

$$
A_{i,j} = \begin{cases} 
1 \textnormal{ se } (i ,j) \in E \\
\\
0 \textnormal{ altrimenti}

\end{cases}
$$
si ha
$$
A \cdot (D^{-1}\bar{x}^{(0)}) =\begin{bmatrix} 
\ A_{1,1} & A_{1,2} & \cdots & A_{1,2n}\ \\
\ A_{2,1} & A_{2,2} & \cdots & A_{2,2n}\ \\
\vdots & \vdots & \ddots & \vdots \\
\ A_{2n,1} & A_{2n,2} & \cdots & A_{2n,2n}
\end{bmatrix} \begin{bmatrix}
\ \frac{x^{(0)}(1)}{d_1}\ \\
\ \frac{x^{(0)}(2)}{d_2}\ \\
\ \vdots\ \\
\ \frac{x^{(0)}(2n)}{d_{2n}} \\
\end{bmatrix} \ = \begin{bmatrix}
\ \frac{\sum_{j \in N(1)}x^{(0)}(j)}{d_j}\ \\
\ \frac{\sum_{j \in N(2)}x^{(0)}(j)}{d_j}\ \\
\ \vdots\ \\
\ \frac{\sum_{j \in N(2n)}x^{(0)}(j)}{d_{j}} \\
\end{bmatrix} \ 
$$
[Dove, $\forall i = 1\ldots,2n,$ $(A(D^{-1}\bar{x}^{(0)}))(i)= \frac{1}{d_i}\sum_{j \in N(i)}x^{(0)}(j)$  è proprio la media dei valori dei vicini di $i$ all'istante $t=0.$ ]

Iterando questa procedura, si ha che per $t \geq 1:$
$$
\bar{x}^{(t)} = P^T \bar{x}^{(t-1)} = (P^T)^{t}\bar{x}^{(0)} = (AD^{-1})^t \bar{x}^{(0)}
$$

Si vuole studiare ora se tale metodo iterativo converge, eventualmente a quale vettore converge e quanto tempo impiega:

Si osserva che $P^T=AD^{-1}$ è stocastica per colonne
- Per ogni $j$ vale $\sum^{2n}_{i=1}P_{i,j}=1$ 
ed essendo positiva a valori reali, ammette quindi un range di $2n$ autovettori $\bar{v}_1,\bar{v}_2,\ldots, \bar{v}_{2n}$ con rispettivi autovalori $\lambda_1,\lambda_2,\ldots, \lambda_{2n}$ tali per cui $-1 \leq \lambda_i \leq 1$ per ogni $i\in\{1,\ldots,2n\}.$   
Inoltre, essendo $G$ connesso e non bipartito, si ha che $\lambda_1 = 1, |\lambda_i| < 1$ per $i=2,\ldots,2n$ e $\lambda_{2n} > -1.$ 
Date queste proprietà, si consideri il primo autovettore $\bar{v}_1$ di $P,$ avente autovalore $\lambda_1 = 1.$ 
Allora, 
$$
P^T\bar{v}_1 = \bar{v}_1 \iff (AD^{-1})\bar{v}_1 = A(D^{-1}\bar{v}_1 ) = \bar{v}_1
$$
Si prendano gli autovettori $\bar{v}_1,\bar{v}_2,\ldots,\bar{v}_{2n}$ di $P^T,$ ciascuno di essi associato al rispettivo autovalore $\lambda_i.$ Tali autovettori formano una base ortogonale in $\mathbb{R}^{2n}.$
E' possibile quindi, qualunque sia il vettore iniziale, scrivere $\bar{x}^{(0)}$ come combinazione lineare di tali autovettori:
$$
\bar{x}^{(0)} = \alpha_{1}\bar{v}_{1}+ \alpha_{2}\bar{v}_{2} \ldots \alpha_{2n}\bar{v}_{2n}
$$

dove $$\alpha_1 = \frac{<\bar{x}^{(0)},\bar{v}_1>}{(\| v_1\|_2)^2} $$  
sviluppando si ottiene
$$
<\bar x^{(0)},\bar{v}_1> = \sum_{i=1}^{2n}\bar{x}^{(0)}(i) \cdot \bar{v}_1(i) 
$$
e
$$
(\| v_1\|_2)^2 = \sqrt{\left(\sum_{i=1}^{2n}\bar{v}_1(i)^2 \right)^2} =  \sum_{i=1}^{2n}\bar{v}_1(i)^2
$$
e quindi 
$$
\alpha_1 = \frac{\sum_{i=1}^{2n}\bar{x}^{(0)}(i) \cdot \bar{v}_1(i) }{\sum_{i=1}^{2n}\bar{v}_1(i)^2}
$$

Allora
$$
P^T \bar{x}^{(0)} = P^T (\alpha_{1}\bar{v}_{1}+ \alpha_{2}\bar{v}_{2} \ldots \alpha_{2n}\bar{v}_{2n}) = \alpha_{1}\bar{v}_{1}+ \alpha_{2}\lambda_2\bar{v}_{2} \ldots \alpha_{2n}\lambda_{2n}\bar{v}_{2n}
$$
e, iterando, si ha che $\forall t \geq 1:$
$$
(P^T)^{t} \bar{x}^{(0)} = \alpha_{1}\bar{v}_{1}+ \alpha_{2}\lambda^t_2\bar{v}_{2} \ldots \alpha_{2n}\lambda^t_{2n}\bar{v}_{2n}
$$

Essendo che $|\lambda_i| < 1$ per $i=2,\ldots,2n$ si ha che, al crescere di $t,$ $\lambda^t_i \rightarrow 0$ per $i = 2,\ldots, 2n.$ 
Quindi $(P^T)^t \bar{x}^{(0)}$ converge ad $\alpha_1 \bar{v}_1.$ Si deve trovare quindi $\bar{v}_1$ tale per cui $P^T\bar{v}_1 = \bar{v}_1,$ ossia  tale per cui $(AD^{-1}) \bar{v}_1 = \bar{v}_1.$
Per ogni $i=1,\ldots,2n,$ si definisce
$$
\bar{v}_1(i) = \frac{d_i}{2m}
$$
Si ha quindi che 
$$
P^T \bar{v}_1 =  \begin{bmatrix} 
\ \frac{1}{d_1} & 0 & \cdots &\frac{1}{d_{2n}} \ \\
\ 0 & \frac{1}{d_2} & \cdots & \frac{1}{d_{2n}}\ \\
\vdots & \vdots & \ddots & \vdots \\
\ 0 & 0 & \cdots & \frac{1}{d_{2n}}
\end{bmatrix} 
\begin{bmatrix}
\ \frac{d_1}{2m} \ \\
\ \frac{d_2}{2m}\ \\
\ \vdots\ \\
\ \frac{d_{2n}}{2m} \ \\
\end{bmatrix}\ =\ 
\begin{bmatrix}
\ y(1)\ \\
\ y(2)\ \\
\ \vdots\ \\
\ y(2n) \\
\end{bmatrix} \ 
$$
(Nota: $P^T$ ha nella riga $i,$ $d_i$ entrate diverse da $0,$ in particolar modo, se $(i,j) \in E,$ $P^T[i,j] = \frac{1}{d_j})$  
ed essendo $$y(i) = \sum_{j\in N(i)} \frac{1}{d_j} \cdot \frac{d_j}{2m}  = \frac{d_i}{2m} = \bar{v}_1(i)$$per ogni $i,$ si ha proprio che $P^T \bar{v}_1 = \bar{v}_1.$ 
Si ha quindi che, per $t \rightarrow \infty,$ vale
$$
(P^T)^t \bar{x}^{(0)} \rightarrow \alpha_1 \bar{v}_1 =  \frac{\sum_{i=1}^{2n}\bar{x}^{(0)}(i) \cdot \bar{v}_1(i) }{\sum_{i=1}^{2n}\bar{v}_1(i)^2} \cdot \bar{v}_1
$$
e quindi  per ogni $j=1,\ldots,2n$ 
$$((P^T)^t \bar{x}^{(0)})(j) \rightarrow \frac{\sum_{i=1}^{2n}\bar{x}^{(0)}(i) \cdot \bar{v}_1(i) }{\sum_{i=1}^{2n}\bar{v}_1(i)^2} \cdot \bar{v}_1(j) = \frac{\sum_{i=1}^{2n}\bar{x}^{(0)}(i) \cdot \frac{d_i}{2m} }{\sum_{i=1}^{2n}\left(\frac{d_i}{2m}\right)^2} \cdot \frac{d_j}{2m} = \frac{\sum_{i=1}^{2m}\bar{x}^{(0)}d_i}{\sum_{i=1}^{2n}d_i^2} \cdot d_j $$ 

Ma quest'ultimo vettore non è la media, quindi in generale, l'avg-dyn su grafi non regolari non tende alla media.
## Grafi regolari 
Si studia ora il caso di grafi regolari. In questo caso, $P$ è simmetrica.
Si consideri un qualsiasi vettore colonna iniziale $\bar{x}^{(0)}=<x^{(0)}(1),x^{(0)}(2),\ldots,x^{(0)}(2n)>^{T} \in [\mathbb{R}^+]^{2n}$ e sia $M=<\bar{x}^{(0)},\bar{1}>= \sum x^{(0)}(i) = \lVert\bar{x}^{(0)}\rVert_1.$
Si ha che 
$$
\bar{x}^{(1)} = P \bar{x}^{(0)} = (D^{-1}A)\bar{x}^{(0)} = (D^{-1}A\bar{x}^{(0)})
$$
essendo che
$$
A_{i,j} = \begin{cases} 
1 \textnormal{ se } (i ,j) \in E \\
\\
0 \textnormal{ altrimenti}

\end{cases}
$$
si ha
$$
A\bar{x}^{(0)} = \begin{bmatrix} 
\ A_{1,1} & A_{1,2} & \cdots & A_{1,2n}\ \\
\ A_{2,1} & A_{2,2} & \cdots & A_{2,2n}\ \\
\vdots & \vdots & \ddots & \vdots \\
\ A_{2n,1} & A_{2n,2} & \cdots & A_{2n,2n}
\end{bmatrix} 
\begin{bmatrix}
\ x^{(0)}(1)\ \\
\ x^{(0)}(2)\ \\
\ \vdots\ \\
\ x^{(0)}(2n)\ \\
\end{bmatrix}\ =\ 
\begin{bmatrix}
\ \sum_{j \in N(1)} x^{(0)}(j)\ \\
\ \sum_{j \in N(2)} x^{(0)}(j) \\
\ \vdots\ \\
\ \sum_{j \in N(2n)} x^{(0)}(j) \\
\end{bmatrix} \ 
$$
Quindi
$$
D^{-1}(A\bar{x}^{(0)}) = \begin{bmatrix} 
\ \frac{1}{d} & 0 & \cdots & 0\ \\
\ 0 & \frac{1}{d} & \cdots & 0\ \\
\vdots & \vdots & \ddots & \vdots \\
\ 0 & 0 & \cdots & \frac{1}{d}
\end{bmatrix} \begin{bmatrix}
\ \sum_{j \in N(1)} x^{(0)}(j)\ \\
\ \sum_{j \in N(2)} x^{(0)}(j) \\
\ \vdots\ \\
\ \sum_{j \in N(2n)} x^{(0)}(j) \\
\end{bmatrix} \  =\begin{bmatrix}
\ \frac{1}{d}\sum_{j \in N(1)}x^{(0)}(j)\ \\
\ \frac{1}{d}\sum_{j \in N(2)}x^{(0)}(j)\ \\
\ \vdots\ \\
\ \frac{1}{d}\sum_{j \in N(2n)}x^{(0)}(j) \\
\end{bmatrix} \ 

$$

Dove, $\forall i = 1\ldots,2n,$ $(D^{-1}(A\bar{x}^{(0)}))(i)= \frac{1}{d}\sum_{j \in N(i)}x^{(0)}(j)$  è proprio la media dei valori dei vicini di $i$ all'istante $t=0.$
Iterando questa procedura, si ha che per $t \geq 1:$
$$
\bar{x}^{(t)} = P \bar{x}^{(t-1)} = P^{t}\bar{x}^{(0)} = (D^{-1}A)^t \bar{x}^{(0)}
$$

Si vuole studiare ora se tale metodo iterativo converge, eventualmente a quale vettore converge e quanto tempo impiega:

Si osserva che $P=D^{-1}A$ è doppiamente stocastica, ossia stocastica sia per righe che per colonne:
- Per ogni $j$ vale $\sum^{2n}_{i=1}P_{i,j}=1$ (stocastica per colonne)
- Per ogni $i$ vale $\sum_{j=1}^{2n} P_{i,j} = 1$ (stocastica per righe)
ed essendo anche simmetrica positiva a valori reali, ammette quindi un range di $2n$ autovettori $\bar{v}_1,\bar{v}_2,\ldots, \bar{v}_{2n}$ con rispettivi autovalori $\lambda_1,\lambda_2,\ldots, \lambda_{2n}$ tali per cui $-1 \leq \lambda_i \leq 1$ per ogni $i\in\{1,\ldots,2n\}.$   
Inoltre, essendo $G$ connesso e non bipartito, si ha che $\lambda_1 = 1, |\lambda_i| < 1$ per $i=2,\ldots,2n$ e $\lambda_{2n} > -1.$ 
Date queste proprietà, si consideri il primo autovettore $\bar{v}_1$ di $P,$ avente autovalore $\lambda_1 = 1.$ 
Allora, 
$$
P\bar{v}_1 = \bar{v}_1 \iff (D^{-1}A)\bar{v}_1 = D^{-1}(A\bar{v}_1 ) = \bar{v}_1
$$
Dove $P$ è fatta in maniera tale che, sia $P_{i}$ la sua $i-$esima riga, allora tale riga contiene esattamente $d$ entrate poste a $1/d,$ mentre le altre $2n-d$ sono poste a zero.

Si prendano gli autovettori $\bar{v}_1,\bar{v}_2,\ldots,\bar{v}_{2n}$ di $P,$ ciascuno di essi associato al rispettivo autovalore $\lambda_i.$ Tali autovettori formano una base ortogonale in $\mathbb{R}^{2n}.$
E' possibile quindi, qualunque sia il vettore iniziale, scrivere $\bar{x}^{(0)}$ come combinazione lineare di tali autovettori:
$$
\bar{x}^{(0)} = \alpha_{1}\bar{v}_{1}+ \alpha_{2}\bar{v}_{2} \ldots \alpha_{2n}\bar{v}_{2n}
$$

Allora
$$
P \bar{x}^{(0)} = P (\alpha_{1}\bar{v}_{1}+ \alpha_{2}\bar{v}_{2} \ldots \alpha_{2n}\bar{v}_{2n}) = \alpha_{1}\bar{v}_{1}+ \alpha_{2}\lambda_2\bar{v}_{2} \ldots \alpha_{2n}\lambda_{2n}\bar{v}_{2n}
$$
e, iterando, si ha che $\forall t \geq 1:$
$$
P^{t} \bar{x}^{(0)} = \alpha_{1}\bar{v}_{1}+ \alpha_{2}\lambda^t_2\bar{v}_{2} \ldots \alpha_{2n}\lambda^t_{2n}\bar{v}_{2n}
$$

Essendo che $|\lambda_i| < 1$ per $i=2,\ldots,2n$ si ha che, al crescere di $t,$ $\lambda^t_i \rightarrow 0$ per $i = 2,\ldots, 2n.$ 
Quindi $P^t \bar{x}^{(0)}$ converge ad $\alpha_1 \bar{v}_1.$ Si deve trovare quindi $\bar{v}_1$ tale per cui $P\bar{v}_1 = \bar{v}_1,$ ossia  tale per cui $(D^{-1}A) \bar{v}_1 = \bar{v}_1.$
Per ogni $i=1,\ldots,2n,$ si definisce
$$ 
\bar{v}_1(i) = \frac{d}{2m} = \frac{d}{\frac{2dn}{2}} = \frac{1}{2n}
$$
Si ha quindi che 
$$
P \bar{v}_1 =  \begin{bmatrix} 
\ \frac{1}{d} & 0 & \cdots &\frac{1}{d} \ \\
\ 0 & \frac{1}{d} & \cdots & \frac{1}{d}\ \\
\vdots & \vdots & \ddots & \vdots \\
\ 0 & 0 & \cdots & \frac{1}{d}
\end{bmatrix} 
\begin{bmatrix}
\ \frac{1}{2n} \ \\
\ \frac{1}{2n}\ \\
\ \vdots\ \\
\ \frac{1}{2n} \ \\
\end{bmatrix}\ =\ 
\begin{bmatrix}
\ y(1)\ \\
\ y(2)\ \\
\ \vdots\ \\
\ y(2n) \\
\end{bmatrix} \ 
$$
ed essendo $$y(i) = \sum_{j\in N(i)} \frac{1}{d}\cdot\frac{1}{2n}   = \frac{1}{2n}$$ per ogni $i,$ si ha proprio che $P \bar{v}_1 = \bar{v}_1.$ 


Si prende una base ortonormale di autovalori di $P,$ che si costruisce come segue:
- Si calcola $$\| \bar{v}_1 \|_2 = \sqrt{\sum_{i=1}^{2n} \bar{v}_1(i)^2 } = \sqrt{\sum_{i=1}^{2n}\left(\frac{1}{2n}\right)^2} =  \sqrt{\frac{1}{2n}}$$
- Per ogni $j=1,\ldots,2n,$ si calcola $$\bar{u}_i = \frac{1}{\| \bar{v}_1 \|_2} \cdot \bar{v}_i$$
Dove, in particolar modo 
$$
\bar{u}_1 = \frac{1}{\frac{1}{\sqrt{2n}}}\cdot \frac{1}{2n} \bar{1} = \frac{\sqrt{2n}}{2n} \bar{1} =\frac{1}{\sqrt{2n}} \bar{1} 
$$
La sequenza di vettori $\bar{u}_1, \bar{u}_2, \ldots, \bar{u}_{2n}$ cosi ottenuta è una base ortonormale di $P.$
Come prima, vale

$$
P^t \bar{x}^{(0)} = \alpha_{1}\bar{u}_{1}+ \alpha_{2}\lambda^t_2\bar{u}_{2} \ldots \alpha_{2n}\lambda^t_{2n}\bar{u}_{2n}
$$
e per $t \rightarrow \infty,$ $P^t \bar{x}^{(0)}$ tende ad $\alpha_1 \bar{u}_1$ dove, essendo $\bar{u}_1,\ldots, \bar{u}_{2n}$ una base ortonormale:
$$
 \alpha_1 = <\bar{x}^{(0)},\bar{u}_1>
$$
e quindi 
$$
\alpha_1\bar{u}_1 = <\bar{x}^{(0)},\bar{u}_1> \bar{u}_1 = \sum_{i=1}^{2n}x^{(0)}(i) \cdot \frac{1}{\sqrt{2n}} \cdot\frac{1}{\sqrt{2n}} \cdot \bar{1}= \frac{\sum_{i=1}^{2n}x^{(0)}(i)}{2n} \bar{1} 
$$
Quindi, tutti i valori dei nodi convergono alla media globale dei valori iniziali.

Ci si chiede quale sia ora il tempo di convergenza, ossia quante iterazioni $t$ siano necessarie affinché si raggiunga una soluzione buona. Si termina il metodo iterativo quando $\| P^{t+1}\bar{x}^{(0)} - P^t \bar{x}^{(0)} \|_1 \leq \varepsilon.$ Essendo $P^{t} \bar{x}^{(0)} = \alpha_{1}\bar{v}_{1}+ \alpha_{2}\lambda^t_2\bar{v}_{2} \ldots \alpha_{2n}\lambda^t_{2n}\bar{v}_{2n}$ si ha che 
$$
\begin{align}
\| P^{t+1}\bar{x}^{0} - P^t \bar{x}^{(0)}\|_1 &\leq \| (\alpha_1\bar{v}_1 - \alpha_1\bar{v}_1) + (1-\lambda_2)\lambda_2^t\alpha_2 \bar{v}_2 + \ldots + (1-\lambda_{2n})\lambda_{2n}^t\alpha_{2n}\bar{v}_{2n} \|_1 \\
\\
&\leq 1(1-\bar{\lambda})\bar{\lambda}^t \alpha_{max} \| v_{i} \|_1 2n \leq (M2n)\bar{\lambda}^t
\end{align}
$$
dove
- $M= \| \bar{x}^{(0)} \|_1$ , $\alpha_{max} \leq M.$  
- $\bar{\lambda} = \max\{\lambda_i | i \geq 2\};$ $| \bar{\lambda}| < 1$ 

Si ha che 
$$
(M2n)\bar{\lambda}^t < \varepsilon \iff \bar{\lambda}^t > \frac{\varepsilon}{M2n} \iff t \log \lambda > \log\left(\frac{\varepsilon}{M2n}\right) \iff t > \frac{\log \left(\frac{\varepsilon}{M2n}\right)}{\log \lambda}
$$ 
# Distributed Block Reconstruction
La block reconstruction di un grafo clusterizzato è una $2-$colorazione dei nodi che separa le comunità da individuare, e può essere definito in due versioni a seconda se una piccola frazione di outliers sia ammessa o meno.

Con il termine outliers, ci si riferisce ad un valore anomalo in un insieme di osservazioni statistiche, ossia un valore distante dalle altre osservazioni disponibili. Sono quindi valori numericamente distanti dal resto dei dati raccolti.

Si da quindi una definizione formale di block reconstruction:
```ad-Definizione
title: Definizione (Block reconstruction)
Sia $G=((V_1,V_2),E)$ un grafo con $V_1 \cap V_2 = \emptyset.$ Una $\varepsilon-$ricostruzione debole è una funzione $f: V_1 \cup V_2 \rightarrow \{\text{red,blue}\}$ tale che esistono due sottoinsiemi $W_1 \subseteq V_1, W_2 \subseteq V_2$ tali per cui 
$$
|W_1 \cup W_2| \geqslant (1-\varepsilon)|V_1 \cup V_2|\ \wedge \ f(W_1) \cap f(W_2) = \emptyset
$$
Quando $\varepsilon = 0,$ si dice che $f$ è una ricostruzione forte.
```

Dato quindi un grafo $G=((V_1,V_2),E),$ il problema della block recostruction richiede di calcolare una $\varepsilon-$ricostruzione di $G.$ Per risolvere questo task, si studia un protocollo distribuito basato sull'averaging dynamics, e che produce una colorazione dei nodi al termine di ogni round.

____
**Averaging protocol:**

**Initialization:** Al round $t=0$ ogni nodo $v\in V$ sceglie indipendentemente il suo valore da $\{-1,+1\}$ uniformly at random.

**Updating rule:** Ad ogni round successivo $t \geqslant 1,$ ogni nodo $v \in V:$
1. (Averaging dynamics) Aggiorna il suo valore $x^{(t)}(v)$ con la media dei valori posseduti dai suoi vicini al termine del round precedente $t-1,$
2. (Coloring) Se $x^{(t)}(v) \geqslant x^{(t-1)}(v)$ allora $v$ imposta $\texttt{color}^{(t)}(v) = \text{blue},$ altrimenti $v$ imposta $\texttt{color}^{(t)}(v) = \text{red}.$ 
___

Prima di entrare nei dettagli dell'analisi del protocollo, si fanno le seguenti osservazioni:
- L'algoritmo è completamente ignaro del tempo, essendo questa una dinamica nel senso più stretto, ossia, dopo l'inizializzazione, il protocollo viene eseguito iterativamente da ogni nodo. La convergenza ad una ricostruzione è una proprietà del protocollo, la quale non è nota ai nodi. E' qualcosa che prima o poi accade.
- Il criterio di clustering è completamente *locale,* nel senso che una decisione è individualmente e indipendentemente fatta da ciascun nodo in ciascun round, basata solo sullo stato posseduto da esso nel round corrente e nel round precedente.

Si da la seguente definizione:
```ad-Definizione
title: Definizione
Per $i=1,2,$ si definisce $\mathbf{1}_{V_i}$ come il vettore $|V|$-dimensionale avente come $j-$esima componente $1$ se $j\in V_i,$ $0$ altrimenti.
Se $(V_1,V_2)$ è una bipartizione dei nodi con $|V_1|=|V_2|=n,$ si definisce il vettore indicatore di partizione $\mathbf{\aleph} = \mathbf{1}_{V_1}-\mathbf{1}_{V_2}$
```

Se $G$ è $d-$regolare, allora $P=(1/d)A$ è una matrice simmetrica a valori reali positivi, $P$ ed $A$ hanno lo stesso insieme di autovettori, e $\bar{1}$ è un autovettore con autovalore $1.$ 
Ricordando che si denota con $\bar{v}_1,\bar{v}_2, \ldots, \bar{v}_{2n}$ una base di vettori ortonormali, dove $v_i$ è l'autovettore associato all'autovalore $\lambda_i$
$$
\bar{v}_1 = (1/\sqrt{2n}), \bar{v}_2,\ldots, \bar{v}_{2n}
$$

Si può quindi scrivere $\bar{x}^{(0)}$ come combinazione lineare 
$$
P^t \bar{x}^{(0)} = \frac{1}{2n} \left(\sum_{i}\bar{x}^{(0)}(i)\right)\bar{1} + \sum_{i=2}^{2n} \lambda_i^t \alpha_i v_i
$$
e si è mostrato che $\bar{x}^{(t)} = P^t \bar{x}^{(0)}$ tende ad $\alpha_1 \bar{v}_1$ per $t \rightarrow \infty,$ ossia, converge al vettore avente la media di $\bar{x}^{(0)}$ in ogni coordinata.

Si dice che un grafo $d-$regolare $G$ è $(2n,d,b)-$regolare se una partizione dei nodi $(V_1,V_2)$ esiste tale per cui ogni nodo in $V_1$ ha $b$ vicini in $V_2$ ed ogni nodo in $V_2$ ha $b$ vicini in $V_1.$
Sia quindi $G=((V_1,V_2), E)$ un grafo $(2n,d,b)$-regolare, con $|V_1|=|V_2|=n$. Si ha quindi che $\forall i \in V_1 \cup V_2,$ $d^{(out)}(i)=b.$ Si prende $b<<d-b$


**Osservazione:** Se $G$ è un grafo $(2n,d,b)-$regolare, allora $\mathbf{\aleph} = \mathbf{1}_{V_1} - \mathbf{1}_{V_2}$ è un autovettore della matrice di transizione $P$ di $G$ con autovalore $1-2b/d.$ 
**Dimostrazione:** Ogni nodo $i$ ha $b$ vicini $j$ nel lato opposto della partizione, tali per cui $\aleph(j)= -\aleph(i),$ e $d-b$  vicini $j$ nello stesso lato, tali per cui $\aleph(j)=\aleph(i),$ quindi

$$
(P\aleph)_i = \frac{1}{d} ((d-b)\aleph(i)-b\aleph(i)) = \left(1 - \frac{2b}{d}\right) \aleph(i)
$$


Si mostra che se il grafo regolare è "ben" clusterizzato, allora il protocollo di averaging produce una ricostruzione forte dei due clusters con alta probabilità. Per grafi ben clusterizzati, si intende un grafo $(2n,d,b)-$regolare dove $(V_1,V_2)$ rappresenta l'unico taglio minimo.

```ad-Definizione
title: Definizione
Un grafo $(2n,d,b)-$clusterizzato regolare $G=((V_1,V_2),E)$ è un grafo $(2n,d,b)-$regolare tale per cui $1-2b/d$ è il secondo più grande autovalore di $P$, ossia $\lambda_2 = 1 - 2b/d$ e $\lambda_i < \lambda_2$ per $i=3,\ldots,2n$.
```

Si da ora un lemma che mostra che dopo $t$ rounds della dinamica di averaging su un grafo $(2n,d,b)-$clusterizzato regolare, la configurazione $(\bar{x}^{(0)})^t$ è vicino a una combinazione lineare di $\bar{1}$ e $\mathbb{\aleph}.$ 

```ad-Lemma
Si assuma di eseguire la dinamica di averaging su un Grafo $G$ $(2n,d,b)-$clusterizzato regolare con un qualsiasi vettore iniziale $\bar{x}^{{0}} \in \{-1,1\}^{2n}.$ Allora esistono due reali $\alpha_1, \alpha_2$ tali per cui, ad ogni round $t,$ si ottiene
$$
\bar{x}^{(t)} = \alpha_1 \bar{1} + \alpha_2 \lambda_2^t \mathbf{\aleph} + \mathbf{e}^{(t)}
$$
dove
$$
\| \mathbf{e}^{(t)} \|_{\infty} \leqslant \lambda^t \sqrt{2n}
$$
```

**Dimostrazione:**

Essendo $\bar{x}^{(t)} = P^t \bar{x}^{(0)},$ si può scrivere
$$
P^t \bar{x}^{(0)} = \sum_{i} \lambda_i^t <\bar{x}^{(0)},\bar{v}_i> \bar{v}_i
$$

dove $1= \lambda_1 > \lambda_2 = 1 - 2b/d > \lambda_3 \geqslant \ldots \geqslant \lambda_{2n}$ sono gli autovalori di $P$ e $\bar{v}_1 = \frac{1}{\sqrt{2n}} \bar{1}, \bar{v}_2 = \frac{1}{\sqrt{2n}} \mathbb{\aleph}, \bar{v}_3, \ldots, \bar{v}_{2n}$ una corrispondente sequenza di vettori ortonormali. Allora
$$
\begin{align}
\bar{x}^{(t)} &= \frac{1}{2n}<\bar{x}^{(0)}, \bar{1}> \bar{1} + \lambda_2^t \frac{1}{2n}<\bar{x}^{(0)}, \mathbb{\aleph}>\mathbb{\aleph} + \sum_{i=3}^{2n} \lambda_i^t \alpha_i \bar{v}_i 
\\
\\
&=\alpha_1 \bar{1} + \alpha_2\lambda_2^t \mathbb{\aleph} + \sum_{i=3}^{2n}\lambda_i^t\alpha_i\bar{v}_i
\end{align}
$$

dove $\alpha_1 = \frac{1}{2n}<\bar{x}^{(0)}, \bar{1}>$ e $\alpha_2 =  \frac{1}{2n}<\bar{x}^{(0)}, \mathbb{\aleph}>.$ Si da un bound sulla norma $\infty$ dell'ultimo termine:

$$
\left|\left| \sum_{i=3}^{2n} \lambda_i^t \alpha_i \bar{v}_i \right|\right|_{\infty} \leqslant \left|\left| \sum_{i=3}^{2n} \lambda_i^t \alpha_i \bar{v}_i \right|\right|_{2}  = \sqrt{\sum_{i=3}^{2n} \lambda_i^{2t} \alpha_i^2  } \leq \lambda^t \sqrt{\sum_{i=1}^{2n}  \alpha_i^2 } = \lambda^t \|\bar{x}^{(0)}\|_2 = \lambda^t \sqrt{2n}
$$


Informalmente, quest'equazione suggerisce la scelta della regola di colorazione nel protocollo di averaging, una volta che si è considerata la differenza di due valori consecutivi di un qualsiasi nodo $u,$ ossia 
$$
\bar{x}^{(t-1)}(u) - \bar{x}^{(t)}(u) = \alpha_2 \lambda_2^{t-1}(1-\lambda_2)\aleph(u) + e^{(t-1)}(u)-e^{(t)}(u)
$$

Intuitivamente, se $\lambda$ è sufficientemente piccolo, si può sfruttare il bound su $\| \mathbf{e}^{(t)} \|_{\infty}$ per mostrare che, dopo una breve fase iniziale, il segno di $x^{(t-1)}(u)-x^{(t)}(u)$ è essenzialmente determinato da $\aleph(u),$ quindi dalla comunità a cui $u$ appartiene, con alta probabilità. 
Il seguente teorema formalizza questo fatto

```ad-Teorema
title: (Ricostruzione forte)
Sia $G=((V_1,V_2),E)$ un grafo connesso $(2n,d,b)$-clusterizzato regolare con $\lambda_2 = 1-2b/d > (1+\delta)\lambda$ per una costante arbitrariamente piccola $\delta >0.$ Allora il protocollo di averaging produce una ricostruzione forte entro $O(\log n)$ rounds, con alta probabilità.
```

**Dimostrazione:**
Per l'equazione precedente, si ha che $\texttt{sgn}(x^{(t-1)}(u)-x^{(t)}(u)) = \texttt{sgn}(\alpha_2 \aleph(u))$ ogni volta che vale 
$$
\left| \alpha_2\lambda_2^{(t-1)}(1-\lambda_2)\right| > \left|e^{(t-1)}(u)-e^{(t)}(u) \right|  (\star1\star)
$$

Per il bound su $\| \mathbf{e}^{(t)} \|_{\infty}$ si ha che $|e^{(t)}(u)|\leq \lambda^t \sqrt{2n},$ quindi tale disuguaglianza è vera per tutti i $t$ tali per cui 
$$
t-1 \geqslant \log \left( \frac{2\sqrt{2n}}{|\alpha_2|(1-\lambda_2)}\right) \cdot \frac{1}{\log(\lambda_2/\lambda)}
$$
Il lato destro di quest'ultima disuguaglianza può essere delimitata superiormente come segue
$$
\begin{align*}
\log \left(\frac{2\sqrt{2n}}{|\alpha_2|(1-\lambda_2)}\right) \cdot \frac{1}{\log(\lambda_2/\lambda)} &= \log \left(\frac{d\sqrt{2n}}{b|\alpha_2|}\right) \cdot \frac{1}{\log(\lambda_2/\lambda)} < \log \left(\frac{d\sqrt{2n}}{b|\alpha_2|}\right) \cdot \frac{1}{\log(1+\delta)} \\
&\leq \frac{2}{\delta} \left(\frac{n\sqrt{2n}}{|\alpha_2|}\right) \ (\star2\star)
\end{align*}
$$

dove la prima uguaglianza segue dal fatto che $\lambda_2 = 1 -2b/d$ nel caso di grafi $(2n,d,b)-$clusterizzati regolari, la seconda disuguaglianza segue dall'ipotesi sul gap spettrale tra $\lambda_2$ e $\lambda,$ mentre la terza disuguaglianza segue dal fatto che $\log(1+\delta)\geqslant \delta/2$ per $\delta$ sufficientemente piccolo e dal fatto che $d/b \leqslant n.$ 
Il secondo passo chiave della dimostrazione si basa sulla randomness del vettore iniziale.
Essendo $x^{(0)}$ un vettore di variabili aleatorie indipendente ed uniformemente distribuite in $\{-1,1\},$ la differenza in valore assoluto tra le due medie parziali nelle due comunità, ossia $|\alpha_2|,$ è sufficientemente grande con alta probabilità. Più precisamente, si fa riferimento al seguente lemma:

Se si prende $x^{(0)}$ uniformy at random da $\{-1,1\}^{2n}$ allora, per ogni $\delta >0$ e per ogni vettore fissato $w \in \{-1,1\}^{2n}$ si ha che 
$$
\mathbf{Pr} \left(\left| \langle(1/\sqrt{2n}) w,x^{(0)} \rangle\right| \leqslant \delta\right) \leqslant O(\delta)
$$

Quindi, sia $R$ la somma delle $2n$ variabili aleatorie, per ogni $0 < \eta < 1,$ si ha 
$$
\mathbf{Pr} \left(|R| \leqslant \eta \sqrt{2n}\right) \leqslant O(\eta)
$$

essendo $\alpha_2= \frac{1}{2n} \langle \aleph, x^{(0)}\rangle,$ la disuguaglianza precedente implica che
$$
|\alpha_2| = \frac{1}{2n} \langle \aleph, x^{(0)} \rangle = \frac{1}{2n} \left(\left| \sum_{j\in V_1}x^{(0)}(j) - \sum_{j \in V_2}x^{(0)}(j) \right|\right)\geqslant n^{-\gamma}
$$

per una costante positiva $\gamma$ con alta probabilità. Allora, si può delimitare superiormente $(\star2\star)$ come segue:
$$
\frac{2}{\delta} \log \left(\frac{n \sqrt{2n}}{|\alpha_2|}\right) \leqslant \frac{2}{\delta} \log \left(\sqrt{2} \cdot n^{y+3/2}\right)
$$
Quindi, per per qualche $\gamma$, con alta probabilità $(\star 1 \star)$ è soddisfatta ogni volta che
$$
t-1 \geqslant \frac{2}{\delta} \log\left(\sqrt{2} n^{\gamma +3/2}\right) = \frac{2}{\delta} \left(\gamma + \frac{3}{2}\right)\log n + \frac{2}{\delta} \log \sqrt{2}
$$
