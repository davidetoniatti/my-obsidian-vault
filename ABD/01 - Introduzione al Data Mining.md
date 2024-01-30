# Data Mining
Con il termine *data mining* si fa spesso riferimento al processo di creazione di un modello a partire da determinati dati. Più generalmente, l'obiettivo del data mining è quello di definire un algoritmo. In generale, la definizione di un modello per Data Sets complessi e/o di grandi dimensioni può essere schematizzato come segue:
- Fase 1: Trasformare il Data Set in input originale in un modello/struttura formale $\mathbf{I}$ in maniera tale che il problema reale originale possa essere ridotto ad un task computazionale ben definito $\mathbf{T}$ su input $\mathbf{I}$.
- Fase 2: Trovare il miglior algoritmo che risolva $\mathbf{T}$ su input $\mathbf{I}$.

**Esempio:** Si consideri il problema di individuare le mail di phishing. Una soluzione algoritmica può essere schematizzata come segue:
1. Si classificano le mail ricevute in due sottoinsiemi: Phishing/Non-phishing.
2. Si estraggono le parole (o frasi) che appaiono insolitamente spesso nelle mail di phishing (ad esempio "send money").
3. Si assegnano pesi positivi alle parole di phishing individuate al passo precedente e pesi negativi alle restanti.
4. Si definisce il seguente algoritmo: per ogni e-mail in ingresso, calcola la somma dei pesi delle parole. Se tale somma è maggiore di una certa soglia, imposta la mail come Phishing. Altrimenti, impostala come Non-phishing.

Si fa presente ora come, in questo esempio, i task fondamentali più complessi sono i seguenti:
2. Definire un modello statistico adatto $\mathbf{M}$ (ad esempio una distribuzione di probabilità) sull'input di dati reali in maniera tale che l'informazione nascosta possa emergere come un evento probabile in $\mathbf{M}$, e successivamente estrarre queste informazioni in maniera efficiente.
3. Assegnare i giusti pesi alle parole in maniera tale che la somma totale di una mail entrante sia proporzionale alla probabilità che questa sia di phishing.
# Statistical modeling: definizione informale
Consiste nella costruzione di una distribuzione di probabilità sottostante al modello, dalla quale vengono campionati i dati visibili "grezzi".

**Esempio:** Il data set grezzo è un insieme $\mathbf{D}$ di numeri. Un modello statistico per $\mathbf{D}$ è dato dall'assunzione che i numeri vengano campionati secondo una *distribuzione Gaussiana* su $\mathbf{D}.$ In questo caso, la media e la deviazione standard caratterizzano completamente la distribuzione in questione, la quale diventerebbe il modello per l'*Input Data*. 
# Adversarial data models: definizione informale
Altri scenari tipici nel data mining adottano la *worst-case hypotesis*, ossia l'ipotesi del caso peggiore:
Il Data Set in input, ossia da dove deve venir estratta l'informazione cercata, è gestito da una fonte avversaria la quale genera i dati con l'obiettivo di minimizzare l'efficienza della soluzione algoritmica utilizzata o, ancor peggio, la sua validità.

**Esempio:** Calcolare (e aggiornare) il numero di $1$ nell'ultima finestra di lunghezza $N$ su un flusso infinito di bits gestito da un avversario, il quale sceglie il prossimo bit in funzioni delle precedenti scelte effettuate dall'algoritmo.
# Data Mining & Machine Learning
Il Machine Learning risulta spesso un ottimo approccio al data mining. Consiste nell'usare parte del Data Set in input come Training Set, per istruire i sistemi ML. Risulta essere un buon metodo nelle situazioni in cui non è possibile definire una funzione obiettivo chiara sul Data Set, ossia quando non si sa cosa l'input data dice in merito al problema da risolvere.
# Limiti statistici sul Data Mining
L'obiettivo tipico dei problemi di Data Mining è quello di individuare eventi inusuali all'interno di un Data Set di grandi dimensioni. Spesso, gli elementi che costituiscono un Data Set possono essere ottenuti combinando diverse fonti di informazione. L'integrazione dell'informazione, ossia l'idea di relazionare e combinare diverse fonti di dati per ottenere risultati non disponibili a partire da una singola fonte, risulta essere spesso un passo principale per la risoluzione di un problema. Tale approccio può però portare a un problema tecnico: se il Data Set contiene un gran numero di elementi ottenuti da diverse fonti, allora, in maniera casuale, potrebbero verificarsi eventi inusuali, i quali possono non avere un particolare significato, essendo semplicemente artefatti statistici senza significato
## Principio di Bonferroni
Si supponga di avere un data set $DS$ in input, e di cercare uno specifico evento $E$ al suo interno. Ci si può aspettare che tale evento accada, anche se $DS$ è completamente casuale, e che il numero di occorrenze di tale evento aumenti al crescere della dimensione di $DS$. Queste occorrenze prendono il nome di *falsi* ("bogus"): il loro verificarsi non ha nessuna causa, sennonché dati random avranno sempre un certo numero di caratteristiche inusuali che sembrano rilevati, ma che in realtà non lo sono. La correzione di Bonferroni è un teorema che fornisce un metodo per l'individuazione di queste risposte *"falso-positive"* a seguito della ricerca dell'occorrenza di un dato evento all'interno di un data set. Il principio di Bonferroni è una versione informale del rispettivo teorema, che permette di evitare di trattare le occorrenze casuali di un evento come se queste fossero reali:
- Si calcola il numero atteso di occorrenze di un dato evento di interesse, assumendo che il data set sia random.
- Se questo numero risulta essere significativamente più grande del numero di istanze reali che si spera di individuare, allora ci si aspetta che la maggior parte di occorrenze individuate sia un falso.

Si vede ora un esempio di bogus: Gangs in Social Network. In questo problema si ha una rete sociale, modellata mediante grafo, dove ogni agente che vi appartiene è un potenziale criminale. Gli agenti visitano periodicamente dei luoghi pubblici. Assumendo che i criminali si incontrino periodicamente in gruppo, si vogliono individuare queste gangs. 
## Gangs in social network
Sia $G=(V,E)$ un grafo. Un insieme $V$ di agenti, potenziali criminali  ($|V|=n$ con $n$ grande) , visita un insieme $H$ di luoghi pubblici in una città ($|H|=h$ con $h$ grande).
L'arco $(u,v)$ appartiene ad $E$ se $u$ e $v$ visitano lo stesso luogo (appartenente ad $H$) lo stesso giorno per una sequenza di $T>>0$ giorni, dove $T=\textnormal{numero di giorni}$. Si assume che il processo di visita degli agenti nei vari luoghi sia sufficientemente casuale e uniforme. Inoltre, sia per ipotesi $h>>T$. Si vogliono individuare le possibili *gangs*, che, per modellazione del problema, sono rappresentate all'interno di $G$ mediante *cliques*.
Per fare ciò, ci si chiede quale sia la dimensione massimale delle cliques in $G$.
Si ha che $\forall u,v \in V$
$$\mathbf{Pr}[(u,v) \in E] \simeq \frac{T}{h} \doteq p$$
Sia $S \subseteq V$ con $|S|=s$. Si ha che
$$\mathbf{Pr}[S \textnormal{ è una clique}] \simeq p^{(s^2/2)}$$
Quindi si ha che
$$
 \mathbf{E}[\textnormal{\# cliques di size }s] = C(n,s) \ \cdot \   p^{(s^2/2)} \ \simeq  \Bigl( \frac{n}{e \cdot s} \Bigl)^s \cdot \ \  p^{(s^2/2)} \tag{1} 
$$
Osservando un grande sistema spazio-tempo, ad esempio per $n \simeq 10^7, \ h \simeq 10^4, \ T \simeq 10^3$ si ha che $p \simeq \frac{1}{10}$ . Dalla ($1$) ci si aspetta un gran numero di cliques aventi size $s \simeq 10$. Assumendo che non ci si aspetta minimamente un numero così grande di gangs criminali, la presenza di molte cliques aventi size $10$ non permette quindi l'individuazione dei criminali, e quindi l'occorrenza di questo evento non risultano utili per l'informazione cercata.