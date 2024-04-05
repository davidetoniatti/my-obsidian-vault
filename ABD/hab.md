La funzione $h_{a,b}$ appena definita non può essere utilizzata per l'implementazione di un dizionario. Questo perché $p \geq N$, di conseguenza risulterebbe necessario un array $H$ di dimensione $p$, e ciò sarebbe di dimensioni spropositate. Si definisce quindi un'altra funzione hash partendo da $h_{a,b}$:

Sia $m \ll N$ la dimensione dell'hash table e sia $p>N$ un numero primo, dove si ricorda che $U=[N]$ e quindi $|U|=N$. Siano $a \in \{1,..,p-1\}$ e $b \in \{0,1,...,p-1\}$ due valori scelti uniformly at random dai rispettivi insiemi di appartenenza.
Si definisce la funzione hash:
$$
h_{a,b}(x) = ((a x + b) \textnormal{ mod }p) \textnormal{ mod m}
$$
e sia 
$$
\mathcal{H} = \{h_{a,b}:1\leq a\leq p-1, 0 \leq b \leq p-1 \}
$$

$\mathcal{H}$ è una famiglia di funzioni hash universale.
Si conta il numero di funzioni in $\mathcal{H}$ per le quali due elementi distinti $x_1,x_2 \in U$ collidono.
Si osserva inizialmente che, per ogni $x_1 \neq x_2$ vale 
$$
ax_1 + b \neq ax_2 + b \mod p
$$
Questo è vero perché $ax_1 + b = ax_2 + b \pmod p$ implica che $a(x_1 - x_2)= 0 \pmod p,$ ma sia $a$ che $(x_1 - x_2)$ sono diversi da $0$ modulo $p.$
Infatti, per ogni coppia di valori $(y_{1},y_{2})$ tali per cui $y_{1} \neq y_{2}$ e $0 \leq y_{1},y_{2} \leq p-1,$ esiste esattamente una sola coppia di valori $(a,b)$ per i quali $ax_1 + b = y_{1} \pmod p$ e $ax_2+b = y_{2} \pmod p$
Questa coppia di equazioni ha due incognite, e l'unica soluzione è data da 
$$
a = \frac{y_2-y_1}{x_2 - x_1} \mod p
$$
$$
b = y_1-ax_1 \mod p
$$
Essendovi quindi esattamente una funzione hash per ogni coppia $(a,b)$, segue che vi è esattamente una funzione hash in $\mathcal{\bar{H}}$ tale per cui 
$$
ax_1 + b = y_1 \pmod p \ \ \textnormal{ e } \ \ ax_2+b = y_2 \pmod p
$$
Quindi, per dare un bound alla probabilità che $h_{a,b}(x_1) = h_{a,b}(x_2)$ quando $h_{a,b}$ è scelta u.a.r. da $\mathcal{H},$ è sufficiente contare il numero di coppie $(y_{1},y_{2}), 0 \leq y_{1}, y_{2} \leq p-1$ tali per cui $y_{1} \neq y_{2}$ ma $y_{1} = y_{2} \pmod m.$
Per ogni scelta di $y_1$ vi sono al più $\lceil p/m \rceil -1$ possibili valori appropriati per $y_{2}$, perché ci sono al più $\lceil p/m \rceil$ interi equivalenti ad $y_1$ modulo $m$, dai quali bisogna escludere $y_1$. Allora ci sono al più
$$
p\left(\lceil p/m \rceil -1 \right) \leq \frac{p(p-1)}{m}
$$
coppie. Ogni coppia corrisponde ad una tra le possibili $p(p-1)$ funzioni hash, quindi
$$
\mathbf{Pr}\left[h_{a,b}(x_1)=h_{a,b}(x_2)\right] \leq \frac{p(p-1)/m}{p(p-1)} = \frac{1}{m}
$$
dimostrando che $\mathcal{H}$ è universale.