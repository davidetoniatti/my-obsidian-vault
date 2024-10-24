Le fondamenta del machine learning risiedono nell'avere a disposizione una **collezione di dati**, ognuno dei quali rappresenta l'**osservazione** dei valori di un insieme di variabili predefinite. Questi valori possono sembrare casuali, ma in realtà si assume che vengano generati da un processo sconosciuto e dunque si assume che tali dati posseggano una **struttura intrinseca** sconosciuta. L'essenza dell'apprendimento automatico risiede in due obiettivi primari, ognuno dei quali affronta un aspetto diverso dell'analisi dei dati:
- **Unsupervised Learning**: l'obiettivo è quello di estrarre informazioni riguardo la struttura di fondo dei dati, approfondendo cosi la comprensione della loro natura intrinseca. Questo approccio spesso coinvolge tecniche come il clustering, riduzione della dimensionalità (dimensionality reduction) o modellazione generativa. Una applicazione importante dell'apprendimento non supervisionato è la possibilità di generare automaticamente nuovi dati che risultano indistinguibili rispetto ai dati nel dataset originale, nel senso che questi dati sintetici sembrano essere prodotti dallo stesso processo sconosciuto, imitando di fatto le caratteristiche intrinseche dei dati.
- **Supervised Learning**. In questo paradigma, l'obiettivo è quello di prevedere informazione aggiuntiva per ogni dato sulla base delle conoscenze pregresse. Ciò comporta tipicamente l'addestramento di modelli su dati identificati per fare previsioni su istanze non viste. L'apprendimento supervisionato include un'ampia gamma di compiti, tra cui la classificazione, la regressione e la predizione di sequenze.
In entrambi gli scenari, si assume di essere in possesso di un insieme di osservazioni prodotte dal processo sconosciuto. Questo insieme di dati iniziale serve come base sulla quale costruire i modelli e ricavare informazioni.
Qualcosa su reinforcement learning.
Si entra ora nel dettaglio di questi ambienti operativi.
# Tipologie di problemi
Nell'apprendimento supervisionato e non supervisionato, si assume di avere a disposizione una **collezione di dati**, detto **training set**. Tale collezione è modellata come:
- Un insieme di $n$ elementi rappresentati come una sequenza di **vettori di caratteristiche (features)** $x_1, ..., x_n \in \mathbb{R}^d$, usati per per derivare un **modello**. $$\mathbf{x} = (x_1, ..., x_n) \in \mathbb{R}^{n \times d}$$
- Nel caso dell'apprendimento supervisionato, il training set è arricchito con informazioni aggiuntive. Nello specifico, include un **vettore target** $\mathbf{t} = (t_1, ..., t_n)$, dove ogni $t_i$ è detta **classe** e specifica il valore da predire sulla base del vettore di input corrispondente $x_i$. Dunque $t_i \in \mathbb{N}$ è la classe a cui appartiene l'elemento $x_i$. $$\mathbf{t} = (t_1, ..., t_n) \in \mathbb{N}^n$$
Si osserva che i vettori nel training set sono una specifica **rappresentazione** degli elementi (cioè degli elementi del mondo reale). L'insieme delle features utilizzate nel processo di apprendimento è una componente *fondamentale* e può avere un effetto importante sulla qualità e l'efficienza della predizione. L'adattamento dell'insieme delle features (sia definendo nuove caratteristiche a partire da opportune composizioni di quelle date, sia identificando le caratteristiche significative per il compito da svolgere) è un problema fondamentale in Machine learning, che può coinvolgere approcci algoritmici sofisticati, applicati nell'ambito dell'unsupervised learning.
## Supervised learning
Nell'apprendimento supervisionato l'obiettivo è quello di predire il valore di una caratteristica (feature) sconosciuta, detta **target** $t$, per un dato elemento $x$, a partire dai valori di un insieme di features. In particolare, se $t \in \mathbb{R}$ si parla di **regressione**, se invece $t \in \{ 1,\dots,k \}$ allora si parla di **classificazione**.
L'approccio generale a questo problema consiste nel definire un modello (**predittore**), funzionale o probabilistico, della relazione tra l'insieme delle feature e dei valori target. Tale modello è derivato attraverso un processo di apprendimento a partire da un insieme di esempi, che illustrano la relazione tra l'insieme delle feature e dei target. Gli esempi sono contenuti in un **training set** $\mathcal{T} = (\mathbf{X},t)$, e ogni esempio è composto da
- Un vettore di feature $x_{i} = \{ x_{i_{1}},\dots,x_{i_{m}} \}$;
- Il corrispondente valore target $t_{i}$
Il modello può avere una delle due seguenti forme:
- Il predittore è sotto forma di funzione $y(\cdot)$ che, per ogni elemento $x,$ restituisce un valore $y(x)$ come stima di $t$. Questa funzione agisce come un predittore diretto, mappando le feature in input nello spazio dei target.
- Il predittore è sotto forma di distribuzione di probabilità che associa ad ogni possibile valore $t$ nel dominio dei target un valore di probabilità $\mathbf{Pr}(t|x)$. Questo approccio fornisce una visione più sfumata, cogliendo l'incertezza insita nel task della previsione.
L'approccio basato sulle funzioni offre previsioni semplici, mentre il modello probabilistico fornisce una rappresentazione più ricca delle incertezze sottostanti e dei risultati potenziali.
## Unsupervised learning
Nell'apprendimento non supervisionato, l'obiettivo è quello di individuare schemi e strutture intrinseche all'interno di una determinata collezione di elementi, al fine di estrarre informazioni sintetiche da questi dati. La collezione di elementi è un **dataset** $\mathbf{X}=\{ x_{1},\dots,x_{n} \}\in \mathbb{R}^{d \times n}$ in cui nessun elemento è associato ad un target. Le informazioni da estrarre dal dataset posso avere diverse forme:
- **Clusters**. sottoinsiemi di oggetti *simili*;
- **density estimation**. la distribuzione degli elementi nel dominio;
- **dimensionality reduction**. Una proiezione dei dati un spazio di dimensione minore, che preserva quante più informazioni possibili. Le tecniche di riduzione della dimensionalità mirano a caratterizzare gli elementi utilizzando un insieme ridotto di caratteristiche, rivelando potenzialmente strutture latenti e riducendo la complessità computazionale per le analisi successive.
	- **Feature selection.** Si identifica e mantiene il sottoinsieme di feature più informativo;
	- **Feature extraction.** Si crea un nuova rappresentazione dei dati che ha dimensione minore e ne cattura le caratteristiche essenziali
	
Anche nel contesto dell'apprendimento non supervisionato, dove mancano variabili target esplicite, è prassi comune definire e applicare un modello adeguato che catturi le relazioni e i pattern tra le feature. Questo modello serve a diversi scopi:  
1. Fornisce una rappresentazione compatta della struttura sottostante dei dati.  
2. Può essere utilizzato per generare nuovi dati sintetici che condividono le caratteristiche dei dati originali.  
3. Consente di rilevare le anomalie per mezzo dell'identificazione di dati che si discostano significativamente dai modelli appresi.  
4. Facilita l'interpretazione e la visualizzazione di dati ad alta dimensionalità.
Per mezzo di tali tecniche e modelli di apprendimento non supervisionato, si ottengono informazioni preziose sulla struttura intrinseca dei dati, anche in assenza di variabili target predefinite. Tale approccio è particolarmente utile per la definizione di modelli generativi (come LLM e tutti i sistemi di intelligenza artificiale generativa), per l'analisi esplorativa dei dati, per l'ingegnerizzazione delle caratteristiche e come fase di pre-elaborazione per successivi task di apprendimento supervisionato.
## Reinforcement learning
Nel paradigma del reinforcement learning, apprendimento con rinforzo, l'obiettivo è quello di identificare una sequenza ottimale di azioni all'interno di un dato ambiente o situazione. Tale sequenza deve **massimizzare** una data metrica, detta generalmente reward o **profitto**. 

> Non si tratta nel corso, quando avrò voglia completo sta sezione.
# Supervised learning
Un task di machine learning è definito rispetto ad una coppia di domini:
- Un **dominio** $\mathcal{X}$. Questo è l'insieme degli oggetti si devono classificare (assegnare un valore). Ogni elemento $x \in \mathcal{X}$ è rappresentato come un vettore di **features**. Il numero di features definisce la **dimensionalità** del problema. Formalmente, $\mathcal{X} \subseteq \mathbb{R}^d$ e dunque si rappresenta $x$ come $$ x = (x_{1},\dots,x_{d}) $$
- Un **insieme di classi** $\mathcal{Y}$. Questo è l'insieme delle possibili etichette, o classi, associate agli elementi di $\mathcal{X}$. La natura dell'insieme $\mathcal{Y}$ determina il tipo di task di apprendimento:
	- se $\mathcal{Y}$ è continuo, allora si ha un problema di **regressione**;
	- se $\mathcal{Y}$ è discreto, si ha un problema di **classificazione**:
		- se $|\mathcal{Y}| = 2$, si tratta di **classificazione binaria**;
		- se $|\mathcal{Y}| > 2$, si tratta di **classificazione multiclasse**;

Si ha a disposizione un **training set**, ovvero un insieme di dati del tipo
$$
\mathcal{T} \equiv \lbrace (x_1, t_1), ..., (x_n,t_n) \rbrace \subseteq \mathcal{X} \times \mathcal{Y}
$$
Si può indicare $\mathcal{T}$ come la coppia di elementi:
- **feature matrix** $$\mathbf{X} = (x_1, ..., x_n)^T = \begin{pmatrix} x_{1,1} & x_{1,2} & \dots & x_{1,d} \\ x_{2,1} & x_{2,2} & \dots & x_{2,d} \\ \vdots & \vdots & \ddots & \vdots \\ x_{n,1} & x_{n,2} & \dots & x_{n,d} \end{pmatrix}$$
- **target vector** $$\mathbf{t} = \begin{pmatrix} t_1 \\ t_2 \\ \vdots \\ t_n \end{pmatrix}$$
Nel *supervised learning* si vuole **derivare** da $\mathcal{T}$ un **algoritmo predittore** $\mathcal{A}$, il quale deve essere in grado di generare una previsione $y \in \mathcal{Y}$ per ogni possibile elemento $x \in \mathcal{X}$. La previsione può essere generata in accordo ad approcci differenti, in base a quale sia la previsione che si vuole ottenere.
- **Direct target valure prediction**. In questo caso, l'algoritmo $\mathcal{A}$ predice un valore $y$ il quale è un'ipotesi del valore del target di $x$. Ovvero, $\mathcal{A}$ calcola una funzione $h: \mathcal{X} \to \mathcal{Y}$.
- **Probability distribution prediction**. In questo caso, l'algoritmo $\mathcal{A}$ predice una distribuzione di probabilità su $\mathcal{Y}$. Allora $\mathcal{A}$ ritorna, per ogni $y \in \mathcal{Y}$, una stima della probabilità che $y$ sia il valore del target di $x$ $$ \mathbf{Pr}(y|x) $$
## Derivare un predittore funzionale
Esistono diversi approcci che permettono di derivare un predittore funzionale.
### Direct prediction computation
In questo caso, viene definito un algoritmo $\mathcal{A}$ che computa una funzione
$$
\begin{align}
h&: \mathcal{X} \times (\mathcal{X} \times \mathcal{Y})^n \to \mathcal{Y} \\
&\iff \\
h&: \mathcal{X} \times \mathcal{T} \to \mathcal{Y}
\end{align}
$$
Ovvero un algoritmo che, usando in modo diretto i dati del training set $\mathcal{T}$, calcola un possibile calore target $y \in \mathcal{Y}$ per ogni possibile input $x \in \mathcal{X}$.
$$
h(x, \mathbf{X}, \mathbf{t}) \mapsto y
$$
Dunque per ogni predizione, l'algoritmo prende in considerazione tutto il training set. Si osserva che non viene effettuato alcun apprendimento.
![center|600](EB2E277D341A692F1FED45318ECDB887.png)
Un esempio di questo approccio è l'algoritmo $k$-nearest neighbors per la classificazione. Il training set è composto da un insieme $\mathbf{X}$ di punti in uno spazio, ognuno dei quali appartenente ad una classe $t \in \mathbf{t}$. Dato un $x$ qualsiasi non appartenente ad $\mathbf{X}$, l'algoritmo $k$-nearest neighbors ritorna la classe *"più popolare"* tra i $k$ punti di $\mathbf{X}$ più vicini ad $x$, seconda una qualche misura di distanza.
### Model-based Learning Approach
Questo secondo approccio prevede l'apprendimento di una funzione da una classe di modelli predefinita. Questo metodo può essere formalizzato nel modo seguente:
- si definisce una classe di funzioni $\mathcal{H}: \mathcal{X} \to \mathcal{Y}$;
- si utilizza un algoritmo di *apprendimento* $\mathcal{A}$ che deriva una specifica funzione $h_{\mathcal{T}} \in \mathcal{H}$ dato il training set $\mathcal{T}$. Dunque $\mathcal{A}$ implementa una funzione da $\mathcal{T}$ in $\mathcal{H}$.
Dunque l'algoritmo $\mathcal{A}$ fornisce una funzione $h_{\mathcal{T}}$ in $\mathcal{H}$, e dunque un algoritmo $\mathcal{A}_{\mathcal{T}}$ che implementa $h_{\mathcal{T}}$, che predice *"meglio"* $y$ da $x$ quando viene applicato agli elementi di $\mathcal{X}$. Per ogni nuovo elemento $x$, il corrispondente valore del target è calcolato applicando $\mathcal{A}_{\mathcal{T}}$ con input $x$, i.e. $h_{\mathcal{T}}(x)$.
![center|600](A886C1F9D83C718292D3F1CD2B772A43.png)

Un esempio di questo secondo approccio è la [[Linear Regression]], nella quale si predice il target $y$ di un qualsiasi input $x = (x_1, ..., x_d) \in \mathcal{X}$  per mezzo di una **combinazione lineare** delle sue features.
$$
y = w_0 + \sum_{i=0}^{d} w_i \cdot x_i
$$
I $d+1$ coefficienti $w_0, w_1, ..., w_d$ sono **appresi** da $\mathcal{A}$ mediante i dati $\mathcal{T}$.
### Ensemble Learning Approach
Il terzo approccio consiste nel creare una intera collezione di predittori e dunque calcolare una previsione come composizione delle previsioni date dai singoli predittori nella collezione. Tale approccio spesso porta a migliori prestazioni e ad una maggiore robustezza del risultato. Dunque è caratterizzato da tre fasi:
- **Ensemble construction.** A partire dal training set $\mathcal{T}$ si deriva un insieme di algoritmi $\mathcal{A}_{\mathcal{T}}^{(1)}, \mathcal{A}_{\mathcal{T}}^{(2)},\dots,\mathcal{A}_{\mathcal{T}}^{(s)}$ che calcolano ognuno una differente funzione $h_{\mathcal{T}}^{(i)}: \mathcal{X} \to \mathcal{Y}$ di una data classe $\mathcal{H}$. Ogni $\mathcal{A}_{\mathcal{T}}^{(i)}$ è dunque un predittore del valore $y$ dato $x$ derivato dallo stesso insieme degli esempi $\mathcal{T}$;
- **Weight assignment.** Ad ogni predittore si assegna un peso corrispondente $w^{(1)},\dots,w^{(s)}$. Ogni peso è direttamente correlato alla qualità stimata delle predizioni fornite da $\mathcal{A}^{(i)}$ per quanto riguarda gli elementi di $\mathcal{T}$;
- **Prediction combination.** Per ogni $x$, si calcola il valore finale della predizione combinando tutte le predizioni date dall'insieme di algoritmi $y^{(1)} = h_{\mathcal{T}}^{(1)},\dots,y^{(s)} = h_{\mathcal{T}}^{(s)}$ pesate rispetto $w^{(1)},\dots,w^{(s)}$.
Riassumendo, la previsione del valore target per l'elemento $x$ è data dalla combinazione lineare dei valori $y^{(1)},\dots,y^{(s)}$, predetti dall'insieme dei predittori $\mathcal{A}^{(1)},\dots,\mathcal{A}^{(s)}$, ognuno pesato rispetto al corrispondente peso $w^{(1)},\dots,w^{(s)}$. Ogni $A^{(i)}$ è un semplice predittore derivato da $\mathcal{T}$.
Un'importante variante di questo approccio è rappresentato dalla predizione **completamente bayesiana**, in cui l'insieme dei diversi predittori è continuo, ciascuno corrispondente a un diverso valore di un insieme di parametri $(w_{1},\dots,w_{d}) \in \mathbb{R}^d$. In questo caso, la somma viene sostituita da un integrale (solitamente multidimensionale).
![center|600](998323B8135416815B53616DE9E34343.png)

In conclusione, i tre approcci si differenziano in quanto:
- nel primo caso, un algoritmo viene applicato ai dati che comprendono sia l'elemento $x$ e l'intero training set;
- nel secondo caso, l'algoritmo che si applica agli elementi $x$ viene derivato in funzione del training set;
- nel terzo caso, nessun algoritmo viene applicato ad $x$; la previsione viene invece calcolata a partire dalle previsioni restituite da un insieme di predittori.
## Probabilistic framework for ML
- **Training object generation model**. Si suppone che gli oggetti del training set siano campionati da $\mathcal{X}$ secondo una distribuzione di probabilità $p_{M}$. Cioè, per ogni $x \in \mathcal{X}$, $p_{M}(x)$ è la probabilità che $x$ sia il prossimo elemento campionato nel training set.
- **Training target generation model**. Nel caso generale, si assume che le etichette associate agli elementi del training set siano generate secondo una distribuzione di probabilità $p_{C}$ condizionata a $\mathcal{X}$.  Cioè, per ogni $t \in \mathcal{Y}$, $p_C (t|x)$ è la probabilità che l'etichetta osservata dell'elemento $x$ nel training set sia $t$. Per il momento, si assume che la relazione tra elemento ed etichetta sia deterministica, cioè che esista una funzione $f$ sconosciuta tale che $t = f(x)$.
## Prediction Risk
Restringendosi, per semplicità, all'approccio model-based descritto in precedenza, emergono diversi concetti rilevanti. Dato un qualsiasi elemento $x \in \mathcal{X}$, il suo target $t \in \mathcal{Y}$ e un **predittore** $h$, si definiscono i seguenti concetti.
- **Error**: l'errore di un predittore $h$ derivante dal confronto tra $h(x)$ e $t$. ^350fae
- **Loss**: è una **funzione** usata per misurare un **errore** $$L: \mathcal{Y} \times \mathcal{Y} \to \mathbb{R}$$ $$L(h(x), t) \in \mathbb{R}$$ ^0b44e2
- **Prediction Risk**: Il **rischio** di una predizione $h(x)$ è l'applicazione della funzione loss $$\mathcal{R}(h(x),x)=L(h(x),t)$$
  Nel caso generale, in cui non esiste una funzione che definisce la relazione $f(x) = t$, bensì si assume una distribuzione $p_{C}(t \vert x)$ (per ogni valore di $t$), allora si definisce il rischio $\mathcal{R}$ in come la media $$\mathcal{R}(h(x),x) = \mathbb{E} \left[ L(h(x),t) \right] = \int_{\mathcal{Y}} L(h(x),t) \cdot p_{C}(t \vert x) \,dt$$ o nel caso di classificazione $$\mathcal{R}(h(x),x) = \mathbb{E} \left[ L(h(x),t) \right] = \sum_{t \in \mathcal{Y}} L(h(x),t) \cdot p_{C}(t \vert x)$$ ^04c36e
  
La predizione **ottimale** $y^*$ è la predizione che **minimizza** il rischio, ovvero:
- nel caso $t=f(x)$ $$y^* = \arg \min_{y \in \mathcal{Y}} L(y, f(x))$$ dove $f(x)$ è la funzione che mette in relazione $\mathcal{X}$ ad $\mathcal{Y}$.
- nel caso probabilistico $$y^* = \arg \min_{y \in \mathcal{Y}} \mathbb{E}_p\left[ L(y, t) \right] = \arg \min_{y \in \mathcal{Y}} \int_{\mathcal{Y}} L(y,t) \cdot p_{C}(t \vert x) \,dt$$ dove $p$ è la distribuzione dei target $t \in \mathcal{Y}$ rispetto all'elemento $x$.

Questa predizione ottima $y^*$ è anche nota come **stimatore Bayesiano**. Sfortunatamente, questo approccio non può essere applicato in quanto sia la funzione $f$ che la distribuzione $p_C$ sono sconosciute.
Si assuma che gli elementi di $\mathcal{X}$ siano distribuiti secondo una distribuzione $p_{M}$. Il **rischio** di un qualsiasi predittore $h$ è pari alla [[#^0b44e2|loss]] media rispetto a **tutti** gli elementi di $\mathcal{X}$.
- nel caso i target siano definiti da una funzione $f: \mathcal{X} \to \mathcal{Y}$, allora $$\mathcal{R}(h) = \mathbb{E}_{p_{M}, f}\left[ L(h(x), f(x)) \right] = \int_{\mathcal{X}} L(h(x), f(x)) \cdot p_{M}(x) \,dx$$
- nel caso i target siano definiti da una distribuzione $p_{C}(t \vert x)$, allora $$\mathcal{R}(h) = \mathbb{E}_{p_{M}, p_{C}}\left[ L(h(x), t) \right] = \int_{\mathcal{Y}} \int_{\mathcal{X}} L(h(x), f(x)) \cdot p_{M}(x) \cdot p_{C}(t \vert x) \,dx \,dt$$
Purtroppo in contesti reali non si conosce mai ne la funzione $f(\cdot)$ ne la distribuzione $p_{C}( \cdot \vert x)$. Dunque si definisce un **rischio** rispetto ai **dati** a disposizione nel training set $\mathcal{T}$. Tale misura è nota come **rischio empirico** ed è definita come
$$
\overline{\mathcal{R}}_{\mathcal{T}}(h) = \frac{1}{\vert \mathcal{T} \vert} \sum_{(x,t) \in \mathcal{T}}L(h(x),t)
$$
## From Learning to Optimization
L'approccio fondamentale a un problema di machine learning è quello di derivare dal training set un predittore $h$ che **minimizzi** (almeno approssimativamente) il rischio empirico calcolato sul training set a disposizione.
Dunque un problema di learning si riduce ad un problema di minimizzazione in un qualche **spazio di funzioni** $\mathcal{H}$, l'insieme di tutti i possibili predittori $h$.
$$
h^* \approxeq \arg \min_{h \in \mathcal{H}}\overline{\mathcal{R}}_{\mathcal{T}}(h)
$$
L'insieme $\mathcal{H}$ è anche detto insieme di **ipotesi** o **bias induttivo**.
### Problemi relativi al bias induttivo
Scegliere $\mathcal{H}$ in modo adeguato è un problema fondamentale del Machine Learning.
- quali sono gli effetti nella scelta della struttura di $\mathcal{H}$?
- come definire $\mathcal{H}$ in modo tale che $h^*$ sia computabile?
Si può vedere la scelta di $\mathcal{H}$ come una sorta di **conoscenza a priori** del problema, da parte di chi definisce il modello, nel senso che si suppone che una funzione della classe $\mathcal{H}$ sia un predittore per il task che sbaglia poco.
#### Scelta dell'insieme delle ipotesi
Un modo banale per derivare predittori con un basso tasso di errore è quello di definire una classe molto ricca, cioè scegliendo $\mathcal{H}$ il più grande possibile: come limite, $\mathcal{H}$ potrebbe essere definito proprio come l'insieme di tutte le funzioni $f: \mathcal{X} \to \mathcal{Y}$. Ci sono però delle problematiche che occorrono.

Si assuma di avere un task di **classificazione binaria**, ovvero dove $\mathcal{Y} = \lbrace 0, 1 \rbrace$. Dato il training set $\mathcal{T} = (\mathbf{X}, \mathbf{t})$, si definisce la seguente loss function
$$
L(y, t) := \begin{cases}
1 &\text{se } y = t\\
0 &\text{altrimenti}
\end{cases}\;\;\; \forall y,t \in \lbrace 0,1 \rbrace
$$
ovvero, la loss è uno se l'elemento è classificato in modo errato, zero altrimenti. Di conseguenza, il rischio è il numero atteso di errori di classificazione, mentre il rischio empirico è la frazione degli elementi nel training set che sono classificati in modo errato.
Si assuma che i target siano **equamente** distribuiti in $\mathcal{X}$, ovvero che
$$
p(t \vert x) = \frac{1}{2}\;\; \forall x \in \mathcal{X}, \forall t \in \lbrace 0, 1 \rbrace
$$
Sia $h(\cdot)$ il predittore
$$h(x) := \begin{cases}
t_i &\text{se } \exists x_i \in \mathbf{X} : x = x_i\\
0 &\text{altrimenti}
\end{cases}$$
ossia, se $x$ è presente nel training set, il predittore assegna ad $x$ il suo target; in ogni altro caso assegna ad $x$ il valore 0.
Allora, il rischio empirico per costruzione è 0, perciò certamente il predittore è ottimo: $h = h^*$. Il rischio invece in media sarà $\approx 1/2$.
La qualità di questo predittore dunque è simile alla qualità di un predittore che **casualmente** assegna 0 o 1. Perciò $h^*$ si comporta molto bene sui soli dati del training set, mentre in generale si comporta male.
Tale fenomeno è anche detto **overfitting**: il predittore si comporta bene sugli elementi del training set, ma male su altri elementi di $\mathcal{X}$.

Rispetto alla grandezza di $\mathcal{H}$ si fanno le seguenti osservazioni:
- se $\mathcal{H}$ è troppo grande (**complesso**) si può incorrere in **overfitting**: un predittore che funziona molto (troppo) bene sui dati del training set ma male in generale.
- se $\mathcal{H}$ è troppo piccolo (**semplice**) si può incorrere in **underfitting**: un predittore che si comporta sempre male sia sul training set che su nuovi dati.
Questo fenomeno è anche noto come **tradeoff bias-varianza**.