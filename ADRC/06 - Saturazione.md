Si introduce la tecnica della **saturazione**, che permette la risoluzione di svariati problemi in un ambiente distribuito con una topologia ad albero.
# Tecnica della saturazione
Per applicare questa tecnica, si consideri una rete distribuita con una topologia ad albero e sia tale informazione sulla topologia nota a tutti i nodi della rete. In particolare, ogni nodo sa se è una foglia oppure un nodo interno. La tecnica della saturazione consiste nell'identificare una singola coppia di nodi adiacenti all'interno della rete. Tale coppia di nodi sarà in grado di far iniziare altre computazioni, a seconda del problema che si vuole risolvere. In altri termini, la tecnica della saturazione permette di *eleggere* un arco della rete: questa tecnica prende anche il nome di *link election*. Si osserva che non c'è nessuna garanzia su quale arco viene eletto: questa tecnica garantisce l'elezione di uno tra i possibili archi, ma non garantisce che sia proprio uno specifico arco ad essere eletto.

Tipicamente la tecnica della saturazione è usata per risolvere problemi distribuiti di varia natura. In particolare, la saturazione è inserita all'interno di un processo più grande, che può essere diviso nelle seguenti tre fasi:
- **Fase di Attivazione**, nella quale i nodi iniziatori attivano i restanti nodi della rete utilizzando un protocollo di WAKE-UP;
- **Fase di Saturazione**, nella quale a partire dalle foglie si identifica un'unica coppia di nodi adiacenti, anche detti *nodi saturati*;
- **Fase di Risoluzione**, nella quale la coppia di nodi saturati danno inizio ad una o più computazioni necessarie alla risoluzione dello specifico problema di interesse.

Si formalizzano le prime due fasi del processo appena descritto, definendo la tripla $P = \langle P_{init}, P_{final}, R \rangle$ dove
- $P_{init}$: tutti i nodi sono available;
- $P_{final}$: tutti i nodi tranne due sono ACTIVE, i due nodi restanti sono SATURATED e adiacenti.
- $R = \begin{cases} \text{Total Reliability (TR)} \\ \text{Bidirectional Link (BL)} \\ \text{Connectivity (CN)} \\ \text{Knowledge of the Topology (KT)} \\ \text{Ordered Messages (MO)}\end{cases}$
## Protocollo FULL-SATURATION
Il protocollo fa uso degli stati seguenti
- $S = \{ \text{available, active, processing, saturated} \}$
- $S_{init} = \{ \text{available,idle} \}$
- $S_{term} = \{ \text{active, saturated} \}$
Il comportamento dei nodi è descritto dalle seguenti regole
```python
```
Si osserva che nel protocollo il concetto di *parent* è utilizzato in due modi differenti
- Nello stato available, si intende il padre di un nodo foglia, ovvero l'unico nodo adiacente al nodo foglia;
- Nello stato active, il parenti di un nodo $x$ rappresenta l'unico nodo adiacente che ancora non ha inviato il messaggio $M$ al nodo $x$.
### Correttezza
Per mostrare la correttezza del protocollo, si deve il argomentare il fatto che esattamente due nodi passano dallo stato processing allo stato saturated, e che questi due nodi sono adiacenti.

Si osserva che
- Un nodo passa nello stato di processing solo dopo che ha mandato il messaggio $M$ al suo parent;
- Un nodo passa nello stato saturated solo dopo che riceve il messaggio $M$ dal suo parent.

Sia quindi $x$ un nodo arbitrario e si consideri il percorso fatto dai messaggi $M$ a partire da $x$
![](adrc_sat1.png)
Dato che non ci sono cicli, ad un certo punto del cammino si incontra un nodo $s_1$ che si trova nello stato saturated, in quanto ha ricevuto dal suo parent, il nodo $s_2$, il messaggio $M$. Ma allora quando $s_2$ riceve da $s_1$ il messaggio $M$ anche $s_2$ diventa saturated. Dunque il protocollo trova sempre almeno due nodi adiacenti saturati.

Si dimostra ora che i nodi saturati sono esattamente due. Siano per assurdo tre i nodi saturati, siano questi $x,y,z$. Data la topologia ad albero, due di questi nodi non sono adiacenti. Si consideri il cammino tra questi nodi non adiacenti, siano essi $x$ e $z$.
![](adrc_sat2.png)
Allora per definizione del protocollo, $x$ e $z$ diventano saturati quando ricevono il messaggio $M$. Se i due nodi non sono adiacenti, deve esistere un nodo $w$ nel cammino che invia due messaggi $M$: uno verso $z$ e uno verso $x$. Ma per definizione del protocollo, $w$ non può inviare $M$ a due archi.
In conclusione, si osserva che in ogni caso la scelta di quali nodi vengono saturati dal protocollo dipende dal ritardo dei messaggio. Questi implica che potenzialmente ogni coppia di nodi adiacenti può essere la coppia saturata.
### Complessità
Per quanto riguarda la message complexity, vale che:
- Nella fase di **attivazione**, vale la complessità del protocollo WAKE-UP e dato che in un albero $m = n-1$, allora vengono spediti $n+k-2$ messaggi, dove $k$ indica il numero di iniziatori;
- Nella fase di **saturazione** vengono inviati $n$ messaggi;
Allora in totale la message complexity del protocollo è pari a
$$
2n+k-2
$$
#TODO aggiungere protocollo plugin (?)
# Applicazioni della saturazione
La tecnica della saturazione può essere utilizzata per risolvere vari problemi computazionali in modo distribuito.

#TODO protocolli mancanti (usare plugin?)
## Calcolo del minimo
Si consideri il task distribuito in cui ogni nodo $x$ detiene un valore $value(x)$, e al termine del task ogni nodo $x$ deve sapere se $value(x)$ è il minimo oppure no rispetto ai valori detenuti dagli altri nodi. I valori non sono necessariamente distinti, dunque possono esistere più nodi che detengono il minimo valore.
Si osserva che in un albero radicato il problema è banale. In tale ambiente è sufficiente che ogni nodo invii il proprio valore alla radice, che calcola il minimo e poi lo distribuisce nella rete, in modo da far capire ad ogni nodo se il proprio valore è il minimo oppure no.

Questo non è possibile in albero generico, non radicato. In questo caso, si applica la tecnica della saturazione:
- I nodi foglia inviano il messaggio di saturazione $M$ allegando il proprio valore;
- Quando un nodo interno riceve tutti i messaggi di saturazione $M$ contenenti i valori di ogni nodo figlio, ne calcola il minimo e lo invia tramite messaggio $M$ al nodo genitore;
- I due leader eletti al termine del processo di saturazione conoscono il valore del minimo, dunque lo inviano in broadcast agli altri nodi.
Il protocollo è il seguente
```python
```

### Complessità
Per quanto riguarda la **message complexity**, vengono inviati $2n+k-2$ messaggi per la saturazione e $n-2$ messaggio per il broadcast finale, dunque in totale si ottiene
$$
3n+k-4
$$
dove $k$ indica il numero di iniziatori.
## Calcolo di funzioni distribuito
In generale, la saturazione può essere utilizzata per calcolare una classe di funzioni $F(\cdot)$ in modo distribuito su una topologia ad albero. In particolare, $F$ sia una operazione di semi-gruppo, ovvero che rispetti le seguenti proprietà
- **Associatività**: $F(F(a,b),c) = F(a,F(b,c))$
- **Commutatività**: $F(a,b) = F(b,a)$
Operazioni di questo tipo sono ad esempio il calcolo del massimo, del minimo, la somma, il prodotto e i connettori logici. 

## Calcolo della media
Anche il calcolo della media può essere effettuato sempre tramite saturazione. In questo caso i messaggi $M$ sono utilizzati per calcolare sia il numero di nodi, sia la somma del valore dei nodi. Una volta che queste informazioni vengono passate ad uno dei due nodi saturati, si è in grado di calcolare il valore medio tra tutti i valori dei nodi.
Il protocollo per risolvere il problema è il seguente.

## Calcolo delle eccentricità
Sia $d(x,y)$ la distanza tra $x$ e $y$ nella rete.
```ad-Definizione
title: Definizione (Eccentricità di un nodo)
In un grafo $G=(V,E)$ l'**eccentricità** di un nodo $x \in V$ è definita come la massima distanza tra $x$ e un qualsiasi altro nodo $y \in V$. Formalmente
$$
r(x) := \max_{y \in V}\{d(x,y)\}
$$
```
Il calcolo di $r(x)$ esprime quanto *centrale* è il nodo $x$ nella rete.
Si analizzano due protocollo che calcolano in modo distribuito l'eccentricità per ogni nodo della rete: il primo non usa la tecnica della saturazione, mentre il secondo la utilizza.
### Protocollo 1
La prima idea di un protocollo che calcola $r(x) \ \forall x$ è la seguente:
1. Il nodo $x$ invia una richiesta in broadcast a tutti i nodi della rete. In questa richiesta viene aggiornato volta per volta il numero di archi attraversati;
2. Le foglie inviano un messaggio indietro a $x$ per comunicare quanto si trovano distanti da $x$ (convergecast).
In questo modo ogni nodo $x$ è in grado di collezionare tutte le distanze, e dunque calcolare qual'è la distanza massima, ovvero la sua eccentricità.
In termini di _message complexity_ il calcolo di $r(x)$ richiede $2(n-1)$ messaggi. Allora il calcolo di tutte le eccentricità richiede $2(n^2-n)$ messaggi.
### Protocollo 2 (con <u>saturazione</u>)
Questo protocollo utilizza la saturazione per eleggere un particolare arco $(s_1,s_2)$, graficamente

dove $h_1,h_2$ sono rispettivamente le altezze dei sottoalberi $T_1,T_2$. Queste altezze possono essere calcolate direttamente durante il processo di saturazione.
Con tali informazioni, i nodi $s_1$ ed $s_2$ sono in grado di calcolare la loro eccentricità. Infatti,
- $r(s_{1}) = \max \{ h_{1}+1,h_{2}+2 \}$;
- $r(s_{2}) = \max \{ h_{2}+1,h_{1}+2 \}$;
Una volta calcolati $r(s_1)$ e $r(s_2)$ i nodi che si trovano a distanza $1$ da $s_1$ e $s_2$ sono in grado di calcolare a loro volta la loro eccentricità.
Graficamente vale

si osserva che il nodo $y$ per calcolare la propria eccentricità deve ricevere dal nodo $s_1$ certe informazioni. Il nodo $s_1$ non può semplicemente inviare a $u$ il valore $r(s_1)$, in quanto questo potrebbe introdurre degli errori nel calcolo di $r(u)$ nel caso in cui il nodo più distante da $s_1$ si trova proprio nell'albero $T_3$.
L'idea è quella di mantenere nel nodo $s_1$ l'altezza di ogni sottoalbero radicato nel nodo. Queste informazioni vengono poi inviate al nodo $u$, che le può utilizzare per calcolare la sua eccentricità per tutti i nodi.
Il protocollo è il seguente

In termini di message complexity, i protocollo invia i messaggi della saturazione, più i messaggi per effettuare il calcolo delle eccentricità. In totale si ottiene
$$
3n+k-4 \leqslant 4n-4
$$
dove $k$ indica il numero di iniziatori.
## Calcolo dei centri