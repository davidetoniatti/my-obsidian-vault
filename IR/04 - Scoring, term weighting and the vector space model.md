# Problemi della Boolean Search
In un **Boolean search model**, i documenti vengono recuperati per mezzo di una query di tipo booleana; rispetto a tale richiesta, un documento o la soddisfa o non la soddisfa. Questa natura binaria porta a varie conseguenze:
- La scrittura di una query booleana che recupera il sottoinsieme di documenti che soddisfa l'information need dell'utente può essere una operazione complessa, dunque richiede una certa esperienza da parte dell'utente: la maggioranza dell'utenza non è in grado di scrivere le giuste query booleane;
- Spesso le richieste recuperano troppi documenti (**feast**), o troppo pochi (**famine**). Nel primo caso risulta difficile da parte di un utente medio gestire un grande mole di documenti; viceversa, pochi documenti, in genere, non sono in grado di soddisfare l'information need dell'utente.
Dunque nel retrieval booleano è necessaria molta competenza per comporre una richiesta che produca un sottoinsieme di documenti gestibile in termini di dimensione e soddisfacente in termini di informazione.
# Ranking retrieval
Per risolvere le problematiche del retrieval booleano si introduce l'idea di **ranking** che consiste nel, data una richiesta effettuata dall'utente, assegnare un valore di ranking ad ogni documento. Tale ranking intuitivamente esprime la rilevanza del documento rispetto alla richiesta. Dunque è possibile ordinare i documenti rispetto al ranking a loro assegnato e quindi mostrare all'utente solamente i documenti che hanno un ranking più alto. In questo modo l'utente riceve un sottoinsieme di documenti che risulta:
- sufficientemente piccolo, facile da gestire;
- in grado di soddisfare l'information need.
Un sistema di questo tipo funziona bene se l'algoritmo che assegna il ranking ai documenti è tale che i documenti più rilevanti hanno un valore maggiore rispetto ai documenti meno rilevanti. In particolare, l'algoritmo deve assegnare ad ogni coppia $(\text{query, doc})$ un punteggio in $[0,1]$ che esprima quanto sono ben accoppiati la richiesta e il documento della coppia. Dunque, si ordinano i documenti secondo i punteggi assegnati.
## Come effettuare il ranking
Il passo fondamentale per costruire un sistema di retrieval basato su ranking è, data una coppia $(\text{query, doc})$, assegnare un punteggio a tale coppia. In particolare:
- se nessun termine della query appare nel documento, il punteggio della coppia deve essere, intuitivamente, zero;
- dato un termine presente nella query, il punteggio deve crescere al crescere della frequenza del termine nel documento;
- il punteggio deve crescere al crescere del numero di termini nella query che appaiono anche nel documenti.
Dunque si possono definire diverse metriche e funzioni matematiche che permettono di calcolare questi punteggi.
## Coefficiente di Jaccard
Siano $A$ e $B$ due insiemi. Il **coefficiente di Jaccard** è definito come il rapporto tra la cardinalità dell'intersezione e la cardinalità dell'unione dei due insiemi. In formula,
$$
\text{Jaccard}(A,B) = \frac{|A \cap B|}{| A \cup B |}
$$
Osservare che almeno uno dei due insiemi deve essere diverso dall'insieme vuoto. In particolare,
- $\text{Jaccard}(A,A) = 1$;
- $\text{Jaccard}(A,B) = 0 \text{ se } A \cap B = 0$;
Il coefficiente di Jaccard permette di assegnare un numero in $[0,1]$ a una coppia di insiemi. In particolare, il numero cresce al crescere degli elementi in comune tra i due insiemi. Inoltre, $A$ e $B$ non devono avere la stessa dimensione.
---
**Esempio.** Sia $q$ la query e $d$ il documento seguenti
$$
\begin{align}
q &= \text{"ides of March"} \\
d &= \text{"Caesar died in March"}
\end{align}
$$
allora il coefficiente di Jaccard di $q$ e $d$ è pari a
$$
\text{Jaccard}(q,d) = \frac{|\{ \text{March} \}|}{|\{ \text{ides, of, March, died, Caesar, in} \}|} = \frac{1}{6}
$$
---
Il problema fondamentale del coefficiente di Jaccard è che non considera la frequenza dei termini. In generale, i termini più rari sono quelli maggiormente informativi; viceversa, i termini frequenti sono meno informativi. Questo fatto non viene considerato con il coefficiente di Jaccard.
## Term frequency
Si considerino solo richieste formate da un solo termine e sia $d$ un documento. Se il termine non occorre in $d$, allora il punteggio della coppia dovrebbe essere zero. Viceversa, maggiore è il numero di occorrenze del termine in $d$, maggiore dovrebbe essere il punteggio della coppia.
### Rappresentazione di un documento
La collezione di documenti può essere rappresentata tramite:
- Una **matrice di incidenza binaria**, dove ogni documento è rappresentato da un vettore binario: l'entrata $i$ del vettore è pari ad $1$ se il termine associato ad $i$ è contenuto nel documento che il vettore rappresenta, $0$ altrimenti;
- Una **Count Matrix**, dove ogni documento è rappresentato da un *count vector*: l'entrata $i$ del vettore è pari a $m$ se il termine associato ad $i$ occorre $m$ volte nel documento che il vettore rappresenta.
### Modello bag of word
Si considerano inoltre i documenti secondo un modello detto **bag of words**, caratterizzato dal fatto che tiene conto solo della presenza dei termini in un documento ed eventualmente del numero di occorrenze. Dunque non si considera l'ordine delle parole nel documento.
### Term frequency
Sia la **term frequency** $\text{tf}_{t,d}$ del termine $t$ nel documento $d$ definita come il numero di occorrenze di $t$ in $d$. Si utilizza tale quantità per calcolare il ranking da assegnare ad una coppia $(\text{query,doc})$.
1. Usare la **term frequency** senza ulteriori trasformazioni. Non è una soluzione ideale, in quanto la rilevanza di un documento non cresce in modo lineare con il crescere della frequenza di un termine. Infatti, un documento con $\text{tf} = 10$, è più rilevante di un documento con $\text{tf}=1$ , ma non $10$ volte più rilevante.
2. Rendere il punteggio di una coppia crescente in modo logaritmico rispetto alla term frequency. In particolare si definisce la **log frequency weight** $$w_{t, d} = \begin{cases}
     1 + \log{(\text{tf}_{t, d})}, &\text{tf}_{t, d} > 0 \\
     0, &\text{ altrimenti} \\
     \end{cases}$$
Quest'ultima definizione copre il caso in cui la richiesta è composta da un solo termine. Si definisce il punteggio di una coppia $(\text{q,d})$ con $|\text{q}|>1$ (**tf-matching-score**) come la somma su tutti i termini presenti sia nella richiesta che nel documento della log frequency weight, formalmente
$$
\text{tf-matching-score}(q,d) = \sum_{t \in q \cap d} w_{t,d}
$$
Si osserva che il punteggio è pari a zero se nessuno dei termini nella richiesta è presente nel documento.
## Inverse document frequency (idf)
Con il ranking appena introdotto si utilizza solo la frequenza del termine nel documento per calcolare il punteggio. In particolare quindi, l'importanza di un termine non dipende dalla sua rarità. Questo non è ideale, in quanto tipicamente i termini rari contengono più informazione. Un documento che condivide con la richiesta un termine raro è dunque più rilevante rispetto ad un altro documento che non lo contiene. L'obiettivo è quello di assegnare un peso maggiore ai termini rari e dei pesi più bassi (positivi) ai termini più comuni.
Il concetto di rarità di un termine viene formalizzato tramite la frequenza del termine rispetto all'intera collezione di documenti.
Si definisce con **document frequency** $\text{df}_{t}$ di un termine $t$ il numero di documenti della collezione che contengono $t$. Dunque è una misura inversa della quantità informativa di $t$ (maggiore è la document frequency, minore è la quantità d'informazione che $t$ contiene).
Sia $N$ il numero di documenti della collezione. Si definisce il peso associato al termine $t$ rispetto alla sua document frequency (**idf weight** di $t$) come segue
$$
\text{idf}_{t} = \log_{10} \frac{N}{\text{df}_{t}}
$$
dove $\text{idf}$ sta per **inverse document frequency** ed è una misura del contenuto informativo del termine. Analogamente a prima, si applica il logaritmo per smorzare il peso dell'idf di base. Si osserva che l'idf influenza il punteggio dei documenti per una query con almeno due termini, mentre ha un influenza molto limitata sul punteggio quando la query contiene un solo termine.
## tf-idf weighting
Si definisce ora uno degli schemi più utilizzati per attribuire un punteggio a coppie $(\text{query, doc})$. Tale schema prende il nome di **pesatura tf-idf**: per ogni coppia termine $t$ e documento $d$ si definisce la *pesatura tf-idf* $w_{t,d}$ come il prodotto della *term frequency* $\text{tf}_{t,d}$ per la *inverse document frequency* $\text{idf}_{t}$ (entrambe scalate).
$$
w_{t,d} = (1+\log\text{tf}_{t,d}) \log \frac{N}{\text{df}_{t}}
$$
In particolare, la pesatura tf-idf:
- aumenta al crescere del numero di occorrenze all'interno di un documento (term frequency);
- aumenta al crescere della rarità del termine nella collezione (inverse document frequency);
# Vector Space Model
Data la pesatura tf-idf per ogni termine e per ogni documento, si ottiene una rappresentazione tramite *weight matrix* della collezione dei documenti. In particolare, ogni documento $d$ è rappresentato da un vettore a componenti reali, dove l'$i$-esimo valore è il peso tf-idf della coppia documento $d$ e termine $t_{i}$ con $i \in |V|$, dove $V$ è l'insieme dei termini. Dunque ogni documento è un vettore in $\mathbb{R}^{|V|}$, ossia un vettore che vive in uno spazio vettoriale $|V|$-dimensionale a valori reali; i termini sono gli assi dello spazio e i documenti sono punti, o vettori, in tale spazio. Anche le query vengono rappresentate come vettori, o punti, nello stesso spazio $|V|$ dimensionale. Questo modello di rappresentazione prende il nome di **Vector Space Model**.
Con questo modello, è possibile effettuare il ranking dei documenti considerando la *somiglianza* tra i vettori che rappresentano i documenti e il vettore che rappresenta la query. A tale scopo si osserva che
$$
\text{similarity} = \text{proximity} \approx \text{negative distance}
$$
ovvero usando una qualche **norma vettoriale**, come ad esempio la **distanza euclidea**, per misurare la distanza tra i vettori, si misura una approssimazione della loro prossimità, cioè una approssimazione della loro similarità. In questo modo, si può effettuare un ranking dei documenti rispetto ad una query, considerando i documenti in ordine inverso rispetto alla distanza dei vettori associati ai documenti dal vettore associato alla query.
Si osserva che la distanza Euclidea non è utile a tale scopo, in quanto tiene in considerazione la lunghezza dei vettori: due vettori possono risultare molto distanti se uno è molto più lungo dell'altro. In particolare, due vettori possono risultare molto distanti tra loro rispetto alla distanza euclidea ma avere una distribuzione dei termini molto simile.
![](9F3B416A7892704709A9C193EA612A5F.png)
## Cosine similarity
Invece di considerare la distanza tra i vettori, si considera l'**angolo sotteso** $\theta$ tra i vettori come misura di similarità: più simili sono due vettori, più piccolo è l'angolo $\theta$. Per lavorare con una misura che cresce al crescere della similarità, si considera il coseno dell'angolo $\theta$: in $[0,\pi]$, al crescere di $\theta$ il $\cos(\theta)$ decresce, ovvero più le due rappresentazioni vettoriali dei documenti sono distanti e più il coseno dell'angolo sotteso tra questi due diminuisce. Inoltre il valore massimo di similarità è assunto quando $\theta = 0$, ovvero quando i due vettori hanno esattamente lo stesso angolo.
In breve, la trasformazione $\theta \to \cos(\theta)$ definisce una misura di similarità il cui valore cresce all'aumentare della similarità tra i documenti coinvolti nella misurazione.
Dunque, le due nozioni seguenti sono equivalenti.
- Ordinare i documenti in accordo all'angolo tra la query e il documento (nel vector space model) in ordine decrescente;
- Ordinare i documenti in accordo al coseno dell'angolo tra la query e il documento (nel vector space model) in ordine crescente.
### Normalizzazione
Al fine di calcolare il coseno di ogni coppia di vettori una procedura utile è la **normalizzazione**, che permette di mappare lo spazio vettoriale nel cerchio centrato nell'origine di raggio $1$. In particolare, per normalizzare il vettore $x$ si divide per la sua norma $L_{2}$. Sia $x'$ il vettore normalizzato, allora
$$
x' := \frac{x}{\Vert x \Vert_{2}} = \frac{x}{\sqrt{ \sum_{i} x_{i}^2 }}
$$
così facendo la lunghezza del vettore $x'$ risulta pari ad $1$
$$
\Vert x' \Vert_2 = \Bigg\Vert \frac{x}{\Vert x \Vert_2} \Bigg\Vert_2 = \Bigg\Vert \frac{1}{\Vert x \Vert_2} \cdot x \Bigg\Vert_2 = \frac{1}{\Vert x \Vert_2} \cdot \Vert x \Vert_2 = 1
$$
### Basic computation
Per calcolare il coseno sotteso tra due angoli si calcola il prodotto scalare dei due vettori. Sia $q_{i}$ il peso $\text{tf-idf}$ del termine $i$ nella query $q$, e sia $d_{i}$ il peso $\text{tf-idf}$ del termine $i$ nel documento $d$. La **cosine similarity** di $q$ e $d$ è pari a
$$
\cos(q, d) = \text{sim}(q, d) = \frac{q}{\left\Vert q \right\Vert_2} \cdot \frac{d}{\left\Vert d \right\Vert_2} = \frac{q \cdot d}{\left\Vert q \right\Vert_2 \left\Vert d \right\Vert_2} = \frac{\sum\limits_{i = 1}^{|V|} q_i d_i }{\sqrt{\sum_i q_i^2} \sqrt{\sum_i d_i^2}}
$$
Se i vettori $q$ e $d$ sono normalizzati, allora la cosine similarity è pari al semplice **prodotto scalare** dei due vettori.
$$
\cos(q, d) = \text{sim}(q, d) = q \cdot d = \sum\limits_i q_i  d_i
$$
### Actual computation
Esistono almeno due approcci per calcolare il cosine score:
- **TAAT** (Term-At-A-Time). Consiste nello scorrere i termini della query uno alla volta. Per ciascun termine si scorre la posting list del termine e si aggiorna lo score di ogni documento. Segue uno pseudocodice che implementa questo calcolo.![600](9AEBB8227C38232D2B07FC8B3E0AAC09.png)
- **DAAT** (Document-At-A-Time).
Si osserva che memorizzare direttamente il peso tf-idf per ogni coppia termine $t$, documento $d$ può risultare dispendioso, in quanto sono numeri a virgola mobile. In pratica, è sufficiente memorizzare la term frequency per ogni coppia $t,d$ (valore intero) e memorizzare la inverse document frequency di $t$ (valore intero) nella testa della posting list associata a $t$. 
Infine, si può utilizzare una coda con priorità per gestire i $K$ documenti più simili alla query, risparmiando l'ordinamento di tutti i documenti.
## Varianti della pesatura tf
In figura si presentano diverse funzioni che variano la pesatura della term frequency sul punteggio finale.
![center|600](8F619F309DEDAA06DC91599929DC99CA.png)
- $\text{TF}_{\text{total}},\text{TF}_{\text{sum}}, \text{TF}_{\text{max}}$ corrispondono ad assumere *indipendenza* tra le occorrenze: la term frequency aumenta della stessa quantità per ogni occorrenza successiva, indipendentemente dal numero di occorrenze già osservate;
- A $\text{TF}_{\text{total}}$ non è applicata nessuna normalizzazione rispetto alla lunghezza del documento: documenti più lunghi avranno un punteggio più alto; non sembra ragionevole, se due documenti di lunghezza diversa hanno la stessa frazione delle occorrenze di un certo termine $t$, allora i due documenti dovrebbero avere lo stesso punteggio rispetto a $t$;
- In $\text{TF}_{\text{sum}}$ si normalizza rispetto alla lunghezza del documento: i documenti con lunghezza diversa ma stessa frazione delle occorrenze di un termine $t$ ricevono lo stesso punteggio (rispetto a $t).$ In certi casi però (quali boh) è preferibile dare ai documenti più lunghi un punteggio leggermente più alto;
- $\text{TF}_{\text{max}}$ consiste in un approccio intermedio: per una stessa frazione di occorrenze, i documenti più lunghi hanno punteggio più alto, ma non troppo più alto come accade per $\text{TF}_{\text{total}}$.
- $\text{TF}_{\text{frac}}$ introduce un guadagno marginale decrescente rispetto al numero di occorrenze: l'incremento derivante dalla $n$-esima occorrenza del termine è minore per valori grandi di $n$. Lo stesso vale per $\text{TF}_{\text{log}}$. ![600](DABFEFCBDC73EDE3C6EDE24BFFE8777B.png)
## Varianti della pesatura idf
![center|600](251C15B20D74D5B2C21222EB32352D9E.png)