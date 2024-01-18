# Nascita della Algorithmic Game Theory
L'**algorithmic game theory (AGT)** nasce dalla combinazione della *teoria dei giochi* con la *teoria degli algoritmi*.

Gli studenti di Informatica conoscono già la **teoria degli algoritmi**: è l'area di ricerca che cerca di rispondere a problemi di natura *computazionale*; in particolare:
- Cosa può essere calcolato? (Teoria della calcolabilità, macchine di Turing ecc)
- Quante risorse sono necessarie per calcolare una soluzione? (Teoria della complessità, NP-Completezza ecc)
- Qual'è la qualità della soluzione calcolata rispetto alla soluzione ottima? (Algoritmi approssimanti ecc)

La **teoria dei giochi** invece è un'area di studio che nasce dopo la pubblicazione del libro "Games and Economic Behavior", co-scritto da John Von Neumann nel 1944: il libro, per la prima volta, analizza le interazioni che avvengono tra *individui egoistici*. Tra le problematiche affrontate nella teoria dei giochi si trovano:
- Qual'è il risultato (**outcome**) dell'interazione tra agenti egoistici?
- Quali *obiettivi sociali* sono compatibili con l'egoismo dei giocatori?

Prese singolarmente le due teorie fanno ciascuna delle particolari assunzioni:
- **Teoria degli algoritmi in sistemi distribuiti**, progettazione di protocolli distribuiti dove:
	- I processori sono *obbedienti* al protocollo, sono *faulty* cioè possono fallire (progettazione di protocolli *tolleranti ai guasti*) e *avversariali*, cioè ci sono alcuni processori che cercano di far fallire il protocollo (processori *bizantini*);
	- I sistemi sono *grandi* e dispongono di risorse computazionali *limitate*: i protocolli e algoritmi progettati devono ottimizzare l'utilizzo di queste risorse;
- **Teoria dei giochi**, studio dell'interazione tra giocatori o agenti dove:
	- I giocatori sono strategici o **egoistici**, ossia ogni giocatore massimizza la sua funzione obiettivo indipendentemente da ciò che fanno gli altri agenti;
	- I sistemi studiati sono *piccoli* e dispongono di risorse *illimitate*.

Con l'avvento di Internet si sono venuti a creare molti sistemi contenti tanti utenti, ciascuno dei quali dispone di risorse limitate che deve utilizzare al fine di soddisfare determinati obiettivi egoistici. Sono quindi nati una serie di problemi in cui è di fondamentale importanza gestire bene sia gli aspetti _strategici_ degli utenti e sia quelli _computazionali_ delle risorse utilizzate. La **Algorithmic Game Theory** studia proprio questa tipologia di problemi.

L'AGT presenta quindi un ponte che connette queste due teorie tra loro. Mentre la **GT** (Game Theory), offre una serie di concetti e strumenti utili per risolvere problemi computazionali in scenari non-cooperativi, la **TA** (Theory of Algorithms) permette di interpretare e dare senso ad alcuni risultati della **GT** classica.
# Elementi di base della Teoria dei Giochi
Informalmente, un **gioco** nello scenario della teoria di giochi consiste in:
- Un insieme di *giocatori* o *agenti* egoistici;
- Un insieme di *strategie* dei giocatori che specifica cosa possono fare i possibili giocatori: chi deve agire quando e quali azioni si possono eseguire;
- Per ogni possibile combinazione di strategie dei giocatori bisogna specificare i **payoff** (guadagni) per ogni giocatori: il payoff è un valore numerico specifico per ogni giocatore che dipende dalle strategie e che indica o un guadagno o un pagamento per ogni giocatore.
La teoria dei giochi classica cerca di fare una previsione sull'**outcome** del gioco, cioè una previsione sul risultato dell'interazione tra i giocatori.
## Esempi di gioco
Per capire meglio il concetto di gioco, si vede ora un paio di esempi classici
### Esempio 1: The Prisoner's Dilemma
Due prigionieri, $\mathbf{P1}$ e $\mathbf{P2}$, sono in giudizio per aver commesso un crimine, e ciascuno di essi si ritrova a decidere se confessare la sua colpevolezza o rimanere in silenzio. Se entrambi rimangono in silenzio, le autorità non saranno in grado di dimostrare le accuse contro di essi, ed entrambi dovranno scontare una pena di $2$ anni per reati minori. Se solo uno dei due confessa, la sua pena verrà ridotta ad $1$ anno, e verrà usato come testimone per incriminare l'altro, il quale riceverà una pena di $5$ anni. Infine, se entrambi confessano, entrambi riceveranno uno sconto di pena per aver cooperato con le autorità, e dovendo scontare $4$ anni in carcere invece che $5$. I costi sostenuti dai giocatori, dipendenti dalla strategia adottata da entrambi, possono essere schematizzati nella seguente *matrice dei costi*.
![01-agt_img01|center|500](01-agt_img01.png)
Si analizza ora la variante **one-shot** del gioco, ossia dove ciascun giocatore, simultaneamente, sceglie un'azione dal suo insieme di possibili strategie. Inoltre, si fa presente come i partecipanti operino in maniera **egoistica**, adottando la strategia che possa minimizzare (o eventualmente massimizzare) i loro costi (eventualmente profitti) indipendentemente dalle scelte altrui.
L'unica soluzione stabile nel gioco è quella per cui entrambi i prigionieri confessino.
Infatti, per $P1$ si ha che:
- Nello scenario in cui $\mathbf{P2}$ sceglie di non confessare, allora per $\mathbf{P1}$ il costo viene minimizzato confessando.
- Nello scenario in cui $\mathbf{P2}$ sceglie di confessare, allora per $\mathbf{P1}$ il costo viene minimizzato confessando.
Quindi per $\mathbf{P1}$ risulta conveniente confessare *indipendentemente* dalla scelta fatta da $\mathbf{P2}$: tale strategia risulta essere **dominante**.
Analogamente, si ha per $\mathbf{P2}$ che:
- Nello scenario in cui $\mathbf{P1}$ sceglie di non confessare, allora per $\mathbf{P2}$ il costo viene minimizzato confessando.
- Nello scenario in cui $\mathbf{P1}$ sceglie di confessare, allora per $\mathbf{P2}$ il costo viene minimizzato confessando.
Quindi per $\mathbf{P2}$ risulta conveniente confessare *indipendentemente* dalla scelta fatta da $\mathbf{P1}$: tale strategia risulta anch'essa essere **dominante**.
Risulta quindi che, per entrambi, la strategia migliore sia quella di confessare indipendentemente dalla scelta dell'altro. Diventa quindi una **strategia dominante** per entrambi. In particolar modo, la coppia di strategie **(Confess,Confess)** diventa la **strategia di equilibrio dominante**. Si osserva esplicitamente che, nel caso in cui i due prigionieri potessero comunicare, sarebbe beneficiario per entrambi rimanere in silenzio, ma tale combinazione di strategia non è un equilibrio, essendo che entrambi risultano essere incentivati a cambiarla.
### Esempio 2: ISP routing game
Si hanno due Internet Service Providers $\mathbf{ISP1}$ e $\mathbf{ISP2}$ i quali si devono inviare del traffico, ciascuno di essi avente la sua propria rete. Le due reti possono scambiarsi traffico mediante due punti di transito $C$ ed $S$, che prendono il nome di peering points.
![01-agt_img02|center|500](01-agt_img02.png)
L'utilizzo di ciascun link ha costo $1$, il quale verrà pagato dall'ISP proprietario della rete a cui esso appartiene, eccetto per quelli che vanno da $S$ a $t_1$ e da $S$ a $t_2$, che non hanno costo.
In particolar modo, $\mathbf{ISP1}$ vuole inviare traffico da $s_1$ a $t_1$ mentre $\mathbf{ISP2}$ vuole inviare traffico da $s_2$ a $t_2$.
$\mathbf{ISP1}$ può adottare una delle seguenti strategie:
- passare per $S$, pagando quindi $2$;
- passare per $C$ pagando $1$ e facendo pagare $3$ ad $\mathbf{ISP2}$, essendovi $3$ link nel cammino da $C$ a $t_1$ appartenenti alla rete di proprietà di quest'ultimo Internet Service Providers.
Simmetricamente, $\mathbf{ISP2}$ può adottare una tra le seguenti strategie:
- passare per $S$, pagando quindi $2$;
- passare per $C$ pagando $1$ e facendo pagare $3$ ad $\mathbf{ISP1}$, essendovi $3$ link nel cammino da $C$ a $t_2$ appartenenti alla rete di proprietà di quest'ultimo Internet Service Providers.
I payoff per l'adozione di ciascuna coppia di strategie sono rappresentabili mediante la matrice vista per l'esempio precedente.
Anche in questo esempio, risulta evidente come la strategia di equilibrio dominante sia quella per cui ciascuno dei due ISP sceglie il percorso, dalla propria sorgente alla propria destinazione, passante per la rete dell'altro ISP.
# Giochi, strategie, costi e payoffs
## Formalizzazione di un gioco
Formalmente, un gioco consiste in un insieme di $n$ giocatori $\{1,2,...,n\}$. Ogni giocatore $i$ dispone del proprio *insieme di possibili strategie* $S_i$. Per giocare, ogni giocatore $i$ seleziona una strategia $s_i \in S_i$. Si indica con $s=(s_1,\ldots,s_n)$ il *vettore di strategie* (o *profilo di strategia*) selezionate dai giocatori e con $S=\times_iS_i$  l'insieme di tutti i possibili modi in cui i giocatori possono selezionare le strategie, ossia l'insieme di tutti i possibili vettori di strategia.
Si osserva che, dati $n$ giocatori e date $m$ possibili strategie per ciascun giocatore, ossia $|S_i|=m$ per ogni $i \in \{1,...,n\}$, allora
$$
|S| = m^n
$$
Il vettore di strategie $s \in S$ selezionato dai giocatori determina l'esito per ciascuno di essi. In generale, gli esiti saranno differenti per ogni giocatore. Per specificare il gioco risulta necessario definire, per ciascun giocatore, un *ordine di preferenza* su questi esiti, dando una relazione completa, transitiva e riflessiva sull'insieme di tutti i vettori di strategia $S$. 
Dati due elementi di $S$, tale relazione per il giocatore $a_i$, dice quali dei due esiti per le due strategie $a_i$ *preferisce debolmente*: si dice che $a_i$ preferisce debolmente $S_1$ ad $S_2$ se $a_i$ preferisce $S_1$ ad $S_2$ o se considera equivalentemente buoni i loro esiti.
La maniera più semplice per specificare una preferenza è data assegnando, per ciascun giocatore, un valore per ogni esito. In alcuni giochi risulta naturale pensare ai valori come l'utilità per i giocatori e in altri casi ai costi impiegati dai giocatori. Si denotano queste funzioni per ogni giocatore $a_i$ rispettivamente come
$$
u_i:S \longrightarrow \mathbb{R} \ \ \ \textnormal{ e } \ \ \ c_i:S\longrightarrow \mathbb{R}
$$
Chiaramente, costi e utilità possono essere usati intercambiabilmente, essendo che $u_i(s)=-c_i(s)$.
Si osserva esplicitamente che se si fosse definita, per ogni giocatore $a_{i}$, $u_i$ (equivalentemente $c_i$) come funzione di $s_i$, ossia la strategia scelta dal giocatore $i$, piuttosto che come funzione dell'intero vettore di strategie $s$ scelto da tutti gli $n$ giocatori, allora si sarebbero ottenuti $n$ problemi di ottimizzazione indipendenti. Invece, in un gioco, il payoff di ogni giocatore dipende non solo dalla sua strategia, ma anche dalle strategie scelte dagli altri giocatori.
## Equilibrio di strategia dominante
I due esempi visti in precedenza hanno una particolare proprietà: in ciascuno di questi giochi, ogni giocatore ha un'unica strategia migliore, indipendentemente dalle strategie adottate dagli altri giocatori. Si dice che un gioco ha una *strategia dominante* se possiede questa proprietà.
Più formalmente, per un vettore di strategia $s \in S$  sia $s_i$ la strategia giocata dal giocatore $a_i$ e sia $s_{-i}$ il vettore di dimensione $n-1$ delle strategie giocate da tutti i restanti giocatori. Si ha quindi che $u_i(s)=u_i(s_i,s_{-i})$.
```ad-lemma
title: Definizione
Un **equilibrio con strategia dominante** è un vettore di strategia $s^{*}=(s_1^*,s_2^*,\ldots,s_n^*)$ tale per cui $s_i^*$ è una **strategia dominante** per ogni $i$, ossia, per ogni possibile profilo di strategia alternativo $s=(s_1,s_2,\ldots,s_i,\ldots,s_n)$ si ha che:
- Se $p_i$ è un'utilità, allora $p_i(s_i^*,s_{-i}) \geq p_i(s_i,s_{-i})$
- Se $p_i$ è un costo, allora $p_i(s_i^*,s_{-i}) \leq p_i(s_i,s_{-i})$
```
Seguono alcune osservazioni.
- Per ogni giocatore, una strategia dominante è la miglior risposta per qualsiasi strategia adottata dai restanti giocatori.
- Se un gioco ha un equilibrio con strategia dominante, allora i giocatori convergeranno immediatamente ad esso.
- Non tutti i giochi hanno una equilibrio con strategia dominante
- Un equilibrio con strategia dominante può non dare ai giocatori un payoff ottimale, ossia, può esistere un profilo di strategia che ottimizzi i profitti dei giocatori.
### Una strategia dominante è unica?
Si fa presente come un gioco, in generale, non può avere più di una strategia dominante, a meno che non siano presenti strategie dominanti che forniscono ai giocatori lo stesso payoff. Tale affermazione può essere argomentata come segue.

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
```ad-lemma
title: Definizione
Un **equilibrio di Nash** è un vettore di strategia $s^{*}=(s_1^*,s_2^*,\ldots,s_n^*)$ tale per cui per ogni $i$, $s_i^*$ è la miglior risposta a $s_{-i}^*$, ossia, per ogni possibile strategia alternativa $s_i$ del giocatore $i$ si ha che:
- Se $p_i$ è un'utilità, allora $p_i(s_i^*,s_{-i}^*)\geq p_i(s_i,s_{-i}^*)$
- Se $p_i$ è un costo, allora $p_i(s_i^*,s_{-i}^*)\leq p_i(s_i,s_{-i}^*)$
```
In altre parole, nessun giocatore $i$ può migliorare il suo payoff cambiando la sua strategia da $s_i^*$ ad $s_i$, assumendo che gli altri giocatori non cambino le strategie scelte in $s^*$. Si osserva esplicitamente che nel momento in cui tale profilo di strategia venga assunto dai giocatori, è nell'interesse di tutti mantenerlo, ed è quindi definito *stabile*. Inoltre, è chiaro che un equilibrio con strategia dominante sia un equilibrio di Nash. Ancor di più, nel caso in cui il profilo di strategia sia strettamente dominante (ossia se la sua adozione migliora strettamente l'esito del gioco), allora questo è l'unico equilibrio di Nash. 

Gli equilibri di Nash possono non essere unici. Ad esempio, giochi di coordinazione possono avere più equilibri. Inoltre, possono non essere ottimali per i giocatori. Per i giochi con più equilibri di Nash, equilibri differenti possono avere payoffs differenti per i giocatori.
Si vede ora un esempio in cui vi sono più esiti che possono essere considerati stabili.
### Esempio 1: Battaglia dei sessi
Questo esempio riguarda un gioco di coordinazione. In generale, un semplice gioco di coordinazione è definito in maniera tale che due giocatori devono scegliere tra due possibili opzioni, volendo scegliere la stessa.

Si considerino due giocatori, un ragazzo ed una ragazza, i quali devono decidere su come passare il pomeriggio assieme. Considerano entrambi due possibilità: andare al cinema o andare allo stadio. Il ragazzo preferirebbe andare allo stadio, mentre la ragazza preferirebbe andare al cinema, ma entrambi vorrebbero passare il pomeriggio assieme piuttosto che separatamente. Le preferenze dei giocatori sono espressi mediante payoffs (utilità in questo caso) come segue, facendo presente che **B** è la strategia associata allo stadio mentre **S** al cinema.
![01-agt_img03|center|500](01-agt_img03.png)
Chiaramente, le due soluzioni dove i due giocatori scelgono strategie differenti sono non stabili: in ciascuno dei casi, uno dei giocatori può migliorare il suo payoff cambiando la strategia scelta. Le due opzioni rimanenti sono entrambi soluzioni stabili, infatti:
- $(B,B)$ è un equilibrio di Nash. 
- $(S,S)$ è un equilibrio di Nash.
Questo perché, fissata la strategia di un giocatore, il restante può massimizzare il suo profitto scegliendo la stessa strategia dell'altro.

La risposta ottima, o best response, di un giocatore è una strategia che produce l'esito più favorevole, per quel giocatore, in risposta ad una data combinazione di strategie degli altri giocatori.
### Esempio 2: routing congestion game
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
## Equilibrio di Nash in strategie miste
Nei giochi visti sino ad ora, vi erano esiti stabili nel senso che nessun giocatore non trarrebbe profitto dal deviare individualmente da tale soluzione. Si vede ora un esempio di gioco dove non vi è alcuna soluzione stabile.
### Esempio: matching pennies
Due giocatori, ciascuno di essi aventi una moneta, devono scegliere tra una di due strategie, testa ($H$) e croce $(T)$. Il primo giocatore vince se le due monete hanno la stessa faccia, mentre il secondo vince se le due monete hanno faccia diversa. Ciò è rappresentato nella seguente matrice dei payoff dove $1$ indica la vittoria e $-1$ la sconfitta.
![[01-ar_img05.png]]
Per ciascuna configurazione, uno dei giocatori preferisce cambiare la sua strategia, e quindi non esiste un equilibrio di Nash. Sembra quindi che ai giocatori convenga selezionare la propria strategia casualmente.

Quando un giocatore può selezionare casualmente la sua strategia, utilizzando una distribuzione di probabilità sul suo insieme di possibili strategie, vale il seguente risultato.

>Ogni gioco con un insieme finito di giocatori ed un insieme finito di strategie ammette un equilibrio di Nash di strategie miste, ossia il payoff atteso non può essere migliorato cambiando unilateralmente la distribuzione di probabilità selezionata.

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
title: Definizione (Prezzo della Stabilità)
Dato un gioco $G$ ed una funzione di scelta sociale $\mathcal{C}$, sia $S$ l'insieme di tutti gli equilibri di Nash. Se il payoff rappresenta un costo (rispettivamente, un'utilità) per un giocatore, sia $\mathbf{OPT}$ l'esito di $G$ che minimizza (rispettivamente, massimizza) $\mathcal{C}$. Allora, il Prezzo della stabilità di $G$ rispetto a $C$ è 
$$
\mathbf{PoS}_G(\mathcal{C}) = \textnormal{inf}_{s\in S} \frac{\mathcal{C}(s)}{\mathcal{C}(\mathbf{OPT})} \ \ \ \left( \textnormal{rispettivamente, } \textnormal{sup}_{s\in S} \frac{\mathcal{C(s)}}{\mathcal{C}(\mathbf{OPT})}\right)
$$
```
Ovviamente, per un gioco con un unico equilibrio di Nash, il suo prezzo dell'anarchia ed il suo prezzo della stabilità sono identici. Per un gioco con molteplici equilibri, il suo prezzo di stabilità è almeno vicino ad $1$ quanto il suo prezzo dell'anarchia, e può essere anche molto più vicino rispetto ad esso.
Un bound sul prezzo della stabilità, il quale garantisce solamente che un equilibrio sia approssimativamente ottimo, fornisce una garanzia molto più debole rispetto ad un bound sul prezzo dell'anarchia. Nonostante ciò, per alcune applicazioni un bound non banale risulta possibile solamente per il prezzo della stabilità. 
Inoltre, il prezzo della stabilità ha un'interpretazione naturale in molti giochi su reti: se si immagina che l'esito sia inizialmente progettato da un'autorità centrale per un successivo utilizzo da parte di giocatori egoisti, allora il miglior equilibrio è una soluzione ovvia da proporre. Infatti, in molte applicazioni di rete, gli agenti non sono completamente indipendenti. Piuttosto, interagiscono mediante un protocollo sottostante che propone essenzialmente una soluzione collettiva a tutti i partecipanti, i quali possono accettarla o rifiutarla. Il prezzo della stabilità permette di misurare il beneficio di tali protocolli.

Si evidenziano ora alcune osservazioni fondamentali sulle misure di inefficienza appena descritte:
- Il $\mathbf{PoA}$ e il $\mathbf{PoS}$ sono $\geq 1$ per problemi di minimizzazione e $\leq 1$ per problemi di massimizzazione.
- Il $\mathbf{PoA}$ e il $\mathbf{PoS}$ sono *piccoli* quando sono vicino ad $1$.
- Il $\mathbf{PoS}$ è almeno tanto vicino ad $1$ quanto il $\mathbf{PoA}$.
- In un gioco con un unico equilibrio, $\mathbf{PoA} = \mathbf{PoS}$.
- Il $\mathbf{PoA}$ è simile al concetto di *rapporto di approssimazione* di un'euristica.
- Una delimitazione sul $\mathbf{PoS}$ provvede una garanzia significativamente più debole rispetto ad un bound sul prezzo dell'anarchia.
- Il $\mathbf{PoS}$ quantifica la degradazione necessaria nella qualità della soluzione per il vincolo della stabilità. 

Si applicano ora le misure appena definite in un esempio, introducendo il modello di *selfish routing*.
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
```ad-lemma
title: Definizione (Flusso di equilibrio non atomico)
Sia $f$ un flusso ammissibile per l'istanza nonatomica $(G,r,c)$. Il flusso $f$ è un flusso di equilibrio se, per ogni prodotto $i \in \{1,2,\ldots,k\}$ e ogni coppia $P,\hat{P} \in \mathcal{P}_i$ di cammini $s_i-t_i$ con $f_P >0$,
$$
c_P(f) \leq c_{\hat{P}}(f)
$$
```
In altre parole, tutti i cammini usati da un flusso di equilibrio $f$ hanno il possibile costo minimo, dati la loro sorgente, il loro pozzo e la congestione causata da $f$. In particolar modo, tutti i cammini di un dato prodotto usati da un flusso di equilibrio hanno costo uguale.

**Esempio (Gioco di Pigou):**
Si consideri la rete in figura.
![01-agt_img06|center|500](01-agt_img06.png)
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
![01-agt_img07|center|500](01-agt_img07.png)
Si supponga che, per diminuire il costo pagato dal traffico, si costruisca un arco avente costo zero che collega i punti centrali dei due cammini esistenti. Il precedente equilibrio di flusso non persiste nella nuova rete: il costo del nuovo cammino $s \rightarrow v \rightarrow w \rightarrow t$ non è mai peggiore di quello dei due cammini precedenti, e risulta essere strettamente minore quando del traffico non lo utilizza. Come conseguenza di ciò, l'unico flusso di equilibrio instrada tutto il traffico nel nuovo cammino. Per via della congestione sugli archi $(s,v)$ e $(w,t)$, tutto il traffico paga due unità di costo. Il paradosso di Braess mostra come l'intuitiva idea di aggiungere un nuovo arco avente costo zero all'interno di una rete può aumentare il costo di tutto il traffico.
![01-agt_img08|center|500](01-agt_img08.png)
Il prezzo dell'anarchia nella seconda rete (e anche della stabilità, essendovi un unico equilibrio di Nash) è quindi di $4/3$, lo stesso visto nell'esempio di Pigou.
Questa non è una coincidenza, infatti
```ad-theorem
title: Teorema
Il Prezzo dell'Anarchia del Selfish Routing Game con funzione di latenza lineare, ossia della forma $ax+b$, è al più $4/3$.
```