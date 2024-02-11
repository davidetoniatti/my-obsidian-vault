Con **Meccanismi senza pagamenti** si intendono quei meccanismi in cui il sistema non può pagare gli agenti per convincerli a riportare il tipo reale. Dunque un tale meccanismo è essenzialmente un algoritmo che date le preferenze degli agenti calcola un outcome.
Questa tipologia di meccanismi esiste e viene studiata perché esistono degli scenari in cui non si possono usare dei soldi per convincere gli agenti a cooperare. Ad esempio nei sistemi di voto o nei sistemi di donazione degli organi non si possono fare pagamenti per motivi legali.
In generale, progettare un meccanismo senza poter pagare gli agenti comporta grandi limitazioni: la ricerca ha mostrato forti risultati di impossibilità (vedi [Teorema di Arrow]). Ciò nonostante, ci sono alcuni problemi specifici per cui esistono meccanismi truthful senza pagamenti.
# House allocation problem & Top Trading Cycle algorithm
## House allocation problem
L'**House allocation problem** è un problema in cui ci sono $N$ agenti, ognuno dei quali possiede inizialmente una casa. Ogni agente $a_{i}$ ha un *ranking* di preferenze rispetto alle case degli altri, rappresentato da un ordinamento totale sulle $n$ case piuttosto che da valutazioni numeriche. In particolare, non è necessario che un agente preferisca la propria casa rispetto alle altre. 

L'obiettivo del problema è trovare una nuova allocazione delle case agli agenti in modo tale da *renderli più felici* rispetto alle preferenze. Dunque si deve progettare un algoritmo che rialloca le case con tale obiettivo; tale algoritmo deve essere **truthful**, cioè a tutti gli agenti deve convenire dichiarare le reali preferenze.

Per risolvere il problema ottenendo i due obiettivi, si utilizza un algoritmo detto **Top Trading Cycle (TTC)**.
## Top Trading Cycle Algorithm (TTC)
L'allocazione delle case avviene per fasi. In particolare, ad ogni iterazione:
- tutti gli agenti rimasti, cioè gli agenti a cui non è stata assegnata una nuova casa, partecipano all'allocazione con la propria casa iniziale;
- tutti gli agenti rimasti, *puntano* (indicano) la casa che preferiscono fra quelle rimaste (cioè quelle ancora non assegnate); questo passaggio genera un grafo diretto, dove ogni agente $a_i$ è un nodo e un arco diretto $(i,l)$ indica che la casa preferita di $a_i$ fra quelle rimaste è la casa dell'agente $a_l$. Si osserva che ogni agente deve puntare alla casa preferita, dunque ogni nodo ha un arco uscente: se la casa preferita dell'agente $a_i$ è proprio la sua casa iniziale, allora il grafo conterrà l'arco $(i,i)$.
- si guardano i cicli formati nel grafo e si scambiano le case come suggerito da questi cicli.
- gli agenti che si sono scambiati le case, cioè quelli facenti parte dei cicli, vengono rimossi.
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
Sia $N_k$ l'insieme degli agenti rimossi all'iterazione $k$ dell'algoritmo TTC, cioè gli agenti nei cicli individuati in tale iterazione.
Ogni agente in $N_k$ riceve la casa preferita al di fuori di quelle già allocate agli agenti in $N_1 \cup \dots \cup N_{k-1}$. Inoltre, tale casa appartiene inizialmente ad un agente che viene servito all'iterazione $k$, cioè un agente in $N_k$.
```
Si guarda ora l'aspetto *game teoretico* dell'algoritmo (o meccanismo). In particolare, un agente potrebbe dichiarare un ranking differente in modo tale che gli venga assegnata una casa che rispetto al suo reale ranking è migliore. Con questo algoritmo ciò non avviene, infatti si dimostra che l'algoritmo TTC è truthful, in particolare si dimostra che è sempre strategia dominante dichiarare il ranking reale per ogni agente.
#### Teorema
Usando l'algoritmo TTC per allocare le case, per ogni agente è strategia dominante dichiarare la verità.
##### Dimostrazione
Sia $a_i$ un agente fissato e siano fissate le dichiarazioni degli altri agenti.
Si assume che $a_i$ riporta la verità e sia $N_k$ l'insieme degli agenti che vengono rimossi all'iterazione $k$. Si supponga che $a_i \in N_j$, dunque ad $a_i$ viene assegnata una casa in $N_j$.
L'agente $a_i$ non può, cambiando il ranking, ottenere una casa migliore.
Infatti, per il lemma precedente, $a_i$ ottiene la casa migliore nel ranking rispetto a quelle che rimangono all'iterazione $j$. Inoltre, $a_i$ non può mentire per ottenere una casa migliore che è stata assegnata nelle iterazioni precedenti, infatti:
- per ogni $k = 1,\dots,j-1$ nessun agente $a_l \in N_k$ punta alla casa dell'agente $a_i$. Se cosi non fosse, allora $a_i$ apparterebbe allo stesso ciclo di $a_l$, dunque apparterrebbe ad $N_k$ invece che a $N_j$;
- nessun agente $a_l \in N_k$ punta alla casa dell'agente $a_i$ in iterazioni precedenti a $k$. Se cosi non fosse, allora $a_l$ punterebbe ad $a_i$ anche all'iterazione $k$.
Dunque $a_i$ non può far parte di nessun ciclo che coinvolge agenti in $N_1 \cup \dots \cup N_{j-1}$.
### Una riallocazione ottimale
Si osserva che il teorema precedente non è così sorprendente. Infatti, per esempio, anche il meccanismo che non rialloca mai le case è truthful.
La vera caratteristica importante dell'algoritmo TTC è che, oltre a garantire la truthfulness, garantisce una allocazione delle case che è la migliore possibile, *ottimale*. Si chiarisce formalizzando questa proprietà.
```ad-Definizione
title: Definizione (Blocking Coalition)
Data una qualsiasi allocazione delle case agli agenti, un sottoinsieme di agenti forma una **blocking coalition** per questa allocazione se possono scambiarsi le case originali (cioè quelle possedute inizialmente da ogni agente) in modo tale che *nessuno* di loro peggiora e *almeno uno* di loro migliora.
```
Una allocazione che presenta delle blocking coalition non è una allocazione *stabile*, perché gli agenti della coalizione possono scambiarsi le case iniziali per migliorare.
```ad-Definizione
title: Definizione (Core Allocation)
Una **core allocation** è una allocazione senza nessuna blocking coalition.
```
Una core allocation è una allocazione *stabile*, cioè l'allocazione *ottimale*: gli agenti non possono scambiarsi le case iniziali per migliorare.

Dunque si può dimostrare il teorema seguente.
#### Teorema
Per ogni House Allocation Problem, l'allocazione calcolata dall'algoritmo TTC è l'*unica core allocation*.
##### Dimostrazione
Per dimostrare il teorema, si dimostrano due claim.
###### Claim 1: ogni allocazione diversa da quella del TTC non è una core allocation
Si dimostra che fissata una allocazione diversa da quella del TTC esiste una blocking coalition in tale allocazione.
Nell'allocazione del TTC, ogni agente in $N_1$ riceve la prima scelta nel ranking. Quindi, gli agenti in $N_1$ formano una blocking coalition per ogni allocazione che si discosta dall'allocazione fatta dal TTC per un agente in $N_1$. Dunque ogni core allocation deve essere uguale all'allocazione del TTC per gli agenti in $N_1$. Similmente, ogni agente in $N_2$ riceve la prima scelta nel ranking al di fuori delle case in $N_1$. Quindi, gli agenti in $N_2$ formano una blocking coalition per ogni allocazione che si discosta dall'allocazione fatta dal TTC uguale a quella su $N_1$, per un agente in $N_2$. Dunque ogni core allocation deve essere uguale all'allocazione del TTC per gli agenti in $N_1$ e $N_2$. Continuando induttivamente, si conclude che ogni allocazione che si discosta da quella dall'allocazione del TTC non è una core allocation.
###### Claim 2: L'allocazione trovata da TTC è una core allocation
Sia $S$ un sottoinsieme arbitrario di agenti. Si consideri una riallocazione interna delle case di proprietà degli agenti in $S$. Questa riallocazione partiziona $S$ in cicli diretti. Sia $C$ uno di questi cicli diretti.
- Se $C$ contiene agenti di $N_j$ e $N_k$ con $j < k$ allora dalla riallocazione almeno un agente $a_i \in N_{j}$ ottiene una casa che è posseduta da un'agente in $N_k$, dunque $a_i$ peggiora rispetto all'allocazione del TTC;
- Se $C$ contiene tutti agenti in $N_k$, ogni agente che non riceve la sua casa favorita tra quelle in $N_k$ peggiora rispetto all'allocazione del TTC.
Si conclude che se la riallocazione interna delle case in $S$ si discosta dall'allocazione calcolata dall'algoritmo TTC, allora almeno un agente in $S$ peggiora. Siccome $S$ è arbitrario, l'allocazione TTC non ha blocking coalitions ed è una core allocation.
# Case study: Kidney exchange
## Background
Molte persone soffrono di insufficienza renale e necessitano di un trapianto di rene. Negli Stati Uniti, più di 100.000 persone sono in lista d'attesa per tali trapianti. Un idea per risolvere questo problema, utilizzata anche per altri organi, riguarda i donatori deceduti: quando qualcuno muore ed è un donatore di organi registrato, i loro organi possono essere trapiantati in altre persone. Una caratteristica speciale dei reni è che una persona sana ne ha due e può sopravvivere tranquillamente con uno solo. Questo crea la possibilità di donatori viventi, come un familiare del paziente bisognoso.
Sfortunatamente, avere un donatore vivente non è sempre sufficiente; infatti, in generale, una coppia paziente-donatore può essere *incompatibile*, cioè il rene del donatore molto probabilmente non funzionerà correttamente nel paziente.
Il problema principale sta nella compatibilità rispetto al gruppo sanguigno. Per esempio, un paziente di tipo 0 può ricevere un rene solo da un donatore con lo stesso gruppo sanguigno; similmente un donatore di tipo AB può solo donare ad un paziente di tipo AB. Un idea per risolvere questo problema è lo *scambio dei reni*, meglio noto come *kidney exchange*.

Siano $(P1,D1)$ e $(P2,D2)$ due coppie paziente-donatore. Si supponga che il paziente $P1$ non è compatibile col suo donatore $D1$ perché sono di tipo $A$ e $B$ rispettivamente. Si supponga invece che il paziente $P2$ non è compatibile col suo donatore $D2$ perché sono di tipo $B$ e $A$ rispettivamente. Allora è ragionevole far scambiare i donatori, cioè far ricevere a $P1$ il rene di $D2$ e far ricevere a $P2$ il rene di $D1$. Questa è l'idea di base della *kidney exchange*.
![|center|500](07-agt_img03.png)
Alcuni scambi di reni sono stati effettuati all'inizio di questo secolo. Questi successi isolati hanno evidenziato la necessità di uno scambio nazionale di reni, in cui coppie paziente-donatore incompatibili possono registrarsi e essere abbinati ad altri, con l'obiettivo di consentire il maggior numero possibile di abbinamenti.
Attualmente, il compenso per la donazione di organi è illegale negli Stati Uniti e in ogni paese tranne l'Iran. Invece, lo scambio di reni è legale e viene naturalmente modellato come un problema di mechanism design senza pagamenti.
## Algoritmo TTC per Kidney Exchange
Una prima idea è quella di modellare il problema del *kidney exchange* come un problema di *house allocation*. L'idea è di considerare ogni coppia paziente-donatore come un'agente, dove il donatore incompatibile corrisponde alla casa che viene inizialmente assegnata all'agente nell'house allocation problem. Dunque ogni paziente definisce un ordinamento totale sui donatori, cioè un *ranking* dei donatori rispetto alla probabilità di successo del trapianto, basato sul gruppo sanguigno e altri fattori.
A questo punto è sufficiente applicare l'algoritmo TTC per trovare una reallocazione dei donatori. Per le proprietà dell'algoritmo, la reallocazione suggerita può solamente migliorare la probabilità di successo del trapianto al paziente.
In particolare, il risultato migliore si ottiene quando l'algoritmo TTC trova cicli come quello nella figura successiva, che corrisponde al kidney exchange nella figura precedente.
![|center|500](AGT/attachments/07-agt_img04.png)
Il problema principale dell'applicazione dell'algoritmo TTC è che potrebbe suggerire riallocazioni che utilizzano cicli lunghi, come mostrato nella Figura seguente.
![|center|500](07-agt_img05.png)
Infatti un ciclo di lunghezza due corrisponde già a quattro interventi chirurgici: due per estrarre i reni dai donatori e due per impiantarli nei pazienti. Inoltre, questi quattro interventi devono avvenire simultaneamente. Infatti se, ad esempio, gli interventi chirurgici per $P1$ e $D2$ avvengono per primi, c'è il rischio che $D1$ possa ritirare la sua offerta di donare il suo rene a $P2$ (è illegale stipulare un contratto per una donazione di organi, un donatore può rifiutarsi in qualsiasi momento).
Dunque da una parte, $P1$ ottiene un rene *gratuitamente*. Il problema molto più serio è che $P2$ non può più partecipare ad uno scambio di reni, dato che il suo donatore $D2$ ha donato. A causa di questo rischio, gli interventi chirurgici non simultanei non sono quasi mai utilizzati negli scambi di reni.
Dato che far avvenire tante operazioni simultaneamente in un ospedale è molto dispendioso, è fondamentale ottenere dei cicli di lunghezza corta.
Un altro problema dell'approccio con TTC è che modellare le preferenze dei pazienti come un ordinamento sui donatori è eccessivo; è sufficiente modellarle come preferenze binarie.
## Modellazione come graph matching
I due obiettivi di avere cicli corti e preferenze di tipo binario suggeriscono la modellazione del problema come un problema di **graph matching**.
Un *matching* in un grafo non diretto è un sottoinsieme di archi che non condividono alcun nodo. 
In questo caso, il grafo è dato dall'insieme dei vertici $V$ corrispondente alle coppie paziente-donatore incompatibili (un vertice per coppia) in cui esiste un arco non diretto tra due nodi $\{(P1,D1),(P2,D2)\}$ se e soltanto se $P1$ e $D2$ sono compatibili e $P2$ e $D1$ sono compatibili. Il grafo corrispondente all'esempio iniziale è il seguente.
![|center|500](07-agt_img06.png)
Dunque un matching in tale grafo corrisponde ad un'insieme di coppie che possono effettuare uno scambio: ogni scambio implica quattro interventi simultanei.
Allora l'obiettivo è massimizzare il numero di coppie che possono effettuare uno scambio, che corrisponde a trovare il matching di dimensione massima nel grafo.
Inoltre, si assume che ogni paziente $i$ abbia un insieme $E_i$ di donatori compatibili che appartengono ad coppie paziente-donatore, e che $i$ può dichiarare come donatori compatibili un qualsiasi sottoinsieme $F_i \subseteq E_i$ al meccanismo. Questa assunzione è ragionevole dato che un paziente può rifiutare uno scambio per qualsiasi motivo; inoltre nessun paziente può dichiarare un insieme che contiene dei donatori non compatibili.
Un algoritmo (meccanismo) che massimizza li numero di kidney exchange rispetto alle dichiarazioni $E_i$ è il seguente.
![|center](07-agt_img07.png)
Tale meccanismo è truthful se per ogni paziente $i$ è strategia dominante dichiarare completamente l'insieme $E_i$ al meccanismo. In particolare, la truthfulness del meccanismo dipende da come viene scelto il matching massimo tra quelli possibili:
- Se due matching massimi differenti accoppiano lo stesso insieme di vertici, allora è sufficiente sceglierne uno. Infatti, i pazienti non sono interessati al donatore scelto per lui fintantoché è compatibile;![|center|500](07-agt_img08.png)
- Più significativo il caso in cui due matching massimi accoppiano un insieme di nodi differente. Nella figura seguente, un nodo si trova in ogni matching massimo, ma solo uno degli altri tre nodi può essere accoppiato con lui: come scegliere questo nodo? 
![|center|300](07-agt_img09.png)

Una soluzione è quella di dare delle priorità alle coppie paziente-donatore prima che il meccanismo inizi. La maggior parte degli ospedali si affida già a schemi di priorità per gestire i propri pazienti. La priorità di un paziente in lista d'attesa è determinata dalla durata del tempo in cui è stata in lista, dalla difficoltà di trovare un rene compatibile e da altri fattori.
In dettaglio, si implementa il terzo passo del meccanismo nel modo seguente, supponendo che i vertici $V = \{1, 2, \dots n\}$ di $G$ siano ordinati dalla priorità più alta a quella più bassa.
![|center](07-agt_img10.png)
Dunque, dopo aver calcolato l'insieme dei matching massimi di $G$, si considerano i vertici secondo la priorità; si verifica in ogni iterazione $i$ se esiste un matching massimo che:
1. soddisfa gli accoppiamenti già stabiliti per i pazienti con maggiore priorità;
2. accoppia il paziente $i$.
Se tale matching esiste, allora si include il paziente $i$ nel matching finale.
Se tale matching non esiste, cioè se gli accoppiamenti già stabili precludono l'accoppiamento di $i$ con un donatore, si esclude $i$ dal matching e si considera il paziente successivo nell'ordinamento.
Allora per induzione su $i$, $M_i$ è un sottoinsieme non vuoto di matching massimi di $G$, e $M_n$ contiene dei matching che accoppiano lo stesso insieme di vertici, dunque la scelta del matching finale tra quelli di $M_n$ è irrilevante.

Si dimostra che usando questo meccanismo basato su priorità, per ogni paziente $i$ è strategia dominante dichiarare l'intero insieme $E_i$ dei donatori compatibili.
### Il meccanismo con priorità è truthful
Nel meccanismo con priorità per il kidney exchange, per ogni paziente $i$ e per ogni dichiarazione fatta dagli altri pazienti, nessuna dichiarazione $F_i \subset E_i$ porta ad un outcome migliore per $i$ rispetto a dichiarare $F_{i} = E_i$.
#### Dimostrazione
Sia $M$ il matching scelto dal meccanismo quando ogni paziente $i$ dichiara $F_i = E_i$. Sia $i$ un agente che, dichiarando $F_i = E_i$, non viene accoppiato in $M$; allora $i$ decide di sotto dichiarare $F_i \subset E_i$.
Si osserva che in seguito a tale azione:
- Non si creano nuovi matching, dato che non vengono aggiunti archi: potenzialmente alcuni matching potrebbero non esistere più;
- Il matching $M$ continua ad esistere, dato che non contiene nessun arco rimosso dalla sotto dichiarazione di $i$; se cosi non fosse, allora $i$ risulterebbe accoppiato quando dichiara $F_i = E_i$.
Siccome nessun nuovo matching viene creato e il matching $M$ continua ad esistere, allora il meccanismo sceglie $M$ anche quando $F_i \subset E_i$, escludendo nuovamente $i$ dal matching.
Dunque ad ogni paziente $i$ non accoppiato dal meccanismo non conviene sotto dichiarare $E_i$.
## Incentivi per gli ospedali
Nel mondo reale, le coppie paziente-donatore vengono segnalate nel sistema nazionale per lo scambio dei reni da parte degli ospedali.
In generale, l'obiettivo dell'ospedale di accoppiare il maggior numero dei suoi pazienti non è perfettamente in linea con l'obiettivo sociale di accoppiare il maggior numero di pazienti su tutti quanti. Si fanno degli esempi per evidenziare le differenze.
### Esempio 1. Riportare tutte le coppie
Siano $H_1,H_{2}$ due ospedali, ognuno con tre coppie paziente-donatore, come in figura.
![|center|500](07-agt_img11.png)
Ogni arco collega coppie che sono mutualmente compatibili. Ogni ospedale ha due coppie paziente-donatore che si accoppiano internamente, cioè le due coppie risiedono nello stesso ospedale. Allora gli ospedali potrebbero non riportare queste coppie nel sistema nazionale, e effettuare l'accoppiamento e lo scambio internamente. Così facendo però non si potrebbero risolvere tutti gli accoppiamenti possibili. Nell'esempio, non riportando al sistema nazionale le coppie 1,2,5 e 6, le coppie 3 e 4 rimangono disaccoppiate. Se invece i due ospedali riportano tutte le coppie, allora 1,2 e 3 possono essere accoppiati con 4,5 e 6 rispettivamente e tutti i pazienti possono ricevere un rene nuovo.
In generale, l'obiettivo è incentivare gli ospedali a dichiarare tutte le coppie paziente-donatore per salvare il maggior numero di vite.
### Esempio 2: risultato di impossibilità
Si considerano nuovamente due ospedali, con sette pazienti distribuiti come in figura.
![|center|500](07-agt_img12.png)
Sia inoltre il meccanismo di exchange in grado di calcolare il matching massimo rispetto alle coppie dichiarate dagli ospedali.
Si osserva che con un numero dispari di coppie, ogni matching lascia un paziente non accoppiato.
- se $H_1$ riporta solo il paziente 7 e $H_2$ riporta tutti i pazienti, allora l'ospedale $H_1$ accoppia ogni paziente: l'unico matching massimo è quello che accoppia 6 con 7 (e 4 con 5) e $H_1$ può accoppiare 2 con 3 internamente;
- se $H_2$ riporta solo 1 e 4 e $H_1$ riporta tutti i pazienti, allora $H_2$ accoppia ogni paziente: l'unico matching massimo è quello che accoppia 1 con 2 e 4 con 3, mentre $H_2$ può accoppiare 5 con 6 internamente.
Dunque si può concludere che, a prescindere dal matching massimo calcolato dal sistema, almeno un ospedale ha convenienza a mentire riportando parzialmente le proprie coppie.

Questo è un *risultato di impossibilità*, per cui non esiste nessun meccanismo veritiero (per cui è strategia dominante dichiarare tutte le coppie nel proprio ospedale) che calcola sempre un matching massimo.

La ricerca attuale sulla progettazione di meccanismi per la segnalazione ospedaliera prende in considerazione vincoli di incentivazione più flessibili e una ottimalità approssimata.