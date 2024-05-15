Ricordando che la maggioranza è colorata di $1$, sia $s_{t} = b_{t} - \frac{n}{2}$ il valore del *bias* al round $t$ e sia $s_{t+1}$ una variabile aleatoria che indica il *bias* al round $t+1$. Sia $\forall i \in [n], \forall t = 0,1,2,\dots, \ Z_t^i$ la variabile aleatoria che vale $1$ se il nodo $i$ si colora di $1$ al round $t$ e $0$ altrimenti. Sapendo che $b_{t} = s_{t}+\frac{n}{2}$, vale che
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