Si vogliono studiare i limiti di algoritmi computazionalmente efficienti per la convergenza e per il calcolo di un equilibrio di Nash puro per un *congestion game*. Per fare ciò, risulta necessario far riferimento alla teoria della PLS-completezza.
# Congestion game
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
# FNP: Functional NP
Un problema in NP è un problema decisionale, definito da un certificatore polinomiale di presunte soluzioni per una data istanza, dove gli input di tale certificatore prendono il nome di *witnesses* (testimoni) dell'istanza. I testimoni hanno dimensioni polinomiali e sono definiti per le "istanze si", ossia le istanze per le quali vi è almeno un witness.
Dato quindi un problema in NP, una soluzione per un'istanza può essere "si" o "no", a seconda che per essa vi sia almeno un witness.

I problemi di calcolo di equilibri non rientrano nella categoria di problemi decisionali, perché in output richiedono un equilibrio.
Si fa riferimento quindi alla classe $FNP$, dove i problemi definiti al suo interno sono come i problemi in $NP$, ma per i quali è necessario produrre un witness per un'istanza "si". Questi problemi sono chiamati anche *problemi di ricerca*. Un algoritmo per un problema in $FNP$ prende in input un'istanza di un problema in $NP$, e restituisce un testimone per tale istanza se questo esiste (ossia, se l'istanza in input è un'istanza "si"). Se non esiste nessun testimone, allora l'algoritmo restituisce "no". 
Il problema CG-NE è quindi appartenente ad $FNP$.

Una riduzione da un problema di ricerca $L_1$ verso un problema $L_2$ è definita mediante due algoritmi polinomiali:
1. L'algoritmo $A_1$ mappa le istanze $x\in L_1$ in istanze $A_1(x)\in L_2$.
2. L'algoritmo $A_2$ mappa i witnesses di $A_1(x)$ nei witnesses di $x$, e "no" in "no".
Quindi, se $L_2$ è risolvibile in tempo polinomiale ,allora anche $L_1$ è a sua volta risolvibile in tempo polinomiale.

Per tale classe $FNP$, si ha il seguente risultato condizionato riguardante il congestion game definito in precedenza:
## Teorema
Il problema CG-NE non è $FNP$-completo a meno che $NP=coNP$
### Dimostrazione
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
Si osserva che nella dimostrazione si è usato il fatto che ogni istanza di congestion game ha almeno un equilibrio di Nash, mentre un'istanza di functional SAT non necessariamente può avere un witness.

Ci si restringe quindi ad una sottoclasse di $FNP$, nota come $TFNP$, *total functional NP*. I problemi in $TFNP$ sono quei problemi in $FNP$ tali per cui ogni istanza ha almeno un witness.
Per tale classe, si ha il seguente risultato
```ad-theorem
title: Teorema
Se un problema in $TFNP$ è $FNP-$completo, allora $NP=coNP$.
```
# Classi di complessità sintattiche e semantiche.
L'appartenenza a $TFNP$ di un problema, o in particolar modo di un gioco, preclude la possibilità di dimostrare che il calcolo di un suo equilibrio di Nash sia $FNP-$completo (a meno che $NP=coNP$). Si può pensare quindi di voler dimostrare che il problema in questione sia $TFNP-$completo, ossia difficile tanto quanto ogni altro problema in $TFNP$.
Anche tale obiettivo risulta essere troppo ambizioso, questo perché, per la struttura di tale classe, sembra che non esistano in essa problemi $TNFP-$completi. Per argomentare ciò, si fa inizialmente riferimento alle classi per le quali si è riuscito a dimostrare l'esistenza di problemi completi appartenenti ad esse, ad esempio $P,NP$ e $PSPACE$. Tutte queste classi di complessità hanno in comune l'essere *"sintattiche"*, ossia che l'appartenenza di un problema ad una di esse può essere caratterizzato mediante un modello computazionale concreto, come ad esempio macchine di Turing deterministiche o non deterministiche che operano in spazio o tempo polinomiale. Vi è quindi una ragione generale per l'appartenenza di un problema ad una data classe. Le classi di complessità sintatticamente definite hanno sempre in esse un problema "generico" completo, dove l'input è costituito da una descrizione di un problema in termini della macchina che lo accetta ed un'istanza del problema stesso, e l'obiettivo è quello di risolvere la data istanza del dato problema. 
Ad esempio, il generico problema $NP-$completo prende in input la descrizione di un certificatore, una delimitazione temporale di ordine polinomiale, e la codifica di un'istanza, e l'obiettivo è quello di decidere se esista o meno un witness, ossia una stringa che causi l'accettazione da parte del certificatore della data istanza entro il numero di passi prestabilito dalla delimitazione temporale.
La classe $TFNP$ non ha delle motivazioni generiche per le quali un problema debba appartenere ad esso. Classi di questo tipo prendono l'aggettivo di *"semantiche"*. Ad esempio, il problema *fattorizzazione* appartiene a $TFNP$, e la sua appartenenza ad essa risiede in motivazioni date dalla teoria dei numeri. Anche il problema di trovare un equilibrio di Nash in strategie miste è in $TFNP$, ma le motivazioni sono distinte dal problema fattorizzazione.  Non sembra possibile trovare un argomento generico che dimostri l'appartenenza quindi a tale classe per problemi di natura distinta.

Non è possibile quindi dimostrare che CG-NE è $TFNP-$completo. 
Bisogna far riferimento ad un sottoinsieme della classe $TFNP$, nota come $PLS$, la classe dei *polynomial local search problems,* o *abstract local search problem*.
I testimoni di un'istanza di un problema di $PLS$ sono i suoi ottimi locali.
Il problema CG-NE rientra nella classe $PLS$.
Essendo tale classe sottoinsieme di $TFNP$, ogni istanza di un problema $PLS$ ha almeno un testimone.
# Maximum Cut Problem
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

Si osserva che il terzo algoritmo della descrizione di un problema in $PLS$ è un verificatore efficiente di witnesses. Infatti tale algoritmo, quando riceve in input una soluzione non ottima localmente, non restituisce semplicemente "no" come un verificatore di un problema in $NP$, ma fornisce una soluzione alternativa con un miglior valore della funzione obiettivo.

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
```ad-lemma
title: Definizione (Problema $PLS$-completo)
Un problema $L$ è $PLS$-completo se $L \in PLS$ ed ogni problema in $PLS$ è riducibile ad esso, ossia, $\forall L^{'}\in PLS$
$$
L^{'} \leq_p L
$$
```
Per definizione quindi, esiste un algoritmo polinomiale per risolvere un problema $PLS$-completo se e solo se ogni problema in $PLS$ può essere risolto in tempo polinomiale.
Un problema $PLS$-completo è un singolo problema di ricerca locale che, simultaneamente, codifica ogni problema di ricerca locale.

```ad-theorem
title: Teorema
Calcolare un massimo locale per un'istanza di maximum cut con pesi degli archi generici non negativi è un problema $PLS$-completo.
```

Indipendentemente dal fatto che ogni problema in $PLS$ possa o non possa essere risolto in tempo polinomiale, la dimostrazione del teorema appena enunciato implica che l'algoritmo di ricerca locale per max cut richieda tempo esponenziale nel caso peggiore.

```ad-theorem
title: Teorema
Calcolare un massimo locale per un'istanza di maximum cut con pesi degli archi generici non negativi usando la ricerca locale può richiedere un numero esponenziale in $|V|$ di iterazioni, indipendentemente da come venga scelta una mossa locale in ogni iterazione.
```

Si osserva che ogni istanza di un problema $PLS$ ha almeno un testimone, questo perché per definizione, la ricerca locale si deve per forza fermare ad un ottimo locale. $PLS$ è allora sottoinsieme di $TFNP$, e quindi nessuno dei problemi appartenenti ad esso, come il calcolo di un equilibrio di Nash puro di un congestion game, può essere $FNP-$completo, a meno che $NP=coNP$. 

Inoltre, si osserva che al contrario di $TFNP$, i problemi in questa classe possiedono una ragione generica per l'appartenenza ad essa, ossia che sono risolvibili mediante una procedura di ricerca locale.
# $PLS-$completezza per Congestion Game
Calcolare un equilibrio di Nash puro di un congestion game è tanto difficile quanto ogni altro problema di ricerca locale.
## Teorema
Il problema di calcolare un equilibrio di Nash puro per un congestion game (CG-NE) è $PLS-$completo.
### Dimostrazione
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
# Complessità del calcolo di un equilibrio di Nash in strategie miste - $PPAD$

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
```ad-theorem
title: Teorema
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
![08-agt_img02|center|500](08-agt_img02.png)
```ad-theorem
title: Teorema
Calcolare un equilibrio di Nash misto per un bimatrix game è $PPAD-$completo.
```