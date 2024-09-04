Si studia in che modo sintetizzare le informazioni in possesso dei singoli individui in una rete la fine di derivare una **singola informazione cumulativa**, che  permetterà di prendere una decisione fra una serie di alternative, fra le quali si vuole scegliere la migliore, o stilare una graduatoria di esse. Nel fenomeno dell'[Herding](07%20-%20Seguire%20il%20gregge.md), un individuo prende una decisione sulla base delle scelte degli individui che lo precedono nella scelta, e questo può indurre a decisioni sbagliate, ossia, non sempre è buona idea seguire il comportamento della massa. Nei **sistemi di voto** si vuole estrapolare una informazione dalla rete tramite una votazione che, in qualche modo, rispecchi le informazioni della rete e in cui i singoli individui decidono in maniera del tutto **indipendente** dagli altri.
Dunque ci si pone nello scenario nel quale un insieme di individui deve prendere una decisione di gruppo, ricevendo ciascuno di essi un *segnale privato* e prendendo *decisioni individuali*: ciascun individuo effettua una scelta senza osservare il comportamento degli altri individui.
# Un nuovo modello di decision making
1. Ogni individuo deve prendere una decisione fra due alternative: $X$ o $Y$. Una delle due scelte è la scelta *giusta*, l'altra quella *sbagliata*; in particolare, se $X$ è la scelta giusta si scriverà $X>Y$ e viceversa. Le due alternativa, a priori, sono equiprobabili $\mathbf{Pr}(X>Y)=\mathbf{Pr}(Y>X) = \frac{1}{2}$.
2. Ogni individuo riceve un segnale privato: $x$ o $y$; dove $$ \mathbf{Pr}(x|X>y) = \mathbf{Pr}(y|Y>X) = q > 1/2 $$
3. Ogni individuo sceglie in accordo al proprio segnale privato;
4. Ciascun individuo opera la propria scelta fra $X$ e $Y$, la scrive su una scheda che poi inserisce in un'urna;
5. Infine, viene presa una decisione collettiva fra $X$ e $Y$, sulla base delle scelte dei singoli individui.
Si deve determinare il criterio con cui viene stabilita tale decisione collettiva. Vale il seguente risultato

```ad-Teorema
title: Teorema della giuria di Condorcet
Nel modello di decision making appena definito, se $X>Y$ e il numero di individui coinvolti nella decisione collettiva è $k$, allora quasi sicuramente
$$
\lim_{k\rightarrow \infty} \frac{|\{i: 1 \leqslant i \leqslant k \land r_i = X\}|}{k} = q
$$
ovvero
$$
\lim_{k\rightarrow \infty} \mathbf{Pr}\left(\frac{|\{i: 1 \leqslant i \leqslant k \land r_i = X\}|}{k} = q\right) = 1
$$
dove $r_i$ è la scelta dell'individuo $i$.
```

- Il teorema dimostra che, qualora si dovesse prendere una decisione collettiva in accordo alla scelta della maggioranza di individui, la decisione collettiva porterebbe a scegliere l'alternativa *giusta*: il modello di decision making appena definito è un esempio nel quale si manifesta "la saggezza della folla";
- Il teorema è valido nel modello di decision making appena descritto, nel quale è previsto che ogni individuo sceglie in accordo al proprio segnale privato. Tale decisione è perfettamente ragionevole, in quanto nel modello si assume che il segnale in favore della scelta *giusta* sia quello più probabile.
- Tuttavia, anche sotto tale ipotesi, si può verificare lo scenario nel quale un individuo ritenga migliore la scelta in disaccordo al suo segnale privato (già visto nell'Herding).
## Voto non sincero
Si vuole quindi studiare come il voto *non sincero*, ossia in disaccordo con il proprio segnale privato, abbia senso anche quando si debba prendere una decisione collettiva e ciascun individuo scelga **senza** osservare gli altri. Ossia, si vuole studiare se esistono casi in cui il voto non sincero di un individuo può massimizzare la probabilità che la scelta collettiva sia quella *giusta*.
### Esempio
Si torna al gioco delle due urne, nel quale si ha un'urna $UG$ contenente 10 palline gialle e un'urna $UV$ contenente 9 palline verdi e una gialla.
Si sceglie una delle due urne u.a.r., e le regole del gioco sono un po' diverse:

- Ci sono 3 giocatori, ciascuno estrae una pallina dall'urna e la reinserisce, senza comunicare l'esito dell'estrazione;
- Dopo che i 3 giocatori hanno estratto ciascuno una pallina, simultaneamente, dichiarano la propria scelta;
- se la maggioranza ha indovinato di quale urna si tratti, tutti e 3 vincono un premio, altrimenti nessuno vince.

Le differenze importanti rispetto al gioco nell'herding sono:

- Le risposte **simultanee** invece che sequenziali;
- Una decisione **collettiva** a maggioranza in cui o vincono tutti o perdono tutti;

Si consideri ora la strategia di un giocatore rispetto al suo segnale privato:
- se estrae giallo, allora $$ \mathbf{Pr}(UG|g) = \frac{\mathbf{Pr}(g|UG)\mathbf{Pr}(UG)}{\mathbf{Pr}(g|UG)\mathbf{Pr}(UG) + \mathbf{Pr}(g|UV)\mathbf{Pr}(UV)} = \frac{\frac{10}{10}\frac{1}{2}}{\frac{10}{10}\frac{1}{2}+\frac{1}{10}\frac{1}{2}} = \frac{10}{11} $$
- se estrae verde, allora $$ \mathbf{Pr}(UV|v) = \frac{\mathbf{Pr}(v|UV)\mathbf{Pr}(UV)}{\mathbf{Pr}(v|UV)\mathbf{Pr}(UV) + \mathbf{Pr}(v|UG)\mathbf{Pr}(UG)} = \frac{\frac{9}{10}\frac{1}{2}}{\frac{9}{10}\frac{1}{2}+0} = 1 $$
Si vuole capire il ragionamento che uno dei giocatori deve fare al fine di dare una risposta che massimizzi la probabilità di vincere. Sia $G$ un giocatore: $G$ sa che tutti e tre vincono se almeno due danno la risposta corretta, allora si chiede quale sia il caso in cui la sua risposta risulti *veramente influente* ai fini della vittoria. Si osserva che, se la risposta degli altri due giocatori sono concordi, allora la risposta di $G$ non è influente in quanto non modifica la maggioranza indipendentemente dalla risposta. Allora l'unico caso in cui la risposta di $G$ è influente è quando le risposte degli altri due giocatori sono *discordi*. 
Si osserva che , se le risposte degli altri due giocatori sono discordi, allora uno di essi ha risposto $UV$, ossia, assumendo che gli altri due giocatori rispondano *sinceramente* (in accordo ai propri segnali privati), le risposte di tali giocatori sono discordi quando uno di loro estrae verde, ossia, quando l'urna è sicuramente $UV$.
Quindi, indipendentemente dalla pallina estratta da $G$, assumendo che gli altri due giocatori rispondano sinceramente, la risposta di $G$ è influente ai fini della vittoria *solo* quando l'urna è $UV$: ciò implica che a $G$ conviene rispondere sempre $UV$ indipendentemente dall'estrazione.
Naturalmente, se gli altri due giocatori non rispondono sinceramente, la strategia si fotte.
### Voto non sincero nelle giurie - unanimità
Si studia ora uno scenario nel quale, al fine di prendere una decisione collettiva, tutti i partecipanti debbano convergere sulla stessa scelta.
Si consideri una giuria che deve decidere se un imputato è innocente $I$ o colpevole $C$. A priori, si assume che
$$
\mathbf{Pr}(I) = \mathbf{Pr}(C) = \frac{1}{2}  
$$
Ciascun giurato riceve un segnale $c$ o $i$ tali che
$$
\mathbf{Pr}(c|C) = \mathbf{Pr}(i|I) = q > \frac{1}{2}  
$$
Si devono aggregare i voti dei giurati per giungere ad un verdetto di colpevolezza o innocenza, e per condannare un imputato è necessario che l'insieme $S$ dei segnali ricevuti dai giocatori siano indice di colpevolezza con probabilità molto alta. In particolar modo, si vuole che
$$
\mathbf{Pr}(C|S) \gg \frac{1}{2} 
$$
Si assume quindi che per condannare un imputato è necessario che **tutti** i giurati votino a favore della condanna.
Ci si chiede quando la risposta di un giurato $G$ risulti veramente influente ai fini del verdetto:

- se fra tutti gli altri giurati ne esiste uno che vota a favore dell'innocenza, allora il voto di $G$ è ininfluente, indipendentemente da quale esso sia;
- se fra tutti gli altri giurati, nessuno vota a favore dell'innocenza, ossia, tutti votano a favore della colpevolezza, allora è proprio il voto di $G$ a decidere il verdetto.

Si supponga che la giuria sia composta da $k$ giurati, e che tutti tranne $G$ votano in accordo ai propri segnali. Si calcola la probabilità che l'imputato sia colpevole quando un solo giurato riceve il segnale $i$:
$$
\begin{align}
\mathbf{Pr}(C|ic^{k-1}) &= \frac{\mathbf{Pr}(ic^{k-1}|C)\mathbf{Pr}(C)}{\mathbf{Pr}(ic^{k-1}|C)\mathbf{Pr}(C) + \mathbf{Pr}(ic^{k-1})\mathbf{Pr}(I)} \\
&= \frac{(1-q)q^{k-1} \cdot \frac{1}{2}}{(1-q)q^{k-1} \cdot \frac{1}{2} + q(1-q)^{k-1} \cdot \frac{1}{2}} \\
&= \frac{q^{k-2}}{q^{k-2}+(1-q)^{k-1}}
\end{align}
$$
Dunque, essendo $q> \frac{1}{2}$ e quindi $\frac{1-q}{q} < 1$, vale
$$
\lim_{ k \to \infty } \mathbf{Pr}(C|ic^{k-1})  = \lim_{ k \to \infty } \frac{1}{1+\left( \frac{1-q}{q} \right)^{k-2}} = 1
$$
ossia, al crescere del numero dei giurati, se uno solo di esso riceve il segnale $i$, allora l'imputato è colpevole quasi sicuramente: è meglio che $G$ voti *colpevole*.
# Sistemi di voto
Si sono considerati due metodi per giungere ad una decisione collettiva partendo da un insieme di scelte individuali: la maggioranza e l'unanimità per una delle due alternative. Ciò è stato fatto nel caso in cui le alternative sono due.
In generale, possono venir proposte più scelte fra le quali esprimere una preferenza, e talvolta, può essere più interessante o necessario stilare una classifica o **ranking** piuttosto che decretare un vincitore. Possono essere diverse, oltre a maggioranza e unanimità, le regole per derivare una decisione collettiva partendo da un insieme di decisioni individuali. Tali regole prendono il nome di **sistemi di voto**.
## Voto individuale
Si formalizza il concetto di sistema di voto. Si ha
- $A = \{ a_{1},a_{2},\dots, a_{n} \}$, o $A=[n]$ un insieme di alternativa;
- $V = \{ v_{1},v_{2},\dots,v_{k} \}$, o $V=[k]$ un insieme di votanti;
Ciascun votante $v_h \in V$ esprime il suo voto in una di due forme possibili:
1. Nella forma di **graduatoria o ranking** $r_h$, ossia come sequenza ordinata delle $n$ alternative, ordinate in modo decrescente di preferenza. Un ranking $r_h$ è quindi un elemento dell'insieme delle permutazioni degli elementi di $A$, sia questo indicato con $\Pi(A)$. Dunque $$ r_{h} = \langle a_{h_{1}}, a_{h_{2}}, \dots, a_{h_{n}} \rangle \in \Pi(A) $$
2. Nella forma di **relazione binaria completa e transitiva**, indicata con $>_h$ ($a >_h a'$ vuol dire che $a$ preferita ad $a'$ da $v_h$):
	- **completa**: $\forall a,a' \in A : a \neq a'$, allora $a >_h a'$ o $a' >_h a$;
	- **transitiva**: se $a >_h a'$ e $a' >_h a''$ allora $a >_h a''$ per una qualunque terna di alternative $a,a',a''$;

Le due forme possibili di espressione di voto sono equivalenti:
- Derivare (2) da (1) è immediato: sia $r_{h} = \langle a_{h_{1}}, a_{h_{2}}, \dots, a_{h_{n}} \rangle$ il ranking di $h\in V$. Per ogni $i \in [n]$ e per ogni $j \in [n]$ tale che $j > i$, si pone $a_{hi} >_h a_{hj}$.
- Derivare (1) da (2) si procede come segue:
	- poiché $>_h$ è completa e transitiva, allora $\exists l \in [n]$ tale che $\forall j \in [n] \setminus \{l\}$ vale $|\{ i \in [n]: a_{l} >_{h} a_{i} \}| > | \{ i \in [n]: a_{j} >_{h} a_{i} \}|$ (Lemma 1);
	- allora, per ogni $j \in [n]\setminus \{ l \}$, vale $a_{l} >_{h} a_{j}$;
	- si pone quindi $a_{h1} = a_l$ e, osservando che $>_h$ è completa e transitiva anche se $A \setminus \{a_{h1}\}$, si ripete tale ragionamento su quest'ultimo insieme per individuare $a_{h2}$. Si itera per individuare $a_{h3}, \dots, a_{hn}$.

Si dimostrano ora formalmente i due lemmi ora utilizzati
### Lemma 1
Se $>_h$ è una relazione binaria completa e transitiva nell'insieme $A$, allora esiste $l \in [n]$ tale che $\forall j \in [n] \setminus \{l\}$ vale $|\{ i \in [n]: a_{l} >_{h} a_{i} \}| > | \{ i \in [n]: a_{j} >_{h} a_{i} \}|.$
#### Dimostrazione
Per ogni alternativa $j \in [n]$, si indica con $p_j$ il numero di alternative che $j$ batte nel voto del votante $h$, dunque
$$
p_{j} = |\{ i \in [n]: a_{j} >_{h} a_{i} \}|
$$
Certamente, esiste $l \in [n]$ tale che, per ogni $j \in [n] \setminus \{ l \}$,
$$
p_{l} = |\{ i \in [n]: a_{l} >_{h} a_{i} \}| \geqslant | \{ i \in [n]: a_{j} > a_{i} \}| = p_{j}
$$
Si supponga ora che esista $m \in [n] \setminus \{ l \}$ tale che
$$
|\{ i \in [n]: a_{m} >_{h} a_{i} \}| = | \{ i \in [n]: a_{l} > a_{i} \}|
$$
ossia, $p_l = p_m$.
Poiché $>_h$ è completa, allora $a_l >_h a_m$ oppure $a_m >_h a_l$
1. Se $a_m >_h a_l$, poiché $>_h$ è transitiva, allora per ogni $j$ tale che $a_l >_h a_j$, si ha che $a_m >_j a_j$, ossia $$ \{ i \in [n]: a_{l} >_{h} a_{i} \} \cup \{ a_{l} \} \subseteq \{ i \in [n]: a_{m} >_{h} a_{i} \} $$ e quindi, poiché $a_l \not\in \{ i \in [n]: a_{l} >_{h} a_{i} \}$, si ha $$ \begin{align} p_{m} = |\{ i \in [n]: a_{m} >_{h} a_{i}\}| &\geqslant |\{ i \in [n]: a_{l} >_{h} a_{i} \}|+1 \\ &> |\{ i \in [n]: a_{l} >_{h} a_{i} \}| = p_{l} \end{align}  $$ Quindi $p_{m} > p_{l}$, ma per ipotesi $p_m = p_l$, assurdo;
2. Se $a_l >_h a_m$, poiché $>_h$ è transitiva, allora per ogni $j$ tale che $a_m >_h a_j$, si ha che $a_l >_j a_j$, ossia $$ \{ i \in [n]: a_{m} >_{h} a_{i} \} \cup \{ a_{m} \} \subseteq \{ i \in [n]: a_{l} >_{h} a_{i} \} $$ e quindi, poiché $a_m \not\in \{ i \in [n]: a_{m} >_{h} a_{i} \}$, si ha $$ \begin{align} p_{l} = |\{ i \in [n]: a_{l} >_{h} a_{i}\}| &\geqslant |\{ i \in [n]: a_{m} >_{h} a_{i} \}|+1 \\ &> |\{ i \in [n]: a_{m} >_{h} a_{i} \}| = p_{m} \end{align}  $$ Quindi $p_{l} > p_{m}$, ma per ipotesi $p_m = p_l$, assurdo;

In entrambi i casi si contraddice quanto detto riguardo $m$. Quindi $m$ non esiste e per ogni $j \in [n] \setminus \{l \}$ vale
$$
|\{ i \in [n]: a_{l} >_{h} a_{i} \}| > | \{ i \in [n]: a_{j} >_{h} a_{i} \}|
$$
### Lemma 2
Se $>_h$ è una relazione binaria completa e transitiva nell'insieme $A$ e sia $l \in [n]$ tale che $|\{ i \in [n]: a_{l} >_{h} a_{i} \}| > | \{ i \in [n]: a_{j} >_{h} a_{i} \}|$, allora, per ogni $j \in [n]\setminus \{ l \}$, vale $a_{l} >_{h} a_{j}$.
#### Dimostrazione
In virtù del lemma 1, esiste $l \in [n]$ tale che $\forall j \in [n] \setminus \{l\}$ vale $|\{ i \in [n]: a_{l} >_{h} a_{i} \}| > | \{ i \in [n]: a_{j} >_{h} a_{i} \}|.$ Si supponga che esiste  $m \in [n] \setminus \{ l \}$ tale che $a_m >_h a_l$. Allora, per transitività di $>_h$, per ogni $j$ tale che $a_{l}>_{h}a_{j}$, si avrebbe $a_{m}>_{h}a_{j}$, ossia
$$
 \{ i \in [n]: a_{l} >_{h} a_{i} \} \cup \{ a_{l} \} \subseteq \{ i \in [n]: a_{m} >_{h} a_{i} \} 
$$
e quindi, poiché $a_l \not\in \{ i \in [n]: a_{l} >_{h} a_{i} \}$, si ha
$$
|\{ i \in [n]: a_{m} >_{h} a_{i}\}| > |\{ i \in [n]: a_{l} >_{h} a_{i} \}|
$$
il che è assurdo.
## Sistema di voto
Un sistema di voto è quindi una regola che permette di associare un voto collettivo ad un insieme di voti individuali. Formalmente
- $A=[n],V=[k]$ dove $A$ è l'insieme delle alternative e $V$ è l'insieme dei votanti;
- Si assume che i $k$ votanti esprimono i loro voti mediante ranking;
- Un **voto aggregato per n alternative e k votanti** è una funzione $$ f_{n,k}: \Pi([n])^k \to \Pi([n]) $$ in modo che $$ f_{n,k}(r_{1},r_{2},\dots,r_{k}) = r $$con $r_1,r_2,\dots,r_{k},r \in \Pi([n]);$
- Un **sistema di voto** è un predicato $\sigma$ che specifica, per ogni  $\langle r_1,r_2,\dots,r_{k} \rangle \in \Pi([n])^k$, le regole che devono essere rispettate dal voto aggregato $f_{n,k}(r_{1},r_{2},\dots,r_{k})$.
### Esempio: sistema di voto a maggioranza
Si supponga che i voti individuali dei $k$ votanti siano espressi mediante relazioni binarie $>_{1},\dots,>_{k}$ e sia $\succ$ il simbolo per indicare la relazione binaria $f_{n,k}(>_1,\dots,>_k)$, ossia, $\succ$ è il voto collettivo corrispondente a $>_{1},\dots,>_{k}$. Il **sistema di voto a maggioranza** è descritto dal predicato $\sigma_{M}$ tale che
$$
\begin{align}
\sigma_{M}(>_{1},\dots,&>_{k}) = \forall i \in [n], \forall j \in [n] \\
 &\left[ i \succ j \iff |\{ h \in [k]: a_{i} >_{h} a_{j} \}| > |\{ h \in [k]: a_{j} >_{h} a_{i} \}| \right]
\end{align}
$$
ossia, nella valutazione finale $i$ è preferito a $j$ se e solo se $i$ è preferito a $j$ dalla maggioranza dei votanti.

Il sistema di voto a maggioranza risulta essere valido nel caso in cui $|A|=2$, cioè quando le alternative sono due. Si mostra che, nel caso $|A|>2$, la relazione binaria collettiva, ottenuta dall'aggregazione di più relazioni binarie individuali transitive, può **non** essere transitiva per il sistema di voto a maggioranza.
#### Esempio
Tre amici $X,Y,Z$, che hanno un budget limitato, devono scegliere se acquistare miele, marmellata o cioccolato per colazione. In merito alle preferenze dei singoli individui, valgono le tre seguenti relazioni binarie, complete e transitive:
$$
\begin{align}
🍫 &>_{X} 🍒 >_{X} 🍯 \\
🍒 &>_{Y} 🍯>_{Y} 🍫 \\
🍯 &>_{Z} 🍫 >_{Z} 🍒
\end{align}
$$
Tuttavia, se si prova a produrre una relazione binaria $\succ$ aggregando, secondo la regola di maggioranza, le tre relazioni, si ottiene
- 🍫 è preferita a 🍒 due volte su tre $\implies 🍫 \succ 🍒$;
- 🍒 è preferita a 🍯 due volte su tre $\implies 🍒 \succ 🍯$;
- 🍯 è preferito a 🍫 due volte su tre $\implies 🍯 \succ 🍫$.
Ovvero si ottiene che
$$
🍫 \succ 🍒 \succ 🍯 \succ 🍫
$$
Dunque la relazione $\succ$ non è coerente. Inoltre, la relazione non è transitiva, infatti anche se è vero che 🍫 $\succ$ 🍒 e 🍒 $\succ$ 🍯, non è vero che 🍫 $\succ$ 🍯.
#### Paradosso di Condorcet
Tale situazione prende il nome di **Paradosso di Condorcet** che afferma

> _anche se le relazioni binarie individuali sono transitive, la relazione binaria collettiva ottenuta dalla loro aggregazione può non essere transitiva._

Si osserva che la proprietà di transitività deve necessariamente venir rispettata affinché l'aggregazione dei voti individuali abbia significato. Perciò, usare il sistema di voto a maggioranza nel caso di $|A|>2$ risulta essere problematico. Tale sistema può però venir utilizzato come punto di partenza per costruire un nuovo sistema di voto che non soffre del paradosso di Condorcet: **il Torneo**.
### Esempio: il Torneo
In un torneo i $k$ votanti esprimono ancora la propria preferenza mediante relazioni binarie, e la graduatoria finale viene costruita sulla base di una successione di scontri diretti: se si scontrano le due alternative $a$ e $a'$, risulta $a \succ a'$ se
$$
\{ h \in [k]: a_{i} >_{h} a_{j} \}| > |\{ h \in [k]: a_{j} >_{h} a_{i} \}|
$$
Ovviamente, se $k$ è dispari, allora per ogni coppia $a,a' \in A$ si avrà $a \succ a'$ o $a' \succ a$, ossia, si riesce sempre ad avere un vincitore in uno scontro diretto.
In un torneo non hanno luogo tutti i possibili sconti, ma solo quelli fissati da **un'agenda**, ossia, prima dell'inizio del torneo vengono fissate le coppie che devono confrontarsi. Poi, si scontrano i vincitori di ciascuno scontro, fino a quando si decreta il vincitore e il secondo classificato. Per comporre il resto della graduatoria, saranno necessari altri scontri.
![center|400](Pasted%20image%2020240903184632.png)

Si osserva che l'agenda può avere un ruolo determinante nel decretare il vincitore.
#### Esempio
Tornando all'esempio cioccolato, miele e marmellata:
 - $X$ propone l’agenda secondo la quale il primo scontro è 🍒-🍯, e il vincitore si scontrerà con 🍫.
- $Y$ propone l’agenda secondo la quale il primo scontro è 🍯-🍫, e il vincitore si scontrerà con 🍒.
- $Z$ propone l’agenda secondo la quale il primo scontro è 🍫-🍒, e il vincitore si scontrerà con 🍯.

![center|600](Pasted%20image%2020240903232837.png)

Poiché si aveva $🍫 \succ 🍒$, $🍒 \succ 🍯$, $🍯 \succ 🍫$, nell'agenda di $X$ vince cioccolata, nell'agenda di $Y$ vince marmellata e nell'agenda di $Z$ vince miele: ciascun amico ha proposto un'agenda che porta alla vittoria il proprio prodotto preferito.
Il sistema di voto a torneo è sensibile dunque allo **Strategic agenda setting**, ossia, l'impostare l'agenda in maniera tale che un individuo possa trarre beneficio, essendo che la graduatoria finale con questo sistema di voto dipende dall'ordinamento iniziale delle alternative.
## Sistemi di voto posizionali
Si supponga ora che i voti individuali siano espressi mediante ranking, così che il voto del votante $h$ è $r_h = \langle a_{h1}, \dots, a_{hn} \rangle$.
A ciascun ranking $r_h$ si associa una **funzione peso** che assegna un valore numerico a ciascuna alternativa *dipendentemente* dalla sua posizione nel ranking.
Ad esempio, al ranking $r_h = \langle a_{h1}, \dots, a_{hn} \rangle$ si associa la funzione $w_{h}: A \to \mathbb{N}$ tale che $w_{h}(a_{hi}) = 100-10i$; tipicamente, la funzione peso è decrescente nella posizione, ossia
$$
\forall i = 1,\dots n-1, \ w_{h}(a_{hi}) > w_{h}(a_{hi+1})
$$
In un sistema di voto posizionale, dopo aver associato ad ogni $h \in [k]$ una funzione peso $w_h$ al ranking $r_h$, il **ranking collettivo** è ottenuto calcolando il peso totale di ciascuna alternativa come somma dei pesi che quella alternativa ha nei $k$ ranking, ossia, per ogni $a \in [n]$
$$
w(a) = \sum_{1 \leqslant h \leqslant k} w_{h}(a)
$$
si ordinano poi le alternative secondo il loro peso totale per ottenere il ranking collettivo.

```ad-note
title: Nota
Funzione peso associata al ranking, **non** al votante
```

Il **Borda Count** è un particolare sistema di voto posizionale nel quale, per ogni $h \in [k]$, la funzione peso associata al ranking $r_h$ è la funzione suriettiva
$$
\rho_{h}: A \to \{ 0,1,\dots,n-1 \}
$$
tale che
$$
\rho_{h}(a_{hi}) = n-i
$$
e le alternative vengono ordinate per peso finale non crescente.
Anche in questa tipologia di sistemi di voto si manifesta il paradosso di Condorcet, sotto forma di **ex equo**, ossia, sotto forma di pareggi.

```ad-note
title: Nota
Ignorando i pareggi, il Borda Count produce sempre una relazione completa e transitiva per un'insieme di alternative.
```
### Esempio
Si considerino i tre amici con i tre ranking corrispondenti alle loro relazioni di equivalenza. Si applica il Borda Count.
- $r_{X} = \langle 🍫, 🍒, 🍯 \rangle$, da cui $\rho_{X}(🍫)=2, \rho_{X}(🍒)=1,\rho_{X}(🍯)=0$;
- $r_{Y} = \langle 🍒, 🍯, 🍫 \rangle$, da cui $\rho_{X}(🍫)=0, \rho_{X}(🍒)=2,\rho_{X}(🍯)=1$;
- $r_{Z} = \langle 🍯, 🍫, 🍒 \rangle$, da cui $\rho_{X}(🍫)=1, \rho_{X}(🍒)=0,\rho_{X}(🍯)=2$;
quindi $\rho_{X}(🍫)=3, \rho_{X}(🍒)=3,\rho_{X}(🍯)=3$ .
___
In un sistema di voto posizionale il problema degli ex aequo viene trattato con metodi di **spareggio**.
### Rilevanza delle alternative irrilevanti
Si studia un'altra caratteristica indesiderabile dei sistemi di voto posizionali, con un esempio.
#### Esempio
Cinque critici cinematografici devono scegliere se assegnare un premio cinematografico a Via col vento ($VV$) o a il Padrino ($IP$). Tre critici preferiscono $VV$, mentre gli altri due preferiscono $IP$. Secondo un sistema di voto, ad esempio quello di maggioranza, il premio andrebbe a $VV$. Prima di arrivare al voto finale, però, il comitato organizzatore del premio cinematografico decide di inserire fra i candidati anche il film Pulp Fiction ($PF$). Si decide quindi di ricorrere ad un sistema di voto posizionale, ad esempio, il Borda Count. Ciascuno dei cinque critici ritiene che $PF$ non sia adatto alla competizione. Tuttavia, i due critici che preferiscono $IP$ rispetto a $VV$, si accorgono che possono ricorrere a un espediente: entrambi possono votare secondo il ranking $\langle IP,PF,VV \rangle$; mentre gli altri tre critici, che preferiscono $VV$, votano, onestamente, il ranking $\langle VV,IP,PF \rangle$ così che
$$
\rho(VV) = 6, \ \rho(IP)=7, \ \rho(PF)=2
$$
e il premio va quindi a *Il Padrino*.

Inserendo quindi, nella posizione opportuna, il film $PF$, che non avrebbe mai vinto, i due critici che preferivano $IP$ hanno fatto in modo che esso vincesse, e per fare ciò, hanno classificato **l'alternativa irrilevante** ai fini della vittoria in posizione intermedia, nonostante questa non corrispondesse al loro giudizio reale. Se avessero votato onestamente, avrebbe vinto $VV$.
Si riconsiderino ora i ranking dei due gruppi di critici. Si osserva che se ci si limita ad osservare le posizioni *relative* di $VV$ e $IP$ nei due ranking, $VV$ precede $IP$ nella maggioranza dei votanti e quindi sarebbe ragionevole aspettarsi che questo ordine venga rispettato nel voto collettivo, indipendentemente da come viene posizionato $PF$. Invece, il posizionamento opportuno di quest'ultimo film da parte di due votanti ha permesso di modificare le posizioni relative di $VV$ e $IP$ nella graduatoria finale.
Quindi, gli esiti in un sistema di voto posizionale possono dipendere dalla presenza di alternative che, intuitivamente, possono sembrare irrilevanti, ma che possono venir utilizzate per manipolare gli esiti del ranking.
## Sistemi di voto affidabili
Si vogliono definire sistemi di voto che non soffrano delle debolezze evidenziate in precedenza, ossia, che risultino essere affidabili.
Un sistema di voto, per essere considerato affidabile, deve garantire come minimo:
- di rappresentare le scelte di **tutti** i votanti nel caso in cui esse siano **tutte** concordi;
- di **non** consentire l'utilizzo di alternative irrilevanti per orientare la graduatorie finale.
Si ricorda che una delle anomalie di cui soffrono i sistemi di voto è il paradosso di Condorcet. Nei sistemi di voto che si considerano ora, si ha che il voto aggregato (o collettivo) viene descritto mediante un ranking, ed un ranking corrisponde **sempre** ad una relazione binaria transitiva. Inoltre, per produrre un ranking potrebbe essere necessario introdurre anche una regola che risolve gli ex aequo. Pertanto, la problematica descritta dal paradosso di Condorcet non si verifica.
### Due principi per i sistemi di voto
Si enunciano ora due proprietà che un sistema di voto affidabile ragionevole deve possedere.
Sia $\sigma$ un sistema di voto; sia $[n]$ un insieme di alternative e sia $[k]$ un insieme di votanti tali che, per ogni $h \in [k]$, il votante $h$ esprime il ranking $r_{h} = \langle a_{h1}, \dots, a_{hn} \rangle$ sulle $n$ alternative; sia $r$ il voto collettivo corrispondente ai voti individuali $r_{1},r_{2},\dots,r_{k}$ derivato in accordo a $\sigma$, ossia $\sigma(r_{1},r_{2},\dots,r_{k},r)=\text{vero}$ e siano, per ogni $h \in [k]$, $\rho_{h}$ la funzione peso  Borda Count associata a $r_h$ e $\rho$ la funzione peso associata a $r$.
#### Principio di Unanimità (U)
Il sistema di voto $\sigma$ soddisfa il **principio di unanimità** se
$$
\forall i,j \in [n] \left[\forall h \in [k] \ \ \rho_{h}(i)>\rho_{h}(j) \implies \rho(i) > \rho(j)\right]
$$
cioè ogni qualvolta **tutti** i votanti preferiscono un'alternativa $i$ a un'alternativa $j$, allora $i$ è preferita a $j$ anche nella graduatoria finale.
#### Principio di Indipendenza dalle Alternative Irrilevanti (IIA)
Il sistema di voto $\sigma$ soddisfa il **Principio di Indipendenza dalle Alternative Irrilevanti** se
$$
\begin{align}
\forall i,j \in [&n], \forall (r_{1},r_{2},\dots,r_{k}),(r'_{1},r'_{2},\dots,r'_{k}) \in \Pi([n])^k  \\
&\left[[ \forall h \in [k] \ \ \rho_{h}(i)>\rho(j) \iff \rho'_{h}(i) > \rho'_{h}(j)]  \\
\implies [\rho(i) > \rho(j) \iff \rho'(i) > \rho'(j )]\right]
\end{align}
$$
dove $\rho'$ è la funzione peso associata al ranking collettivo corrispondente a $(r'_{1},r'_{2},\dots,r'_{k})$ derivato in accordo a $\sigma$.

Questo principio dice che, ogni qualvolta si considerino due insiemi di $k$ votanti per i quale due alternative $i$ e $j$ hanno le stesse posizioni relative per **tutte** le coppie di votanti omologhi, allora $i$ e $j$ hanno la stessa posizione relativa anche nelle graduatorie finali relative ai due gruppi di votanti, ovvero, la posizione relativa di due alternative nella graduatoria finale dipende unicamente dalle posizioni relative delle due alternative nelle graduatorie individuali.
In altre parole, se in una graduatoria finale $i$ è preferita ad $j$, anche se introduciamo una nuova alternativa $p$, il sistema deve produrre un nuovo ranking in cui $i$ rimane ancora il preferito rispetto a $j$.
# Teorema di Arrow
Si enuncia ora un teorema che individua l'unico sistema di voto che rispetta **U** e **IIA**.
## Enunciato teorema di Arrow
Se il sistema di voto $\sigma$ soddisfa i principi **U** e **IIA**, allora, per ogni $k \in \mathbb{N}$ e per ogni $n \in \mathbb{N}$ tale che $n>2$, esiste $j \in [k]$ tale che, per ogni $k-$upla $\langle r_{1},r_{2},\dots,r_{k} \rangle$ di ranking per $n$ alternative, il voto collettivo $r$ corrispondente ai voti individuali $r_{1},r_{2},\dots,r_{k}$ dei $k$ votanti derivato in accordo a $\sigma$ è $r = r_j$.
Quindi, per ogni insieme $[k]$ di votanti esiste un $j \in [k]$ tale che il voto collettivo $r$ corrispondente ai voti individuali $r_{1},r_{2},\dots,r_{k}$ dei $k$ votanti derivato in accordo con $\sigma$ che rispetta **U** e **IIA** è $r=r_j$, ossia, l'unico sistema di voto che rispetta **U** e **IIA** è **LA DITTATURA WRAAAA**.

```ad-note
title: Forma compatta del teorema
Se $\sigma$ soddisfa **U** e **IIA** allora per ogni $k \in \mathbb{N}$ e per ogni $n \in \mathbb{N}$ con $n>2$, esiste un $j \in [k]$ tale che per ogni $(r_1,r_2,\dots,r_k) \in \Pi([n])^k$, sia $r$ tale che $\sigma(r_1,r_2,\dots,r_k,r)=\text{vero}$, allora $r=r_j$.
```
## Dimostrazione del teorema di Arrow
Prima di procedere con la dimostrazione del teorema, si danno le seguenti definizioni.

```ad-Definizione
title: Definizione (Profilo)
Dati un insieme $[n]$ di alternative e un insieme $[k]$ di votanti, un **profilo** è una $k$-upla $P = \langle r_1,r_2,\dots,r_k \rangle$ di ranking, ciascuno espressione del voto di uno dei votanti per quelle alternative.
```

**Esempio:** un profilo di 6 votanti $(r_1,\dots,r_6)$ su 4 alternativa $(a,b,c,d)$.
![](Pasted%20image%2020240904101631.png)

```ad-Definizione
title: Definizione (Alternativa polarizzante)
Dati un insieme $[n]$ di alternative e un insieme $[k]$ di votanti e un profilo $P = \langle r_1,r_2,\dots,r_k \rangle$, una **alternativa polarizzante per P** è un'alternativa $x \in [n]$ tale che 
$$
\forall h \in [k], \ \rho_h(x)=0 \lor \rho_h(x)=n-1
$$
```

Esempio in cui $a$ è una alternativa polarizzante.
![](Pasted%20image%2020240904101914.png)

La dimostrazione del teorema di Arrow avverrà in tre passi
1. Si mostra che se $x$ è un'alternativa polarizzante per un profilo $P = \langle r_1,r_2,\dots,r_k \rangle$ allora $\rho(x)=0 \lor \rho(x)=n-1$, dove $\rho$ è la funzione peso associata al voto collettivo $r$, che soddisfa $\sigma$, corrispondente a $P$;
2. Si definisce una successione di $k$ profili in ciascuno dei quali una stessa alternativa $x \in [n]$ è polarizzante e, tramite essi, si individua un dittatore potenziale;
3. Si dimostra che il dittatore potenziale è, effettivamente, il dittatore cercato.
### Step 1: se $x$ è un'alternativa polarizzante per un profilo $P = \langle r_1,r_2,\dots,r_k \rangle$ allora $\rho(x)=0 \lor \rho(x)=n-1$
Si supponga per assurdo che $0 < \rho(x) < n-1$: allora esistono due alternative $y,z \in [n]\setminus \{ x \}$ tali che $\rho(z) < \rho(x) < \rho(y)$.
Si crea un nuovo profilo $P' = \langle r'_1,r'_2,\dots,r'_k \rangle$ come segue:
- per ogni $i \in [k]$ tale che $\rho_{i}(y) < \rho_{i}(z)$, $r'_i = r_i$;
- per ogni $i \in [k]$ tale che $\rho_{i}(y) > \rho_{i}(z)$, $r'_{i}$ è ottenuto da $r_{i}$ spostando $z$ alla sinistra di $y$, in modo che $\rho'_{i}(z) = \rho'_{i}(y)+1$
![center](Pasted%20image%2020240904102714.png)

Essendo che $x$ non è stato in alcun modo coinvolta negli spostamenti, $x$ è un'alternativa polarizzante anche per $P'$. Inoltre, per ogni $i \in [k]$, in $r_i$ e $r'_i$ non sono variati gli ordini relativi di $x$ e $y$ e di $x$ e $z$. Infine, per ogni $i \in [k]$, $\rho'_{i}(y)< \rho'_{i}(z)$, cioè $y$ sta **sempre** in una posizione inferiore rispetto a $z$.
Poiché per ogni $i \in [k]$, in $r_i$ e $r'_i$ non sono variati gli ordini relativi di $x$ e $z,$ allora per **IIA** vale
$$
\rho(x) < \rho(z) \iff \rho'(x) < \rho'(z)
$$
Ma si è supposto che $\rho(z) < \rho(x) < \rho(y)$, allora $\rho'(z) < \rho'(x)$.

Inoltre, poiché per ogni $i \in [k]$, in $r_i$ e $r'_i$ non sono variati gli ordini relativi di $x$ e $y$, allora per **IIA** vale
$$
\rho(x) < \rho(y) \iff \rho'(x) < \rho'(y)
$$
Ma si è supposto che $\rho(z) < \rho(x) < \rho(y)$, allora $\rho'(x) < \rho'(y)$.

Inoltre, per ogni $i \in [k]$ vale $\rho'_{i}(y) < \rho'_{i}(z)$: allora, per il principio **U** deve essere $\rho'(y) < \rho'(z)$. Dunque infine, unendo tutte le disuguaglianze, si ottiene che $\rho'(z) < \rho'(x) < \rho'(y) < \rho'(z)$, **un assurdo**.
### Passo 2: si definisce una successione di $k$ profili in ciascuno dei quali una stessa alternativa $x \in [n]$ è polarizzante e, tramite essi, si individua un dittatore potenziale
Si sceglie un'alternativa $x \in [n]$.
- Nel profilo $P^0 = \langle r_{1}^0, r_{2}^0,\dots,r_{k}^0 \rangle$ l'alternativa $x$ è in ultima posizione in tutti i ranking: per ogni $i \in [k]$, $\rho_{i}^0(x) = 0$;
- Nel profilo $P^1 = \langle r_{1}^1, r_{2}^1,\dots,r_{k}^1 \rangle$ l'alternativa $x$ è in prima posizione nel ranking $r_{1}^1$ e in ultima posizione in tutti i restanti ranking: $\rho_{1}^1(x)=n-1$  e per ogni $i \in [k] \setminus \{ 1 \}$, $\rho_{i}^1(x) = 0$;
- In generale, nel profilo $P^h = \langle r_{1}^h, r_{2}^h,\dots,r_{k}^h \rangle$ l'alternativa $x$ è in prima posizione nei ranking da $1$ ad $h$ e in ultima posizione nei ranking da $h+1$ a $k$ nel profilo $P^h$, ossia $$ \begin{align}
\forall i \leqslant h, &\rho_{i}^h(x)=n-1 \\ \forall i > h, &\rho_{i}^h(x) = 0
\end{align} $$
Per ogni $h \in \{ 0,\dots,k \}$ si indica con $r^h$ il ranking collettivo associato a $P^h$ e con $\rho^h$ la funzione peso ad esso associata.
![center](Pasted%20image%2020240904111301.png)

Quindi, per ogni $h \in [k]$, i due profili $P^{h-1}$ e $P^h$ variano **solo** per il modo in cui l'$h$-esimo votante giudica $x$: nel profilo $P^{h-1}$ l'$h$-esimo votante giudica $x$ ultima, ossia, $\rho_{h}^{h-1}(x)=0$, mentre nel profilo $P^h$ l'$h$-esimo votante giudica $x$ prima, ossia $\rho_{h}^h(x) = n-1$. Le posizione relative delle altre alternative rimangono invariate nei due profili.
![center](Pasted%20image%2020240904111604.png)

In virtù del principio **U**, vale $\rho^0(x) = 0$ e $\rho^k(x) = n-1$. Allora esiste un profilo $j \in [k]$ (si osserva che $j>0$ perché $\rho_{0}^0(x)=0$) tale che $\rho^h(x)=0$ per ogni $h<j$, e $\rho^j(x) > 0$. Quindi, poiché $x$ è polarizzante per $P^j$ , e $\rho^j(x)=n-1$, risulta essere $j$ il **dittatore potenziale**. Si osserva quindi che il votante $j$ ha molto potere nel posizionare $x$ nella graduatoria finale: lo fa passare dall'ultima alla prima posizione.
### Passo 3: si mostra che $j$ è, effettivamente, il dittatore cercato
Sia $Q = \langle r_{1}^Q, r_{2}^Q,\dots,r_{k}^Q \rangle$ un profilo di $k$ votanti per $n$ alternative; per ogni $h \in [k]$, si indica con $\rho^Q_{h}$ la funzione peso associata a $r_{h}^Q$ e si indica con $r^Q$ il ranking collettivo che soddisfa $\sigma$ corrispondente a $Q$ e con $\rho^Q$ la funzione peso associata a $r^Q$.
Si deve dimostrare che, qualunque sia $Q$, risulta $r^Q = r_j^Q$, ossia, che qualunque sia $Q$, comunque si scelgono due alternative $y,z \in [n]$,
$$
\rho^Q(y) > \rho^Q(z) \iff \rho_{j}^Q(y)> \rho_{j}^Q(z)
$$
Lo si fa in due passi
1. Si mostra che se $y \neq x$ e $z \neq x$, allora $$ \rho^Q(y) > \rho^Q(z) \iff \rho_{j}^Q(y)> \rho_{j}^Q(z) $$
2. Si mostra che se $y \neq x$, allora $$ \rho^Q(y) > \rho^Q(x) \iff \rho_{j}^Q(y)> \rho_{j}^Q(x) $$
#### Passo 3.1
Senza perdita di generalità, si supponga che sia $\rho_{j}^Q(y) > \rho_{j}^Q(z)$. Si costruisce da $Q$ un nuovo profilo $T$:
- per ogni $h \leqslant j$, si pone $x$ in testa di $r_{h}^T$ lasciando tutte le altre alternative nello stesso ordine nel quale si trovano in $r_{h}^Q$;
- per ogni $h>j$, si pone $x$ in coda di $r_{h}^T$ lasciando tutte le altre alternative nello stesso ordine nel quale si trovano in $r_{h}^Q$;
- Si sposta $y$ dalla posizione in cui si trova in $r_{j}^Q$ ponendola in testa a $r_j^T$.
![center](Pasted%20image%2020240904120256.png)
Quindi da $Q$ si è costruito il profilo $T$ dove
- per $h <j$, $x$ è in testa di $r_h^T$ ($\rho_{h}^T(x) = n-1$);
- per $h =j$, ossia in $r_{j}^T$, $y$ è in testa, seguito da $x$;
- per $h >j$, $x$ è in coda di $r_h^T$ ($\rho_{h}^T(x) = 0$);
Si consideri ora il profilo $P^j$, dove si ricorda che in esso
$$
\forall i \leqslant j, \rho_{i}^j(x) = n-1 \land \forall i > j, \rho_{i}^j(x) = 0
$$
Allora $T$ è molto simile a $P^j$
![](Pasted%20image%2020240904121534.png)
Essendo, per esso, $\rho^j(x)>0$ e quindi $\rho^j(x) = n-1$, allora
$$
\rho^j(x)> \rho^j(z)
$$
Poiché l'ordine relativo di $x$ e $z$ è lo stesso in $P^j$ e in $T$, allora per **IIA** vale
$$
\rho^T(x)> \rho^T(z)
$$

Si consideri ora il profilo $P^{j-1}$. Allora $T$ è molto simile anche a $P^{j-1}$
![](Pasted%20image%2020240904121737.png)
In esso si è mostrato che $\rho^{j-1}(x)=0$, allora
$$
\rho^{j-1}(x)< \rho^{j-1}(y)
$$
Poiché l'ordine relativo di $x$ e $y$ è lo stesso in $P^{j-1}$ e in $T$, essendo che in $P^{j-1}$ per ogni $h\geqslant j$ vale $\rho_{h}^j(x)=0$, allora per **IIA** vale
$$
\rho^T(x) < \rho^T(y)
$$
Quindi
- dalla stessa posizione relativa di $x$ e $y$ in $P^{j-1}$ e in $T$, dal fatto che $\rho^{j-1}(x)< \rho^{j-1}(y)$ e in virtù di **IIA**, si è concluso che $$ \rho^T(x) < \rho^T(y) $$
- dalla stessa posizione relativa di $x$ e $z$ in $P^{j}$ e in $T$, dal fatto che $\rho^{j}(x)> \rho^{j}(z)$ e in virtù di **IIA**, si è concluso che $$ \rho^T(z) < \rho^T(x) $$
ossia
$$
\rho^T(y) > \rho^T(z)
$$
Si osserva che, per tutti i votanti, l'ordine relativo di $z$ e $y$ è lo stesso in $Q$ e in $T$ (si è assunto che $\rho_{j}^Q(y) > \rho_{j}^Q(z)$) e, quindi per **IIA**, vale
$$
\rho^Q(y) > \rho^Q(z)
$$
ed essendo $y$ e $z$ variabili mute, e quindi interscambiabili, questo mostra che
$$
\rho^Q_{j}(y) < \rho^Q_{j}(z) \implies \rho^Q(y) < \rho^Q(z)
$$
#### Passo 3.2
Si dimostra che se $y \neq x$, allora
$$
\rho^Q(y) > \rho^Q(x) \iff \rho_{j}^Q(y)> \rho_{j}^Q(x)
$$
Poiché $n > 2$, allora esiste $z \in [n]$ tale che $z \neq x$ e $z \neq y$. Si deriva da $T^0,T^1,\dots,T^k$ una nuova sequenza $T^0,T^1,\dots,T^k$ di profili spostando l'alternativa $z$:
- si indica $T^h$ come $T^h = \langle t_{1}^h,t_{2}^\mathbf{h},\dots,t_{k}^n \rangle$ (vedi immagine);
- $T^0$ è ottenuto spostando $z$ in ultima posizione in ciascun ranking del profilo $P^0$. Quindi, in $T^0$, ciascun ranking avrà $x$ in penultima posizione e $z$ in ultima;
- per $h>0$, $T^h$ viene ottenuto dal profilo $P^h$ come segue: l'alternativa $z$ viene spostata in prima posizione in tutti i ranking $t_{i}^h$ cone $i \leqslant h$, e viene spostata in ultima posizione in tutti gli altri, ossia $$ \forall i \leqslant h, \ \rho_{i}^{T^h}(z) = n-1 \quad \land \quad \forall i > h, \ \rho_{i}^{T^h}(z) = 0 $$
![center|600](Pasted%20image%2020240904150152.png)
Quindi, i ranking individuali $t_i^h$ per $i=1,\dots,h$ avranno $z$ in prima posizione ed $x$ in seconda posizione, mentre per i ranking individuali con $i>h$ si ha $z$ in ultima posizione e $x$ in penultima.
Per ogni $h \in \{ 0,1,\dots,k \}$ si indica con $t^h$ il ranking collettivo associato a $T^h$ e con $\rho^{T^h}$ la funzione peso ad esso associata. Esattamente come per i profili $P^0,P^1,\dots,P^k$, esiste $l\in [k]$ tale che
$$
\rho^{T^h}(z) = 0  \ \ \forall h < l \quad \land \quad \rho^{T^l}(z)= n-1
$$
e esattamente come si è dimostrato per il dittatore potenziale $j$ al punto 3.1, si può dimostrare che per ogni profilo $Q$, se $y\neq z$ e $v\neq z$ allora
$$
\rho^{Q}(y)>\rho^{Q}(v) \iff \rho^{Q}_{l}(y)>\rho^{Q}_{l}(v)
$$
Si mostra quindi che per ogni profilo $Q$, se $y\neq z$ e $v\neq z$ allora
$$
\rho^{Q}(y)>\rho^{Q}(x) \iff \rho^{Q}_{l}(y)>\rho^{Q}_{l}(x)
$$
Per fare ciò, bisogna dimostrare che $l=j$. Lo si fa mostrando che: 1. non può essere $l<j$ e 2. non può essere $l>j$.
1. Si mostra che esiste almeno un profilo $P$ tale che $\rho^{P} \neq \rho^{P}_{l}$. Avendo scelto $j$ tale che $\forall h<j, \rho^{h}(x)=0$ e $\rho^j(x)>0$, allora nel profilo $P^{j-1}$ vale che $\rho^{j-1}(x)=0$ e quindi $$ \rho^{j-1}(x) < \rho^{j-1}(y) $$ ma poiché $\rho_{i}^{h}(x)=n-1$ per ogni $i\leqslant h$, allora $\rho_{l}^{j-1}=n-1$ e dunque $\rho_{l}^{j-1}(x)>\rho_{l}^{j-1}(y)$, ossia $\rho^{j-1}\neq \rho_{l}^{j-1}$.
2. Si mostra che esiste almeno un profilo $P$ tale che $\rho^P \neq \rho^{P}_{l}$. Nel profilo $P^{j}$ vale che $\rho^{j}(x)=n-1$ e quindi $$ \rho^{j}(x)>\rho^{j}(y) $$ ma poiché $\rho_{i}^{h}(x)=0$ per ogni $i>h$, allora $\rho^{j}_{l}(x)=0$, allora $\rho^{j}_{l}(x) < \rho^{j}_{l}(y)$, ossia $\rho^{j}\neq \rho^{j}_{l}$.
Quindi, $l=j$.
## Ombre e luci del teorema di Arrow
Il teorema di Arrow afferma quindi che un sistema di voto, per non essere soggetto a manipolazioni interessate usando alternative irrilevanti, ed essere in grado di rispecchiare la volontà dell'unanimità, deve essere una dittatura.
Però, non mostra che il voto sia necessariamente impossibile, ma che questo sia soggetto a trade-offs inevitabili: ogni sistema scelto, diverso dalla dittatura, esibisce forme di comportamento indesiderato. Si può quindi studiare come gestire questi trade-off e come valutare i differenti sistemi di voto alla luce dei risultati ottenuti. Infatti, se le alternative presentano determinate caratteristiche e se i ranking dei votanti soddisfano una determinata proprietà, il teorema di Arrow può essere *aggirato*.
# Single peaked preferences
Si supponga che le alternative siano un insieme **totalmente ordinato**, ossia $A=\{ a_{1},a_{2},\dots,a_{n} \}$ con $a_{i}<a_{i+1}$ per ogni $i=1,\dots,n-1$, dove con $<$ si indica una qualche relazione d'ordine.
Il ranking del votante $h$ è **single peaked** se, comunque si scelgano tre alternative $a_{i}<a_{j}<a_{l}$, non accade che $\rho_{h}(a_{j}) <\rho_{h}(a_{i})$ e $\rho_{h}(a_{j}) < \rho_{h}(a_{l})$:
$$
\forall a_{i},a_{j},a_{l}\in A: a_{i}<a_{j}<a_{l}, \quad \lnot(\rho_{h}(a_{j}) <\rho_{h}(a_{i}) \ \land \ \rho_{h}(a_{j}) < \rho_{h}(a_{l}))
$$
Ossia, considerando $\rho_{h}$ come una funzione definita su un dominio continuo, $\rho_{h}$ non ha minimi relativi. In figura si osservano tre ranking single peaked.
![center|600](Pasted%20image%2020240904233050.png)
Un ranking single peaked è un'ipotesi del tutto ragionevole in queste situazioni:
- si possono ordinare gli schieramenti politici lungo un asse a partire da quelli di estrema sinistra a quelli di estrema destra, e difficilmente un elettore il cui schieramento politico preferito è di estrema sinistra avrà come seconda preferenza uno schieramento di estrema destra e come terza preferenza uno schieramento di centro;
- tipicamente, il livello di un college statunitense è tanto migliore quanto più elevata la sua retta; un genitore che deve iscrivere il figlio, tipicamente, avrà un range di preferenze concentrate intorno alla retta che può permettersi di pagare. Difficilmente avrà come prima scelta un college più esclusivo, come seconda scelta quello più economico e come terza preferenza, di nuovo un college molto costoso.
Si supponga ora che **ogni votante** esprima un ranking single peaked. In questo caso, si può supporre senza perdita di generalità, ossia a meno di un riordinamento dei votanti, che, per ogni $h \in [k]$, il picco del votante $h$ non preceda il picco del votante $h-1$. In altri termini, sia $P= \langle r_1,r_{2},\dots,r_{h} \rangle$ un profilo nel quale ogni ranking è single peaked. Per ogni $h \in [k]$ si indica con $M_{h}$ l'alternativa preferita dal votante $h$, cioè, $M_{h} \in A$ e $\rho_{h}(M_{h})=n-1$. Allora i votanti sono ordinati in modo tale che
$$
M_{1}\leqslant M_{2} \leqslant\dots\leqslant M_{k}
$$
Per esempio in figura si ha che il votante verde è il primo votante, il rosso è il secondo e quello blu è il terzo.
## Teorema del votante mediano
Nel caso in cui le alternative sono un insieme totalmente ordinato e i ranking di tutti i votanti sono single peaked si può utilizzare il sistema di voto a maggioranza con la certezza di non incorrere nel paradosso di Condorcet. Questo è garantito dal seguente teorema.
### Teorema
Sia $A=\{ a_{1},a_{2},\dots,a_{n} \}$ un insieme di alternative tale che $a_{i}<a_{i+1}$ per ogni $i=1,\dots,n-1$, e sia $P= \langle r_1,r_{2},\dots,r_{2k-1} \rangle$ un profilo per $A$ nel quale ogni ranking è single peaked e tale che $M_{1}\leqslant M_{2} \leqslant\dots\leqslant M_{2k-1}$. Allora, per ogni $y \in A \setminus \{ M_{k} \}$
$$
|\{ h: \rho_{h}(M_{k}) > \rho_{h}(y) \}| > |\{ h: \rho_{h}(y) > \rho_{h}(M_{k}) \}|
$$
Il teorema del votante mediano dice che, nelle ipotesi della sua validità, esiste una alternativa che batte qualunque altra alternativa, nei confronti a coppie, per il maggior numero dei votanti. Essa è l'alternativa che corrisponde al picco del votante che si trova in posizione centrale nell'insieme dei picchi ordinati: nel sistema di voto a maggioranza, essa è l'alternativa prima classificata.
Si osserva che rimuovendo l’alternativa prima classificata dall'insieme di alternative, i $2k – 1$ ranking sulle alternative rimanenti sono ancora single peaked. Allora, si possono ordinare per picchi non decrescenti e si può dedurre, dal teorema del votante mediano, che il picco che occupa la posizione centrale nell'ordinamento non decrescente dei picchi sulle $n – 1$ alternative corrisponde all'alternativa seconda classificata, e così via. Si può vedere un esempio nei lucidi.
### Dimostrazione
Sia $M_{k}=a_m$ l'alternativa preferita del votante mediano $k$, ovvero tale che $\rho_{k}(a_{m})=n-1$. Allora si consideri un'alternativa $y=a_{t}\neq a_{m}$. Si hanno due casi.
#### Punto 1: $t>m$
Dato che le alternative sono **totalmente ordinate**, se $t>m$ allora $a_t > a_m$. Inoltre, per ogni votante $h<k$ vale che $M_{h} \leqslant M_{k}=a_{m}< a_{t}$ e, dato che $r_{h}$ è single peaked, vale
$$
\rho_{h}(M_{h})\geqslant\rho_{h}(a_{m})>\rho_{h}(a_{t})
$$
Ossia, tutti i votanti di indice $h<k$ preferiscono $a_{m}$ ad $a_{t}$.

![center|600](Pasted%20image%2020240904234041.png)
nell'esempio in figura, $k=$rosso, $m=4$, $t=6$ e $h=$blu, dove si assume blu=1, rosa=2 e verde=3.

Allora, i votanti che preferiscono $a_{t}$ ad $a_{m}$ possono essere solo quelli di indice $h>k$:
$$
\{ h \in [2k-1]: \rho_{h}(a_{t}) > \rho_{h}(a_{m}) \} \subseteq \{ h \in [2k-1]: h>k \}
$$
e quindi
$$
|\{ h \in [2k-1]: \rho_{h}(a_{t}) > \rho_{h}(a_{m}) \}| \leqslant (2k-1)-k=k-1
$$
cioè, per ogni $y > M_{k}$:
$$
|\{ h \in [2k-1]: \rho_{h}(M_{k}) > \rho_{h}(y) \}| > |\{ h \in [2k-1]: \rho_{h}(y) > \rho_{h}(M_{k}) \}|
$$
In parole, ci sono almeno $k$ individui che preferiscono $a_{m}$ ad $a_{t}$ e al più $k-1$ individui che preferiscono $a_{t}$ ad $a_{m}$, ovvero la maggior parte preferisce $a_{m}$ ad $a_{t}$ se $t>m$.
#### Punto 2: caso $t<m$
Dato che le alternative sono **totalmente ordinate**, se $t<m$ allora $a_t < a_m$. Inoltre, per ogni votante $h>k$ vale che $M_{h} \geqslant M_{k}=a_{m}> a_{t}$ e, dato che $r_{h}$ è single peaked, vale
$$
\rho_{h}(M_{h})\geqslant\rho_{h}(a_{m})>\rho_{h}(a_{t})
$$
Ossia, tutti i votanti di indice $h>k$ preferiscono $a_{m}$ ad $a_{t}$.

![center|600](Pasted%20image%2020240904234445.png)
nell'esempio in figura, $k=$roso, $m=4$, $t=2$ e $h=$verde, dove si assume blu=1, rosa=2 e verde=3.

Allora, i votanti che preferiscono $a_{t}$ ad $a_{m}$ possono essere solo quelli di indice $h<k$:
$$
\{ h \in [2k-1]: \rho_{h}(a_{t}) > \rho_{h}(a_{m}) \} \subseteq \{ h \in [2k-1]: h<k \}
$$
e quindi
$$
|\{ h \in [2k-1]: \rho_{h}(a_{t}) > \rho_{h}(a_{m}) \}| \leqslant (2k-1)-k=k-1
$$
cioè, per ogni $y > M_{k}$:
$$
|\{ h \in [2k-1]: \rho_{h}(M_{k}) > \rho_{h}(y) \}| > |\{ h \in [2k-1]: \rho_{h}(y) > \rho_{h}(M_{k}) \}|
$$
In parole, ci sono almeno $k$ individui che preferiscono $a_{m}$ ad $a_{t}$ e al più $k-1$ individui che preferiscono $a_{t}$ ad $a_{m}$, ovvero la maggior parte preferisce $a_{m}$ ad $a_{t}$ se $t<m$.

In conclusione, allora
1. per ogni alternativa $y=a_{t}$ con $t>m$: $$ |\{ h \in [2k-1]: r_{h}(M_{k}) > r_{h}(y) \}| > |\{ h \in [2k-1]: r_{h}(y) > r_{h}(M_{k}) \}|$$
2. per ogni alternativa $y=a_{t}$ con $t<m$: $$ |\{ h \in [2k-1]: r_{h}(M_{k}) > r_{h}(y) \}| > |\{ h \in [2k-1]: r_{h}(y) > r_{h}(M_{k}) \}|$$
ossia, per ogni alternativa $y \in A \setminus \{ M_{k} \}$
$$
|\{ h \in [2k-1]: r_{h}(M_{k}) > r_{h}(y) \}| > |\{ h \in [2k-1]: r_{h}(y) > r_{h}(M_{k}) \}|
$$
cioè applicando il ragionamento nei due punti per ogni alternativa $y \neq a_m$ si ottiene che la maggior parte dei votanti preferisce $a_{m}=M_{k}$.