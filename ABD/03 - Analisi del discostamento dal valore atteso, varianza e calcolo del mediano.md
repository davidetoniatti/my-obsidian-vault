Analisi del valor medio.. è sufficiente per essere sicuro delle performance dell'algoritmo?
Molto spesso nelle applicazioni, ci si accontenta di un buon risultato del valor medio. 
Valor medio non da nessuna indicazione su com'è la distribuzione della variabile aleatoria.
Esistono strumenti che mi danno indicazioni o una stima per capire se il valor medio è affidabile o meno.
Dev Standard: misura che indica quanto una v.a. si discosta dal valor medio.
Con le disugualianze di --- limitiamo la probabilità della varianza.
Per ogni variabile aleatoria non negativa vale 
$$ Pr(X \geq a) \leq \frac{E[X]}{a} $$
ad esempio per RQS ricordiamo che $E[X] = O(n\log{n})$, allora
$$ Pr(X \geq (n\log{n})\cdot \log{n}) \leq \frac{n\log{n}}{n\log^2{n}} = \frac{1}{\log{n}} $$
### Algoritmo per il calcolo del mediano
