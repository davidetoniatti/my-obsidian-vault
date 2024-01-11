# 1 Concetti di base
## Esempi introduttivi

La teoria dei giochi ha come obiettivo quello di modellare situazioni nelle quali molteplici partecipanti interagiscono tra loro, o tali per cui le loro azioni condizionano gli esiti di ciascuno degli altri.
Un gioco, informalmente, consiste in:
- Un insieme di **giocatori**.
- Un insieme di regole di incontro: **chi** deve agire e **quando**, e **quali** sono le possibili azioni **(strategie)**.
- Una specifica dei **payoff** (guadagni) per ciascuna combinazione di strategie.

La teoria dei giochi cerca di predire l'esito di un gioco (ossia una **soluzione**) prendendo in considerazione il comportamento individuale dei giocatori, ossia a trovare un **equilibrio**. 

Si vedono ora degli esempi introduttivi di giochi.

**Esempio (Dilemma dei prigionieri):**
Due prigionieri, $\mathbf{P1}$ e $\mathbf{P2}$, sono in giudizio per aver commesso un crimine, e ciascuno di essi si ritrova a decidere se confessare la sua colpevolezza o rimanere in silenzio. Se entrambi rimangono in silenzio, le autorità non saranno in grado di dimostrare le accuse contro di essi, ed entrambi dovranno scontare una pena di $2$ anni per reati minori. Se solo uno dei due confessa, la sua pena verrà ridotta ad $1$ anno, e verrà usato come testimone per incriminare l'altro, il quale riceverà una pena di $5$ anni. Infine, se entrambi confessano, entrambi riceveranno uno sconto di pena per aver cooperato con le autorità, e dovendo scontare $4$ anni in carcere invece che $5$. I costi sostenuti dai giocatori, dipendenti dalla strategia adottata da entrambi, possono essere schematizzati nella seguente *matrice dei costi*:


 ![[01-agt_img01.png]] 
Si analizza ora la variante **one-shot** del gioco, ossia dove ciascun giocatore, simultaneamente, sceglie un'azione dal suo insieme di possibili strategie. Inoltre, si fa presente come i partecipanti operino in maniera **egoistica**, adottando la strategia che possa minimizzare (o eventualmente massimizzare) i loro costi (eventualmente profitti) indipendentemente dalle scelte altrui.
L'unica soluzione stabile nel gioco è quella per cui entrambi i prigionieri confessino. 
Infatti, per $\mathbf{P1}$ si ha che:
- Nello scenario in cui $\mathbf{P2}$ sceglie di non confessare, allora per $\mathbf{P1}$ il costo viene minimizzato confessando.
- Nello scenario in cui $\mathbf{P2}$ sceglie di confessare, allora per $\mathbf{P1}$ il costo viene minimizzato confessando.
Quindi per $\mathbf{P1}$ risulta conveniente confessare *indipendentemente* dalla scelta fatta da $\mathbf{P2}$: tale strategia risulta essere **dominante**.
Analogamente, si ha per $\mathbf{P2}$ che:
- Nello scenario in cui $\mathbf{P1}$ sceglie di non confessare, allora per $\mathbf{P2}$ il costo viene minimizzato confessando.
- Nello scenario in cui $\mathbf{P1}$ sceglie di confessare, allora per $\mathbf{P2}$ il costo viene minimizzato confessando.
Quindi per $\mathbf{P2}$ risulta conveniente confessare *indipendentemente* dalla scelta fatta da $\mathbf{P1}$: tale strategia risulta anch'essa essere **dominante**.
Risulta quindi che, per entrambi, la strategia migliore sia quella di confessare indipendentemente dalla scelta dell'altro. Diventa quindi una **strategia dominante** per entrambi. In particolar modo, la coppia di strategie **(Confess,Confess)** diventa la **strategia di equilibrio dominante**. Si osserva esplicitamente che, nel caso in cui i due prigionieri potessero comunicare, sarebbe beneficiario per entrambi rimanere in silenzio, ma tale combinazione di strategia non è un equilibrio, essendo che entrambi risultano essere incentivati a cambiarla.

**Esempio (ISP routing game):**
Si hanno due Internet Service Providers $\mathbf{ISP1}$ e $\mathbf{ISP2}$ i quali si devono inviare del traffico, ciascuno di essi avente la sua propria rete. Le due reti possono scambiarsi traffico mediante due punti di transito $C$ ed $S$, che prendono il nome di peering points.
   ![[01-agt_img02.png]]
Per ciascun ISP si ha un nodo di origine ed un nodo di destinazione del traffico, rispettivamente $s_i$ e $t_i$. L'utilizzo di ciascun link ha costo $1$, eccetto per quelli che vanno da $S$ a $t_1$ e da $S$ a $t_2$, il quale verrà pagato dall'ISP proprietario della rete a cui appartiene tale link.
In particolar modo, $\mathbf{ISP1}$ vuole inviare traffico da $s_1$ a $t_1$ mentre $\mathbf{ISP2}$ vuole inviare traffico da $s_2$ a $t_2$.
$\mathbf{ISP1}$ può adottare una delle seguenti strategie:
- passare per $S$, pagando quindi $2$
- passare per $C$ pagando $1$ e facendo pagare $3$ ad $\mathbf{ISP2}$, essendovi $3$ link nel cammino da $C$ a $t_1$ appartenenti alla rete di proprietà di quest'ultimo Internet Service Providers.
Simmetricamente, $\mathbf{ISP2}$ può adottare una tra le seguenti strategie:
- passare per $S$, pagando quindi $2$
- passare per $C$ pagando $1$ e facendo pagare $3$ ad $\mathbf{ISP1}$, essendovi $3$ link nel cammino da $C$ a $t_2$ appartenenti alla rete di proprietà di quest'ultimo Internet Service Providers.
I payoff per l'adozione di ciascuna coppia di strategie sono rappresentabili mediante la matrice vista per l'esempio precedente.
Anche in questo esempio, risulta evidente come la strategia di equilibrio dominante sia quella per cui ciascuno dei due ISP sceglie il percorso, dalla propria sorgente alla propria destinazione, passante per la rete dell'altro ISP.

## Giochi, strategie, costi e payoffs

### Formalizzazione di un gioco

Formalmente, un gioco consiste in un insieme di $n$ giocatori $\{1,2,...,n\}$. Ogni giocatore $i$ dispone del proprio *insieme di possibili strategie*, sia questo $S_i$. Per giocare, ogni giocatore $i$ seleziona una strategia $s_i \in S_i$. Si indica con $s=(s_1,\ldots,s_n)$ il *vettore di strategie* (o *profilo di strategia*) selezionate dai giocatori e con $S=\times_iS_i$  l'insieme di tutti i possibili modi in cui i giocatori possono selezionare le strategie, ossia l'insieme di tutti i possibili vettori di strategia.
```ad-Osservazione
title: Osservazione 1.1
Dati $n$ giocatori e date $m$ possibili strategie per ciascun giocatore, ossia $|S_i|=m$ per ogni $i \in \{1,...,n\}$, allora
$$
|S| = m^n
$$
```

Il vettore di strategie $s \in S$ selezionato dai giocatori determina l'esito per ciascuno di essi. In generale, gli esiti saranno differenti per ogni giocatore. Per specificare il gioco risulta necessario definire, per ciascun giocatore, un *ordine di preferenza* su questi esiti, dando una relazione completa, transitiva e riflessiva sull'insieme di tutti i vettori di strategia $S$. 
Dati due elementi di $S$, tale relazione per il giocatore $i$, dice quali dei due esiti per le due strategie $i$ *preferisce debolmente*: si dice che $i$ preferisce debolmente $S_1$ ad $S_2$ se $i$ preferisce $S_1$ ad $S_2$ o se considera equivalentemente buoni i loro esiti.
La maniera più semplice per specificare una preferenza è data assegnando, per ciascun giocatore, un valore per ogni esito. In alcuni giochi risulta naturale pensare ai valori come l'utilità per i giocatori e in altri casi ai costi impiegati dai giocatori. Si denotano queste funzioni per ogni giocatore $i$ rispettivamente come
$$
u_i:S \longrightarrow \mathbb{R} \ \ \ \textnormal{ e } \ \ \ c_i:S\longrightarrow \mathbb{R}
$$
Chiaramente, costi e utilità possono essere usati intercambiabilmente, essendo che $u_i(s)=-c_i(s)$.
Si osserva esplicitamente che se si fosse definita, per ogni giocatore $i$, $u_i$ (equivalentemente $c_i$) come funzione di $s_i$, ossia la strategia scelta dal giocatore $i$, piuttosto che come funzione dell'intero vettore di strategie $s$ scelto da tutti gli $n$ giocatori, allora si sarebbero ottenuti $n$ problemi di ottimizzazione indipendenti. Invece, in un gioco, il payoff di ogni giocatore dipende non solo dalla sua strategia, ma anche dalle strategie scelte dagli altri giocatori.   

## Equilibrio di strategia dominante
I due esempi visti in precedenza hanno una particolare proprietà: in ciascuno di questi giochi, ogni giocatore ha un'unica strategia migliore, indipendentemente dalle strategie adottate dagli altri giocatori. Si dice che un gioco ha una *strategia dominante* se possiede questa proprietà. 
Più formalmente, per un vettore di strategia $s \in S$  sia $s_i$ la strategia giocata dal giocatore $i$ e sia $s_{-i}$ il vettore di dimensione $n-1$ delle strategie giocate da tutti i restanti giocatori. Si ha quindi che $u_i(s)=u_i(s_i,s_{-i})$. Usando questa notazione, si definisce il concetto di **equilibrio con strategia dominante**:

```ad-Definizione
title: Definizione 1.1 (Equilibrio di strategia dominante)
Un equilibrio con strategia dominante è un vettore di strategia $s^{*}=(s_1^*,s_2^*,\ldots,s_n^*)$ tale per cui $s_i^*$ è una strategia dominante per ogni $i$, ossia, per ogni possibile profilo di strategia alternativo $s=(s_1,s_2,\ldots,s_i,\ldots,s_n)$ si ha che:
- Se $p_i$ è un'utilità, allora $p_i(s_i^*,s_{-i}) \geq p_i(s_i,s_{-i})$
- Se $p_i$ è un costo, allora $p_i(s_i^*,s_{-i}) \leq p_i(s_i,s_{-i})$
```

```ad-Osservazione
title: Osservazione 1.2
- Per ogni giocatore, una strategia dominante è la miglior risposta per qualsiasi strategia adottata dai restanti giocatori.
- Se un gioco ha un equilibrio con strategia dominante, allora i giocatori convergeranno immediatamente ad esso.
- Non tutti i giochi hanno una equilibrio con strategia dominante
- Un equilibrio con strategia dominante può non dare ai giocatori un payoff ottimale, ossia, può esistere un profilo di strategia che ottimizzi i profitti dei giocatori.
```

Si fa presente come un gioco, in generale, non può avere più di una strategia dominante, a meno che non siano presenti strategie dominanti che forniscono ai giocatori lo stesso payoff. Tale affermazione può essere argomentata come segue:

Sia $p_i(s_i,s_{-i})$ il payoff del giocatore $i$ per la strategia $s_i$, date le strategie $s_{-i}$ fissate dagli altri giocatori. Si supponga che esistano due strategie dominanti per $i$, siano queste $s_{i1}$ ed $s_{i2}$.
Se $s_{i1}$ è una strategia dominante allora, per definizione, si ha

$$
p_i(s_{i1},s_{-i}) \geq p_i(s_i,s_{-i}) \ \ \forall s_i \neq s_{i1}
$$
Da ciò segue che
$$
p_i(s_{i1},s_{-i}) \geq p_i(s_{i2},s_{-i})
$$
Analogamente, se $s_{i2}$ è una strategia dominante, si ha

$$
p_i(s_{i2},s_{-i}) \geq p_i(s_{i1},s_{-i}) $$
E ciò è vero se e solo se
$$
p_i(s_{i1},s_{-i}) = p_i(s_{i2},s_{-i})
$$
## Equilibrio di Nash
Raramente i giochi possiedono equilibri con strategie dominanti. Si definisce ora un concetto di soluzione meno stringente e più applicabile.
```ad-Definizione
title: Definizione 1.2 (Equilibrio di Nash)
Un equilibrio di Nash è un vettore di strategia $s^{*}=(s_1^*,s_2^*,\ldots,s_n^*)$ tale per cui per ogni $i$, $s_i^*$ è la miglior risposta a $s_{-i}^*$, ossia, per ogni possibile strategia alternativa $s_i$ del giocatore $i$ si ha che:
- Se $p_i$ è un'utilità, allora $p_i(s_i^*,s_{-i}^*)\geq p_i(s_i,s_{-i}^*)$
- Se $p_i$ è un costo, allora $p_i(s_i^*,s_{-i}^*)\leq p_i(s_i,s_{-i}^*)$
```

In altre parole, nessun giocatore $i$ può migliorare il suo payoff cambiando la sua strategia da $s_i^*$ ad $s_i$, assumendo che gli altri giocatori non cambino le strategie scelte in $s^*$. Si osserva esplicitamente che nel momento in cui tale profilo di strategia venga assunto dai giocatori, è nell'interesse di tutti mantenerlo, ed è quindi definito *stabile*. Inoltre, è chiaro che un equilibrio con strategia dominante sia un equilibrio di Nash. Ancor di più, nel caso in cui il profilo di strategia sia strettamente dominante (ossia se la sua adozione migliora strettamente l'esito del gioco), allora questo è l'unico equilibrio di Nash. 

Gli equilibri di Nash possono non essere unici. Ad esempio, giochi di coordinazione possono avere più equilibri. Inoltre, possono non essere ottimali per i giocatori. Per i giochi con più equilibri di Nash, equilibri differenti possono avere payoffs differenti per i giocatori.


**Esempio (Battaglia dei sessi)**:
In questo esempio, un gioco di coordinazione, vi sono più esiti che possono essere considerati stabili. In generale, un semplice gioco di coordinazione è definito in maniera tale che due giocatori devono scegliere tra due possibili opzioni, volendo scegliere la stessa.

Si considerino due giocatori, un ragazzo ed una ragazza, i quali devono decidere su come passare il pomeriggio assieme. Considerano entrambi due possibilità: andare al cinema o andare allo stadio. Il ragazzo preferirebbe andare allo stadio, mentre la ragazza preferirebbe andare al cinema, ma entrambi vorrebbero passare il pomeriggio assieme piuttosto che separatamente. Le preferenze dei giocatori sono espressi mediante payoffs (utilità in questo caso) come segue, facendo presente che **B** è la strategia associata allo stadio mentre **S** al cinema.

![[01-agt_img03.png]]
Chiaramente, le due soluzioni dove i due giocatori scelgono strategie differenti sono non stabili: in ciascuno dei casi, uno dei giocatori può migliorare il suo payoff cambiando la strategia scelta. Le due opzioni rimanenti sono entrambi soluzioni stabili, infatti:
- (B,B) è un equilibrio di Nash. 
- (S,S) è un equilibrio di Nash.
Questo perché, fissata la strategia di un giocatore, il restante può massimizzare il suo profitto scegliendo la stessa strategia dell'altro.

La risposta ottima, o best response, di un giocatore è una strategia che produce l'esito più favorevole, per quel giocatore, in risposta ad una data combinazione di strategie degli altri giocatori.

**Esempio (Routing congestion game)**
Si supponga che due flussi di traffico abbiano origine dal nodo $O$, e che debbano venir instradati all'interno di una rete. Si supponga che $O$ sia connesso al resto della rete mediante due punti di connessione $A$ e $B$, dove $A$ è leggermente più vicino ad $O$ rispetto a $B.$ Entrambi i punti di connessioni vengono congestionati facilmente, quindi inviare entrambi i flussi tramite lo stesso punto di connessione causa un ritardo aggiuntivo. In particolar modo, si ha che, senza congestione
$$
c(O,A)=1 \ \ \ \textnormal{e} \ \ \ c(O,B)=2
$$
mentre, nel caso di congestione
$$
c(O,A)=5 \ \ \ \textnormal{e} \ \ \ c(O,B)=6
$$

![[01-agt_img04.png]]
Si modella questa situazione mediante un gioco dove i due flussi sono i giocatori. Ciascun giocatore ha due possibili strategie, passare per $A$ o passare per $B$, portando a quattro possibili profili di strategia, i quali costi in termine di ritardo, dipendenti dalla scelta dei percorsi, sono espressi nella matrice.

Seguono una serie di osserazioni.
- In un equilibrio di Nash, nessun agente può unilateralmente cambiare la sua strategia se fissate le strategie degli altri partecipanti al gioco.
- Ogni agente deve prendere in considerazione le strategie degli altri agenti.
- Un equilibrio con strategia dominante è un equilibrio di Nash, ma non è vero il contrario.
- Se il gioco è giocato ripetutamente, e i giocatori convergono ad una soluzione, allora questa è un equilibrio di Nash.

Per giochi di strategia pura, ossia dove ciascun giocatore effettua deterministicamente la scelta di una strategia, non si può dare un risultato generale in merito all'esistenza dell'equilibrio di Nash. In altre parole, può esistere nessuno, uno o molti equilibri di Nash, a seconda del gioco.
### Equilibrio di Nash in strategie miste

Nei giochi visti sino ad ora, vi erano esiti stabili nel senso che nessun giocatore non trarrebbe profitto dal deviare individualmente da tale soluzione.
Si vede ora un esempio di gioco dove non vi è alcuna soluzione stabile.

**Esempio (Matching pennies):**
Due giocatori, ciascuno di essi aventi una moneta, devono scegliere tra una di due strategie, testa ($H$) e croce $(T)$. Il primo giocatore vince se le due monete hanno la stessa faccia, mentre il secondo vince se le due monete hanno faccia diversa. Ciò è rappresentato nella seguente matrice dei payoff dove $1$ indica la vittoria e $-1$ la sconfitta.
![[01-ar_img05.png]]
Per ciascuna configurazione, uno dei giocatori preferisce cambiare la sua strategia, e quindi non esiste un equilibrio di Nash. Sembra quindi che ai giocatori convenga selezionare la propria strategia casualmente.

Quando un giocatore può selezionare casualmente la sua strategia, utilizzando una distribuzione di probabilità sul suo insieme di possibili strategie, vale il seguente risultato:
#### Teorema
Ogni gioco con un insieme finito di giocatori ed un insieme finito di strategie ammette un equilibrio di Nash di strategie miste, ossia il payoff atteso non può essere migliorato cambiando unilateralmente la distribuzione di probabilità selezionata.

Dove con **strategia mista** si fa riferimento alla selezione di una distribuzione di probabilità sull'insieme delle possibili strategie per ciascun giocatore.
Si assume che i giocatori selezionino le strategie in maniera indipendente, utilizzando la distribuzione di probabilità scelta. Le scelte aleatorie indipendenti dei giocatori portano ad una distribuzione di probabilità del vettore di strategia $s$.

Quando i giocatori selezionano le strategie casualmente, bisogna capire come questi valutino l'esito casuale. Per l'equilibrio di Nash in strategie miste, si assume che i giocatori siano neutrali al rischio, ossia, operano in maniera tale per massimizzare il payoff atteso.

Tornando all'esempio di Matching pennies, se ciascun giocatore sceglie
$$
\mathbf{Pr}(H)=\mathbf{Pr}(T)= \frac{1}{2}
$$
allora il payoff atteso di ciascun giocatore è $0$, e questo è un equilibrio di Nash, essendo che nessun giocatore può migliorare tale soluzione scegliendo una distribuzione di probabilità differente.
## Qualità di un equilibrio di Nash
In generale, l'analisi di un gioco porta allo studio delle seguenti questioni:
1. Stabilire se un gioco ha sempre un equilibrio di Nash.
2. Trovarne uno, dopo aver mostrato che esiste.
3. In un gioco ripetuto, stabilire se e in quanti passi il sistema alla fine convergerà ad un equilibrio di Nash.
4. Stabilire la qualità di un equilibrio di Nash.

Si sono visti diversi giochi che mostrano come l'esito del comportamento razionale di giocatori egoisti può essere lontano dalla soluzione che ottimizza il profitto per i partecipanti al gioco.
Ci si chiede quindi quanto sia inefficiente un equilibrio di Nash a confronto di una situazione ideale nella quale i giocatori si impegnino a collaborare tra loro, con l'obiettivo comune di scegliere l'esito migliore.
Ad esempio, ricordando il dilemma dei prigionieri, entrambi i giocatori pagano un costo di $4$ nell'unico equilibrio di Nash del gioco, quando entrambi potrebbero incorrere in un costo di $2$ coordinandosi. 
Bisogna quindi definire una misura di "inefficienza dell'equilibrio di un gioco", e per fare ciò risulta necessario definire una **funzione di scelta sociale** $\mathcal{C}:S \longrightarrow \mathbb{R}$, che mappa i profili di strategia in numeri reali. Tale funzione misura la qualità generale di un esito $s \in S$, ad esempio $\mathcal{C}(s)$ può essere la somma delle utilità (o costi) di tutti i giocatori, dove quest'ultima funzione prende il nome di funzione *utilitaria*.
L'utilizzo di questa *funzione obiettivo* permette di *quantificare* l'inefficienza di un equilibrio, ed in particolar modo, permette di definire se l'esito di un gioco sia *ottimale* o *approssimativamente* ottimale.
L'esito di un gioco si dice *ottimale* se ottimizza la funzione di scelta sociale utilizzata. Ad esempio, sempre nel dilemma dei prigionieri, l'esito coordinato (dove i prigionieri cooperano) è ottimale per la funzione utilitaria.

In sintesi, per valutare la qualità di un equilibrio sono necessarie misure per poter dire quanto questo sia buono rispetto alla strategia che garantisce il payoff ottimale.
### Prezzo dell'anarchia (Price of Anarchy)
Il prezzo dell'anarchia è la misura di inefficienza di equilibri più popolare, la quale risolve il problema della presenza di molteplici equilibri adottando un approccio worst-case. Tale misura è definita come il rapporto tra il peggior valore assumibile dalla funzione di scelta sociale per un equilibrio del gioco e il valore di un esito ottimale. Formalmente.
```ad-lemma
title: Definizione (Prezzo dell'Anarchia)
Dato un gioco $G$ ed una funzione di scelta sociale $\mathcal{C}$, sia $S$ l'insieme di tutti gli equilibri di Nash. Se il payoff rappresenta un costo (rispettivamente, un'utilità) per un giocatore, sia $\mathbf{OPT}$ l'esito di $G$ che minimizza (rispettivamente, massimizza) $\mathcal{C}$. Allora, il Prezzo dell'Anarchia di $G$ rispetto a $C$ è 
$$
\mathbf{PoA}_G(\mathcal{C}) = \textnormal{sup}_{s\in S} \frac{\mathcal{C}(s)}{\mathcal{C}(\mathbf{OPT})} \ \ \ \left( \textnormal{rispettivamente, } \textnormal{inf}_{s\in S} \frac{\mathcal{C(s)}}{\mathcal{C}(\mathbf{OPT})}\right)
$$
```
Si vogliono identificare giochi dove il prezzo dell'anarchia è vicino ad $1$. In questi, tutti gli equilibri sono buone approssimazioni di un esito ottimale. 
Si osserva esplicitamente che un gioco con molteplici equilibri ha un grande prezzo dell'anarchia anche se solo uno dei suoi equilibri è altamente inefficiente.
### Prezzo della stabilità (Price of Stability)
Il prezzo della stabilità è una misura di inefficienza designata a distinguere tra giochi dove *tutti* gli equilibri sono inefficienti e quelli per cui *alcuni* equilibri sono inefficienti. In particolar modo, il prezzo della stabilità di un gioco è il rapporto tra il miglior valore assumibile dalla funzione di scelta sociale per un equilibrio del gioco e il valore di un esito ottimale. Formalmente:
```ad-lemma
title: Definizione (Prezzo dell'Anarchia)
Dato un gioco $G$ ed una funzione di scelta sociale $\mathcal{C}$, sia $S$ l'insieme di tutti gli equilibri di Nash. Se il payoff rappresenta un costo (rispettivamente, un'utilità) per un giocatore, sia $\mathbf{OPT}$ l'esito di $G$ che minimizza (rispettivamente, massimizza) $\mathcal{C}$. Allora, il Prezzo dell'Anarchia di $G$ rispetto a $C$ è 
$$
\mathbf{PoS}_G(\mathcal{C}) = \textnormal{inf}_{s\in S} \frac{\mathcal{C}(s)}{\mathcal{C}(\mathbf{OPT})} \ \ \ \left( \textnormal{rispettivamente, } \textnormal{sup}_{s\in S} \frac{\mathcal{C(s)}}{\mathcal{C}(\mathbf{OPT})}\right)
$$
```
Ovviamente, per un gioco con un unico equilibrio di Nash, il suo prezzo dell'anarchia ed il suo prezzo della stabilità sono identici. Per un gioco con molteplici equilibri, il suo prezzo di stabilità è almeno vicino ad $1$ quanto il suo prezzo dell'anarchia, e può essere anche molto più vicino rispetto ad esso.
Un bound sul prezzo della stabilità, il quale garantisce solamente che un equilibrio sia approssimativamente ottimo, fornisce una garanzia molto più debole rispetto ad un bound sul prezzo dell'anarchia. Nonostante ciò, per alcune applicazioni un bound non banale risulta possibile solamente per il prezzo della stabilità. 
Inoltre, il prezzo della stabilità ha un'interpretazione naturale in molti giochi su reti: 
se si immagina che l'esito sia inizialmente progettato da un'autorità centrale per un successivo utilizzo da parte di giocatori egoisti, allora il miglior equilibrio è una soluzione ovvia da proporre. Infatti, in molte applicazioni di rete, gli agenti non sono completamente indipendenti. Piuttosto, interagiscono mediante un protocollo sottostante che propone essenzialmente una soluzione collettiva a tutti i partecipanti, i quali possono accettarla o rifiutarla. Il prezzo della stabilità permette di misurare il beneficio di tali protocolli.

Si evidenziano ora alcune osservazioni fondamentali sulle misure di inefficienza appena descritte:
- Il $\mathbf{PoA}$ e il $\mathbf{PoS}$ sono $\geq 1$ per problemi di minimizzazione e $\leq 1$ per problemi di massimizzazione.
- Il $\mathbf{PoA}$ e il $\mathbf{PoS}$ sono *piccoli* quando sono vicino ad $1$.
- Il $\mathbf{PoS}$ è almeno tanto vicino ad $1$ quanto il $\mathbf{PoA}$.
- In un gioco con un unico equilibrio, $\mathbf{PoA} = \mathbf{PoS}$.
- Il $\mathbf{PoA}$ è simile al concetto di *rapporto di approssimazione* di un'euristica.
- Una delimitazione sul $\mathbf{PoS}$ provvede una garanzia significativamente più debole rispetto ad un bound sul prezzo dell'anarchia.
- Il $\mathbf{PoS}$ quantifica la degradazione necessaria nella qualità della soluzione per il vincolo della stabilità. 

### Selfish routing
Tale modello definisce giochi su reti. Una rete è data da un grafo diretto $G=(V,E)$, dove $V$ è l'insieme dei vertici ed $E$ è l'insieme degli archi, e da un insieme $\{(s_1,t_1),\ldots,(s_k,t_k): \forall i=1,\ldots,k, s_i,t_i \in V\}$ di coppie sorgente-pozzo. Tali coppie prendono il nome di *prodotto*, e ciascun giocatore è identificato da una coppia. Si osserva che giocatori differenti possono originare da vertici sorgenti diversi e possono viaggiare verso vertici pozzo differenti. Si usa la notazione $\mathcal{P}_i$ per denotare il cammino $s_i-t_i$ di una rete. Si considerano solamente reti in cui $\mathcal{P}_i\neq \emptyset$ per ogni $i$, e si definisce $\mathcal{P}= \bigcup_{i=1}^k \mathcal{P}_i$. Il grafo $G$ può contenere archi paralleli, ossia archi distinti che collegano la stessa coppia di nodi, ed un vertice può partecipare in più coppie sorgente-pozzo, ossia, può essere il punto di partenza o di arrivo per diversi cammini.
I percorsi scelti dai giocatori sono descritti mediante un *flusso*, un vettore ad entrate non negative indicizzato dall'insieme $\mathcal{P}$ di cammini sorgente-pozzo. Per un flusso $f$ ed un cammino $P \in \mathcal{P}_i$, si definisce $f_P$ come la quantità di traffico del prodotto *i* che sceglie il cammino $P$ per viaggiare da $s_i$ a $t_i$. Il traffico è "inelastico", nel senso che vi è una quantità prescritta $r_i$ di traffico per ogni prodotto $i$. Un flusso $f$ è *ammissibile* per un vettore $r$ se questo instrada tutto il traffico, ossia, per ogni $i \in\{1,2,...,k\}$, 

$$\sum_{P\in \mathcal{P}_i}f_P=r_i$$
In particolar modo, non si impongono capacità esplicite sugli archi.
Ad ogni arco è associata una *funzione costo* $c_e:\mathbb{R}^+ \longrightarrow \mathbb{R}^+$. Si assume che le funzioni costo siano sempre non negative, continue e non decrescenti.
Sinteticamente, una rete di grandi dimensioni può essere modellata utilizzando la teoria dei giochi, dove i giocatori sono i suoi utenti mentre le strategie consistono nei cammini su i quali gli utenti possono instradare il proprio traffico.
Si definisce quindi un *nonatomic selfish routing game* mediante la tripla $(G,r,c)$.

Schematicamente, per il nonatomic selfish routing si ha che:
- La rete è utilizzata da un grande numero di utenti egoisti.
- Ogni utente controlla una piccola frazione del traffico.
- Ogni arco ha una funzione costo che misura il tempo di viaggio in funzione della quantità di traffico su di esso.
- Ogni utente punta a minimizzare il suo tempo di viaggio tra una coppia di nodi ad esso associata.
- La funzione di costo sociale che si vuole minimizzare è il tempo medio impiegato dagli utenti.

Si formalizza ora la nozione di equilibrio in giochi di nonatomic selfish routing.
Si definisce il costo di un cammino $P$ rispetto al flusso $f$ come la somma dei costi degli archi che lo costituiscono:

$$
c_P(f)= \sum_{e \in P}c_e(f_e)
$$
dove 
$$
f_e = \sum_{P \in \mathcal{P}:e\in P}f_P
$$
denota l'ammontare di traffico che usa il cammino contenente l'arco $e$.
Essendo che ci si aspetta che il traffico egoista abbia come obiettivo quello di minimizzare il suo costo, si ha la seguente definizione:
```ad-Definizione
title: Definizione 1.5 (Flusso di equilibrio non atomico)
Sia $f$ un flusso ammissibile per l'istanza nonatomica $(G,r,c)$. Il flusso $f$ è un flusso di equilibrio se, per ogni prodotto $i \in \{1,2,\ldots,k\}$ e ogni coppia $P,\hat{P} \in \mathcal{P}_i$ di cammini $s_i-t_i$ con $f_P >0$,
$$
c_P(f) \leq c_{\hat{P}}(f)
$$
```
In altre parole, tutti i cammini usati da un flusso di equilibrio $f$ hanno il possibile costo minimo, dati la loro sorgente, il loro pozzo e la congestione causata da $f$. In particolar modo, tutti i cammini di un dato prodotto usati da un flusso di equilibrio hanno costo uguale.

**Esempio (Gioco di Pigou):**
Si consideri la rete in figura.
![[01-agt_img06.png]]
Due archi disgiunti collegano un nodo sorgente $s$ ad un nodo destinazione $t$. Ogni arco è etichettato con una *funzione costo* $c(\cdot)$, la quale descrive il costo (ad esempio il tempo di viaggio) impiegato dagli utilizzatori di tale arco in funzione della quantità di traffico instradato in esso. L'arco superiore ha associato la funzione con costo costante $c(x)=1$, il quale quindi rappresenta un percorso relativamente lungo ma immune alla congestione. Il costo dell'arco inferiore, $c(x)=x$, cresce all'aumentare della sua congestione. In particolar modo, tale arco è meno costoso di quello superiore se e solo se viene utilizzato da meno di un'unità di traffico. Si vuole studiare il prezzo dell'anarchia di tale gioco.
Si supponga che sia necessario distribuire un'unità di traffico all'interno della rete, la quale rappresenta una popolazione molto grande di giocatori, e che ciascun giocatore scelga uno dei due percorsi da $s$ a $t$ indipendentemente dalle scelte degli altri giocatori. Assumendo che ogni giocatore abbia come obiettivo quello di minimizzare il suo costo, ossia quello associato all'arco da lui utilizzato, prendere la strada inferiore risulta essere una strategia dominante. Nell'unico equilibrio di questo gioco, tutti i giocatori seguono questa strategia, e ciascuno di essi incorre quindi in un costo di un'unità. 
Per definire un esito ottimo, si assume che la funzione obiettivo sia quello di minimizzare il costo medio pagato dai giocatori. Nell'equilibrio appena descritto, il costo medio è $1$. 
E' semplice osservare come dividere il traffico equamente nei due archi è la strategia ottimale. Ciò è vero perché la soluzione ottimale è il valore minimo di $c(x)=x\cdot x +(1-x)\cdot x$, che si ottiene ponendo a $0$ la derivata prima di tale funzione. Quindi:
$$
c'(x) = 2x -1 \implies \mathbf{OPT}=\frac{1}{2} \implies c(\mathbf{OPT}) = \frac{1}{2}\cdot\frac{1}{2} + \left(1 - \frac{1}{2}\right) \cdot 1 = \frac{3}{4}
$$

In questo esito, metà dei giocatori incorre in costo $1$, mentre l'altra metà paga costo $1/2$ ciascuno. Essendo che il costo medio del traffico in questo **unico** esito ottimale è $3/4$, sia il prezzo dell'anarchia che il prezzo della stabilità in questo gioco sono uguali a
$$
\frac{1}{\left(\frac{3}{4} \right)} = \frac{4}{3}
$$

Si osserva come in questo esempio, instradare tutto il traffico nel secondo link definisce un flusso di equilibrio. Solo uno di essi trasporta flusso, e l'unica altra alternativa ha costo uguale. Dividere il traffico egualmente tra i due cammini definisce un flusso che non è un equilibrio di flusso: il primo trasporta una quantità strettamente positiva di traffico ed il suo costo è $1$, ma vi è un'alternativa strettamente più conveniente, ossia il secondo link con costo $1/2$.

```ad-Osservazione
title: Osservazione 1.4
La descrizione dei giochi di nonatomic selfish routing e del loro equilibrio non definisce esplicitamente l'insieme dei giocatori. Mentre tipi più generali di giochi sono definiti esplicitamente in termini di insiemi di giocatori, profili di strategia e funzioni di payoff dei giocatori, i giochi di selfish routing posseggono una struttura speciale. In particolar modo, il costo impiegato da un giocatore dipende esclusivamente dal suo cammino e dall'ammontare di flusso negli archi di tale cammino, nvece che sulle identità degli altri giocatori. Per via di questa struttura, è sufficiente e più semplice lavorare direttamente con i flussi nei giochi di routing egoistico nonatomico. 
```
### Paradosso di Braess
Si vuole vedere ora con un esempio come, nella progettazione di reti, bisogna prendere in considerazione il comportamento egoistico dei giocatori.
Si consideri la rete in figura. Sono presenti due cammini disgiunti da $s$ a $t$, ciascuno avente costo $1+x$, dove $x$ è la quantità di traffico che utilizza il cammino. Si assuma che ci sia un'unità di traffico da instradare nella rete. Nel flusso di equilibrio, il quale risulta essere anche l'unico equilibrio di Nash, il traffico è diviso equamente tra i due cammini, e tutto il traffico paga un costo pari a $3/2$. Si osserva anche che tale profilo di strategia risulti portare all'esito ottimale.
   ![[01-agt_img07.png]]
Si supponga che, per diminuire il costo pagato dal traffico, si costruisca un arco avente costo zero che collega i punti centrali dei due cammini esistenti. Il precedente equilibrio di flusso non persiste nella nuova rete: il costo del nuovo cammino $s \rightarrow v \rightarrow w \rightarrow t$ non è mai peggiore di quello dei due cammini precedenti, e risulta essere strettamente minore quando del traffico non lo utilizza. Come conseguenza di ciò, l'unico flusso di equilibrio instrada tutto il traffico nel nuovo cammino. Per via della congestione sugli archi $(s,v)$ e $(w,t)$, tutto il traffico paga due unità di costo. Il paradosso di Braess mostra come l'intuitiva idea di aggiungere un nuovo arco avente costo zero all'interno di una rete può aumentare il costo di tutto il traffico.

   ![[01-agt_img08.png]]
Il prezzo dell'anarchia nella seconda rete (e anche della stabilità, essendovi un unico equilibrio di Nash) è quindi di $4/3$, lo stesso visto nell'esempio di Pigou.
Questa non è una coincidenza, infatti
```ad-Teorema
title: Teorema 1.2
Il Prezzo dell'Anarchia del Selfish Routing Game con funzione di latenza lineare, ossia della forma $ax+b$, è al più $4/3$.
```

# 2 Network Formation Games
La teoria dei giochi fornisce una struttura per modellare il come individui indipendenti ed egoisti possano generare una rete affinché vengano ottimizzati i costi e la qualità delle operazioni che questi devono sostenere su di essa. Questi modelli facilitano a loro volta uno studio quantitativo del trade-off tra efficienza e stabilità nella formazione di reti.

In particolar modo, possono essere utilizzati per modellare:
- Formazioni di reti sociali.
- Come sottoreti si connettono in reti di computer.
- Formazione di reti che connettono i partecipanti tra loro per lo scambio di files.

In generale, i network formation games modellano maniere distinte nelle quali agenti egoisti possono creare e valutare una rete. Si studiano due modelli:
- Global Connection Game.
- Local Connection Game.
Entrambi i modelli hanno come obiettivo quello di risolvere due questioni concorrenti. Infatti, i giocatori vogliono:
- Minimizzare il costo impiegato nella costruzione della rete.
- Assicurarsi che la rete gli fornisca un'alta qualità di servizio.

Per tutti i giochi considerati, si usa l'equilibrio di Nash come concetto di soluzione. Le reti corrispondenti ad un equilibrio di Nash vengono definite stabili.
Per valutare la qualità generale della rete, si fa riferimento al *costo sociale*, ossia la somma dei costi di tutti i giocatori. Ci si riferisce alle reti che ottimizzano il costo sociale come reti *ottime* o *socialmente efficienti*.
In generale, i giocatori non sono interessati alla formazione di una rete ottima, tanto quanto a costruire una rete efficiente impiegando il minor costo individuale possibile. 
Si vuole quindi dare una delimitazione alla perdita di efficienza di una rete dovuto al comportamento egoistico dei giocatori, ossia al raggiungimento di un profilo di strategia stabile.
## Global connection game
##  Modello
Si ha un grafo diretto $G=(V,E)$, con pesi degli archi $c_e$ non negativi per tutti gli archi $e \in E$. Si hanno $k$ giocatori, e ciascun giocatore $i$ ha associato uno specifico nodo sorgente $s_i$ ed un nodo pozzo $t_i$, dove lo stesso nodo può essere una sorgente o un pozzo per molteplici giocatori. L'obiettivo del giocatore $i$ è quello di costruire una rete nella quale $t_i$ è raggiungibile da $s_i$, pagando il minor costo possibile. Una strategia per il giocatore $i$ è un cammino $P_i$ da $s_i$ a $t_i$ in $G$. Scegliendo $P_i$, il giocatore $i$ partecipa alla costruzione di tutti gli archi in $P_i$ nella rete finale. Dato un vettore di strategia $S$, si definisce la *rete costruita* come $N(S)=\bigcup_i P_i$. 
Bisogna ora allocare il costo di ciascun arco nella rete ai giocatori che lo utilizzano. Questo permetterà ai giocatori di valutare l'utilità di ciascuna strategia.
Per fare ciò, si considera il meccanismo che divide il costo di un arco $e$ equamente tra tutti i giocatori per i quali la loro strategia contiene $e$, ossia, sia $k_e(S)$ il numero di giocatori per i quali il loro cammino contiene l'arco $e$, allora tale arco assegna una frazione di costo pari a $c_e/k_e(S)$ a ciascun giocatore che lo utilizza. Quindi, il costo totale impiegato dal giocatore $i$ per il vettore strategia $S$ è dato da
$$
\textnormal{cost}_i(S) = \sum_{e \in P_i}c_e/k_e(S)
$$
In altri termini, il costo della rete costruita è condiviso da tutti i giocatori mediante la formula appena definita. Si osserva quindi esplicitamente che il costo totale assegnato da tutti i giocatori è esattamente il costo della rete costruita.
Si fa presente come, per comodità di notazione, se $S$ è intuibile dal contesto, si scriverà $k_e$ invece di $k_e(S)$.
Ci si riferisce a questo schema di divisione di costi come *fair* o *Shapley cost-sharing mechanism*. 
L'obiettivo sociale di questo gioco è quindi il costo della rete costruita.

A rigore di quanto detto in precedenza, si evidenziano i seguenti punti:
- Si usa l'equilibrio di Nash come concetto di soluzione.
- Un vettore di strategia $S$ è un equilibrio di Nash se nessun giocatore può migliorare il suo payoff cambiando strategia.
- Dato un vettore di strategia $S$, la rete $N(S)$ è stabile se $S$ è un equilibrio di Nash.
- Per valutare la qualità generale di una rete, si considera il *costo sociale*, ossia, la somma dei costi di tutti i giocatori:$$ \textnormal{cost}(S) = \sum_i \textnormal{cost}_i(S)$$
- Una rete è *ottimale* o *socialmente ottimale* se minimizza il costo sociale.

**Esempio:**
Sia $G$ il grafo mostrato in figura.
![[02-agt_img01.png]]
In questo esempio, la rete socialmente ottima è quella avente gli archi evidenziati in rosso. Il costo di tale rete, e quindi il costo dell'ottimo sociale, è pari alla somma dei pesi degli archi che costituiscono la rete, ossia $13$. Si osserva che, sia $N(S)$ la rete formata per l'adozione da parte dei giocatori del profilo di strategia $S$ esplicitato in figura, che 
$$
\textnormal{cost}(S) = \sum_{e \in N(S)}c_e
$$
Inoltre, si fa presente che, dato un grafo $G$, la *rete ottimale* è il sottografo meno costoso di $G$contenente un cammino da $s_i$ a $t_i$ per ogni $i$. Nello scenario in figura, si ha che $\textnormal{cost}_1=7$ e $\textnormal{cost}_2 =6$.
![[02-agt_img02.png]]
Si osserva che, per le strategie adottate dai player, la rete non risulta essere stabile. Infatti, il profilo di strategia per il quale si forma la rete ottimale non è un equilibrio di Nash, perché il primo giocatore può ridurre il suo costo scegliendo l'arco superiore. A rigore di quanto appena detto, si evidenzia ancora come l'obiettivo dei giocatori non è quello di cooperare per formare una rete ottimale, bensì quella di raggiungere il proprio nodo pozzo dal proprio nodo sorgente scegliendo il cammino che garantisce la minor spesa individuale. Avendo scelto il cammino superiore, il primo giocatore paga $\textnormal{cost}_1=6$, mentre il secondo giocatore si ritrova a sostenere individualmente il costo dei primi due archi del cammino precedente, dovendo spendere $\textnormal{cost}_2=11$. 
![[02-agt_img03.png]]
Anche la rete definita a partire dalla nuova scelta dei cammini non risulta essere stabile. Infatti, tale profilo di strategia non è un equilibrio di Nash. Questo perché il secondo giocatore ottimizza il suo costo individuale scegliendo l'arco inferiore. Tale strategie rende la rete stabile, portando alla definizione di una rete avente costo sociale pari a $16$.
![[02-agt_img04.png]]


Come detto in precedenza, si vuole studiare come il comportamento egoistico dei giocatori, definito dalla scelta di un cammino che risulti avere costo minimo dal nodo sorgente al nodo pozzo, vada a degradare il valore della soluzione ottima, ossia della rete di costo minimo.
Per fare ciò, si cerca di dare una delimitazione al prezzo dell'anarchia e al prezzo della stabilità del gioco. In particolar modo, ci si chiede se
- Esiste sempre una rete stabile.
- E' possibile dare una delimitazione al Prezzo dell'Anarchia.
- E' possibile dare una delimitazione al Prezzo della Stabilità.
- La versione ripetuta del gioco converge sempre ad una rete stabile.

Si definiscono quindi le misure di inefficienza per un grafo $G$ per il Global connection Game.
```ad-lemma
title: Definizione
Sia $G=(V,E)$ un grafo diretto. Sia $S_G^*$ l'ottimo sociale per $G$ nel Global Connection Game.
Si definisce il prezzo dell'anarchia del Global connection Game per il grafo $G$ come
$$
\textnormal{PoA del gioco in }G = \textnormal{max}_{S \textnormal{ t.c } S \textnormal{ è un equilibrio di Nash}} \frac{\textnormal{cost}(S)}{\textnormal{cost}(S_G^*)}
$$
Si definisce il prezzo della stabilità del Global connection Game per il grafo $G$ come
$$
\textnormal{PoS del gioco in }G = \textnormal{min}_{S \textnormal{ t.c } S \textnormal{ è un equilibrio di Nash}} \frac{\textnormal{cost}(S)}{\textnormal{cost}(S_G^*)}
$$
```
Tali misure vogliono essere delimitate nel caso peggiore, si definiscono quindi PoA e PoS per il Global connection game.
```ad-lemma
title: Definizione
Il prezzo dell'anarchia del Global Connection Game è definito come
$$
\begin{align}
\textnormal{PoA del gioco }&= \textnormal{max}_G \textnormal{ PoA del gioco in }G \\
&= \textnormal{max}_G \left(\textnormal{ max}_{S \textnormal{ t.c } S \textnormal{ è un equilibrio di Nash}} \frac{\textnormal{cost}(S)}{\textnormal{cost}(S_G^*)}\right)
\end{align}
$$
Il prezzo della stabilità del Global Connection Game è definito come
$$
\begin{align}
\textnormal{PoS del gioco }&= \textnormal{max}_G \textnormal{ PoS del gioco in }G \\
&= \textnormal{max}_G \left(  \textnormal{min}_{S \textnormal{ t.c } S \textnormal{ è un equilibrio di Nash}} \frac{\textnormal{cost}(S)}{\textnormal{cost}(S_G^*)} \right)
\end{align}
$$
```
### Upper bound sul prezzo dell'anarchia
**Esempio:**
Si consideri il grafo in figura.
![02-agt_img05|center|500](02-agt_img05.png)
Vi sono $k$ giocatori, ciascuno di essi con la stessa sorgente $s$ e la stessa destinazione $t$. I costi degli archi sono $k$ per l'arco superiore e $1+\varepsilon$ per l'arco inferiore, dove $\varepsilon > 0$ è arbitrariamente piccolo. Nell'ottimo sociale, tutti i giocatori scelgono l'arco inferiore.  Questo esito è anche un equilibrio di Nash. La rete costruita per questo profilo di strategia, quindi, ha costo $1+\varepsilon$ e risulta essere stabile. Inoltre è il miglior equilibrio per questa istanza, infatti il prezzo della stabilità del gioco per questo grafo è pari ad $1+\varepsilon$.
D'altra parte, si supponga che tutti i giocatori scelgano l'arco superiore. Ogni giocatore incorre quindi in costo $1$, e nessuno è incentivato a deviare verso l'arco inferiore, essendo che pagherebbe il costo maggiore di $1 + \varepsilon$. Quest'altro profilo di strategia è quindi un secondo equilibrio di Nash, ed ha costo $k$, e risulta essere il peggior equilibrio per l'istanza presentata. Il prezzo della stabilità del gioco per questo grafo è quindi esattamente pari a $k$, il numero di giocatori.

Questo esempio motiva lo studio del prezzo della stabilità in giochi di progettazione di reti che adottano lo Shapley cost-sharing mechanism. Infatti, si ricorda che il prezzo della stabilità ha un'interpretazione naturale in giochi di formazioni di reti, perché misura l'inefficienza della rete che un progettista proporrebbe a giocatori egoisti, ossia il miglior equilibrio.

Si da ora un upper bound sul prezzo dell'anarchia.
```ad-theorem
title: Teorema
Il prezzo dell'anarchia nel Global Connection Game con $k$ giocatori è al più $k$.
```

```ad-Dimostrazione
Sia $G=(V,E)$ un grafo diretto fissato. Sia $S$ un profilo di strategia tale per cui $S$ è un equilibrio di Nash, e sia $S^*$ un profilo di strategia che minimizza il costo sociale. 
Si vuole mostrare che il costo sociale del profilo di strategia $S$ è al più $k$ volte il costo sociale di $S^*$.
Per ogni giocatore $j$, si definisce $\pi_j$ il cammino minimo in $G$ da $s_j$ a $t_j$.
Sia ora $i$ un giocatore. $i$, per il profilo di strategia $S$, non può migliorare il suo payoff cambiando strategia, essendo $S$ un equilibrio di Nash. Allora vale
$$
\textnormal{cost}_i(S) \leq \textnormal{cost}_i(S_{-i},\pi_i)
$$
Ossia, il costo del player $i$ per l'equilibrio di Nash scelto è minore o uguale del cost del profilo di strategia in cui tutti i restanti giocatori mantengono la strategia in $S$, mentre $i$ gioca come strategia un cammino minimo dalla sua sorgente al suo pozzo.


Sia ora $d_G(s_i,t_i)$ la distanza in $G$ da $s_i$ a $t_i$, ossia la lunghezza del cammino minimo $\pi_i$. Si osserva che, scegliendo come strategia $\pi_i$, il giocatore $i$ paga al più la lunghezza di $\pi_i$. Infatti, nel caso in cui la strategia di $i$ sia l'unica a comprendere gli archi di $\pi_i$, allora $i$ paga individualmente il costo del cammino. Altrimenti, alcuni degli archi di $\pi_i$ sono compresi nella strategia di altri giocatori $j \neq i$, i quali costi andranno suddivisi. Di conseguenza, vale che
$$
\textnormal{cost}_i(S) \leq \textnormal{cost}_i(S_{-i},\pi_i) \leq d_G(s_i,t_i)
$$

Sia ora $N(S^*)$ la rete ottima, ossia la rete di costo minimo contenente per ogni $j$ un cammino da $s_j$ a $t_j$. Allora, per il giocatore $i$ scelto in precedenza, $N(S^*)$ conterrà almeno un cammino da $s_i$ a $t_i$, sia questo $\pi$. Il costo di $\pi$ è sicuramente minore o uguale del costo della soluzione ottima, essendo che $\pi \subseteq N(S^*)$, ed essendo un cammino da $s_i$ a $t_i$ tale cammino non può avere costo minore del cammino minimo in $G$ tra questi due ultimi nodi, perché se così non fosse, $\pi_i$ non sarebbe un cammino minimo e ciò è, per ipotesi, assurdo. Vale quindi che
$$
d_G(s_i,t_i) \leq \textnormal{costo di }\pi \leq \textnormal{cost}(S^*)
$$
Ciò dimostra che 
$$
\textnormal{cost}_i(S) \leq \textnormal{cost}(S^*)
$$

Essendo che $\textnormal{cost}(S)=\sum_{i}\textnormal{cost}_i(S)$, ed essendo il gioco definito per $k$ giocatori, si ha che 
$$
\textnormal{cost}(S) \leq k \textnormal{ cost}(S^*)
$$
```
```ad-Osservazione
La notazione $\pi \subseteq N(S)$ è corretta, potendo considerare una rete come un insieme di archi.
```
### 2.1.3 Prezzo della stabilità e metodo della funzione potenziale
Si vuole dare una delimitazione inferiore al prezzo della stabilità. Si consideri il seguente esempio
**Esempio:**
Si consideri il grafo in figura.
![02-agt_img06|center|500](02-agt_img06.png)
Si hanno $k$ giocatori, tutti aventi lo stesso nodo sorgente $t$, e sia $\varepsilon >0$ arbitrariamente piccolo. Per ogni $i\in\{1,\ldots,k\}$, l'arco $(s_i,t)$ ha costo $1/i$. Nell'esito ottimale, ogni giocatore $i$ sceglie il cammino $s_i \rightarrow v \rightarrow t$ ed il costo della rete formata è $1+\varepsilon$. Questo non è un equilibrio di Nash, perché il giocatore $k$ può diminuire la sua spesa da $(1+\varepsilon)/k$ ad $1/k$ scegliendo come strategia il cammino $s_i \rightarrow t$. Inoltre, questo cammino è proprio una strategia dominante per il $k-$esimo giocatore. Induttivamente, si può estendere lo stesso ragionamento per i giocatori $k-1,k-2,\ldots,1$, e ciò mostra che l'unico equilibrio di Nash è l'esito in cui ciascun giocatore sceglie il proprio cammino diretto verso il nodo pozzo. Il costo di questo equilibrio è esattamente il $k-$esimo numero armonico $\mathcal{H}_k = \sum_{i=1}^k (1/i)$, che è circa $\ln k$.  Il prezzo della stabilità può essere quindi arbitrariamente, a seconda di $\varepsilon$, vicino a $\mathcal{H}_k$  in questo tipo di giochi.

Si vogliono dimostrare i seguenti teoremi 
```ad-theorem
title: Teorema
Ogni istanza del global connection game ha un equilibrio di Nash puro, e la dinamica di best response converge sempre ad esso.
```
Una dinamica di best response è un processo di gioco iterativo nel quale, ad ogni passo, i giocatori selezionano una strategia che risulta essere una best response alle strategie scegli da tutti gli altri giocatori al passo precedente.
```ad-theorem
title: Teorema
Il prezzo della stabilità nel global connection game con $k$ giocatori è al più $\mathcal{H}_k$, il $k-$esimo numero armonico.
```
Per fare ciò, si usa il metodo della funzione potenziale.
Questa tecnica permette di dare una delimitazione sul prezzo della stabilità di un gioco utilizzando una funzione potenziale.
Si fa presente che le dimostrazioni di questi risultati si applicano ad una classe più ampia di giochi rispetto al global connection game, ossia i giochi potenziali. Si definisce quindi questa classe di giochi, e si dimostrano i corrispettivi risultati in un contesto più generale, i quali implicheranno la veridicità dei teoremi appena presentati.
```ad-lemma
title: Definizione (Funzione potenziale esatta)
Per ogni gioco finito, una funzione potenziale esatta $\Phi$ è una funzione che mappa ogni vettore di strategia $S$ in valori reali e che soddisfa la seguenti condizione:
$\forall S=(S_1,\ldots,S_k),S_i^{'} \neq S_i$, sia $S^{'}=(S_{-i},S_{i}^{'})$, allora
$$
\Phi(S)-\Phi(S^{'}) = \textnormal{cost}_i(S)-\textnormal{cost}_i(S^{'})
$$
```
In altre parole, se lo stato corrente del gioco è $S$, e il giocatore $i$ cambia strategia da $S_i$ a $S_i^{'}$, allora il risparmio in cui incorre $i$ corrisponde esattamente al decremento nel valore della funzione potenziale. Un gioco che possiede una funzione potenziale esatta prende il nome di *gioco potenziale*. Un gioco può avere al più una singola funzione potenziale esatta.

La presenza di una funzione potenziale esatta per un gioco comporta diverse implicazioni per l'esistenza e la convergenza ad un equilibrio.

```ad-Teorema
title: Teorema 2.4
Ogni gioco potenziale ha almeno un equilibrio di Nash puro, ossia il vettore di strategia $S$ che minimizza $\Phi(S)$.
```
```ad-Dimostrazione
Sia $\Phi$ una funzione potenziale per il gioco, e sia $S$ un vettore di strategia puro che minimizza $\Phi(S)$. Si consideri una qualsiasi mossa da parte di un giocatore $i$, che risulta in un nuovo vettore di strategia $S^{'}$. 
Essendo $S$ il vettore che minimizza $\Phi(S)$, vale $\Phi(S^{'})\geq \Phi(S)$. Quindi, $\Phi(S)-\Phi(S^{'}) \leq 0$.
Per definizione di funzione potenziale, si ha $\Phi(S)-\Phi(S^{'}) = \textnormal{cost}_i(S)-\textnormal{cost}_i(S^{'})$, e ciò implica che
$$
\textnormal{cost}_i(S)\leq \textnormal{cost}_i(S^{'})
$$
Quindi il giocatore $i$ non può diminuire il suo costo, e per tale motivo $S$ risulta essere un equilibrio di Nash.
```

Si osserva ora che ogni stato $S$, con la proprietà che $\Phi$ non può essere diminuita alterando una qualsiasi strategia in $S$, è un equilibrio di Nash per l'argomentazione appena fatta. 
Ancor di più, la dinamica di best response simula una ricerca locale su $\Phi$. Queste osservazioni implicano il seguente risultato
```ad-Teorema
title: Teorema 2.5
In ogni gioco potenziale finito, la dinamica di best response converge sempre ad un equilibrio di Nash.
```
```ad-Dimostrazione
La dinamica di best response simula una ricerca locale su $\Phi$, quindi:
1. Ogni mossa riduce strettamente $\Phi$.
2. Per definizione di gioco finito, si hanno un numero finito di soluzioni.
Per il teorema visto in precedenza, che mostra la presenza di almeno un equilibrio di Nash per un gioco potenziale, e i due punti appena illustrati, ciò dimostra il teorema in questione.
```
```ad-Osservazione
title: Osservazione 2.2
Nei giochi mostrati, una best response può essere calcolata in tempo polinomiale.
```


Si da ora un upper bound generico sul prezzo della stabilità per un gioco potenziale arbitrario.

```ad-Teorema
title: Teorema 2.6
Si supponga di avere un gioco potenziale con funzione potenziale $\Phi$, e si assuma che per ogni esito $S$ si ha
$$
\frac{\textnormal{cost}(S)}{A} \leq \Phi(S) \leq B \cdot \textnormal{cost}(S)
$$
per delle costanti $A,B >0$. Allora il prezzo della stabilità è al più $A\cdot B$.
```
```ad-Dimostrazione
Sia $S^N$ un vettore di strategia che minimizza $\Phi(S)$. Allora, per il teorema $2.4$, che afferma l'esistenza di un equilibrio di Nash puro per un gioco potenziale, si ha che $S^N$ è un equilibrio di Nash. 
Per dimostrare il teorema, è sufficiente mostrare che il costo di questa soluzione non è molto più grande di quello di una soluzione $S^*$ di costo minimo.
Per ipotesi, si ha che $\frac{\textnormal{cost}(S^N)}{A} \leq \Phi(S^N)$. Per definizione di $S^N$, si ha che $\Phi(S^N) \leq \Phi(S^*)$. La seconda disuguaglianza dell'assunzione fatta implica che $\Phi(S^*) \leq B \cdot \textnormal{cost}(S^*)$. Componendo le due disuguaglianze si ha che 
$$
\textnormal{cost}(S^N) \leq AB \cdot \textnormal{cost}(S^*)
$$
```

Si torna ora al global connection game. Mediante i teoremi appena enunciati, si è dimostrato che esiste sempre un equilibrio di Nash per il gioco studiato. Bisogna ora fornire una delimitazione superiore al suo prezzo della stabilità.
Si consideri un'istanza del global connection game, ed un vettore di strategia $S=(P_1,P_2,\ldots,P_k)$ contenente un cammino $s_i - t_i$ per ogni giocatore $i$. Per ogni arco $e$, si definisce una funzione $\Psi(S)$, che mappa i vettori di strategia in valori reali come segue
$$
\Psi_e(S) = c_e \cdot \mathcal{H}_{k_e}
$$
dove $k_e$ è il numero di giocatori che usano l'arco $e$ in $S$, e $\mathcal{H_k}=\sum_{j=1}^k 1/j$ è il $k-$esimo numero armonico. Sia quindi
$$
\Psi(S) = \sum_{e} \Psi_e(S)
$$
Il seguente lemma mostra come $\Psi(S)$ sia una funzione potenziale per il global connection game.
```ad-Lemma
title: Lemma 2.1
Sia $S=(P_1,P_2,\ldots,P_k)$, e sia $P_i^{'} \neq P_i$ un cammino alternativo per un giocatore $i$, e si definisca un nuovo vettore di strategia $S^{'}=(S_{-i},P_i^{'})$. Allora
$$
\Psi(S) - \Psi(S^{'}) = \textnormal{cost}_i(S) - \textnormal{cost}_i(S^{'})
$$
```
```ad-Dimostrazione
Questo lemma afferma che quando un giocatore $i$ cambia le sue strategie, il corrispondente cambio in $\Psi(\cdot)$ rispecchia esattamente il cambio nel payoff di $i$. Sia $k_e$ il numero di giocatori che utilizzano $e$ in $S$. Per ogni arco $e$ che appare in entrambi $P_i$ e $P_i^{'}$ o in nessuno di esi, il costo pagato da $i$ per $e$ è lo stesso sia per $S$ che per $S^{'}$. Quindi, $\Psi_e(\cdot)$ ha lo stesso valore sotto e $S$ e $S^{'}$. Per ogni arco $e$ in $P_i$ ma non in $P_i^{'}$, cambiando strategia da $S$ ad $S^{'}$, $i$ risparmia (e quindi migliora il suo payoff di) $c_e/k_e$, che risulta essere precisamente la diminuizione in $\Psi_e(\cdot)$. Similmente, per un arco $e$ in $P_i^{'}$ ma non in $P_i$, il giocatore $i$ paga un costo di $c_e/(k_e+1)$ cambiando da $S$ ad $S^{'}$, che corrisponde all'aumento in $\Psi_e(\cdot)$. Essendo $\Psi(\cdot)$ semplicemente la somma di $\Psi_e(\cdot)$ su tutti gli archi, il cambio collettivo nel payoff di $i$ è esattamente la negazione del cambio in $\Psi(\cdot)$.
```
```ad-Osservazione
title: Osservazione 2.3
I teoremi $2.4$ e $2.5$, assieme al lemma $2.1$ implicano il teorema $2.2$.
Infatti il lemma $2.1$, mostrando come $\Psi(S)$ sia una funzione potenziale per il global connection game, implica l'appartenenza di tale gioco alla classe dei giochi potenziali. Appurato ciò, i teoremi $2.4$ e $2.5$ risultano essere applicabili al global connection game.
```

```ad-Lemma
title: Lemma 2.2
Per ogni vettore di strategia $S$, si ha che 
$$
\textnormal{cost}(S) \leq \Psi(S) \leq \mathcal{H}_k \textnormal{cost}(S)
$$
```
```ad-Dimostrazione
Sia ora $e$ un arco usato nel profilo di strategia $S$. La funzione $\Psi_e(S)$ è almeno $c_e$, perché ogni arco utilizzato è selezionato da almeno un giocatore, e non più di $\mathcal{H}_kc_e$, essendovi solo $k$ giocatori. Si ha quindi che 
$$
c_e \leq \Psi_e(S) \leq \mathcal{H}_kc_e
$$
Essendo $$\textnormal{cost}(S) = \sum_{e \in N(S)}c_e$$$$\Psi(S)= \sum_{e \in N(S)}\Psi_e(S)=\sum_{e\in N(S)}c_e \mathcal{H}_{k_e(S)}$$
e
$$
\sum_{e\in N(S)}c_e \mathcal{H}_{k_e(S)} \leq \sum_{e \in N(S)} \mathcal{H}_kc_e
$$
dove si ricorda $1\leq k_e(S) \leq k$ per ogni $e \in N(S)$ essere il numero di giocatori che usano l'arco $e$,  si ha che 
$$
\textnormal{cost}(S) \leq \Psi(S) = \sum_{e\in N(S)}c_e \mathcal{H}_{k_e(S)} \leq \sum_{e \in N(S)} \mathcal{H}_kc_e = \mathcal{H}_k\textnormal{cost}(S)
$$
```
```ad-Osservazione
title: Osservazione 2.4
Il teorema $2.6$ assieme al lemma $2.2$ dimostrano il teorema $2.3$.
**ARGOMENTARE**
```

Si vuole studiare ora quanto sia difficile, data un'istanza di un global connection game ed un valore $C$, determinare se il gioco possiede un equilibrio di Nash di costo al più $C$.

```ad-theorem
title: Teorema
Data un'istanza di un Global Connection Game ed un valore $C$, è $\mathbf{NP}-$completo determinare se il gioco a un equilibrio di Nash di costo al più $C$.
```

Il seguente teorema viene dimostrato mediante una riduzione polinomiale dal problema 
$3D-$Matching. 
Il problema può essere descritto come segue:

```ad-Problema
title: $3D-$Matching
Dati tre insiemi disgiunti $X,Y,Z$, ciascuno di dimensione $n$, e dato un insieme $T \subseteq X \times Y \times Z$ di triple ordinate, esiste un insieme di $n$ triple $M \subseteq T$ tale per cui ogni elemento di $X \cup Y \cup Z$ è contenuto in esattamente una di queste triple?
```
```ad-Dimostrazione
Data un'istanza di $3D$-Matching con insiemi di nodi $X,Y,Z$, si costruisce un grafo come segue:
- Inserisci un nodo per ogni nodo in $X,Y,Z$.
- Inserisci un nodo $v_{i,j,k}$ per ogni  arco $3D$ $(x_i,y_j,z_k)$.
- Inserisci un nodo $t$.
- Aggiungi un arco diretto da ciascun nodo $v_{i,j,k}$ verso $t$ avente costo $c_e =3$.
- Per ogni nodo $v$ in $X,Y,Z$ aggiungi un arco diretto da $v$ verso tutti i nodi che rappresentano un arco $3D$ contenente $v$, aventi costo $c_e=0$.
- Sia $k=|X|+|Y|+|Z|$. Aggiungi un giocatore per ciascun nodo $v$ in $X \cup Y \cup Z$. Questo giocatore ha due nodi terminali: $v$ nodo sorgente e $t$ nodo pozzo.

Se esite un $3D$ Matching per l'istanza di $3D-$Matching, allora esiste un equilibrio di Nash nel global connection game di costo $k$:
Sia $M$ il $3D$ Matching, e sia $S_i$, per il giocatore avente come nodi terminali $v$ e $t$, il cammino da $v$ a $t$ contenente:
- l'arco da $v$ verso l'unico nodo $v_{i,j,k}$ corrispondente all'arco $3D$ in $M$, dove l'unicità di $v_{i,j,k}$ è data dal fatto che $M$ è un Matching.
- l'arco da questo $v_{i,j,k}$ a $t$.

Essendo $M$ un matching, il costo di $S$, dove $S$ è il profilo di strategia contenente $S_i$ per ogni giocatore $i$, è esattamente $3k/3=k$. $S$ è quindi un equilibrio di Nash, visto che una qualsiasi deviazione da questo profilo di strategia da parte di un giocatore comporta il pagamento di un arco avente costo $3$, mentre la quantità pagata secondo l'attuale profilo di strategia è $1$, essendo tale arco coinvolto in $S$ condiviso dagli altri giocatori appartenenti alla rispettiva tripla del matching.
Se non esiste un $3D$ Matching, allora ogni soluzione del gobal connection game deve costare più di $k$. Quindi, non può esistere un equilibrio di Nash avente costo al più $k$.
```

## 2.2 Local connection game

Si studia un altro gioco di formazione di reti, il local connection game, dove ciascun giocatore può creare dei collegamenti verso altri giocatori.
Un local connection game è un gioco che modella quindi la creazione di reti.
Informalmente, i giocatori hanno due obiettivi:
- Minimizzare il costo da pagare per la costruzione della rete.
- Assicurarsi che tale rete assicuri un'alta qualità di servizio.
I giocatori sono nodi che:
- Pagano per gli archi che costruiscono.
- Ottengono beneficio da cammini brevi.
### 2.2.1 Modello
I giocatori nel local connection game sono identificati con dei nodi in un grafo $G$, sul quale la rete deve essere costruita. Una strategia per il giocatore $u$ è un insieme di archi non diretti che il giocatore $u$ costruirà, i quali avranno tutti $u$ come uno dei due estremi. Dato un vettore di strategia $S$, l'insieme di archi dato dall'unione delle strategie di tutti i giocatori formerà una rete $G(S)$ su i nodi giocatori. In particolar modo, si fa presente che la rete conterrà l'arco $(u,v)$ se questo è comprato da $u$ o $v$ (o entrambi). 
Sia $dist_{G(S)}(u,v)$ il cammino minimo tra $u$ e $v$ in $G(S)$. Si utilizzerà la notazione $dist(u,v)$ quando $S$ risulterà essere chiaro dal contesto. 
Il costo della costruzione di un arco è specificato da un singolo parametro $\alpha$.
Ciascun giocatore ha come obiettivo:
- Rendere corta la distanza verso gli altri nodi.
- Pagare il meno possibile.
Più precisamente, l'obiettivo del giocatore $u$ è quello di minimizzare la somma dei costi e delle distanze definita come
$$
\textnormal{cost}_u(S) = \alpha n_u \ +\ \sum_{v}dist_{G(S)}(u,v) 
$$dove:
- $n_u$ è il numero di archi comprati da $u$.
- $\alpha n_u$ è il costo di costruzione.
- $\sum_{v}dist_{G(S)}(u,v)$ è il costo di utilizzo.

Anche per questo problema, si fa presente che
- Si usa l'equilibrio di Nash come concetto di soluzione.
- Per valutare la qualità generale di una rete, si considera il costo sociale, ossia la somma dei costi di tutti i giocatori.
- Una rete $G(S)$ si dice ottimale o socialmente efficiente se minimizza i costi sociali.
- Un grafo $G=(V,E)$ si dice *stabile* per un valore $\alpha$ se esiste un vettore di strategia $S$ tale per cui $S$ è un equilibrio di Nash per il local connection game ed $S$ forma $G$, ossia $G(S)=G$. 

Si osserva ora che essendo gli archi non diretti, quando un nodo $u$ compra un arco $(u,v)$, quell'arco è disponibile per l'utilizzo anche da $v$ a $u$, e quindi, in particolar modo, risulta essere disponibile per $v$. Quindi, in un equilibrio di Nash, al più uno dei due nodi $u$ e $v$ paga per l'arco $(u,v)$ che li collega. Se così non fosse, uno dei due nodi potrebbe diminuire il costo rinunciando alla costruzione dell'arco, e quindi il profilo di strategia in questione non sarebbe un equilibrio di Nash.
Inoltre si fa presente che, essendo la distanza $dist(u,v)$ pari ad infinito quando $u$ e $v$ non sonno connessi, nell'equilibrio bisogna avere necessariamente un grafo connesso. Quindi, ogni rete stabile deve essere connessa.
Il costo sociale di una rete $G$ è
$$
SC(G) = \alpha|E|+\sum_{u,v \in V}dist_{G(S)}(u,v)
$$
ossia la somma dei costi dei giocatori. Si evidenzia che la distanza $dist_{G(S)}(u,v)$ contribuisce alla qualità generale della soluzione due volte, una per $u$ ed una per $v$.
Si vogliono quindi confrontare soluzioni stabili per questo problema con soluzioni ottimali.
In particolar modo:
- Si vuole dare una delimitazione al prezzo della stabilità.
- Si vuole dare una delimitazione al prezzo dell'anarchia.
Cosi facendo si potrà dare una delimitazione alla perdita di efficienza della rete dovuta alla stabilità.

### 2.2.2 Caratterizzazione delle soluzioni e prezzo della stabilità.
Si vuole studiare la struttura di una soluzione ottimale in funzione di $\alpha$. Si ricorda che una rete è ottimale o efficiente se minimizza il costo sociale $SC(G)$.
Per fare ciò, si definiscono due tipologie di grafi.

```ad-lemma
title: Definizione (Grafo completo):
Un grafo $G=(V,E)$ si dice completo se $\forall u,v \in V$ tali per cui $u\neq v$, $(u,v) \in E$.
```

```ad-lmma
title: Definizione (Stella)
Un grafo semplice $G=(V,E)$ con $|V|=n$ si dice stella di ordine $n$ se gode delle seguenti proprietà:
- Esiste un solo $u \in V$ tale per cui $d(u)=n-1$, dove $d(u)$ è il grado del nodo $u$.
- $\forall v \in V, v \neq u$ si ha che $d(v)=1$ e $v$ è adiacente solamente ad $u$.
```

![[03-agt_img02.png]] Stella di ordine $8$.

La struttura della rete ottima dipende quindi da $\alpha$. Più $\alpha$ è piccolo, più ai nodi conviene comprare tanti archi. Viceversa, più $\alpha$ è grande e più ai nodi conviene comprare pochi archi.
Intuitivamente ci si aspetta quindi la seguente situazione:
- Se $\alpha >> 1$, ovvero se $\alpha$ è molto grande, allora la rete ottima è il grafo a stella.
- Se $\alpha << 1$, ovvero se $\alpha$ è molto piccolo, allora la rete ottima è il grafo completo.

```ad-Lemma
title: Lemma 2.3
Se $\alpha \geq 2$ allora ogni stella è una soluzione ottimale, e se $\alpha \leq 2$ allora il grafo completo è una soluzione ottimale.
```
```ad-Dimostrazione
Sia $G=(V,E)$ una soluzione ottimale con $|E|=m$ archi.
Si ha che $m \geq n-1$, se cosi non fosse il grafo sarebbe disconnesso, ed avrebbe quindi costo pari ad infinito.
Tutte le coppie ordinate di nodi non adiacenti devono avere una distanza di almeno $2$ tra di essi, e vi sono $n(n-1)-2m$ di tali coppie. Aggiungendo le rimanenti $2m$ coppie con distanza $1$ si ottiene il seguente lower bound per il costo sociale di $G$
$$
\begin{align*}
SC(G)\ &\geq\ \ \alpha m + 2m + 2(n(n-1)-2m)\ \ =\ \ \alpha m + 2n(n-1) - 4m+2m \\
\\

&= (\alpha -2)m + 2n(n-1)\ =\ LB(m)
\end{align*}
$$
Dove $\alpha m$ è il costo necessario per costruire $m$ archi.
Da ciò segue quindi che 
$$
SC(G) \geq LB(m) \geq \textnormal{min}_m LB(m)
$$
Sia una stella che il grafo completo rispettano questo bound.
Si analizzano i due casi separati, a seconda del valore di $\alpha$:
- Se $\alpha \geq 2$, allora $LB(m)$ raggiunge il minimo quando $m=n-1$ e quindi
$$
SC(G) \geq (\alpha -2)(n-1) + 2n(n-1)
$$
Dove $(\alpha -2)(n-1) + 2n(n-1)$ è il costo sociale di un qualsiasi grafo a stella con $n$ nodi. Dunque se $\alpha \geq 2$, non si può fare di meglio di una stella.
- Se $\alpha \leq 2$, allora per minimizzare $LB(m)$ risulta necessario massimizzare $m$. Dato che il massimo valore assumibile ad $m$ è proprio $m=n(n-1)/2$, il numero degli archi di un grafo completo ad $n$ nodi, si ha che
$$
SG(C) \geq LB \left( \frac{n(n-1)}{2}\right)
$$
Dove $LB \left( n(n-1)/2 \right)$ è il costo sociale di un grafo completo con $n$ nodi. Dunque se $\alpha \geq 2$, non si può fare di meglio di un grafo completo.

```

Il costo sociale è quindi minimizzato rendendo $m$:
- il più piccolo possibile per $\alpha \geq 2$, ossia per $m=n-1$, avendo cosi una stella.
- il più grande possibile per $\alpha \leq 2$, ossia per $m=n(n-1)/2$, avendo cosi un grafo completo.

Ci si chiede ora quando, per le due topologie prese in esame, i grafi appartenenti ad esse siano stabili. 
Sia la stella che il grafo completo possono essere ottenuti come un equilibrio di Nash per certi valori di $\alpha$, mostrati nel seguente lemma.
```ad-Lemma
title: Lemma 2.4
Se $\alpha \geq 1$, allora ogni stella è un equilibrio di Nash,  e se $\alpha \leq 1$, allora il grafo completo è un equilibrio di Nash.
```
```ad-Dimostrazione
Si supponga $\alpha \geq 1$ e si consideri una stella.
Risulta che ogni assegnazione di archi a giocatori incidenti corrisponde ad un equilibrio di Nash. Per questo risultato è necessario dimostrare una singola soluzione.
In particolare, si consideri il profilo di strategia nel quale il giocatore $1$, al quale è associato il centro della stella, compra tutti gli archi verso gli altri i restanti $n-1$ giocatori, mentre questi non comprano niente. Il giocatore $1$ non ha incentivo a deviare strategia, perché così facendo il grafo risulterebbe disconnesso, portandolo a dover pagare una penalità pari ad infinito. Ogni altro giocatore può invece deviare da questo profilo di strategia solamente comprando archi. Per ciascuno di essi, aggiungere $k$ archi comporta un risparmio di $k$ in distanza, ma comporta un costo di $\alpha k$, ed essendo $\alpha \geq 1$, non risulta essere una deviazione che migliora il suo payoff. Quindi la stella è un equilibrio di Nash.

Si supponga ora che $\alpha \leq 1$. Si consideri un grafo completo, con ogni arco assegnato ad un giocatore incidente. Un giocatore che smette di pagare per un insieme di $k$ archi risparmia $\alpha k$ in costo, ma aumenta la distanza totale di $k$, quindi, questo esito è stabile.
```

Questi particolari equilibri di Nash, assieme alle soluzioni ottimali mostrate, permettono di dare un upper bound al prezzo della stabilità.
```ad-Teorema
title: Teorema 2.8
Se $\alpha \geq 2$ o $\alpha \leq 1$, il prezzo della stabilità è $1$. Per $1 < \alpha <2$, il prezzo della stabilità è al più $4/3$.
```
```ad-Dimostrazione
Per $\alpha \geq 2$ o $\alpha \leq 1$ il teorema risulta essere vero per il lemma $2.3$ ed il lemma $2.4$.
Infatti, per $\alpha \geq 2$ si ha che ogni stella è sia un equilibrio di Nash, sia una soluzione ottimale, e per definizione, $\mathbf{PoS}=1$.
Analogamente, per $\alpha \leq 1$, si ha che ogni grafo completo è sia un equilibrio di Nash e sia una soluzione ottimale, e per definizione, $\mathbf{PoS}=1$.

Quando $1 < \alpha < 2$, la stella è un equilibrio di Nash, mentre il grafo completo è la struttura ottima. Per determinare il prezzo della stabilità, bisogna calcolare il rapporto del costo di queste due soluzioni. Il peggior caso per questo rapporto avviene quando $\alpha$ tende ad $1$. Infatti, sia $K_n$ il grafo completo con $n$ nodi e $T$ una stella con $n$ nodi, si ha che
$$
\begin{align*}
\mathbf{PoS}\ &\leq\ \frac{SC(T)}{SC(K_n)}\ =\ \frac{(\alpha-2)(n-1)+2n(n-1)}{\alpha \frac{n(n-1)}{2} + n(n-1)} \textnormal{ [massimizzato per } \alpha \rightarrow 1\textnormal{]}\\
\\
&\leq \frac{-1(n-1)+2n(n-1)}{\frac{n(n-1)}{2} + n(n-1)} = \frac{(n-1)(-1+2n)}{(n-1)(\frac{n}{2}+n)} \\
\\
&= \frac{2n-1}{\frac{3n}{2}} = \frac{4n-2}{3n} < \frac{4}{3}
\end{align*}
$$
```

Dimostrare per esercizio che il grafo completo è l'unico equilibrio per $\alpha<1$. Ciò implicherà che il prezzo dell'anarchia è $1$ per questo range. 

**Soluzione**: Se $\alpha <1$, il grafo completo è l'unica rete stabile. Infatti se, per il profilo di strategia $S$, $G(S)$ non generasse un grafo completo, allora esistono due nodi $u,v \in V$ tali per cui $(u,v) \notin E$. Quindi al giocatore $u$ converrebbe deviare strategia pagando l'arco $(u,v)$, in quanto spenderebbe $\alpha$ risparmiando $1$. Dato che per $\alpha < 1$ il grafo completo è anche ottimo, si ha che in questo caso il $\mathbf{PoA}$ del gioco è pari ad $1$.

### 2.2.3 Prezzo dell'anarchia
Si vuole dimostrare che il prezzo dell'anarchia del local connection game è al più $\mathcal{O}(\sqrt{\alpha})$.
Tale delimitazione viene ottenuta mediante due passaggi:
1. Si da un bound sul diametro del grafo ottenuto mediante il raggiungimento di una soluzione stabile.
2. Si usa il diametro per dare un bound al costo.

Per fare ciò si farà riferimento alle seguenti definizioni
```ad-lemma
title: Definizione (Diametro)
Il diametro di un grafo $G=(V,E)$ è la massima distanza tra due nodi in $V$
$$
\textnormal{max}_{u,v\in V} dist(u,v)
$$
```
```ad-lemma
title: Definizione (Cut-edge)
Sia $G=(V,E)$ un grafo. Un arco $e$ è definito cut edge di $G$ se il grafo $G-e=(V,E\backslash\{e\})$ risulta essere disconnesso.
```
```ad-Osservazione
title: Osservazione 2.5
Ogni grafo ha al più $n-1$ cut edges, dove il grafo con esattamente $n-1$ cut edges è la stella.
```

Si studia ora il primo passaggio.
```ad-Lemma
title: Lemma 2.5
Il diametro di una qualsiasi rete stabile è al più $2\sqrt(\alpha)+1$.
```
```ad-Dimostrazione
Sia $G$ una rete stabile, siano $u,v \in V$ due nodi nel grafo e si consideri l'intero $k$ tale per cui 
$$
2k \leq dist_G(u,v) \leq 2k+1
$$
Graficamente, lo scenario studiato può essere rappresentato come segue, con $v = v_0$.
![[LCGDim1.png]]

Si valuta come cambia il costo di $u$ se quest'ultimo decidesse di comprare l'arco $(u,v)$:
- Il nodo $v_0$ passerebbe da una distanza $\geq 2k$ ad una distanza pari ad $1$, con un risparmio conseguito $\geq 2k-1$.
- Il nodo $v_1$ passerebbe da una distanza $\geq 2k-1$ ad una distanza pari a $2$, con un risparmio conseguito $\geq 2k-2$
Procedendo analogamente per i gli altri nodi, si conclude che
- Il nodo $v_{k-1}$ passerebbe da una distanza $\geq k+1$ ad una distanza pari a $k$, con un risparmio conseguito $\geq 1$.

Si hanno quindi $k$ vertici per i quali la loro distanza da $u$ risulta ridotta.
Sia $r$ il risparmio complessivo, si ha quindi che 
$$
r \geq \sum_{i=0}^{k-1}(2i+1)=k^2
$$

Essendo $G$ una rete stabile per ipotesi, si ha che $\alpha$, il costo per l'acquisto dell'arco $(u,v)$, deve essere maggiore del risparmio complessivo.
Quindi
$$
\alpha \geq r \geq k^2 \iff k \leq \sqrt{\alpha}
$$
E ciò implica che
$$
dist_G(u,v) \leq 2k+1 \leq 2\sqrt{\alpha}+1
$$
```

Per dimostrare il secondo passaggio, si fa riferimento a due proposizioni, le quali forniscono una delimitazione al numero di non-cut edges che vengono comprati da un nodo in una rete stabile.

```ad-Proposizione
title: Proposizione 2.1
Sia $G$ una rete con diametro $d$ e sia $e=(u,v)$ un non-cut edge. Allora in $G-e$ ciascun nodo $w$ incrementa la propria distanza da $u$ di al più $2d$.
```
```ad-Dimostrazione
Si consideri il seguente albero radicato in $u$, ottenuto mediante BFS su $G$.
![[LCGDim2.png]]
Essendo che l'arco $(u,v)$ non è un cut edge, deve esistere un arco $(x,y)$ che attraversa il taglio nell'albero BFS indotto dalla rimozione dell'arco $(u,v)$. Allora si ha che 
$$
\begin{align*}
dist_{G-e}(u,w) &\leq dist_G(u,x)+1+dist_G(y,v)+dist_G(v,w) \\
\\
& \leq d + 1 + d + dist_G(u,w) - 1 \ \ \textnormal{[essendo } dist_G(v,w) = dist_G(u,w) - 1\textnormal{]} \\
\\
& \leq dist_G(u,w)+2d
\end{align*}
$$
e quindi
$$
dist_{G-e}(u,w) \leq dist_G(u,w)+2d
$$
```

```ad-Proposizione
title: Proposizione 2.2
Sia $G$ una rete stabile e sia $F$ l'insieme dei non-cut edges comprati da un nodo $u$. Allora
$$
|F| \leq \frac{(n-1)2d}{\alpha}
$$
```
```ad-Dimostrazione
Si consideri il seguente albero radicato in $u$, ottenuto mediante BFS su $G$.
![[LCGDim3.png]]
dove $k=|F|$, ed $n_i$ è il numero dei nodi nel sottoalbero radicato in $v_i$.
Se $u$ rimuove l'arco $(u,v_i)$ risparmia $\alpha$, e facendo riferimento alla proposizione precedente si ha che la sua distanza dai nodi contenuti nel sottoalbero radicato in $v_i$ aumenta al più di $2d$ per ogni nodo. L'incremento totale nel sottoalbero radicato in $v_i$ è quindi $2dn_i$.
Essendo $G$ stabile, si ha che $\alpha \leq 2dn_i$. Estendendo il ragionamento per $i=1,\ldots,k$ e sommando per ogni $i$, si ha che
$$
k\alpha \leq \sum_{i=1}^k 2dn_i \leq 2d(n-1)
$$
e quindi
$$
k = |F| \leq \frac{2d(n-1)}{\alpha}
$$
```

Si fornisce ora un lemma riguardante il costo sociale di una rete stabile.
```ad-Lemma
title: Lemma 2.6
Il costo sociale di una qualsiasi rete stabile $G=(V,E)$ con diametro $d$ è al più $\mathcal{O}(d)$ volte il costo sociale ottimo.
```
```ad-Dimostrazione
In qualsiasi soluzione stabile, in totale si hanno almeno $n-1$ archi. Se così non fosse, almeno un nodo sarebbe sconnesso, e l'acquisto di un arco verso tale nodo, o di un arco da parte di quel nodo, porterebbe ad un profilo di strategia migliore, rendendo quindi la precedente non stabile.

Nel caso migliore, ogni coppia di nodi si trova a distanza $1$ tra essi. Si ha quindi il seguente lower bound
$$
OPT \geq \alpha(n-1)+n(n-1)
$$
Dove $OPT$ è il costo della rete ottima.
Il costo sociale della rete è invece dato da 
$$
SC(G) = \sum_{u,v \in V}dist_G(u,v) + \alpha \cdot |E|
$$
Si osserva ora che
1.
$$
 \sum_{u,v \in V}dist_G(u,v) \leq d \cdot n(n-1) \leq d \cdot OPT
$$



2.
$$
\begin{align*}
\alpha \cdot |E| &= \alpha \cdot |E_{cut}| + \alpha \cdot |E_{non-cut}| \\
\\
& \leq \alpha(n-1) + \alpha \cdot \frac{n(n-1)2d}{\alpha} \\
\\
&\leq 2d \cdot OPT
\end{align*}
$$ 

Dove il secondo punto è stato ottenuto facendo riferimento alla proposizione $2.2$ e al fatto che un grafo $G$ può avere al più $n-1$ cut edges. Componendo il tutto si ha che 
$$
SC(G) = \sum_{u,v \in V}dist_G(u,v) + \alpha \cdot |E| \leq d\cdot OPT + 2d \cdot OPT = 3d \cdot OPT
$$
```

Facendo riferimento quindi al lemma $2.5$ ed al lemma $2.6$, si dimostra il seguente teorema, che definisce una delimitazione superiore al $\mathbf{PoA}$
```ad-Teorema
title: Teorema 2.9
Il prezzo dell'anarchia per il local connection game con parametro $\alpha$ è al più $\mathcal{O}(\sqrt{\alpha})$.
```
```ad-Dimostrazione
Sia $G$ una qualsiasi rete stabile. Dal lemma $2.5$ segue che il diametro di $G$ è $\mathcal{O}(\sqrt{\alpha})$. Dal lemma $2.6$ invece si ha che il costo sociale di $G$ è $\mathcal{O}(\sqrt{\alpha})$ volte quello ottimo. Allora, per definizione, si ha che 
$$
\mathbf{PoA} = \mathcal{O}(\sqrt{\alpha})
$$

```

Si mostra ora il seguente teorema riguardante la complessità del calcolo della best response di un dato giocatore:
```ad-theorem
title: Teorema
E' NP-hard, date le strategie degli altri agenti, calcolare la best response di un dato giocatore.
```
Il seguente teorema viene dimostrato mediante una riduzione polinomiale dal problema Dominating Set. 
```ad-lemma
title: Definizione (Dominating Set)
Sia $G=(V,E)$ un grafo non orientato e sia $U\subseteq V$. $U$ è un insieme dominante per $G$ se ogni nodo in $V-U$ ha un vicino in $U$. 
Formalmente, $\forall v \in V-U$, $\exists u \in U$ tale che $(u,v) \in E$.
```
Il problema può essere descritto come segue:
```ad-Problema
title: Dominating Set
Il problema dominating set consiste nel trovare, dato un grafo $G=(V,E)$, un insieme dominante di cardinalità minima.
Formalmente:
- $\textbf{Input}:$ Grafo $G=(V,E)$.
- $\textbf{Soluzione}:$ $U\subseteq V$ tale per cui $U$ è un insieme dominante.
- $\textbf{Obiettivo}:$ Minimizzare la cardinalità di $U$.
```

```ad-Dimostrazione
Sia $1 < \alpha < 2$ e si consideri un'istanza $I$ del problema Dominating Set.
![[LCGDim4.png]]

Sia $i$ un giocatore e si consideri il grafo dell'istanza $I$ come se fosse il grafo costruito dalle strategie dei restanti giocatori. Si dimostra quindi come, per $1 < \alpha < 2$ il giocatore $i$ ha una strategia che lo porta ad un costo $\leq \alpha k + 2n-k$ se e solo se esiste un Dominating Set di dimensione $\leq k$.
- $(\leftarrow)$: Dato un dominating set $U$ di dimensione $|U|\leq k$, se il giocatore $i$ si connette a tutti i nodi del dominating set paga al più
$$
\alpha \cdot k + 2(n-k)+k = \alpha k + 2n -k
$$
dove $\alpha \cdot k$ è il costo per la costruzione degli archi e $2(n-k)+k$ è il costo per le distanze.
- $(\rightarrow)$: Sia $S_i$ una strategia di costo $\leq \alpha k + 2n-k$. Se in $G(S_i)$ esiste un nodo $v$ che si trova ad una distanza $\geq 3$ dal giocatore $i$ (sia questo associato al nodo $x$), allora si modifica la strategia del giocatore $i$ aggiungendo l'arco $(x,v)$ ad $S_i$. Essendo che $\alpha < 2$, questa modifica non andrà ad aumentare il costo per $i$. 
Al termine di ciò, si ha che in $G(S_i)$ tutti i nodi distano da $i$ o di $1$ o di $2$.

Sia quindi $U$ l'insieme dei nodi che distano $1$ da $x$. Allora $U$ è un dominating set, e inoltre
$$
COST_i(S) = \alpha \cdot |U| + 2n-|U| \leq \alpha \cdot k + 2n-k \iff |U| \leq k
$$
```
# 3 Mechanism design
Il **Mechanism Design**, o *progettazione di Meccanismi* è una sottoarea della Teoria dei giochi il cui tipo di studio ha un aspetto più algoritmico e progettuale rispetto a scenari non cooperativi.
Informalmente, il mechanism design si occupa di definire un gioco in maniera tale che l'outcome desiderato venga necessariamente raggiunto, e che quindi il relativo profilo di strategia risulti essere un equilibrio.
Bisogna far presente che i giochi indotti da meccanismi presentano delle differenze rispetto ai giochi in forma standard:
- Ciascun giocatore possiede delle informazioni, ossia dei valori, i quali risultano essere *privati*, ossia tali per cui solo il giocatore in questione risulti esserne in possesso, ed *indipendenti* da quelli degli altri partecipanti. Tali valori prendono il nome di *tipi,* e si evidenzia come, essendo questi privati, possano venir utilizzati per manipolare il sistema da progettare.
- La matrice dei payoff è una funzione di questi tipi.

In generale, è possibile interpretare gli obiettivi dei meccanismi progettati nei termini astratti di una *scelta sociale*. Una scelta sociale è semplicemente un'aggregazione di preferenze dei diversi partecipanti verso una singola decisione collettiva. Il mechanism design si occupa quindi di implementare le scelte sociali desiderate in uno scenario strategico, assumendo che i diversi agenti coinvolti agiscano in maniera egoistica e razionale.

L'idea del Mechanism design è quella quindi di ottenere delle situazioni ottimali per un dato scenario,  incentivando gli agenti a comportarsi *bene*, in modo socialmente utile. 
Dunque alla base del Mechanism Design vi è la progettazione di un sistema che, caratterizzato da un ambiente popolato da agenti egoistici, deve raggiungere un certo obiettivo. A tale scopo, il sistema incentiva gli agenti a cooperare col sistema tramite un sistema di pagamento che premia gli agenti che collaborano al suo raggiungimento: deve generare una situazione di equilibrio, per il quale agli agenti non conviene fare una scelta diversa rispetto a quella *giusta* per il sistema.

Un aspetto fondamentale nella progettazione di meccanismi è che si applica in situazioni in cui gli agenti hanno determinate preferenze sull'outcome del gioco: si vuole, date queste preferenze, decidere un esito che soddisfi tutti gli agenti. Il problema in queste aggregazioni è, come detto in precedenza, che le informazioni effettivamente dichiarate dagli agenti al sistema possono non rispettare le loro preferenze reali, cioè i giocatori possono mentire al sistema per manipolarlo ed ottenere dei vantaggi. Dunque, idealmente, il sistema deve incentivare i giocatori a esprimere la preferenza reale, cioè a dire la *verità*.
Per esempio, in un sistema di voto che ha l'obiettivo di aggregare le reali preferenza dei giocatori per restituire un risultato che sia concorde all'opinione dei votanti, potrebbero esserci dei soggetti che votano un altro partito rispetto alla loro preferenza (ad esempio perché ha più probabilità di vincere, o perché ne fa parte qualche parente, o perché è stato corrotto). Questi votanti "falsi" alterano il risultato restituito dal sistema, che non rappresenterà le reali preferenze dei cittadini.

## Il problema dell'asta: single item auction
Si considerino $n$ agenti, o compratori, ciascuno dei quali partecipa ad un asta per la vendita di un singolo oggetto, ad esempio un quadro.
Ogni agente $a_i$ ha associato un valore $t_i$, che rappresenta il massimo valore che $a_i$ è disposto a spendere per aggiudicarsi il quadro; si osserva che per ogni $a_i$ il valore $t_i$ è *privato*, cioè non è conosciuto da nessun'altra entità presente all'asta.
Al fine di ottenere il quadro, ciascun agente $a_i$ dichiara in modo *indipendente dagli altri* un'offerta $r_i$ per il quadro. Si osserva che potenzialmente $r_i \neq t_i$, ossia i compratori possono dichiarare un valore diverso da quello privato.
L'agente $a_i$ che vince il quadro dovrà pagare $p$ per ottenere un utile di $u_i = t_i - p$; dunque i compratori puntano a vincere il quadro massimizzando il proprio utile.
L'obiettivo del venditore del quadro è quello di definire un *meccanismo d'asta* che venda il quadro al compratore con il valore privato $t_i$ più alto.
Il meccanismo non conosce i valori privati e gli agenti potrebbero dichiarare dei valori inferiori ai valori privati, dato che essi vogliono massimizzare il profitto (da qui la natura *egoistica* del gioco), dunque il meccanismo deve essere in grado di far dichiarare ad ogni agente il valore reale.
In particolare, il meccanismo d'asta deve:
- Decidere chi vince l'asta in funzione dei valori dichiarati $r_1,\dots,r_n$;
- Decidere quanto dovrà pagare il vincitore per prendersi il quadro.
Inoltre, il meccanismo d'asta è pubblico, dunque tutti gli agenti che partecipano sanno come viene scelto il vincitore e quanto dovrà pagare nel caso di vittoria.
Dunque il meccanismo dovrà regolare queste due decisioni in modo che per ogni agente è conveniente dichiarare come offerta il valore reale che attribuisce al quadro. In questo modo il meccanismo raggiunge il suo scopo, ossia assegnare il quadro all'agente con il valore privato più alto.
Si vedono tre possibili meccanismi d'asta.
### No payment
In questo meccanismo il quadro viene attribuito all'agente $a_i$ che ha dichiarato l'offerta più alta $r_i = \max_{j=1,\dots,k}\{ r_{j} \}$ e il vincitore non paga niente per il quadro.
Si nota facilmente che tale meccanismo non incentiva gli agenti a dire la verità. Infatti, a tutti gli agenti conviene offrire $r_i = +\infty$ per massimizzare il guadagno.
Allora il sistema non è in grado di assegnare il quadro all'agente con valore privato più alto.
### Pay your bid
In questo meccanismo, come nel precedente, il quadro viene attribuito all'agente $a_i$ che ha dichiarato l'offerta più alta $r_i = \max_{j=1,\dots,k}\{ r_{j} \}$. Questa volta, il vincitore paga il quadro tanto quanto il valore da lui dichiarato $r_i$.
Si nota che anche tale meccanismo non incentiva gli agenti a dire la verità. Infatti ogni agente dichiara $r_i < t_i$ in modo da avere utilità positiva in caso di vittoria del quadro. Dunque non sempre il quadro viene assegnato al giocatore il valore privato $t_i$ più alto.
### Vickrey's Second Price Auction
In questo meccanismo il quadro viene sempre attribuito all'agente $a_i$ che ha dichiarato l'offerta più alta $r_i = \max_{j=1,\dots,k}\{ r_{j} \}$. Il vincitore del quadro paga il quadro tanto quanto il secondo valore dichiarato più alto $R = \max_{j \neq i} \{r_i\}$, cioè quanto la seconda miglior offerta.
Questo meccanismo funziona, cioè incentiva gli agenti a dire la verità. Infatti vale il seguente teorema.
```ad-Teorema
title: Teorema 3.1
Nell'asta di Vickrey, $r_i = t_i$ è una strategia dominante  per ogni giocatore $i$.
```
Dunque nell'asta di Vickrey ogni giocatore massimizza la sua utilità quando dichiara la verità, indipendentemente dal tipo riportato dagli altri giocatori.
```ad-Dimostrazione
Siano $i$, $t_i$ e il vettore di strategie degli altri giocatori $r_{-i}$ fissati.
Sia $R=\max_{j\neq i} \{r_j\}$, cioè $R$ è la miglior offerta escludendo quella del giocatore $i$. Si osserva che $R$ non è noto al giocatore $i$, dato che le offerte sono fatte a busta chiusa.
1. Caso $t_i \geqslant R$ 
In questo caso, $i$ è il giocatore che da valore più alto al quadro.
- Dichiarando $r_i = t_i$, il giocatore $i$ riceve un utilità pari a $u_i = t_i - R \geq 0$; infatti il giocatore vince se $t_i > R$, mentre se $t_i = R$ il giocatore può sia vincere che perdere (in base alle regole per lo spareggio), ma l'utilità è sempre zero.
- Dichiarando un qualsiasi $r_i > R$ con $r_i \neq t_i$, il giocatore $i$ vince e continua ad avere utilità $u_i = t_i - R \geq 0$; non si ottiene nessun vantaggio a dichiarare più di $t_i$.
- Dichiarando un qualsiasi $r_i < R$, il giocatore $i$ non vince e riceve utilità pari a zero $u_i = 0$; questo outcome è peggiore per il giocatore $i$.
Non è conveniente per il giocatore $i$ dichiarare un valore diverso da $t_i$.
2. Caso $t_i \leqslant R$ 
In questo caso, esiste un giocatore $j$ che da un valore più alto del quadro rispetto al giocatore $i$.
- Dichiarando $r_i = t_i$, il giocatore $i$ non vince e riceve un utilità pari a $u_i = 0$; infatti vince il giocatore che ha tipo riportato pari a $R$;
- Dichiarando un qualsiasi $r_i < R$ con $r_i \neq t_i$, il giocatore $i$ non vince e continua ad avere utilità pari a zero $u_i = 0$; non si ottiene nessun vantaggio a riportare meno di $t_i$.
- Dichiarando un qualsiasi $r_i > R$, il giocatore $i$ vince ma ottiene utilità  $u_i = r_{i}-R < 0$;  dunque l'outcome è peggiore per il giocatore $i$.
Non è conveniente per il giocatore $i$ dichiarare un valore diverso da $t_i$.

In tutti i casi, riportare un tipo *falso* (cioè un tipo riportato diverso dal tipo) non produce una utilità migliore, dunque riportare $r_i = t_i$ , o dichiarare la verità, è una *strategia dominante*.
```

### Vickrey's Second Price Auction: versione di minimizzazione
Il meccanismo d'asta del secondo prezzo di Vickery nel contesto della vendita del quadro ha lo scopo di assegnare l'opera all'agente che massimizza la sua valutazione reale.
Si può definire una versione di minimizzazione di tale meccanismo: ad esempio quando si vuole allocare un lavoro ad un macchina tra un insieme di macchine.
## Ingredienti per definire un problema di mechanism design
Volendo generalizzare il problema specifico appena affrontato, l'idea dietro alla **algorithmic mechanism design** è quella di incentivare gli agenti a comportarsi in modo tale che le scelte individuali ed egoistiche degli agenti riescono ad ottenere dei buoni livelli di welfare.

Un modo per pensare ad un problema di mechanism design è il seguente: il sistema deve risolvere un problema di ottimizzazione, ma non è a conoscenza di alcune parti dell'istanza in input. Queste informazioni sconosciute sono mantenute da agenti razionali ed egoistici. Tali agenti dovranno dichiarare le informazioni mancanti al sistema e riceveranno un pagamento in base all'informazione rivelata al sistema. In particolare, gli agenti possono **mentire** al sistema per ottenere un maggiore guadagno.
Allora il sistema deve avere un meccanismo composto da:
- un'algoritmo che calcola soluzione ottima (o buona);
- uno schema di pagamento che ricompensa gli agenti in modo da far dichiarare le parti di input *reali*, cioè in grado di far rivelare la verità da ogni agente.
Grazie a questo meccanismo, il sistema può calcolare la soluzione ottima rispetto all'istanza reale in input.

Gli ingredienti da considerare nell'ambito del mechanism design sono i seguenti.
- $N$ **agenti** o giocatori: ogni agente ha una sola informazione privata $t_i \in T_i$ detta **tipo** (type);
- Un insieme di outcome ammissibili $F$: il sistema dovrà decidere uno degli outcome in $F$.
- Per ogni vettore dei tipi $t=(t_1,\dots,t_N)$, una *funzione di scelta sociale* $f(t) \in F$ specifica l'outcome che si vuole ottenere dato $t$; in altre parole, se il sistema conoscesse il vettore $t$ dei tipi di ogni giocatore, allora $f(t)$ è l'outcome da implementare. Inoltre il sistema, vorrebbe calcolare esattamente un outcome di $F$ che dipende dai tipi reali, dunque deve implementare la funzione di scelta sociale $f(t) \in F$.
- Ogni agente ha uno *spazio di strategie* $S_i$. Questa trattazione si restringe ai meccanismi detti *direct revelation mechanism*, in cui l'azione che un agente può eseguire è quella di dichiarare un tipo $r_i \in T_i = S_i$. Si osserva che il tipo riportato può essere diverso dal tipo reale di un'agente, ovvero $r_i \neq t_i$.
- Per ogni possibile outcome $x \in \mathcal{F}$, ciascun agente effettua una **valutazione** $v_i(t_i,x)$ rispetto alla sua preferenza per quel particolare outcome.
- Per ogni possibile vettore $r$ dei tipi riportati, ciascun agente riceve un **pagamento** $p_i(r)$. I pagamento vengono utilizzati dal meccanismo per incentivare gli agenti a collaborare.
- L'**utilità**  dell'agente $a_i$ se l'outcome per $r$ è $x(r)$ è pari a $$u_{i}(t_i,x(r)) = p_{i}(r) - v_{i}(t_{i},x(r))$$
A questo punto, l'obiettivo del mechanism design è quello di implementare un **meccanismo** $M = \langle g(r),p(x) \rangle$, dove
- $g(r)$ è un **algoritmo** che calcola l'outcome del gioco $x = g(r)$ in funzione dei valori riportati dagli agenti $r = (r_1,\dots,r_n)$;
- $p(x)$ è uno **schema di pagamento** che specifica il pagamento di ogni agente nell'outcome $x = g(r)$.
Il meccanismo deve essere implementato in modo tale da garantire che nelle situazioni di equilibrio rispetto alle utilità dei vari agenti si ottiene
$$ x = g(r) = f(t) $$

Se questo è vero in equilibrio di strategie dominanti, allora ogni agente $a_i$ riporterà il tipo $r_i$ che permette di calcolare l'outcome $x = f(t)$.
Il gioco indotto da un problema di mechanism design è il gioco in cui gli $n$ agenti sono i giocatori e la matrice dei payoff è implicitamente definita nelle funzioni di utilità.

Inoltre, si può formalizzare la proprietà principale dell'asta di Vickery, cioè che a tutti gli agenti conviene dire la verità, con le seguenti definizioni.

Un meccanismo $M = \langle g(r),p(x) \rangle$ è un'implementazione con strategie dominanti se esiste un vettore di tipi riportati $r^* = (r_1^*,\dots,r_n^*)$ tale che $f(t) = g(r^*)$ in equilibrio di strategie dominanti, cioè per ogni agente $a_i$  e per ogni vettore dei tipi riportati $r = (r_1,\dots,r_n)$ vale
$$ u_{i}(t_{i},(r_{-i},r_{i}^*)) \geqslant u_{i}(t_{i},(r_{-i},r_{i})) $$
Inoltre, se dire la verità è la strategia dominante, cioè se $r^* = t$, allora il meccanismo è detto **truthful**, o **strategy-proof** o **incentive compatible**. In questo caso, gli agenti riportano i reali tipi privati e non manipolano il valore dichiarato per fini strategici. Dunque l'algoritmo $g(\cdot)$ del meccanismo viene eseguito sulla reale istanza in input.

A questo punto, la questione principale da affrontare è come progettare dei meccanismi truthful, in altre parole:
1. come progettare l'algoritmo $g(r)$;
2. come definire uno schema di pagamenti
in maniera tale che la social choice function sottostante sia implementata in maniera truthful.
## Progettazione di meccanismi truthful
Come già descritto nelle righe precedenti, i problemi di mechanism design possono essere pensati come problemi di **ottimizzazione** in cui parte dell'istanza è nascosta dagli agenti. L'obiettivo del meccanismo è quello di incentivare gli agenti a rivelare la porzione di input **reale** che tengono nascosta.
I risultati riportati nel corso della trattazione saranno per problemi di *minimizzazione* (si possono definire anche per problemi di massimizzazione in modo analogo). Dunque si considera la seguente impostazione.
- per ogni outcome, o soluzione, $x \in \mathcal{F}$, la **funzione di valutazione** $v_i(t_i,x)$ rappresenta il **costo** pagato dall'agente $a_i$ nel particolare outcome $x$;
- la **funzione di scelta sociale** $f(t)$ associa al vettore dei tipi $t$ una soluzione $x$ che **minimizza** una qualche misura su $x$;
- I **pagamenti** vanno sempre dal meccanismo verso gli agenti. Gli agenti negli outcome sostenere un costo, e vengono pagati per sostenerlo. L'utile è dato per ogni agente è dato dal pagamento meno il costo.
Una classe di problemi per cui esiste una famiglia di meccanismi truthful è la classe dei problemi **utilitari**.

Un problema è **utilitario** se l'outcome desiderato è quello che minimizza la somma delle valutazioni degli agenti. Formalmente, un problema è utilitario se la funzione obiettivo (o funzione di scelta sociale) ha la forma
$$ f(t) := \arg\min_{x \in \mathcal{F}} \sum_{i} v_{i}(t_{i},x) $$
Per questa classe di problemi, esiste una classe di meccanismi truthful.

## Il meccanismo VCG
Il meccanismo **VCG** (Vickrey, Clarke, Groves) è l'unico meccanismo **truthful** per i problemi utilitari, ed è descritto come segue:
- L'algoritmo $g(r)$ calcola l'outcome $x$ che minimizza la somma delle valutazioni rispetto ai tipi riportati dagli agenti $$x = g(r) = \arg\min_{y \in \mathcal{F}} \sum_{i} v_{i}(r_{i},y)$$
- Il pagamento per l'agente $i$ è pari a $$p_{i}(x) = h_{i}(r_{-i}) - \sum_{j\neq i} v_{j}(r_{j},x)$$
dove $h_i(r_{-i})$ è una funzione arbitraria qualsiasi che dipende dai valori riportati da tutti gli agenti escluso $a_i$.

Dunque si può dimostrare il seguente teorema.
```ad-Teorema
title: Teorema 3.2
I meccanismi VCG sono truthful per problemi utilitari.
```
```ad-Dimostrazione
Fissato il tipo $t_i$ di un'agente $a_i$ e fissato il vettore dei tipi riportati dagli altri agenti $r_{-i}$, si applica l'algoritmo $g(\cdot)$ per calcolare due outcomes: uno in cui $a_i$ dichiara $r_i = t_i$, cioè dice la verità, e uno in cui $a_i$ dichiara $r_i \neq t_i$, cioè non dice la verità.
- Quando $r_i = t_i$ ($a_i$ **dice la verità**), vale $$ x = g(t_{i},r_{-i}) = g(\hat{r})$$ In questo caso, l'utilità per $a_i$ è pari a $$ \begin{align}
u_{i}(t_{i},x) &= p_{i}(x) - v_{i}(t_{i},x)\\
&= \left(h_{i}(r_{-i}) - \sum_{j\neq i} v_{j}(r_{j},x) \right) - v_{i}(r_{i},x) \\
 &= h_{i}(r_{-i}) - \sum_{j} v_{j}(\hat{r}_{j},x)
\end{align}  $$
- Quando $r_i \neq t_i$ ($a_i$ **mente**), vale $$ x' = g(r_{i},r_{-i}) $$ In questo caso, l'utilità per $a_i$ è pari a $$ \begin{align}
u_{i}(t_{i},x') &= p_{i}(x') - v_{i}(t_{i},x')\\
&= \left(h_{i}(r_{-i}) - \sum_{j\neq i} v_{j}(r_{j},x') \right) - v_{i}(r_{i},x') \\
 &= h_{i}(r_{-i}) - \sum_{j} v_{j}(\hat{r}_{j},x')
\end{align}  $$
A questo punto si osserva che, per come è definito, l'algoritmo $g(\cdot)$ calcola l'outcome $x$ che calcola la soluzione ottima rispetto al vettore dei tipi riportati $\hat{r}=(r_{-i},t_{i})$. Dunque $x$ è l'outcome per cui la quantità  $\sum_{i}v_{i}(\hat{r},x)$ è minima.
Allora vale che
$$\sum_{j}v_{j}(\hat{r}_{j},x) \leqslant \sum_{j}v_{j}(\hat{r}_{j},x')$$
ma dato che
$$
\begin{align}
\sum_{j}v_{j}(\hat{r}_{j},x) &= h_{i}(r_{-i}) - u_{i}(t_{i},x) \\
\sum_{j}v_{j}(\hat{r}_{j},x') &= h_{i}(r_{-i}) - u_{i}(t_{i},x')
\end{align}
$$
si ottiene
$$
h_{i}(r_{-i}) - u_{i}(t_{i},x) \leqslant h_{i}(r_{-i}) - u_{i}(t_{i},x') \implies
u_{i}(t_{i},x) \geqslant u_{i}(t_{i},x')
$$
cioè l'utilità dell'agente $a_i$ è migliore quando dichiara la verità $t_i$.
```

### Come definire $h_i(r_{-i})$
Si osserva che se tutti gli agenti sono *costretti* a partecipare ad un gioco, allora la funzione $h_i(r_{-i})$ può essere definita in modo arbitrario. In generale però questa non è una buona assunzione, dunque è necessario definire una funzione $h_i(r_{-i})$ che sia *ragionevole*: non tutte le funzioni hanno senso.

Ad esempio, si consideri l'asta di Vickery in cui $h_i(r_i)=0$ per ogni $i$.
Sia $a_i$ l'agente che ha vinto l'asta, allora il suo utile vale
$$
\begin{align}
u_{i}(t_{i},x) &= p_{i}(r)-v_{i}(t_{i},x) \\
 &= h_{i}(r_{-i}) - \sum_{j \neq i} v_{j}(r_{j},x) -v_{i}(t_{i},x)\\
&= \underbrace{-\sum_{j \neq i} v_{j}(r_{j},x)}_{0 \text{ perché $a_{i}$ ha vinto}} -v_{i}(t_{i},x) \\
&= - v_{i}(t_{i},x) = - t_{i}
\end{align}
$$
dato che le valutazioni degli agenti sono quantità maggiori o uguali a zero, allora $a_i$, nonostante vince l'asta, ha utilità negativa dato che non viene pagato dal meccanismo.
Similmente, sia $a_i$ un'agente che non ha vinto l'asta e sia $a_h$ il vincitore.
L'utile di $a_i$ è pari a
$$
\begin{align}
u_{i}(t_{i},x) &= p_{i}(r)-v_{i}(t_{i},x) \\
 &= h_{i}(r_{-i}) - \sum_{j \neq i} v_{j}(r_{j},x) -v_{i}(t_{i},x)\\
&= -\sum_{j \neq i} v_{j}(r_{j},x) - \underbrace{v_{i}(t_{i},x)}_{0 \text{ perché $a_{i}$ non ha vinto}} \\
&= -\sum_{j \neq i} v_{j}(r_{j},x) \\
&= -v_{h}(r_{h},x)
\end{align}
$$
dato che le valutazioni degli agenti sono quantità maggiori o uguali a zero, allora $a_i$, nonostante perde l'asta e dunque non deve niente al meccanismo, ha utilità negativa perché la funzione di pagamento del meccanismo *vuole* i soldi dagli agenti perdenti!
```ad-Osservazione
title: Osservazione 3.1
Il meccanismo è **truthful** anche con $h_i(r_{-i})=0$.
```
#### Pagamento di clarke
Uno schema di pagamento coerente per l'asta di Vickrey deve garantire una utilità maggiore di zero per il vincitore e uguale a zero per tutti gli altri agenti partecipanti. Per ottenere tali garanzie, si utilizza un particolare schema di pagamento detto **clarke payment**, dove
$$ h_{i}(r_{-i}) = \sum_{j \neq i}v_{j}(r_{j},g(r_{-i})) $$
dove $g(r_{-i})$ è un outcome in cui il player $i$ non partecipa. Allora il pagamento per ogni agente è pari a
$$ p_{i}(x) = \underbrace{\sum_{j \neq i}v_{j}(r_{j}g(r_{-i}))}_{{(1)}} - \underbrace{\sum_{j \neq i}v_{j}(r_{j},g(r))}_{(2)} $$
si può dimostrare che con questo schema di pagamento, tutte le utilità degli agenti sono non negative. Dunque, tutti gli agenti sono interessati a partecipare all'asta.

In particolare, usando questo schema di pagamento nell'asta di Vickrey, il vincitore $a_i$ viene pagato di una quantità pari alla seconda migliore offerta, dato che:
- $(1)$ è proprio il valore della seconda migliore offerta;
-  $(2)$ è pari a zero perché ogni agente $a_j$ con $j \neq i$ perde l'asta, quindi ha valutazione zero.
mentre ogni agente $a_j$ con $j \neq i$ viene pagato zero, dato che:
- $(1)$ è il valore della migliore offerta, cioè del vincitore $a_i$;
- $(2)$ è sempre il valore della migliore offerta.

Si esamina ora una applicazione del meccanismo VCG ad un problema utilitario su grafo: il problema del cammino minimo tra due nodi in un grafo con archi privati.
## Shortest Path con Selfish-Edges

Si può associare a questo problema il seguente scenario: in un grafo gli archi sono controllati da *agenti egoistici*, per cui solo l'agente conosce il peso associato al proprio arco. Dati due nodi del grafo, l'obiettivo è quello di calcolare un cammino minimo tra i due nodi rispetto ai pesi *reali*. A tale scopo, si deve progettare un *meccanismo truthful*, che con un pagamento opportuno incentiva gli agenti a dire la *verità*.
Intuitivamente, il problema è cosi descritto:
- Input composto da un grafo $G=(V,E)$ in cui ogni arco $e \in E$ è un agente egoistico; inoltre sono dati due nodi $s,t \in V$ di *sorgente* e di *destinazione*, rispettivamente. Il **tipo** di un agente è il costo di utilizzo dell'arco: questa è l'informazione privata dell'agente, che non viene necessariamente riportata. La **valutazione** di un agente è uguale al valore del tipo.
- La **social-choice function** è un *vero* cammino minimo in $G=(V,E,\tau)$ tra $s$ e $t$, dove $\tau$ è il vettore dei tipi degli agenti (aka il peso reale di ogni arco).
Più formalmente:
- Le **soluzioni ammissibili**, cioè i possibili outcome del gioco, è l'insieme $\mathcal{F}$ dei cammini minimi tra $s$ e $t$;
- Il **tipo** dell'agente $e$ è il valore $\tau_e$ che rappresenta il peso *reale* dell'arco $e$ in $G$. Intuitivamente, $\tau_e$ è il costo che l'agente sostiene per utilizzare $e$. In generale, il tipo riportato $r_e$ dall'agente $e$ può essere diverso da $\tau_e$.
- Dato un outcome $P \in \mathcal{F}$, la **valutazione** dell'agente $e$ nel cammino $P$ è data da $$ v_{e}(\tau_{e},P) = \begin{cases}
\tau_{e}, \quad e \in P \\
	0, \quad \text{altrimenti}
\end{cases}$$ cioè l'agente $e$ paga al sistema $\tau_e$ se il suo arco fa parte del cammino minimo, zero altrimenti.
- Dato il vettore dei tipi riportati $\tau$, o i pesi *reali* degli archi, la social-choice function calcola lo **shortest path** in $G=(V,E,\tau)$ tra i due nodi $s,t$.
## Un meccanismo truthful per il problema
Si osserva che il problema presentato è **utilitario**. Infatti, calcolare il cammino minimo significa calcolare il cammino di costo minimo. Dato un cammino $P$, la sua vera lunghezza è data dalla somma dei pesi (tipi reali) degli archi presenti nel cammino, che per come è definita la funzione di valutazione, è proprio pari alla somma delle valutazioni degli agenti nell'outcome $P$. In formule
$$ \sum_{e \in P}\tau_{e} = \sum_{e \in E}v_{e}(\tau_{e},P) $$
Allora, la social-choice function è pari a
$$ f(t) = \arg\min_{P \in \mathcal{F}} \sum_{e \in E} v_{e}(\tau_{e},P) $$
Dunque il problema è utilitario, per cui è possibile utilizzare i meccanismi VCG per risolvere il problema in modo truthful.
### Meccanismo VCG
Si ricorda che un meccanismo $M = \langle g(r),p(r) \rangle$ è VCG se è della forma:
- $g(r) = x^* = \arg\min_{x \in F} \sum_{j} v_{j}(r_{j},x)$
- $p_{i}(x) = \sum_{j \neq i}v_{j}(r_{j},g(r_{-i})) - \sum_{j\neq i} v_{j}(r_{j},x^*)$
Nel caso di questo problema dunque:
- L'algoritmo $g(r)$ calcola un cammino minimo $P_G(s,t)$ in $G=(V,E,r)$ pesato con i tipi riportati;
- Fissato un outcome $g(r) = P_G(s,t)$, l'agente $e \in E$ viene pagato una quantità pari a $$p_{e}(r) = \sum_{j \neq e}v_{j}(r_{j},g(r_{-e})) - \sum_{j\neq e} v_{j}(r_{j},P_{G}(s,t))$$ cioè $$p_{e}(r) = \begin{cases}
d_{G-e}(s,t) - (d_{G}(s,t)- r_{e}), \quad &e \in P_{G}(s,t) \\
0, \quad &\text{altrimenti}
\end{cases}$$
Dunque per calcolare il pagamento ad ogni arco $e \in P_G(s,t)$, è necessario calcolare il **cammino minimo di rimpiazzo** $P_{G-e}(s,t)$ nel grafo $G-e = (V, E \setminus \{e\},r_{-e})$, cioè il cammino minimo da $s$ a $t$ che non contiene l'arco $e$.
#### Esempio
Si consideri il seguente grafo
![[agt_img02.png]]
Per calcolare il pagamento di $e \in P_G(s,t)$, si deve prima calcolare li **cammino minimo di rimpiazzo** per $e$
![[agt_img03.png]]
Dunque vale
$$P_{e} = d_{G-e}(s,t) - (d_{G}(s,t) - r_{e}) = 12 - (11 - 2) = 3$$
### Analisi della complessità
Sia $n = |V|$ e $m = |E|$ e sia $d_G(s,t)$ la **distanza** in $G$ da $s$ a $t$.
Inoltre, si lavora sotto l'ipotesi che nel grafo i nodi $s$ e $t$ sono **2-edge connessi**, cioè esistono in $G$ almeno due cammini tra $s$ e $t$ che sono disgiunti sugli archi. Dunque, per ogni arco $e$ del  cammino $P_G(s,t)$ che viene rimosso esiste almeno un cammino alternativo in $G-e$.
Se così non fosse, allora ci sarebbe almeno un arco $e$ in $P_G(s,t)$ che è un **ponte**, cioè un arco che se rimosso spezza $G$ in due componenti $C_1,C_2$ con $s \in C_1$ e $t \in C_2$. In questo caso, non esiste un cammino minimo da $s$ a $t$ che *non* passa per $e$, cioè $d_{G-e}(s,t) = +\infty$. In altre parole, il possessore di questo arco ponte *tiene in pugno* il sistema e può chiedere una qualsiasi cifra.



Per calcolare i pagamenti si applica per ogni arco $e \in P_G(s,t)$ l'algoritmo di **Dijkstra** al grafo $G-e$. La complessità con questo approccio è pari a
$$ \underbrace{O(n)}_{\text{nro di archi in }P_{G}(s,t)} \cdot \underbrace{O(m+n\log{n})}_{\text{Calcolo $P_{G-e}(s,t)$ fissato $e \in P_{G}(s,t$)}} = O(nm+n^2\log{n}) $$
In realtà vale un risultato ancora migliore, in cui si dimostra che il problema è risolvibile in tempo $O(m+m\log{n})$.
# Meccanismi One-Parameter (OP)
Si definisce ora una seconda classe di meccanismi truthful, utilizzati per risolvere una categoria di problemi detta **one-parameter (OP)**.
```ad-Definizione
title: Definizione 3.1
Un problema è **one-parameter** se:
1. L'informazione posseduta da ogni agente $a_i$ è un **singolo parametro** $t_i \in \mathbb{R}$;
2. La valutazione di $a_i$ ha la forma $$ v_i(t_i,o) = t_i \cdot w_i(o) $$ dove $w_i(o)$ è una quantità detta **carico di lavoro** per $a_i$ nell'outcome $o$.
```
Per questi problemi esiste una classe di meccanismi truthful detta **meccanismi one-parameter**. A differenza dei meccanismi VCG, dove le valutazioni (costi) e i tipi sono *arbitrari* ma possono essere applicati solo a problemi utilitari (aka sono vincolati a problemi con funzione di scelta sociale ben precisa), i meccanismi OP ammettono una funzione di *scelta sociale arbitraria* ma i tipi devono essere a singolo parametro e le valutazioni vincolate.
Si descrivono ora le caratteristiche dei meccanismi one-parameter.
## Meccanismo truthful se e solo se $g(\cdot)$ monotona
Si introduce una definizione utile per dimostrare una condizione necessaria per la truthfullness di un meccanismo per un problema OP.
```ad-Definizione
title: Definizione 3.2
Un algoritmo $g(\cdot)$ per un problema OP di minimizzazione è **monotono** se per ogni agente $a_i$, il carico di lavoro $w_i(g(r_{-i},r_i))$ è **non crescente** rispetto a $r_i$ per tutti gli $r_{-i} = (r_1,\dots,r_{i-i},r_{i+i},\dots,r_n)$.
```
dunque, vale il seguente teorema.

```ad-Teorema
title: Teorema 3.3 (CN truthfulness)
Una *condizione necessaria* affinché un meccanismo $M=\langle g(r), p(r) \rangle$ per un problema OP sia *veritiero* è che $g(r)$ sia monotono.
```
```ad-Dimostrazione
Si suppone per assurdo che l'algoritmo $g(\cdot)$ non sia monotono e si mostra che nessuno schema di pagamento può rendere $M$ veritiero.
Se $g(\cdot)$ non è monotono, esiste un agente $a_i$ e un vettore $r_{-i}$ tale che $w_i(r_{-i},r_i)$ è non *non crescente* rispetto a $r_i$. Graficamente,
![[agt_img05.png]]
Si considerano i quattro casi seguenti:
1. $t_i = x, r_i = x \implies v_i(t_i,o) = x \cdot w_i(g(r_{-i},x))$
2. $t_i = y, r_i = y \implies v_i(t_i,o) = y \cdot w_i(g(r_{-i},y))$
3. $t_i = x, r_i = y \implies v_i(t_i,o) = x \cdot w_i(g(r_{-i},y)) \implies a_i \text{ aumenta il suo costo di } A$
4. $t_i = y, r_i = x \implies v_i(t_i,o) = y \cdot w_i(g(r_{-i},x)) \implies a_i \text{ ha un risparmio di } A+k$
Dove $A$ e $k$ sono le due aree in figura:
![[agt_img06.png]]
Sia $\Delta p = p_i(r_{-i},y) - p_i(r_{-i},x)$, se $M$ è veritiero deve essere:
- $\Delta p \leqslant A$: se cosi non fosse, nel caso in cui $t_i = x$ all'agente $a_i$ converrebbe mentire e dichiarare $r_{i}=y$ in quanto l'aumento di pagamento ($> A$) supererebbe l'aumento di costo ($A$), dunque la sua utilità aumenta.
- $\Delta p \geqslant A+k$: se cosi non fosse, nel caso in cui $t_i = y$ all'agente $a_i$ converrebbe mentire e dichiarare $r_{i}=x$ in quanto l'aumento di pagamento nel dichiarare $y$ non supererebbe il risparmio che si ottiene riportando $x$ , dunque la sua utilità aumenta.
Ma $k$ è un valore strettamente positivo, dunque non possono valere le due condizioni contemporaneamente.
 
```

## Meccanismo one-parameter
Dato un problema one-parameter, un meccanismo per risolvere il problema è $M = \langle g(r),p(r) \rangle$ dove $g(r)$ è un qualsiasi algoritmo **monotono** (condizione necessaria per la truthfullness) e, fissato un outcome $o$, il pagamento per l'agente $a_i$ quando dichiara $r_i$ è pari a
$$
p_{i}(r) = h_{i}(r_{-i}) + r_{i}w_{i}(r) - \int_{0}^{r_{i}} w_{i}(r_{-i},z)  \, dz
$$ 
con $h_i(r_{-i})$ funzione arbitraria indipendente da $r_i$.
Si osserva che il meccanismo presentato è **truthfull**, infatti vale il teorema seguente.

```ad-Teorema
title: Teorema 3.4
Un meccanismo OP, per un problema OP, è veritierio.
```
```ad-Dimostrazione
Per dimostrare il teorema, bisogna mostrare che l'utilità di un agente $a_i$ può *solo decrescere* se $a_i$ dichiara il falso.
Siano $r_{-i}$ le dichiarazioni degli altri agenti fissate. Il pagamento fornito ad $a_i$ quando dichiara $r_i$ è pari a
$$
p_{i}(g(r)) = h_{i}(r_{-i}) + r_{i}w_{i}(g(r)) - \int_{0}^{r_{i}} w_{i}(g(r_{-i},z))  \, dz
$$ 
dato che $h_i(r_{-i})$ non dipende dalla scelta $r_i$ dell'agente $a_i$, si ignora questo fattore ponendo $h_i(r_{-i}) = 0$.
L'utilità dell'agente $a_i$ quando dichiara la verità $t_i$ è pari a
$$
\begin{align}
u_{i}(t_{i},g(r_{-i},t_{i})) &= p_{i}(g(r_{-i},t_{i})) - v_{i}(t_{i},g(r_{-i},t_{i})) \\
&= \left(t_{i} \cdot w_{i}(g(r_{-i},t_{i})) - \int_{0}^{t_{i}} w_{i}(g(r_{-i},z))  \, dz\right) - t_{i} \cdot w_{i}(g(r_{-i},t_{i})) \\
&= - \int_{0}^{t_{i}} w_{i}(g(r_{-i},z))  \, dz
\end{align}
$$
Graficamente,
![[agt_img07.png]]
Se l'agente $a_i$ mente, allora dichiara un valore $r_i \neq t_i$: sia questo valore denotato con $x = r_i$. In questo caso, la valutazione è pari a
$$ 
v_{i}(t_{i},g(r_{-i},x)) = t_{i} \cdot w_{i}(g(r_{-i},x)) = C
$$
il pagamento ad $a_i$  è pari a
$$
p_{i}(g(r_{-i},x)) = x \cdot w_{i}(g(r_{-i},x)) -
\int_{0}^{x} w_{i}(g(r_{-i},z))  \, dz = P
$$
dunque, l'utilità di $a_i$ è pari a
$$
\begin{align}
U &= u_{i}(t_{i},g(r_{-i},x))  \\
&= p_{i}(g(r_{-i},x)) - v_{i}(t_{i},g(r_{-i},x)) \\
&= \left(x \cdot w_{i}(g(r_{-i},x)) - \int_{0}^{x} w_{i}(g(r_{-i},z))  \, dz\right) - t_{i} \cdot w_{i}(g(r_{-i},x)) \\
&= (x - t_{i}) \cdot w_{i}(g(r_{-i},x)) - \int_{0}^{t_{i}} w_{i}(g(r_{-i},z))  \, dz \\
&= P-C
\end{align}
$$

Allora l'agente $a_i$, se mente, dichiara o $x > t_i$ o $x < t_i$:
- se $x > t_i$, graficamente vale, <br>![[agt_img08.png]] cioè l'area sottesa al grafico presa in considerazione è più grande di una quantità $G$. Dato che l'area definita dall'integrale viene sottratta nel calcolo dell'utilità, si conclude che $a_i$ sta perdendo $G$ di utile, dunque non ha convenienza a mentire.
- se $x < t_i$, graficamente vale, <br>![[agt_img11.png]] cioè l'area sottesa al grafico presa in considerazione è più grande di una quantità $G$. Come nel caso precedente, l'area viene sottratta nel calcolo dell'utilità. Si conclude che $a_i$ sta perdendo $H$ di utile, dunque non ha convenienza a mentire.
Allora in entrambi i casi, l'agente $a_i$ non ha convenienza a mentire, cioè a dichiarare un tipo diverso da $t_i$.
```

### Sulla funzione $h_i(r_{-i})$
Per ottenere la **volontaria partecipazione (VP)** ad un gioco, il meccanismo deve garantire che l'utilità di un qualsiasi agente che dichiara il vero ha sempre un utile non negativo.
Per ottenere VP nei meccanismi one-parameter, è sufficiente scegliere la costante
$$
h_{i}(r_{-i}) = \int_{0}^{\infty} w_{i}(g(r_{-i},z)) \, dz
$$
in questo modo, il pagamento diventa
$$
p_{i}(g(r)) = r_{i}w_{i}(g(r)) + \int_{0}^{\infty} w_{i}(g(r_{-i},z)) \, dz
$$
e l'utilità di un agente che dichiara il vero diventa
$$
u_{i}(t_{i},g(r)) = \int_{0}^{\infty} w_{i}(g(r_{-i},z)) \, dz \geqslant 0
$$
## Meccanismi OP: il problema Shortest Path Tree con Selfish-Edges
Come esempio applicativo dei meccanismi one-parameter, si studia ora la versione *egoistica* del problema di calcolare l'albero dei cammini minimi di un grafo.
### SPT non cooperativo
Si considera il seguente problema di *broadcasting*. Data una rete $G=(V,E)$, una sorgente $s \in V$ vuole spedire un messaggio ai restati nodi in $V \setminus \{s\}$. Ad ogni arco della rete $e \in E$ è associato un agente egoistico, il cui dato privato è il *tempo di attraversamento* del messaggio attraverso il collegamento che controlla. L'obiettivo del problema è quello di minimizzare il tempo di consegna di ogni messaggio. 
Formalmente:
- L'input è composto da un grafo $G=(V,E)$ biconnesso sugli archi, in cui ogni arco corrisponde in modo biunivoco ad un insieme di agenti egoisti, e da un nodo $s \in V$;
- Il **tipo** di un agente è un valore strettamente positivo che indica il costo di utilizzo dell'arco che gestisce;
- L'insieme dei possibili outcome $\mathcal{F}$ è l'insieme degli alberi ricoprenti radicati in $s$;
- Fissato un vettore dei tipi $t$, la funzione di scelta sociale è definita come $$ f(t) = \arg\min_{T \in \mathcal{F}} \sum_{v \in V} d_{T}(s,v) = \arg\min_{T \in \mathcal{F}} \sum_{e \in E(T)} t_{e} \cdot \mid\mid e \mid\mid$$ dove $\mid\mid e\mid\mid$ è la **molteplicità** dell'arco $e$ nell'albero (soluzione) $T$, intesa come il il numero di cammini ai quali appartiene in $T$. 
```ad-note
title: Notazione
$E(T)$ indica gli archi di $G$ che sono presenti nell'albero ricoprente $T$
```
Per classificare il problema come utilitario o meno, è necessario definire la funzione di valutazione: si osserva che fissato un albero (outcome) $T \in \mathcal{F}$, la funzione di valutazione dipende da come vengono trasmessi i messaggi nella rete $T$.
Si suppone di utilizzare un protocollo **multicast**, nel quale una sola copia del messaggio viene spedita su ogni arco: eventualmente, il messaggio viene duplicato dai nodi che devono ritrasmettere il messaggio su più archi incidenti a loro.
![[agt_img09.png]]
Fissato un outcome $T \in \mathcal{F}$, la funzione di **valutazione** dell'agente associato all'arco $e$ $a_e$ è definita come
$$
v_{e}(t_{e},T) = \begin{cases}
t_{e}, \quad &e \in E(T) \\
0, \quad &\text{altrimenti}
\end{cases}
$$
dunque il problema è **non utilitario**, in quanto
$$
f(t) \neq \arg\min_{T \in \mathcal{F}} \sum_{e \in E} v_{e}(t_{e},T)
$$
Il problema però è di tipo **one-parameter**, in quanto il tipo di ogni agente $a_e$ è un singolo parametro $t_e \in \mathbb{R}$, mentre la valutazione sotto ipotesi di trasferimento multicast è data da
$$
v_{e}(t_{e},T) = t_{e} \cdot w_{e}(T)
$$
con
$$
w_{e}(T) = \begin{cases}
1, \quad &e \in E(T) \\
0, \quad &\text{altrimenti}
\end{cases}
$$
Dunque si può utilizzare un meccanismo one-parameter per risolvere il problema.
### Progettazione del meccanismo
Un meccanismo one-parameter per l'SPT non utilitario è il meccanismo $M_{\text{SPT}}=\langle g(r),p(r) \rangle$ con:
- $g(r)$ algoritmo che dato il grafo e le dichiarazioni, calcola uno Shortest Path Tree $S_G(s)$ del grafo $G=(V,E,r)$ pesato con i pesi riportati $r$ utilizzando l'**algoritmo di Dijkstra**.
- Il pagamento per ogni agente $a_e$ che controlla l'arco $e \in E$ è dato da $$p_{e}(r) = r_{e} \cdot w_{e}(g(r)) + \int_{r_{e}}^{\infty} w_{e}(g(r_{-e},z)) \, dz$$ così da garantire la **partecipazione volontaria**.
Si osserva che il meccanismo $M_{\text{SPT}}$ è **truthful**, in quanto l'algoritmo di Dijkstra è monotono. Fissato $r_{-e}$ il carico di lavoro per un agente $a_e$ ha sempre la forma
![[agt_img10.png]]
dove il valore $\theta_e$ per cui il carico di lavoro passa da uno a zero è detta **soglia**: se $a_e$ dichiara al più $\theta_e$, allora l'arco $e$ è selezionato; se $a_e$ dichiara più di $\theta_e$, allora l'arco $e$ non è selezionato.
Questo valore di soglia è fondamentale nel calcolo dei pagamenti, infatti
- Se $e$ non viene selezionato, cioè $e \not\in E(T)$ ,allora $$p_{e}(r) = r_{e} \cdot w_{e}(g(r)) + \int_{r_{e}}^{\infty} w_{e}(g(r_{-e},z)) \, dz = 0+0=0$$
- Se $e$ viene selezionato, cioè $e \in E(T)$ ,allora $$p_{e}(r) = r_{e} \cdot w_{e}(g(r)) + \int_{r_{e}}^{\infty} w_{e}(g(r_{-e},z)) \, dz = r_{e}+(\theta_{e}-r_{e})=\theta_{e}$$
### Sul calcolo delle soglie
Per il calcolo dei valori soglia, si consideri $e=(u,v)$ un arco in $S_G(s)$ con $u$ più vicino a $s$ che $v$. Allora $e$ resta in $S_G(s)$ fintantoché si utilizza l'arco $e$ per raggiungere $v$. In particolare, $e$ non viene più scelto nel momento in cui $r_e$ è tale che
$$
d_{G}(s,u) + r_{e} > d_{G-e}(s,v)
$$
quindi il valore soglia per l'arco $e$ è tale che
$$
d_{G}(s,u) + \theta_{e} = d_{G-e}(s,v) \iff \theta_{e} = d_{G-e}(s,v) - d_{G}(s,u)
$$
### Una soluzione banale: risultati di complessità
Una soluzione banale per il problema dello SPT non cooperativo consiste nel calcolare $S_G(s)$ utilizzando l'algoritmo di Dijkstra sul grafo con i pesi riportati, per poi calcolare, per ogni $e = (u,v)$, $d_{G-e}(s,v)$ applicando Dijkstra al grafo $G-e$ in modo tale da calcolare il pagamento per ogni $a_{e}$.
La complessità di tale soluzione è pari a
$$
\underbrace{O(n)}_{\text{nro di archi in }P_{G}(s,t)} \cdot \underbrace{O(m+n\log{n})}_{\text{Calcolo $d_{G-e}(s,v)$ fissato $e \in S_{G}(s)$}} = O(nm+n^2\log{n}) 
$$
Infine, vale il seguente teorema.
```ad-Teorema
title: Teorema 3.5
$M_{\text{SPT}}$ è calcolabile in tempo $O(m+n\log{n})$.
```
## Caso speciale dei problemi OP: Binary Demand
Il problema dello SPT non cooperativo appena affrontato appartiene ad una classe particolare di problemi one-parameter, nota con il nome di problemi **binary demand (BD)**.
Un problema è BD se
1. L'informazione posseduta da ogni agente $a_i$ è un **singolo parametro** $t_i \in \mathcal{R}$;
2. La **valutazione** di $a_i$ ha la forma $$v_{i}(t_{i},o) = t_{i} \cdot w_{i}(o)$$ dove $w_i(o) \in \{0,1\}$ è il **carico di lavoro** per $a_i$ nell'outcome $o$. Quando $w_i(o)=1$ diremo che l'agente $a_i$ è **selezionato** nell'outcome $o$.
Si osserva che per problemi BD, l'algoritmo $g(\cdot)$ deve avere la caratteristica di monotonia vista nel meccanismo per il problema SPT, ossia
```ad-Definizione
title: Definizione 3.3
Un algoritmo $g(\cdot)$ per un problema BD di minimizzazione è **monotono** se per ogni agente $a_i$ e per tutti gli $r_{-i}=(r_1,\dots,r_{i-1},r_{i+1},\dots,r_n)$ la funzione $w_i(g(r_{-i},r_i))$ è della forma
![[agt_img10.png|500|center]]
Dove $\theta_i(r_{-i}) \in \mathcal{R}\cup\{+\infty\}$ è detto **valore soglia** e il **pagamento** per l'agente $a_i$ diventa
$$p_i(r) = \theta_i(r_{-i})$$
```

Si discute ora una generalizzazione del problema dell'asta a singolo oggetto: il problema dell'asta combinatoria.
## Combinatorial Auction Game
Nell'asta combinatoria si hanno un insieme di oggetti da vendere, e ogni agente che partecipa all'asta ne vuole un certo sottoinsieme. Il *tipo* (informazione privata) dell'agente $a_i$ è un valore $t_i$ che intuitivamente rappresenta il valore massimo che egli è disposto a pagare per ottenere il proprio bundle di oggetti. Ogni oggetto può essere assegnato ad un solo agente.
Il meccanismo deve decidere come allocare gli oggetti (quindi l'*outcome*) e quanto dovranno pagare gli agenti. Un outcome ammissibile è l'insieme $W \in \mathcal{F}$ dei vincitori dell'asta: un sottoinsieme di agenti che sono accontentati e compatibili tra loro, cioè non hanno oggetti desiderati in comune.
L'obiettivo del meccanismo è quello di ottenere il miglior outcome possibile, cioè quello il cui valore totale è massimo; il valore di un outcome è pari alla somma dei tipi dei vincitori.
Infine, l'obiettivo di ogni agente $a_{i}$ è quello di massimizzare la propria utilità pari a $u_{i} = t_{i} - p$, dove $p$ è il prezzo pagato da $a_i$.

Formalmente,
- In input si hanno un insieme di $n$ agenti e $m$ oggetti indivisibili;
- Ciascun agente $a_i$ vuole un sottoinsieme $S_i$ degli oggetti, e il **tipo** dell'agente è un numero $t_i \in \mathbb{R}$, che rappresenta il valore complessivo degli oggetti in $S_i$ secondo $a_i$.
- Un possibile *outcome* è un sottoinsieme $W \subseteq \{1, \dots ,n\}$ tale che $$\forall i,j \in W: i \neq j, S_{i} \cap S_{j} = \emptyset$$
- Fissato un outcome $W \in \mathcal{F}$, la **valutazione** di $a_i$ per questo è pari a $$v(t_{i},W) = \begin{cases}
t_{i}, \quad i \in W \\
0, \quad \text{altrmenti}
\end{cases}$$
- La **social-choice function** è definita come segue $$ f(t) := \arg\max_{W \in \mathcal{F}} \sum_{i \in W} t_{i}$$ cioè si devono allocare gli oggetti (equivalentemente, scegliere gli agenti da soddisfare) in modo da massimizzare la somma dei tipi reali dei vincitori.

Essendo un gioco, si ricorda che:
- ogni agente (o giocatore) $a_i$ è egoista;
- solo l'agente $a_i$ conosce il proprio tipo $t_i$, mentre $S_i$ è un'informazione pubblica;
- si vuole massimizzare la somma dei tipi dei vincitori rispetto ai tipi reali degli agenti;
- Il meccanismo deve:
	- Chiedere ad ogni agente il tipo (riportato) $v_i$;
	- Calcolare un outcome sui tipi riportati usando un algoritmo $g(\cdot)$;
	- Prendere un pagamento $p_i$ dagli agenti dato da una funzione di pagamento $p(\cdot)$.
 
Non resta che progettare un meccanismo truthful per il problema.
### Meccanismo VCG
Per come è definito il valore reale di un outcome $W$ ammissibile, si ha che
$$ \sum_{i \in W} t_{i} = \sum_{i}v_{i}(t_{i},W) $$
dunque il problema è un **problema utilitario**, ed è possibile applicare il **[meccanismo VCG]** della forma $M= \langle g(r),p(x) \rangle$ con:
- l'algoritmo $g(r)$ definito come $$ g(r) = x^* := \arg\max_{x \in \mathcal{F}} \sum_{j} v_{j}(r_{j},x)$$
- la funzione di pagamento che paga ogni agente $a_i$ la quantità $$ p_{i}(r) := \sum_{j\neq i}v_{j}(r_{j},g(r_{-i})) - \sum_{j\neq i}v_{j}(r_{j},x^*) $$

### Calcolo dell'Outcome ottimo
Si osserva che per poter applicare questo meccanismo è necessario calcolare, dato il vettore $r$ dei tipi riportati, la soluzione ottima $g(r)$.
Questo risulta però essere un problema computazionalmente difficile, come mostra il seguente risultato.
```ad-Teorema
title: Teorema 3.6
Approssimare Combinatorial Auction con un fattore migliore di $m^{1/2-\epsilon}$ è un problema NP-hard per ogni $\epsilon > 0$
```
per dimostrare il teorema si effettua una **riduzione** a partire dal problema **maximum indipendent set**, per il quale vale il seguente risultato di impossibilità.
```ad-Teorema
title: Teorema 3.7 
Approssimare il problema IS con un fattore migliore di $n^{1-\epsilon}$ è un problema NP-hard per ogni $\epsilon > 0$
```

```ad-Dimostrazione
title: Dimostrazione (Teorema 3.6)
Si mostra una riduzione che trasforma istanze di IS in istanze di CA.
Si consideri una qualsiasi istanza $I = \langle G=(V,E),k \rangle$ del problema IS e si costruisca la relativa istanza $I'$ di CA nel modo seguente:
- ogni *arco* in $E$ diventa un oggetto in $I'$;
- ogni *nodo* in $V$ diventa un agente in $I'$  con:
	- $S_i := \{ \text{oggetti associati ad archi incidenti ad }i \}$;
	- $t_i := 1$.
 
 Allora l'istanza $I'$ di CA ha una soluzione di valore $\geqslant k$ se e soltanto se esiste un Independent Set di size $\geqslant k$ per l'istanza $I$.
 Da questo, segue che una soluzione di valore $k$ per $I'$ (CA) con, per qualche $\epsilon > 0$,
 $$ \frac{\text{OPT}_{\text{CA}}}{k} \leq m^{1/2-\epsilon} $$
 *implicherebbe* l'esistenza di una soluzione di valore $k$ per $I$ con
 $$ \frac{\text{OPT}_{\text{IS}}}{k} = \frac{\text{OPT}_{\text{CA}}}{k} \leq m^{1/2-\epsilon} \leq n^{1-2\epsilon} $$
 in quanto $m \leq n^2$ e quindi 
 $$m^{1/2-\epsilon} \leq (n^2)^{(1/2-\epsilon)} = n^{1-2\epsilon}$$
 
```

## Meccanismo One Parameter 
Anche se il meccanismo VCG non è computabile in tempo polinomiale, notiamo che il problema Combinatorial Auction è anche un **problema ad un parametro**. In particolare, è un problema di *binary demand* dato che:
- Il tipo di  $a_i$ è un **singolo parametro** $t_i \in \mathbb{R}$;
- La **valutazione** di $a_i$ dato un *outcome* $W \in \mathcal{F}$ è della forma $$v_{i}(t_{i},W) = w_{i}(W) \cdot t_{i}$$ dove $w_{i}(W) \in \{ 0,1 \}$ è detto **workload** di $a_i$ nell'outcome $W$ e vale $$w_{i}(W) = \begin{cases}
1, \quad i \in W \\
0, \quad \text{altimenti}
\end{cases}$$ cioè pari ad 1 quando l'agente è selezionato in $W$.

Dato che bisogna progettare un algoritmo *monotono* per applicare il meccanismo OP e il problema alla base della combinatorial auction è un problema di *massimizzazione*, si introduce la seguente definizione di algoritmo monotono.
```ad-Definizione
title: Definizione
Un algoritmo $g()$ per un problema Binary Demand di massimizzazione è **monotono** se per ogni agente $a_i$ e er ogni scelta fissata degli altri giocatori $r_{-i} = (r_1,\dots,r_{i-1},r_{i+1},\dots,r_{n})$, il *carico di lavoro* $w_i(g(r_{-i},r_i))$ è della forma

![[agt_img01.png|center|500]]

dove $\Theta_i(r_{-i}) \in \mathbb{R}\cup\{+\infty\}$ è detto **valore di soglia**
```

L'obiettivo ora è quello di definire un meccanismo tale che:
- L'algoritmo $g(\cdot)$ è **monotono** e calcola una *buona* soluzione: una soluzione **approssimata**;
- Sia $g(\cdot)$  che $p(\cdot)$ sono **calcolabili** in tempo polinomiale.
### Un algoritmo greedy $\sqrt{m}$-apx
Si definisce il seguente algoritmo *greedy* $g(\cdot)$ approssimante per calcolare l'outcome.
![[agt_img04.png]]
L'algoritmo calcola una soluzione ammissibile, dato che un agente $i$ è inserito nella soluzione se e soltanto se nessuno degli elementi del suo bundle è già stato assegnato a un qualche altro agente. Inoltre si vede facilmente che l'algoritmo calcola la soluzione in tempo polinomiale: l'operazione dominante è l'ordinamento dei valori riportati.

Dunque si deve dimostrare che l'algoritmo è appena descritto sia *monotono* e sia in grado di calcolare effettivamente una soluzione *$\sqrt{m}$-approssimante*. Inoltre, si deve mostrare che è possibile calcolare per ogni $a_i$ il pagamento $p_i$, equivalente al valore della soglia $\theta_{i}(r_{-i})$, in tempo polinomiale.

```ad-Lemma
title: Lemma 3.1
L'algoritmo $g(\cdot)$ è monotono.
```
```ad-Dimostrazione
Si deve dimostrare che, fissato un qualsiasi agente $a_i$ che viene selezionato da $g(\cdot)$ , $a_i$ viene comunque selezionato quando si aumenta il suo tipo riportato. Ma questo è vero per come funziona l'algoritmo, in quanto per come vengono ordinati i valori riportati
$$
\frac{v_{1}}{\sqrt{ |S_{1}|}} \geqslant\dots\geqslant \frac{v_{i}}{\sqrt{ |S_{i}|}} \geqslant\dots\geqslant \frac{v_{n}}{\sqrt{ |S_{n}|}} \geqslant
$$
se si aumenta il valore $v_i$, l'agente $a_i$  si può muovere solo a sinistra nell'ordinamento: viene selezionato ancora più velocemente di prima.
```

#### Calcolare i pagamenti
Dato che il problema è di tipo One Parameter Binary Demand, il pagamento per l'agente $a_i$ selezionato dall'algoritmo equivale al valore della soglia $\theta_{i}(r_{-i})$ che determina il valore riportato per cui $a_i$ viene inserito tra i vincitori.

Per calcolare il pagamento $p_i$ dell'agente $a_i$, si consideri l'ordinamento dell'algoritmo senza $i$
$$
\frac{v_{1}}{\sqrt{ |S_{1}|}} \geqslant\dots\geqslant \frac{v_{i}}{\sqrt{ |S_{i}|}} \geqslant\dots\geqslant \frac{v_{n}}{\sqrt{ |S_{n}|}}
$$
e si esegua l'algoritmo su tale ordinamento. Si consideri il primo indice $j$ a destra di $i$ tale che $S_i \cap S_j \neq \emptyset$: l'agente $a_i$ non è più vincitore non appena viene selezionato come nuovo vincitore un agente $a_j$ che rende $a_i$ non compatibile, se $\frac{v_{i}}{\sqrt{ S_{i} }} < \frac{v_{j}}{\sqrt{ S_{j} }}$.
Allora la soglia dipenderà dalla posizione di $a_{j}$ nell'ordinamento: l'agente $a_i$ si troverà nella posizione $j$, se esiste $j$, quando
$$
\frac{v_{i}}{\sqrt{ |S_{i}| }} = \frac{v_{j}}{\sqrt{ |S_{j}| }}
$$
da cui segue
$$
v_{i} = v_{j} \cdot \frac{\sqrt{ |S_{i}| }}{\sqrt{ |S_{j}| }}
$$
Allora, il pagamento $p_i$ dell'agente $a_i$ è pari a
$$
p_{i} = \theta_{i}(r_{-i}) = \begin{cases}
0, \quad j \text{ non esiste} \\
v_{j} \cdot \frac{\sqrt{ |S_{i}| }}{\sqrt{ |S_{j}| }}, \quad \text{altrimenti}
\end{cases}
$$
#### Risultato di approssimazione
```ad-Lemma
title: Lemma 3.2
Sia $\text{OPT}$ la soluzione ottima per l’asta combinatorica, e sia $W$ la   soluzione calcolata dall’algoritmo $\sqrt{m}$-approssimante, allora nel caso peggiore il valore dell’ottimo è più grande di al più di un fattore $\sqrt{m}$.
$$
\sum_{i\in\text{OPT}} v_i \leqslant \sqrt{m}\sum_{i\in W} v_i
$$
Dato il risultato di hardness, questa è la migliore approssimazione che si può ottenere.
```
```ad-Dimostrazione
Si vuole mostrare che, quando l'algoritmo greedy seleziona un agente $i$ che non è parte della soluzione ottima, il valore della soluzione non è troppo lontano rispetto all'ottimo.
Sia $W$ la soluzione calcolata dall'algoritmo e sia $\text{OPT}$ la soluzione ottima.
Per ogni $i \in W$ si definisce l'insieme
$$
\text{OPT}_{i} = \{ j \in \text{OPT}: j \geqslant i \land S_{j} \cap S_{i} \neq \emptyset \}
$$
cioè l'insieme degli agenti $a_{j}$ che fanno parte della soluzione ottima $\text{OPT}$ ma che non vengono selezionati in $W$ dall'algoritmo greedy perché seleziona l'agente $a_{i}$ come vincitore, rendendo incompatibili tutti gli $a_j$. In altre parole, si prendono tutti gli indici $j$ dell'ottimo che sono dopo $i$ nell'ordinamento e che l'algoritmo non ha selezionato perché ha inserito $a_{i}$ nella soluzione, ossia quelli che hanno degli elementi in comune con $a_{i}$.
Si osserva che
$$
\bigcup_{i \in W} \text{OPT}_{i} = \text{OPT}
$$
infatti, dato un $k \in \text{OPT}$, consideriamo l'indice $h$ più piccolo tale che $h \in W$ e $S_h \cap S_k \neq \emptyset$; necessariamente $k \geqslant h$ e quindi $k \in OPT_h$.
Allora si deve dimostrare che
$$
\sum_{j \in \text{OPT}_{i}} v_{j} \leqslant \sqrt{ m }\cdot v_{i}, \quad  \forall i \in W
$$
infatti, se così fosse, si può valutare la disequazione per ogni agente ottenendo
$$
\sum_{i \in W}\sum_{j \in \text{OPT}_{i}} v_{j} \leqslant \sum_{i \in W} \sqrt{ m }\cdot v_{i}
$$
e poiché in $\text{OPT}_i$ c'è almeno un'elemento dell'ottimo, la disequazione è una maggiorazione della soluzione ottima, per cui vale
$$
\sum_{i \in \text{OPT}} v_{i} \leqslant \sum_{i \in W}\sum_{j \in \text{OPT}_{i}} v_{j} \leqslant \sum_{i \in W} \sqrt{ m }\cdot v_{i} = \sqrt{ m }\cdot \sum_{i \in W} v_{i}
$$

Per come è stato definito l'algoritmo greedy, vale che per ogni $j \in \text{OPT}_{i}$
$$
\frac{v_{j}}{\sqrt{ |S_{j}| }} \leqslant \frac{v_{i}}{\sqrt{ |S_{i}| }} \iff v_{j} \leqslant v_{i} \cdot \frac{\sqrt{ |S_{j}| }}{\sqrt{ |S_{i}| }}
$$
dunque per ogni $i \in W$ vale
$$
\sum_{j \in \text{OPT}_{i}} v_{j} \leqslant \sum_{j \in \text{OPT}_{i}} v_{i} \cdot \frac{\sqrt{ |S_{j}| }}{\sqrt{ |S_{i}| }} =  \frac{v_{i}}{ \sqrt{ |S_{i}| }} \cdot \sum_{j \in \text{OPT}_{i}}\sqrt{ |S_{j}| }
$$
A questo punto si utilizza la disugualianza di Cauchy-Schwarz:

La disugualianza di Cauchy-Schwarz dice che dati due vettori $x = (x_i,\dots,x_n)$ e $y = (y_i,\dots,y_n)$, vale che
$$
\sum_{j=1}^n x_i y_j \leqslant \left( \sum_{j=1}^n x_j^2 \right)^{\frac{1}{2}} \cdot \left( \sum_{j=1}^n y_j^2 \right)^{\frac{1}{2}}
$$



Ponendo $n = |\text{OPT}_i|$ e per ogni $j = 1,\dots,n$: $x_j = 1$ e $y_j = \sqrt{|S_j|}$ si ottiene
$$
\sum_{j \in |\text{OPT}_{i}|} \sqrt{ |S_{j}| } \leqslant \sqrt{ |\text{OPT}_{i}| } \cdot \sqrt{ \sum_{j \in |\text{OPT}_{i}|} |S_{j}| }
$$
inoltre, vale che:
- $|\text{OPT}_i| \leqslant |S_i|$; se cosi non fosse, ci sarebbero due agenti interessati allo stesso oggetto; ma gli agenti in $\text{OPT}_i$ fanno parte della soluzione ottima, ossia una soluzione ammissibile, e due agenti in una soluzione ammissibile non possono avere un oggetto in comune.
- $\sum_{j \in |\text{OPT}_{i}|} |S_{j}| \leqslant m$, dato che gli agenti in $\text{OPT}_i$ fanno parte della soluzione ottima, ossia una soluzione ammissibile, e due agenti in una soluzione ammissibile non possono avere un oggetto in comune. Allora il numero di oggetti a cui sono interessati gli agenti in $\text{OPT}_i$ è al più pari al numero $m$ di oggetti totali nell'asta.
Dunque si ottiene
$$
\begin{align}
\sum_{j \in \text{OPT}_{i}} v_{j} &\leqslant \frac{v_{i}}{ \sqrt{ |S_{i}| }} \cdot \sum_{j \in \text{OPT}_{i}}\sqrt{ |S_{j}| } \\
&\leqslant \frac{v_{i}}{ \sqrt{ |S_{i}| }} \cdot \sqrt{ |\text{OPT}_{i}| } \cdot \sqrt{ \sum_{j \in |\text{OPT}_{i}|} |S_{j}| } \\
&\leqslant \frac{v_{i}}{ \sqrt{ |S_{i}| }} \cdot \sqrt{ |S_{i}| } \cdot \sqrt{ m }  \\
&\leqslant v_{i} \cdot \sqrt{ m }
\end{align}
$$
 
```

## Meccanismi senza pagamenti 

Con **Meccanismi senza pagamenti** si intendono quei meccanismi in cui il sistema non può pagare gli agenti per convincerli a riportare il tipo reale. Dunque un tale meccanismo è essenzialmente un algoritmo che date le preferenze degli agenti calcola un outcome.
Questa tipologia di meccanismi esiste e viene studiata perché esistono degli scenari in cui non si possono usare dei soldi per convincere gli agenti a cooperare. Ad esempio nei sistemi di voto o nei sistemi di donazione degli organi non si possono fare pagamenti per motivi legali.
In generale, progettare un meccanismo senza poter pagare gli agenti comporta grandi limitazioni: la ricerca ha mostrato forti risultati di impossibilità. Nonostante ciò, ci sono alcuni problemi specifici per cui esistono meccanismi truthful senza pagamenti.
## House allocation problem & Top Trading Cycle algorithm
### House allocation problem
L'**House allocation problem** è un problema in cui ci sono $N$ agenti, ognuno dei quali possiede inizialmente una casa. Inoltre, ogni agente $a_{i}$ ha un *ranking* di preferenze rispetto alle case degli altri, rappresentato da un ordinamento totale sulle $n$ case piuttosto che da valutazioni numeriche. Infine, non è necessario che un agente preferisca la propria casa rispetto alle altre. 

L'obiettivo del problema è trovare una nuova allocazione delle case agli agenti in modo tale da *renderli più felici* rispetto alle preferenze. Dunque si deve progettare un algoritmo che rialloca le case con tale obiettivo; tale algoritmo deve essere **truthful**, cioè al riallocamento delle case a tutti gli agenti deve convenire dichiarare le reali preferenze.

Per risolvere il problema ottenendo i due obiettivi, si utilizza un algoritmo detto **Top Trading Cycle (TTC)**.
### Top Trading Cycle Algorithm (TTC)
L'allocazione delle case avviene per fasi. In particolare, ad ogni iterazione:
- tutti gli agenti rimasti, cioè gli agenti a cui non è stata assegnata una nuova casa, partecipano all'allocazione con la propria casa iniziale;
- tutti gli agenti rimasti, *puntano* (indicano) la casa che preferiscono fra quelle rimaste (cioè quelle ancora non assegnate); questo passaggio genera un grafo diretto, dove ogni agente $a_i$ è un nodo e un arco diretto $(i,l)$ indica che la casa preferita di $a_i$ fra quelle rimaste è la casa (iniziale) dell'agente $a_l$. Si osserva che ogni agente deve puntare alla casa preferita, dunque ogni nodo ha un arco uscente: se la casa preferita dell'agente $a_i$ è proprio la sua casa iniziale, allora il grafo conterrà l'arco $(i,i)$.
- si guardano i cicli formati nel grafo diretto generato nell'iterazione precedente e si scambiano le case come suggerito da un ciclo.
- gli agenti che si sono scambiati le case vengono rimossi, cioè gli agenti facenti parte del ciclo selezionato.
Formalmente, l'algoritmo è così definito.
![|center|500](07-agt_img01.png)
Si osserva che, per costruzione, almeno un ciclo in $G$ deve esistere: tutti i nodi hanno un arco uscente, dunque l'attraversamento di una sequenza di archi uscenti deve alla fine ripetere un vertice.
Inoltre, dato che ogni nodo ha esattamente un arco uscente (grado uscente pari a uno), quando si formano più cicli questi sono sempre disgiunti sui vertici: se cosi non fosse, allora un nodo deve avere grado uscente pari a due il che non è possibile per costruzione.
Per queste caratteristiche, l'algoritmo termina sempre in $O(N)$ nel caso peggiore.
### Esempio TTC
Sia $N=\{ 1,2,3,4 \}$. Si supponga che ogni agente preferisce la casa dell'agente $1$ su tutte e che le seconde casa favorite dagli agenti 2,3 e 4 siano quelle possedute dagli agenti 3,4 e 2 rispettivamente (non si considera il resto delle preferenze per questo esempio). Come si vede in figura, alla prima iterazione c'è solo un self-loop con l'agente 1. Dunque 1 e la sua casa viene rimossa. Alla seconda iterazione, ogni agente riceve la casa preferita rispetto a quelle ancora disponibili.
![|center|500](07-agt_img02.png)
### Proprietà dell'algoritmo
Si osserva che, per ogni agente, l'algoritmo non assegna una casa peggiore rispetto a quella iniziale. Infatti, ogni agente partecipa all'allocazione con la propria casa, dunque se per un agente $a_i$ le case rimanenti nell'allocazione a cui partecipa non sono migliori della propria, può puntare alla propria casa: si crea un ciclo (self-loop su $a_i$). Dunque ad ogni agente $a_i$ nel caso peggiore viene assegnata la propria casa.
```ad-Lemma
title: Lemma 3.3
Sia $N_k$ l'insieme degli agenti rimossi all'iterazione $k$ dell'algoritmo TTC, cioè gli agenti nel ciclo individuato a tale iterazione.
Ogni agente in $N_k$ riceve la casa in posizione $k$ nel proprio ranking, ossia la casa preferita al di fuori di quelle già allocate agli agenti in $N_1 \cup \dots \cup N_{k-1}$. Inoltre, tale casa appartiene inizialmente ad un agente che viene servito all'iterazione $k$, cioè un agente in $N_k$.
```
Si guarda ora l'aspetto *game teoretico* dell'algoritmo (o meccanismo). In particolare, un agente potrebbe dichiarare un ranking differente in modo tale che gli venga assegnata una casa che rispetto al suo reale ranking è migliore. Con questo algoritmo ciò non avviene, infatti si dimostra che l'algoritmo TTC è truthful, in particolare si dimostra che è sempre strategia dominante dichiarare il ranking reale per ogni agente.

```ad-Teorema
title: Teorema 3.8
Usando l'algoritmo TTC per allocare le case, per ogni agente è strategia dominante dichiarare la verità.
```
```ad-Dimostrazione
Sia $a_i$ un agente fissato e siano fissate le dichiarazioni degli altri agenti.
Si assume che $a_i$ riporta la verità e sia $N_k$ l'insieme degli agenti che vengono rimossi all'iterazione $k$. Si supponga che $a_i \in N_j$, dunque ad $a_i$ viene assegnata una casa in $N_j$.
L'agente $a_i$ non può, cambiando il ranking, ottenere una casa migliore.
Infatti, per il lemma precedente, $a_i$ ottiene la casa migliore nel ranking rispetto a quelle che rimangono all'iterazione $j$. Inoltre, $a_i$ non può mentire per ottenere una casa migliore che è stata assegnata nelle iterazioni precedenti, infatti:
- per ogni $k = 1,\dots,j-1$ nessun agente $a_l \in N_k$ punta alla casa dell'agente $a_i$. Se cosi non fosse, allora $a_i$ apparterebbe allo stesso ciclo di $a_l$, dunque apparterrebbe ad $N_k$ invece che a $N_j$;
- nessun agente $a_l \in N_k$ punta alla casa dell'agente $a_i$ in iterazioni precedenti a $k$. Se cosi non fosse, allora $a_l$ punterebbe ad $a_i$ anche all'iterazione $k$.
Dunque $a_i$ non può far parte di nessun ciclo che coinvolge agenti in $N_1 \cup \dots \cup N_{j-1}$.
```

### Una riallocazione ottimale
Si osserva che il teorema precedente non è così sorprendente. Infatti, per esempio, anche il meccanismo che non rialloca mai le case è truthful.
La vera caratteristica importante dell'algoritmo TTC è che, oltre a garantire la truthfulness, garantisce una allocazione delle case che è la migliore possibile, *ottimale*. Si chiarisce formalizzando questa proprietà.

Data una allocazione delle case agli agenti, un sottoinsieme di agenti forma una **blocking coalition** se questi possono scambiarsi le case iniziali in modo tale che *nessuno* di loro peggiora e *almeno uno* di loro migliora.
Una allocazione che presenta delle blocking coalition non è una allocazione *stabile*, perché gli agenti della coalizione possono scambiarsi le case iniziali per migliorare.

Una **core allocation** è una allocazione senza nessuna blocking coalition: una core allocation è una allocazione *stabile*, cioè l'allocazione *ottimale*. Infatti, gli agenti non possono scambiarsi le case iniziali per migliorare.

Dunque si può dimostrare il teorema seguente
```ad-Teorema
title: Teorema 3.9
Per ogni House Allocation Problem, l'allocazione calcolata dall'algoritmo TTC è l'*unica core allocation*.
```
```ad-Dimostrazione
Per dimostrare il teorema, si dimostrano due claim.
###### Claim 1: ogni allocazione diversa da quella del TTC non è una core allocation
Si dimostra che fissata una allocazione diversa da quella del TTC esiste una blocking coalition.
Nell'allocazione del TTC, ogni agente in $N_1$ riceve la prima scelta nel ranking. Quindi, gli agenti in $N_1$ formano una blocking coalition per ogni allocazione che si discosta dall'allocazione fatta dal TTC per un agente in $N_1$. Dunque ogni core allocation deve essere uguale all'allocazione del TTC per gli agenti in $N_1$. Similmente, ogni agente in $N_2$ riceve la prima scelta nel ranking al di fuori delle case in $N_1$. Quindi, gli agenti in $N_2$ formano una blocking coalition per ogni allocazione che si discosta dall'allocazione fatta dal TTC che è uguale a quella del TTC su $N_1$ per un agente in $N_2$. Dunque ogni core allocation deve essere uguaele all'allocazione del TTC per gli agenti in $N_1$ e $N_2$. Continuando induttivamente, si conclude che ogni allocazione che si discosta da quella dall'allocazione del TTC non è una core allocation.
###### Claim 2: L'allocazione trovata da TTC è una core allocation
Sia $S$ un sottoinsieme arbitrario di agenti. Si consideri una riallocazione interna delle case di proprietà degli agenti in $S$. Questa riallocazione partiziona $S$ in cicli diretti. Sia $C$ uno di questi cicli diretti.
- Se $C$ contiene due agenti $a_{i} \in N_j$ e $a_{t} \in N_k$ con $j < k$ allora dalla riallocazione $a_i$ ottiene una casa che è posseduta da $a_t$, dunque $a_i$ peggiora rispetto all'allocazione del TTC;
- Se $C$ contiene tutti agenti in $N_k$, ogni agente che non riceve la sua casa favorita tra quelle in $N_k$ peggiora rispetto all'allocazione del TTC.
Si conclude che se la riallocazione interna delle case in $S$ si discosta dall'allocazione calcolata dall'algoritmo TTC, allora almeno un agente in $S$ peggiora. Siccome $S$ è arbitrario, l'allocazione TTC non ha blocking coalitions ed è una core allocation.
```

## Case study: Kidney exchange
### Background
Molte persone soffrono di insufficienza renale e necessitano di un trapianto di rene. Negli Stati Uniti, più di 100.000 persone sono in lista d'attesa per tali trapianti. Un idea per risolvere questo problema, utilizzata anche per altri organi, riguarda i donatori deceduti: quando qualcuno muore ed è un donatore di organi registrato, i loro organi possono essere trapiantati in altre persone. Una caratteristica speciale dei reni è che una persona sana ne ha due e può sopravvivere tranquillamente con uno solo. Questo crea la possibilità di donatori viventi, come un familiare del paziente bisognoso.
Sfortunatamente, avere un donatore vivente non è sempre sufficiente; infatti, in generale, una coppia paziente-donatore può essere *incompatibile*, cioè il rene del donatore molto probabilmente non funzionerà correttamente nel paziente.
Il problema principale sta nella compatibilità rispetto al gruppo sanguigno. Per esempio, un paziente di tipo 0 può ricevere un rene solo da un donatore con lo stesso gruppo sanguigno; similmente un donatore di tipo AB può solo donare ad un paziente di tipo AB. Un idea per risolvere questo problema è lo *scambio dei reni*, meglio noto come *kidney exchange*.

Siano $(P1,D1)$ e $(P2,D2)$ due coppie paziente-donatore. Si supponga che il paziente $P1$ non è compatibile col suo donatore $D1$ perché sono di tipo $A$ e $B$ rispettivamente. Si supponga invece che il paziente $P2$ non è compatibile col suo donatore $D2$ perché sono di tipo $B$ e $A$ rispettivamente. Allora è ragionevole far scambiare i donatori, cioè far ricevere a $P1$ il rene di $D2$ e far ricevere a $P2$ il rene di $D1$. Questa è l'idea di base della *kidney exchange*.
![|center|500](07-agt_img03.png)
Alcuni scambi di reni sono stati effettuati all'inizio di questo secolo. Questi successi isolati hanno evidenziato la necessità di uno scambio nazionale di reni, in cui coppie paziente-donatore incompatibili possono registrarsi e essere abbinati ad altri, con l'obiettivo di consentire il maggior numero possibile di abbinamenti.
Attualmente, il compenso per la donazione di organi è illegale negli Stati Uniti e in ogni paese tranne l'Iran. Invece, lo scambio di reni è legale e viene naturalmente modellato come un problema di mechanism design senza pagamenti.
### Algoritmo TTC per Kidney Exchange
Una prima idea è quella di modellare il problema del *kidney exchange* come un problema di *house allocation*. L'idea è di considerare ogni coppia paziente-donatore come un'agente, dove il donatore incompatibile corrisponde alla casa che viene inizialmente assegnata all'agente nell'house allocation problem. Dunque ogni paziente definisce un ordinamento totale sui donatori, cioè un *ranking* dei donatori rispetto alla probabilità di successo del trapianto, basato sul gruppo sanguigno e altri fattori.
A questo punto è sufficiente applicare l'algoritmo TTC per trovare una reallocazione dei donatori. Per le proprietà dell'algoritmo, la reallocazione suggerita può solamente migliorare la probabilità di successo del trapianto al paziente.
In particolare, il risultato migliore si ottiene quando l'algoritmo TTC trova cicli come quello nella figura successiva, che corrisponde al kidney exchange nella figura precedente.
![](AGT/attachments/07-agt_img04.png)

Il problema principale dell'applicazione dell'algoritmo TTC è che potrebbe suggerire riallocazioni che utilizzano cicli lunghi, come mostrato nella Figura seguente.
![](07-agt_img05.png)
Infatti un ciclo di lunghezza due corrisponde già a quattro interventi chirurgici: due per estrarre i reni dai donatori e due per impiantarli nei pazienti. Inoltre, questi quattro interventi devono avvenire simultaneamente. Infatti se, ad esempio, gli interventi chirurgici per $P1$ e $D2$ avvengono per primi, c'è il rischio che $D1$ possa ritirare la sua offerta di donare il suo rene a $P2$ (è illegale stipulare un contratto per una donazione di organi, un donatore può rifiutarsi in qualsiasi momento).
Dunque da una parte, $P1$ ottiene un rene *gratuitamente*. Il problema molto più serio è che $P2$ non può più partecipare ad uno scambio di reni, dato che il suo donatore $D2$ ha donato. A causa di questo rischio, gli interventi chirurgici non simultanei non sono quasi mai utilizzati negli scambi di reni.
Dato che far avvenire tante operazioni simultaneamente in un ospedale è molto dispendioso, è fondamentale ottenere dei cicli di lunghezza corta.
Un altro problema dell'approccio con TTC è che modellare le preferenze dei pazienti come un ordinamento sui donatori è eccessivo; è sufficiente modellarle come preferenze binarie.
### Modellazione come graph matching
I due obiettivi di avere cicli corti e preferenze di tipo binario suggeriscono la modellazione del problema come un problema di **graph matching**.
Un *matching* in un grafo non diretto è un sottoinsieme di archi che non condividono alcun nodo. 
In questo caso, il grafo è dato dall'insieme dei vertici $V$ corrispondente alle coppie paziente-donatore incompatibili (un vertice per coppia) in cui esiste un arco non diretto tra due nodi $\{(P1,D1),(P2,D2)\}$ se e soltanto se $P1$ e $D2$ sono compatibili e $P2$ e $D1$ sono compatibili. Il grafo corrispondente all'esempio iniziale è il seguente.
![](07-agt_img06.png)
Dunque un matching in tale grafo corrisponde ad un'insieme di coppie che possono effettuare uno scambio: ogni scambio implica quattro interventi simultanei.
Allora l'obiettivo è massimizzare il numero di coppie che possono effettuare uno scambio, che corrisponde a trovare il matching di dimensione massima nel grafo.
Inoltre, si assume che ogni paziente $i$ abbia un insieme $E_i$ di donatori compatibili che appartengono ad coppie paziente-donatore, e che $i$ può dichiarare come donatori compatibili un qualsiasi sottoinsieme $F_i \subseteq E_i$ al meccanismo. Questa assunzione è ragionevole dato che un paziente può rifiutare uno scambio per qualsiasi motivo; inoltre nessun paziente può dichiarare un insieme che contiene dei donatori non compatibili.
Un algoritmo (meccanismo) che massimizza li numero di kidney exchange rispetto alle dichiarazioni $E_i$ è il seguente.
![](07-agt_img07.png)
Tale meccanismo è truthful se per ogni paziente $i$ è strategia dominante dichiarare completamente l'insieme $E_i$ al meccanismo. In particolare, la truthfulness del meccanismo dipende da come viene scelto il matching massimo tra quelli possibili:
- Se due matching massimi differenti accoppiano lo stesso insieme di vertici, allora è sufficiente sceglierne uno. Infatti, i pazienti non sono interessati al donatore scelto per lui fintantoché è compatibile;![|center|500](07-agt_img08.png)
- Più significativo il caso in cui due matching massimi accoppiano un insieme di nodi differente. Nella figura seguente, un nodo si trova in ogni matching massimo, ma solo uno degli altri tre nodi può essere accoppiato con lui: come scegliere questo nodo? 
![|center|300](07-agt_img09.png)

Una soluzione è quella di dare delle priorità alle coppie paziente-donatore prima che il meccanismo inizi. La maggior parte degli ospedali si affida già a schemi di priorità per gestire i propri pazienti. La priorità di un paziente in lista d'attesa è determinata dalla durata del tempo in cui è stata in lista, dalla difficoltà di trovare un rene compatibile e da altri fattori.
In dettaglio, si implementa il terzo passo del meccanismo nel modo seguente, supponendo che i vertici $V = \{1, 2, \dots n\}$ di $G$ siano ordinati dalla priorità più alta a quella più bassa.
![](07-agt_img10.png)
Dunque, dopo aver calcolato l'insieme dei matching massimi di $G$, si considerano i vertici secondo la priorità; si verifica in ogni iterazione $i$ se esiste un matching massimo che:
1. soddisfa gli accoppiamenti già stabiliti per i pazienti con maggiore priorità;
2. accoppia il paziente $i$.
Se tale matching esiste, allora si include il paziente $i$ nel matching finale.
Se tale matching non esiste, cioè se gli accoppiamenti già stabili precludono l'accoppiamento di $i$ con un donatore, si esclude $i$ dal matching e si considera il paziente successivo nell'ordinamento.
Allora per induzione su $i$, $M_i$ è un sottoinsieme non vuoto di matching massimi di $G$, e $M_n$ contiene dei matching che accoppiano lo stesso insieme di vertici, dunque la scelta del matching finale tra quelli di $M_n$ è irrilevante.

Si dimostra che usando questo meccanismo basato su priorità, per ogni paziente $i$ è strategia dominante dichiarare l'intero insieme $E_i$ dei donatori compatibili.
### Il meccanismo con priorità è truthful
Nel meccanismo con priorità per il kidney exchange, per ogni paziente $i$ e per ogni dichiarazione fatta dagli altri pazienti, nessuna dichiarazione di $F_i \subset E_i$ porta ad un outcome migliore per $i$ rispetto a dichiarare $F_{i} = E_i$.

@ Dimostrarlo per esercizio 
## Incentivi per gli ospedali
Nel mondo reale, le coppie paziente-donatore vengono segnalate nel sistema nazionale per lo scambio dei reni da parte degli ospedali.
In generale, l'obiettivo dell'ospedale di accoppiare il maggior numero dei suoi pazienti non è perfettamente in linea con l'obiettivo sociale di accoppiare il maggior numero di pazienti su tutti quanti. Si fanno degli esempi per evidenziare le differenze.
### Esempio 1. Riportare tutte le coppie
Siano $H_1,H_{2}$ due ospedali, ognuno con tre coppie paziente-donatore, come in figura.
![](07-agt_img11.png)
Ogni arco collega coppie che sono mutualmente compatibili. Ogni ospedale ha due coppie paziente-donatore che si accoppiano internamente, cioè le due coppie risiedono nello stesso ospedale. Allora gli ospedali potrebbero non riportare queste coppie nel sistema nazionale, e effettuare l'accoppiamento e lo scambio internamente. Così facendo però non si potrebbero risolvere tutti gli accoppiamenti possibili. Nell'esempio, non riportando al sistema nazionale le coppie 1,2,5 e 6, le coppie 3 e 4 rimangono disaccoppiate. Se invece i due ospedali riportano tutte le coppie, allora 1,2 e 3 possono essere accoppiati con 4,5 e 6 rispettivamente e tutti i pazienti possono ricevere un rene nuovo.
In generale, l'obiettivo è incentivare gli ospedali a dichiarare tutte le coppie paziente-donatore per salvare il maggior numero di vite.
### Esempio 2: risultato di impossibilità
Si considerano nuovamente due ospedali, con sette pazienti distribuiti come in figura.
![](07-agt_img12.png)
Sia inoltre il meccanismo di exchange in grado di calcolare il matching massimo rispetto alle coppie dichiarate dagli ospedali.
Si osserva che con un numero dispari di coppie, ogni matching lascia un paziente non accoppiato.
- se $H_1$ riporta solo il paziente 7 e $H_2$ riporta tutti i pazienti, allora l'ospedale $H_1$ accoppia ogni paziente: l'unico matching massimo è quello che accoppia 6 con 7 (e 4 con 5) e $H_1$ può accoppiare 2 con 3 internamente;
- se $H_2$ riporta solo 1 e 4 e $H_1$ riporta tutti i pazienti, allora $H_2$ accoppia ogni paziente: l'unico matching massimo è quello che accoppia 1 con 2 e 4 con 3, mentre $H_2$ può accoppiare 5 con 6 internamente.
Dunque si può concludere che, a prescindere dal matching massimo calcolato dal sistema, almeno un ospedale ha convenienza a mentire riportando parzialmente le proprie coppie.

Questo è un *risultato di impossibilità*, per cui non esiste nessun meccanismo veritiero (per cui è strategia dominante dichiarare tutte le coppie nel proprio ospedale) che calcola sempre un matching massimo.

La ricerca attuale sulla progettazione di meccanismi per la segnalazione ospedaliera prende in considerazione vincoli di incentivazione più flessibili e una ottimalità approssimata.

# 4 PLS-completezza

Si vogliono studiare i limiti di algoritmi computazionalmente efficienti per la convergenza e per il calcolo di un equilibrio di Nash puro per un *congestion game*. Per fare ciò, risulta necessario far riferimento alla teoria della PLS-completezza.

## Congestion game

Un *congestion game* ha $k$ giocatori ed $n$ risorse. Il giocatore $i$ dispone di un insieme $S_i$ di strategia consentite, ciascuna delle quali specifica un sottoinsieme di risorse. Ogni risorsa $j$ ha una funzione costo $c_j(x)$, la quale indica il costo pagato da un qualsiasi giocatore $i$ per il quale la sua strategia scelta include la risorsa $j$. $c_j(x)$ viene definita come funzione *dipendente dal carico*, questo perché dipende da $x$, il numero di giocatori totali tali per cui la loro strategia include $j$. Si fa riferimento ad $x$ come *carico*.
Il costo totale pagato dal giocatore $i$ per aver scelto una strategia $S_i$ risulta essere la somma dei costi per ciascuna risorsa in $S_i$. Quindi, se il carico totale sul collegamento $j$ è $x_j$, allora $i$ paga 
$$
\sum_{j \in S_i}c_j(x_j)
$$
Si introduce la notazione per la descrizione di un congestion game:
- $E$ : Insieme di risorse.
- $k\in \mathbb{N}$ : numero di giocatori.
- Il giocatore $i$ sceglie una strategia $S_i$ da un insieme esplicito di strategie $\mathcal{S}_i \subseteq 2^{E}$.
- Ogni risorsa $e \in E$ ha un possibile costo $c_e(1),c_e(2),\cdots,c_e(k)$.
- Dato un vettore di strategia $S$, il costo del giocatore $i$ è: $$ cost_i(S) = \sum_{e\in S_i}c_e(k_e(S))$$ dove $k_e(S)$ è il numero di giocatori nell'esito $S$ i quali utilizzano una strategia che include la risorsa $e$.


Il Global connection game, ad esempio, è chiaramente un gioco di congestione, dove le risorse sono gli archi, i cammini $s_i - t_i$ sono le strategie disponibili al giocatore $i$, e le funzioni costo sono $c_e(x)=c_e/x$.

Si ricorda la notazione per la descrizione del global connection game:
- $G=(V,E)$ grafo diretto.
- $c_e$ : costo non negativo di $e \in E$.
- Il giocatore $i$ ha un nodo sorgente $s_i$ ed un nodo pozzo $t_i$.
- Dato un vettore di strategia $S$, il costo del giocatore $i$ è $$cost_i(S) = \sum_{e \in P_i}c_e/k_e(S)$$
Si è dimostrato nel lemma $2.1$ che il global connection game appartiene alla classe di giochi potenziali. Di conseguenza, per i teoremi $2.4$ e $2.5$, si ha che, per tale gioco, esiste sempre un equilibrio di Nash, e che una dinamica di best response converge sempre ad esso.

I Congestion Game rientrano a loro volta nella classe di giochi potenziali, infatti, è possibile associare ad essi la funzione potenziale di Rosenthal:
$$
\Phi(S) = \sum_{e \in E} \sum_{i=1}^{k_e(S)}c_e(i)
$$
Che soddisfa $\Phi(S) - \Phi(S^{'}) = \textnormal{cost}_i(S) - \textnormal{cost}_i(S^{'})$ per ogni esito $S$, agente $i$ e deviazione unilaterale $s_i^{'}$ da parte di $i$, dove $S^{'} = (s_i^{'},S_{-i})$. 

Di conseguenza, anche per essi valgono i teoremi $2.4$ e $2.5$, tali per cui:
- Esiste sempre un equilibrio di Nash, in particolar modo ogni minimo locale di $\Phi(\cdot)$ è un equilibrio di Nash.
- La dinamica di better response converge sempre ad un equilibrio di Nash.

Si vuole studiare la trattabilità del seguente problema

```ad-Problema
title: Problema CG-NE
Data un'instanza di un Congestion Game, trovare un qualsiasi equilibrio di Nash.
```

Per fare ciò non si può fare riferimento alla teoria della $NP$-completezza.
## $FNP$: Functional NP

Un problema in $NP$ è un problema decisionale, definito da un certificatore polinomiale di presunte soluzioni per una data istanza, dove gli input di tale certificatore prendono il nome di *witnesses* (testimoni) dell'istanza. I testimoni hanno dimensioni polinomiali e sono definiti per le "istanze si", ossia le istanze per le quali vi è almeno un witness.
Dato quindi un problema in $NP$, una soluzione per un'istanza può essere "si" o "no", a seconda che per essa vi sia almeno un witness.

I problemi di calcolo di equilibri non rientrano nella categoria di problemi decisionali, perché in output richiedono un equilibrio.
Si fa riferimento quindi alla classe $FNP$, dove i problemi definiti al suo interno sono come i problemi in $NP$, ma per i quali è necessario produrre un witness per un'istanza "si". Questi problemi sono chiamati anche *problemi di ricerca*. Un algoritmo per un problema in $FNP$ prende in input un'istanza di un problema in $NP$, e restituisce un testimone per tale istanza se questo esiste (ossia, se l'istanza in input è un'istanza "si"). Se non esiste nessun testimone, allora l'algoritmo restituisce "no". 
Il problema CG-NE è quindi appartenente ad $FNP$.

Una riduzione da un problema di ricerca $L_1$ verso un problema $L_2$ è definita mediante due algoritmi polinomiali:
1. L'algoritmo $A_1$ mappa le istanze $x\in L_1$ in istanze $A_1(x)\in L_2$.
2. L'algoritmo $A_2$ mappa i witnesses di $A_1(x)$ nei witnesses di $x$, e "no" in "no".
Quindi, se $L_2$ è risolvibile in tempo polinomiale ,allora anche $L_1$ è a sua volta risolvibile in tempo polinomiale.

Per tale classe $FNP$, si ha il seguente risultato condizionato riguardante il congestion game definito in precedenza:
```ad-Teorema
title: Teorema 4.1
Il problema CG-NE non è $FNP$-completo a meno che $NP=coNP$
```
```ad-Dimostrazione
Si assuma che esista una riduzione dal problema functional SAT (la variante di ricerca del problema SAT) al problema di calcolare un equilibrio di Nash per il congestion game.
Tale riduzione è definita da due algoritmi:
1. $A_1$ mappa ogni formula $\phi$ di SAT nelle istanze $A_1(\phi)$ di CG-Ne.
2. $A_2$ mappa ogni equilibrio di Nash $S$ di $A_1(\phi)$ ad un assegnamento di verità $A_2(S)$ che soddisfa $\phi$ se esiste, o alla stringa "no" altrimento. 

L'esistenza di questi due algoritmi implica che $NP=coNP$.
Per dimostrare ciò, sia $\phi$ una formula di SAT non soddisfacibile, ed $S$ un equilibrio di Nash di $A_1(\phi)$.
$S$ è una dimostrazione verificabile efficiente e di dimensione polinomiale dell'insoddisfacibilità di $\phi$. Infatti è possibile definire a partire da esso un verificatore polinomiale che opera come segue:
1. Calcola $A_1(\phi)$
2. Verifica che $S$ è un equilibrio di Nash di $A_1(\phi)$.
3. Verifica che $A_2(S)$ restituisce "no".
```
```ad-Osservazione
title: Osservazione 4.1
Nella dimostrazione si è usato il fatto che ogni istanza di congestion game ha almeno un equilibrio di Nash, mentre un'istanza di functional SAT non necessariamente può avere un witness.
```

Ci si restringe quindi ad una sottoclasse di $FNP$, nota come $TFNP$, *total functional NP*. I problemi in $TFNP$ sono quei problemi in $FNP$ tali per cui ogni istanza ha almeno un witness.
Per tale classe, si ha il seguente risultato
```ad-Teorema
title: Teorema 4.2
Se un problema in $TFNP$ è $FNP-$completo, allora $NP=coNP$.
```


## Classi di complessità sintattiche e semantiche.
L'appartenenza a $TFNP$ di un problema, o in particolar modo di un gioco, preclude la possibilità di dimostrare che il calcolo di un suo equilibrio di Nash sia $FNP-$completo (a meno che $NP=coNP$). Si può pensare quindi di voler dimostrare che il problema in questione sia $TFNP-$completo, ossia difficile tanto quanto ogni altro problema in $TFNP$.
Anche tale obiettivo risulta essere troppo ambizioso, questo perché, per la struttura di tale classe, sembra che non esistano in essa problemi $TNFP-$completi. Per argomentare ciò, si fa inizialmente riferimento alle classi per le quali si è riuscito a dimostrare l'esistenza di problemi completi appartenenti ad esse, ad esempio $P,NP$ e $PSPACE$. Tutte queste classi di complessità hanno in comune l'essere *"sintattiche"*, ossia che l'appartenenza di un problema ad una di esse può essere caratterizzato mediante un modello computazionale concreto, come ad esempio macchine di Turing deterministiche o non deterministiche che operano in spazio o tempo polinomiale. Vi è quindi una ragione generale per l'appartenenza di un problema ad una data classe. Le classi di complessità sintatticamente definite hanno sempre in esse un problema "generico" completo, dove l'input è costituito da una descrizione di un problema in termini della macchina che lo accetta ed un'istanza del problema stesso, e l'obiettivo è quello di risolvere la data istanza del dato problema. 
Ad esempio, il generico problema $NP-$completo prende in input la descrizione di un certificatore, una delimitazione temporale di ordine polinomiale, e la codifica di un'istanza, e l'obiettivo è quello di decidere se esista o meno un witness, ossia una stringa che causi l'accettazione da parte del certificatore della data istanza entro il numero di passi prestabilito dalla delimitazione temporale.
La classe $TFNP$ non ha delle motivazioni generiche per le quali un problema debba appartenere ad esso. Classi di questo tipo prendono l'aggettivo di *"semantiche"*. Ad esempio, il problema *fattorizzazione* appartiene a $TFNP$, e la sua appartenenza ad essa risiede in motivazioni date dalla teoria dei numeri. Anche il problema di trovare un equilibrio di Nash in strategie miste è in $TFNP$, ma le motivazioni sono distinte dal problema fattorizzazione.  Non sembra possibile trovare un argomento generico che dimostri l'appartenenza quindi a tale classe per problemi di natura distinta.

Non è possibile quindi dimostrare che CG-NE è $TFNP-$completo. 
Bisogna far riferimento ad un sottoinsieme della classe $TFNP$, nota come $PLS$, la classe dei *polynomial local search problems,* o *abstract local search problem*.
I testimoni di un'istanza di un problema di $PLS$ sono i suoi ottimi locali.
Il problema CG-NE rientra nella classe $PLS$.
Essendo tale classe sottoinsieme di $TFNP$, ogni istanza di un problema $PLS$ ha almeno un testimone.


## Maximum Cut Problem

Si studia ora un problema che verrà utilizzato al fine di dimostrare l'intrattabilità di CG-NE: maximum cut.

In input si ha un grafo non diretto $G=(V,E)$, con pesi non negativi $w_e \geq 0$ per ogni arco $e \in E$.
Le soluzioni ammissibili corrispondono a *tagli* $(X,\bar{X})$, dove $(X,\bar{X})$ è una partizione di $V$ in due sottoinsiemi. Formalmente, sia $X \subseteq V$ e $\bar{X}=V\backslash X$ 
$$
 (X,\bar{X}) = \{(u,v) \in E \ |\ u \in X \ \wedge\ v \in \bar{X}\ \}
$$
L'obiettivo è quello di massimizzare il peso totale degli archi del taglio, ossia gli archi aventi un estremo in $X$ e l'altro in $\bar{X}$:
$$
\sum_{(u,v) \in E: u\in X \wedge v\in \bar{X}} w(u,v)
$$
Il problema maximum cut è $NP$-hard. Assumendo quindi che $P\neq NP$, non esiste un algoritmo in tempo polinomiale che lo risolve.
La ricerca locale è un'euristica utile per molti problemi $NP-$hard, incluso il problema preso in esame. Il relativo algoritmo può essere schematizzato come segue:

- Inizializza con un taglio arbitrario $(X,\bar{X})$.
- $\texttt{while}$ è presente una mossa locale migliore, scegli tale mossa.

Con *mossa locale*, si intende spostare un singolo vertice $v$ da un lato del taglio verso l'altro. Ad esempio, quando si sposta un vertice $v$ da $X$ verso $\bar{X}$, l'aumento nel valore della funzione obiettivo è 
$$
\sum_{u \in X: (u,v)\in E}w_{uv} \ -  \sum_{u\in\bar{X}:(u,v)\in E}w_{u,v}
$$
Dove:
- $\sum_{u \in X: (u,v)\in E}w_{uv}$ è la somma dei pesi degli archi che collegano i nodi in $X$ a $v$, e che faranno parte del nuovo taglio.
- $\sum_{u\in\bar{X}:(u,v)\in E}w_{u,v}$ è la somma dei pesi degli archi che collegano i nodi in $\bar{X}$ a $v$, la quale viene sottratta al valore della funzione obiettivo perché non facenti più parte del taglio.

Se la differenza in questa equazione è positiva, allora la mossa in questione porta ad un *migliore locale*.
La ricerca locale si ferma ad una soluzione priva di mosse locali migliori, ossia un *ottimo locale*. Un ottimo locale non è necessariamente un ottimo globale. 
Non sempre la ricerca di un ottimo locale risulta essere più facile della ricerca di un ottimo globale.
Si prenda, ad esempio, la specializzazione del problema maximum cut per grafi non pesati, dove ogni arco ha costo $1$. Per questa variante, nonostante risulti sempre essere $NP$-hard, l'algoritmo di ricerca locale converge ad una soluzione localmente ottima in tempo polinomiale. Questo perché la funzione obiettivo, in questo caso, può assumere valori solo nell'insieme $\{0,1,2,\cdots,|E|\}$, e quindi la ricerca locale si ferma, in un massimo locale, in al più $|E|$ iterazioni.
Non si conoscono algoritmi polinomiali, basati su ricerca locale o altre tecniche, per il calcolo di un ottimo locale di un'istanza di maximum cut con pesi degli archi arbitrari non negativi.
Si vuole trovare quindi un modo per mostrare che tali algoritmi non esistano, in particolar modo, si vorrebbe dare un risultato "incondizionato", ossia una dimostrazione priva di assunzioni non dimostrate che mostra come un algoritmo polinomiale per il problema non può esistere. Tale risultato separerebbe la classe $P$ dalla classe $NP$.
Essendo questo un problema aperto, per raccogliere delle evidenze sulla probabile non esistenza di un algoritmo polinomiale per max cut, bisognerebbe mostrare che il problema sia $NP$-hard. Ciò implicherebbe che questo ammetta un algoritmo polinomiale se e solo se $P=NP$. Per le differenze viste in precedenza riguardo i problemi di ricerca astratta e i problemi decisionali, si sviluppa quindi un concetto analogo alla $NP$-completezza adatto a problemi di ricerca locale. Cosi facendo, si ottiene anche un forte lower bound incondizionato sul caso peggiore del numero di iterazioni necessarie ad un algoritmo di ricerca locale per il raggiungimento di un ottimo locale.

## $PLS$: Abstract Local Search Problem

Si vuole mostrare che il problema del calcolo di un ottimo locale per un'istanza di max cut è tanto difficile quanto ogni altro problema di ricerca locale. 
Risulta necessario quindi chiarire quale sia la struttura di un generico problema di ricerca locale. In generale, un problema di ricerca locale astratto può essere un problema di massimizzazione o minimizzazione, specificato da tre algoritmi, ciascuno di essi eseguibile in tempo polinomiale nella dimensione dell'input:
1. Il primo algoritmo polinomiale prende in input un'istanza e produce in output una soluzione ammissibile arbitraria.
2. Il secondo algoritmo polinomiale prende in input un'istanza ed una soluzione ammissibile, e restituisce il valore della funzione obiettivo per tale soluzione.
3. Il terzo algoritmo polinomiale prende in input un'istanza ed una soluzione ammissibile e restituisce "localmente ottimo" o produce altrimenti una soluzione con un valore migliore della funzione obiettivo.

```ad-Osservazione
title: Osservazione 4.2
Il terzo algoritmo della descrizione di un problema in $PLS$ è un verificatore efficiente di witnesses. Infatti tale algoritmo, quando riceve in input una soluzione non ottima localmente, non restituisce semplicemente "no" come un verificatore di un problema in $NP$, ma fornisce una soluzione alternativa con un miglior valore della funzione obiettivo.
```

Ad esempio, per il problema max cut:
1. Il primo algoritmo restituisce in output un taglio arbitrario. 
2. Il secondo algoritmo calcola la somma dei pesi degli archi costituenti il taglio restituito in precedenza.
3. Il terzo algoritmo controlla tutte le $|V|$ mosse locali possibili. Se nessuna migliora il valore della funzione obiettivo, restituisce in output "localmente ottimo". Altrimenti, sceglie una mossa locale migliore e restituisce il taglio risultante.

Ogni problema di ricerca locale astratta ammette una procedura di ricerca locale che utilizza i tre algoritmi descritti come sotto-procedure. Data un'istanza,, la procedura di ricerca locale generica utilizza il primo algoritmo per ottenere una soluzione iniziale, ed iterativamente applica il terzo algoritmo sino al raggiungimento di una soluzione localmente ottima. Il ruolo del secondo algoritmo è quello di mantenere il terzo algoritmo onesto, ed assicurarsi che ogni soluzione prodotta abbia valore della funzione obiettivo migliore della precedente. Se la produzione di una soluzione migliore da parte del terzo algoritmo fallisce, la procedura generica può interpretare il suo output precedente come "localmente ottimo".

L'algoritmo termina sempre, questo perché i valori della funzione obiettivo per le soluzioni candidate migliorano strettamente sino a quando una soluzione ottima localmente viene trovata, e sono presenti un numero finito di soluzioni ammissibile.

La classe di complessità $PLS$ è, per definizione, l'insieme di tutti i problemi di ricerca locale astratta.

Si vuole quindi mostrare che il problema di calcolare un ottimo locale per un'istanza di maximum cut è difficile tanto quanto gli altri problemi di ricerca locale.
Per fare ciò, si fa riferimento alle riduzioni polinomiali. Formalmente, una riduzione da un problema $L_1 \in PLS$ ad un problema $L_2 \in PLS$ consiste in due algoritmi polinomiali con le seguenti proprietà:
1. L'algoritmo $A_1$ mappa ogni istanza $x\in L_1$ in un'istanza $A_1(x)\in L_2$.
2. L'algoritmo $A_2$ mappa ogni ottimo locale di $A_1(x)$ in un ottimo locale di $x$.

La definizione di una riduzione assicura che, se risulta possibile risolvere in tempo polinomiale il problema $L_2$ allora, combinando le soluzioni mediante l'utilizzo degli algoritmi $A_1$ ed $A_2$, può essere risolto $L_1$ in tempo polinomiale.

Si da ora la definizione di completezza per la classe $PLS$

```ad-Definizione
title: Definizione 4.1 (Problema $PLS$-completo)
Un problema $L$ è $PLS$-completo se $L \in PLS$ ed ogni problema in $PLS$ è riducibile ad esso, ossia, $\forall L^{'}\in PLS$
$$
L^{'} \leq_p L
$$
```

Per definizione quindi, esiste un algoritmo polinomiale per risolvere un problema $PLS$-completo se e solo se ogni problema in $PLS$ può essere risolto in tempo polinomiale.
Un problema $PLS$-completo è un singolo problema di ricerca locale che, simultaneamente, codifica ogni problema di ricerca locale.

```ad-Teorema
title: Teorema 4.3
Calcolare un massimo locale per un'istanza di maximum cut con pesi degli archi generici non negativi è un problema $PLS$-completo.
```

Indipendentemente dal fatto che ogni problema in $PLS$ possa o non possa essere risolto in tempo polinomiale, la dimostrazione del teorema appena enunciato implica che l'algoritmo di ricerca locale per max cut richieda tempo esponenziale nel caso peggiore.

```ad-Teorema
title: Teorema 4.4
Calcolare un massimo locale per un'istanza di maximum cut con pesi degli archi generici non negativi usando la ricerca locale può richiedere un numero esponenziale in $|V|$ di iterazioni, indipendentemente da come venga scelta una mossa locale in ogni iterazione.
```

```ad-Osservazione
title: Osservazione 4.3
Ogni istanza di un problema $PLS$ ha almeno un testimone, questo perché per definizione, la ricerca locale si deve per forza fermare ad un ottimo locale. $PLS$ è allora sottoinsieme di $TFNP$, e quindi nessuno dei problemi appartenenti ad esso, come il calcolo di un equilibrio di Nash puro di un congestion game, può essere $FNP-$completo, a meno che $NP=coNP$. 
```

```ad-Osservazione
title: Osservazione 4.4
Al contrario di $TFNP$, i problemi in questa classe possiedono una ragione generica per l'appartenenza ad essa, ossia che sono risolvibili mediante una procedura di ricerca locale.
```
## $PLS-$completezza per Congestion Game

Calcolare un equilibrio di Nash puro di un congestion game è tanto difficile quanto ogni altro problema di ricerca locale.

```ad-Teorema
title: Teorema 4.5
Il problema di calcolare un equilibrio di Nash puro per un congestion game (CG-NE) è $PLS-$completo.
```
```ad-Dimostrazione
Bisogna inizialmente mostrare l'appartenenza di CG-NE alla classe $PLS$.
Per fare ciò, si definiscono i $3$ algoritmi che descrivono tale problema:
- Algoritmo $1$: data l'istanza, restituisci un qualsiasi profilo di strategia $S$.
- Algoritmo $2$: Dato un profilo di strategia $S$, calcola $\Phi(S)$.
- Algoritmo $3$: dato un profilo di strategia $S$, calcola una better response per ogni giocatori se ce ne sono, altrimenti restituisci "$S$ è un equilibrio di Nash".

Si mostra ora la $PLS-$completezza di CG-NE mediante una riduzione dal problema del calcolo di un massimo locale di un'istanza di maximum cut, che per il teorema -inserireNumero-, è $PLS-$completo.
Il primo algoritmo polinomiale $A_1$ della riduzione prende in input un grafo $G=(V,E)$ con pesi non negativi $\{w_e\}_{e\in E}$.
L'algoritmo costruisce il seguente congestion game:
1. Ad ogni vertice $v \in V$ si associa un agente.
2. Associa due risorse a ciascun arco $e\in E$, $r_e$ e $\bar{r}_e$.
3. L'agente $v$ dispone di due strategie, ciascuna comprendente $|\delta(v)|$ risorse, dove $\delta(v)$ è l'insieme di archi incidenti a $v$ in $G$: $S_v = \{r_e\}_{e \in \delta(v)}$ ed $\bar{S}_v = \{\bar{r}_e\}_{e \in \delta(v)}$.
4. Una risorsa $r \in \{r_e,\bar{r}_e\}$ con $e=(u,v)$ può essere utilizzata solo dagli agenti corrispondenti ai nodi $u$ e $v$. Il costo di una tale risorsa è $0$ se usato da nessuno o da un solo agente, $w_e$ se utilizzata da entrambi gli agenti: $c_r(0)=c_r(1)=0$ e $c_r(2)=w_e$.
Questa costruzione può essere strutturata in maniera tale che avvenga in tempo polinomiale.

Si osserva che gli equilibri di Nash puri di questo congestion game sono in corrispondenza uno ad uno con gli ottimi locali del problema da cui avviene la riduzione. 
Si dimostra ciò utilizzando una biezione tra i $2^{|V|}$ possibili esiti di questo congestion game ed i tagli del grafo $G$, dove il taglio $(X,\bar{X})$ corrisponde all'esito in cui ogni agente corrispondente a $v \in X$ (rispettivamente, $v \in \bar{X})$ sceglie la sua strategia in maniera tale che contenga risorse del tipo $r_e$ (rispettivamente, $\bar{r}_e)$: 
sia $S$ un profilo di strategia, allora il suo taglio corrispondente è definito come
$$
(X_S := \{v: v \textnormal{ gioca } S_v \textnormal{ in }  S\}, \bar{X}_S:= V \backslash X_S)
$$

Sia ora
$$
\Phi(S) = \sum_{r \in R} \sum_{i=1}^{k_r(S)}c_r(i)
$$
la funzione potenziale di un congestion game, dove $R$ è l'insieme delle risorse.

Questa biezione mappa i tagli $(X,\bar{X})$ di $G$ con peso $w(X,\bar{X})$ ad esiti del gico con valori di funzione potenziale $\Phi(S)$ uguali a $W- w(X,\bar{X})$, dove $W = \sum_{e \in E}w_e$ denota la somma dei pesi degli archi. 
Per vedere ciò, si fissi un taglio $(X,\bar{X})$:
- Per un arco $e \in (X,\bar{X})$, ciascuna risorsa $r_e$ e $\bar{r}_e$, per definizione, è utilizzata da un solo agente e contribuisce quindi $0$ a $\Phi(S)$. 
- Per un arco $e \notin (X,\bar{X})$, che collega due nodi appartenenti alla stessa parte, i due agenti associati agli estremi di tale arco usano una sola risorsa tra $r_e$ ed $\bar{r}_e$, senza usare l'altra. Queste due risorse, quella utilizzata e quella non, contribuiscono rispettivamente quindi $w_e$ e $0$ a $\Phi(S)$.

Si conclude quindi che il valore della funzione potenziale del corrispettivo esito è il peso totale degli archi non tagliati da $(X,\bar{X})$, ossia $W-w(X,\bar{X})$:
$$
\Phi(S) = \sum_{r \in R} \sum_{i=1}^{k_r(S)}c_r(i) = W-w(X,\bar{X})
$$

I tagli di $G$ con peso maggiore quindi corrispondono ad esiti aventi valori di funzione potenziale minore, quindi i tagli in $G$ che definiscono un massimo locale sono in corrispondenza uno ad uno con i minimi locali della funzione potenziale.
Quindi $(X_S,\bar{X}_S)$ è un taglio massimo locale se e solo se $S$ è un minimo locale per $\Phi(S)$.
Per l'uguaglianza 
$$
\Phi(S)-\Phi(S^{'}) = \textnormal{cost}_i(S)-\textnormal{cost}_i(S^{'})
$$
si ha che i minimi locali della funzione potenziale sono in corrispondenza uno ad uno con gli equilibri di Nash puri del congestion game.
Il secondo algoritmo $A_2$ della riduzione traduce semplicemente un equilibrio di Nash puro del congestion game costruito da $A_1$ nel corrispettivo taglio massimo locale di $G$.
```

## Complessità del calcolo di un equilibrio di Nash in strategie miste - $PPAD$

Si vuole ora studiare la complessità del calcolo di un equilibrio di Nash in strategie miste.
In particolar modo, lo si fa per un gioco definito *bimatrix*.
Un gioco *bimatrix* è un gioco a due giocatori, che può essere specificata mediante due matrici $m \times n$ dei payoff $\mathbf{A}$ e $\mathbf{B}$, una per il "giocatore riga" e l'altra per il "giocatore colonna".
Si considera il problema di calcolare un equilibrio di Nash misto di un bimatrix game, o equivalentemente strategie miste $\mathbf{\hat{x}}$ ed $\mathbf{\hat{y}}$ sulle righe e sulle colonne tali per cui 
$$
\mathbf{\hat{x}}^{\intercal} \mathbf{A} \mathbf{\hat{y}} \geq \mathbf{x}^{\intercal} \mathbf{A} \mathbf{\hat{y}}
$$
per tutte le strategie di riga miste $\mathbf{x}$ e  
$$
\mathbf{\hat{x}}^{\intercal} \mathbf{B} \mathbf{\hat{y}} \geq \mathbf{\hat{x}}^{\intercal} \mathbf{B} \mathbf{y}
$$
per tutte le strategie di colonna miste $\mathbf{y}$.
```ad-Problema
title: Problema MNE
Data un'istanza di un gioco a $2$ giocatori in forma normale (bimatrix game), trovare un equilibrio di Nash misto.
```

Si ricorda che il teorema di Nash garantisce che un equilibrio di Nash misto per questo tipo di giochi esiste sempre. Ciò implica che MNE$\in TFNP$.
Non si conosce un algoritmo polinomiale per il problema MNE.
Si vuole mostrare che il problema risulti completo per una classe di complessità adatta, tenendo a mente che non si può dimostrare la $TFNP-$completezza per un dato problema appartenente a tale classe.
Essendo MNE un problema in $TFNP$ ed essendo $TFNP \subseteq FNP$, si ha che MNE$\in TNF$. Ma, come conseguenza del teorema ($4.2$) si ha il seguente risultato:
```ad-Teorema
title: Teorema 4.6
Calcolare un equilibrio di Nash misto di un bimatrix game non è $FNP$-completo a meno che $NP=coNP$.
```

Si fa quindi riferimento ad una sottoclasse di $TFNP$, nota come $PPAD$, la quale caratterizza la complessità computazionale del problema MNE. Prima di entrare nella definizione formale di tale classe, la si descrive mediante analogia con la classe $PLS$, la quale risulta essere simile nel suo spirito ma differente nei dettagli.

Si fa presente che un problema di ricerca locale può essere modellato mediante la ricerca in un grafo diretto aciclico di un nodo pozzo. I vertici di questo grafo corrispondono alle soluzione ammissibili, ad esempio gli esiti di un congestion game, ed il numero di vertici può essere esponenziale nella lunghezza della descrizione dell'istanza. 
Ogni arco diretto corrisponde al passaggio da una soluzione ad un'altra che risulta essere migliore localmente. I vertici senza archi uscenti, ossia i vertici pozzo, corrispondono agli ottimi locali. La ricerca locale procede attraversando gli archi uscenti dei nodi del grafo sino a quando non si raggiunge un nodo pozzo.
Ogni problema in $PLS$ può essere risolto da una generica procedura di ricerca locale che corrisponde a seguire gli archi uscenti sino a quando non si raggiunge un vertice pozzo.
Quindi, i problemi in questa classe possiedono una ragione generica per l'appartenenza ad essa, ossia che sono risolvibili mediante una procedura di ricerca locale.

I problemi in $PPAD$, come i problemi $PLS$, sono risolvibili mediante una procedura generica di *path-following*. Ogni problema in $PPAD$ può essere visto come un grafo diretto, dove i vertici corrispondono a *"soluzioni"* e gli archi diretti a *"mosse"*.
La differenza essenziale tra $PLS$ e $PPAD$ è data dalla struttura dei rispettivi grafi con i quali si possono modellare i relativi problemi appartenenti a tali classi. Il grafo corrispondente ad un problema in $PLS$ è diretto e aciclico, mentre il grafo corrispondente ad un problema in $PPAD$ è diretto con tutti i nodi avente grado entrante e grado uscente al più $1$. Inoltre, per definizione, il grafo corrispondente ad un problema in $PPAD$ possiede un nodo sorgente canonico, ossia un vertice senza archi entranti. Attraversare gli archi di tale grafo partendo dal nodo sorgente non può produrre un ciclo, essendo che ogni vertice ha grado entrante al più $1$, e il nodo sorgente canonico non ha archi entranti. Si ha quindi un nodo pozzo raggiungibile dal nodo sorgente canonico. Diversamente da un problema in $PLS$, non vi è una funzione obiettivo, ed un grafo diretto associato ad un problema in $PPAD$ può possedere cicli. I witnesses per un problema in $PPAD$ sono, per definizione, tutte le soluzioni che corrispondono ad un nodo pozzo o ad un nodo sorgente diverso da quello canonico.
Quindi la ragione generica di appartenenza di un problema a questa classe è data dal fatto che questo può essere risolto seguendo un cammino diretto dal nodo sorgente al nodo pozzo nel grafo che lo modella.

![[08-agt_img02.png]]




```ad-Teorema
title: Teorema 4.7
Calcolare un equilibrio di Nash misto per un bimatrix game è $PPAD-$completo.
```