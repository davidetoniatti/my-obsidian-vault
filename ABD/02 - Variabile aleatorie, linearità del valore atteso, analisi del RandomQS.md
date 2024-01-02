# Variabili aleatorie, valore atteso e linearità del valore atteso
## concetti fondamentali di probabilità
A random variable $X$ on a sample space $\Omega$ is a real valued probability function on $\Omega$;
that is, $X: \Omega \rightarrow R$.
A discrete random variable is a random variable that takes only a finite or countably infinite number of values.
$c=0$

In rolling a dice (the physic event), the number that comes up is a random variable (of course the sample space depends only on the number that comes up, not for example the rolling time). The identity function is perfect in this case, the random variable assume value $i$ when the dice show face $i$-

Consider a gambling game in which a player flips two coins, if he gets head in both coins we win 3\$, else he loses 1\$.
So the random variable $G$ map the payoff of the game -> $G: \{T,C\}^2 \rightarrow \{3,-1\}$
So what's the prob that playing the game we win 3 dollars? we write
$Pr[G(x,y) = 3]$ and it's the probability that we flip the a coin two times and we get head in both launches, so $Pr[G(x,y) = 3] = \frac{1}{2} \cdot \frac{1}{2} = \frac{1}{4}$ .

Two random variables $X$ and $Y$ are independent iff 
$$ Pr[(X = x) \cap(Y=y) ] = Pr(X=x) \cdot (Y=y) $$
Smiliar for $k$ random variables $X_i,\dots,X_k$.

### Expectation (valor medio)
The **expectation** of a discrete random variable $X$, denoted by $E[x]$, is given by
$$ E[X] = \sum_{i} i \cdot Pr(X=i) $$
Where the summation is over all values in the range of $X$.
The expectation is finite if $\sum_{i} |i| \cdot Pr(X=i)$ converges; otherwise, the expectation is unbounded.
The expectation (or mean or average) is a weighted sum over all possible values of the random variable.

The median of a random variable $X$ is a value $m$ such
$$ Pr(X<m) \leq 1/2 \text{} $$

### Linearity of Expectation
For any two random variables $X$ and $Y$ 
$$ E[X+Y] = E[X]+E[Y] $$

## Randomized quick sort


