Si definisce in maniera rigorosa e non ambigua il *modello di calcolo* sul quale verranno studiati gli algoritmi distribuiti, anche detti **protocolli**. Tale modello prende il nome di **ambiente distribuito** e sarà utile non solo per definire algoritmi distribuiti, ma anche per fare l'analisi della loro *correttezza* e *stabilità*.
Informalmente, un ambiente distribuito è composto da un **insieme di agenti** aventi ognuno delle **capacità computazionali**, che interagiscono tra di loro scambiandosi dei messaggi (*message passing*) tramite una serie di **interconnessioni**. Tale definizione di ambiente distribuito sarà modellata rigorosamente tramite un **grafo**.
## Entità
L'ambiente distribuito è composto da **entità** (o **nodi**). Ogni nodo della rete ha delle capacità computazionali, le quali sono definite come un insieme di **operazioni possibili**, tra le quali:
- **Local storage e processing**, ovvero la capacità di conservare informazioni e di processarle **localmente**;
- **Scambio di messaggi** con altri nodi della rete;
- **(re-)impostare il clock**, infatti ogni nodo ha un **clock** interno che scandisce le esecuzioni delle sue operazioni, che può essere impostato dal nodo;
- **Cambiare il valore dei registri**, infatti ogni nodo ha dei **registri** usati per eseguire le operazioni e/o mantenere uno **stato** (generalmente viene utilizzato un **registro di stato** apposito).
### Stato di un entità
Ogni entità ha uno **stato** interno, il quale rappresenta un'informazione *necessaria* per il corretto funzionamento di un protocollo distribuito.
Formalmente, ogni nodo ha un insieme finito di stati possibili $\Sigma$ **sempre definito a priori** e in ogni istante un'entità deve trovarsi in uno dei suoi possibili stati.
Ad esempio, un insieme di stati può essere il seguente
$$
\{ \text{idle}, \text{computing}, \text{waiting}, \dots \}
$$
## Eventi
In ambiente distribuito, il *comportamento* di ogni entità è di **reattivo**, ovvero è scatenato in seguito all'avvenimento di **eventi**. In assenza di stimoli, un'entità rimane inerte.
In generale questi eventi possono essere:
- interni al nodo;
- esterni al nodo ma interni all'ambiente distribuito;
- esterni dall'ambiente distribuito.
Alcuni esempi di possibili eventi che modificano il comportamento di un entità sono:
- il *tick* del clock (evento interno al nodo);
- la ricezione di un messaggio (evento interno alla rete);
- un'impulso spontaneo (evento esterno alla rete).
## Azioni
Un'entità *reagisce* in seguito ad un evento. In particolare, l'azione che un nodo compie in risposta ad un evento dipende anche dal suo stato interno nel momento dello stimolo. Dunque una combinazione di evento e stato interno implicano una **azione** da parte di un nodo
$$
\text{stato} \times \text{evento} \mapsto \text{azione}
$$
dove con **azione** si intende una *sequenza di task* (o attività) da svolgere, come ad esempio
- computare un algoritmo;
- inviare dei messaggi;
- cambiare lo stato interno.
In questo modello, ogni azione è **atomica** e **finita**:
- **atomica**: una volta avviata, un'azione non può essere interrotta;
- **finita**: un'azione termina sempre in tempo finito.
Si osserva che un'entità in un certo stato potrebbe non reagire ad un'evento: in questo caso si assume che l'entità ha eseguito l'azione *nil* o azione nulla.
## Comportamento delle entità
Le associazioni delle coppie stato ed evento con un'azione sono dette **regole**.
In generale, si definisce il **comportamento** di un'entità come un insieme di regole, definite per ogni possibile coppia stato ed evento. Data un'entità $x$, sia $\Sigma$ il suo insieme di stati possibili, sia $\mathcal{E}$  l'insieme degli eventi e sia $A$ l'insieme delle azioni che può eseguire. Il suo **comportamento** $B(x)$ è la funzione
$$
B(x): \Sigma \times \mathcal{E} \to A
$$
tsull'alberoale che e $B(x)$ è **deterministica** e **completa** cioè ad ogni possibile coppia stato ed evento è associata un'unica azione 
$$
\forall(s,e) \in \Sigma \times \mathcal{E}, \exists!a \in A
$$
## Comportamento del protocollo
Raggruppando tutti i comportamenti delle entità di un sistema è possibile definire il *protocollo (o algoritmo) distribuito*.
Formalmente, un protocollo è l'insieme
$$
B:=\{ B(x): x \text{ è un entità del sistema} \}
$$
Un sistema si dice **simmetrico (o omogeneo o anonimo)** se tutte le entità hanno lo stesso comportamento
$$
B(x) \equiv B(y) \forall x,y \text{ entità del sistema}
$$
Si osserva che è possibile trasformare ogni sistema asimmetrico in un sistema simmetrico.
Questo significa che se le entità in un sistema hanno comportamenti diversi, è sempre possibile scrivere un nuovo insieme di regole, uguale per tutti, che li farà comportare nello stesso modo.
Si consideri il seguente esempio: nel sistema esistono due tipi di entità: *server* e *client*, che hanno insieme di regole differenti. In particolare, per ogni coppia $(s,e) \in \Sigma \times \mathcal{E}$, i nodi client hanno la regola
$$
(s,e) \mapsto a_{\text{client}} \in A_{\text{client}}
$$
dove $A_{\text{client}}$ è l'insieme di azioni dei nodi client, mentre i nodi server hanno la regola
$$
(s,e) \mapsto a_{\text{server}} \in A_{\text{server}}
$$
dove $A_{\text{server}}$ è l'insieme di azioni dei nodi server.
Allora è possibile fondere i comportamenti dei due tipi di entità creando un unico insieme di regole in cui l'azione eseguita dipende dalla tipologia dell'entità. In particolare, le nuove regole sono del tipo
$$
(s,e) \mapsto \{\text{if role = client then } a_{\text{client}} \text{ else } a_{\text{server}} \}
$$
Se la coppia $(s,e)$ non costituisce nessuna regole in per il client (analogo server) allora nel nuovo insieme di regole l'azione associata a tale coppia avrà $a_{\text{client}} = \text{nil}$ (analogo, $a_{\text{server}} = \text{nil}$).
## Comunicazione
Le entità della rete distribuita possono e devono interagire tra di loro. Questi interagiscono tramite *scambio di messaggi* (o *message passing*), dove un messaggio è inteso essere una **sequenza finita di bit**.
I messaggio viaggiano su una *rete di comunicazione* che può essere modellata per mezzo di un **grafo diretto** $G=(V,E)$ che specifica per ogni nodo con quali entità possono comunicare.
![](adrc_img01.png)
Si osserva che i nodi della rete non hanno una visione *globale* della rete, cioè non conoscono la topologia del grafo, bensì hanno solo una visione **locale**. Formalmente, ogni nodo $x$ vede solamente due gruppi di porte logiche: le porte dalle quali arrivano i messaggi in entrata e le porte dalle quali vengono mandati i messaggi in uscita:
- $N_{\text{out}}(x) \subset V$: insieme dei vicini *uscenti* dell'entità $x \in V$;
- $N_{\text{in}}(x) \subset V$: insieme dei vicini *entranti* dell'entità $x \in V$;
In particolare, il *vicinato* di un nodo $x$ è l'insieme
$$
N(x)= N_{\text{out}}(x) \cup N_{\text{in}}(x)
$$
Dunque, un nodo $x$ può inviare i messaggi solamente ai suoi vicini uscenti in $N_{\text{out}}(x)$
![](adrc_img02.png)
e ricevere messaggi dai suoi vicini entranti in $N_{\text{in}}(x)$
![](adrc_img03.png)
## Assiomi della comunicazione
Ogni argomento verrà trattato sotto i seguenti assiomi riguardo la comunicazione.
### Finite transmission delays
In assenza di *perdite di messaggi*, si assume che il tempo di trasmissione di un messaggio da un nodo verso un suo vicino raggiungibile da un arco uscente sia **finito** ma **sconosciuto**. Formalmente, per ogni messaggio $m$ esiste un tempo $t \in \mathbb{R}^+$ tale che entro il tempo $t$ il messaggio $m$ viene trasmesso lungo l'arco.
### Local orientation
Un entità può comunicare direttamente con un sottoinsieme delle entità: l'insieme dei suoi vicini, e ogni nodo *distingue* i proprio vicini,
In particolare:
- Un entità è sempre in grado di inviare un messaggio solo ad uno specifico vicino uscente, cioè non deve necessariamente inviare il messaggio attraverso tutti i suoi archi uscenti;
- Un entità è sempre in grado di individuare quale dei suoi vicini gli ha inviato un messaggio;
In particolare, per ogni arco orientato $(x,y)$ esistono due etichette: $\lambda_x(x,y)$ per il nodo $x$ e $\lambda_y(x,y)$ per il nodo $y$.
![](adrc_img05.png)
Si osserva che non è detto che $\lambda_x(x,y) = \lambda_y(x,y)$, ovvero l'etichetta di un arco è **locale** ad un nodo. Inoltre, in generale, non è vero che $\lambda_x(x,y) = \lambda_{x}(y,x)$, ovvero un nodo non sa distinguere se una coppia di archi in entrata e uscita lo connettono a uno stesso vicino o meno.
Secondo questa definizione di orientamento locale è possibile definire la **topologia** di un grafo come un grafo _etichettato_ $(G,\lambda_{v})_{v \in V}$.
## Restrizioni del modello
Spesso i protocolli vengono definiti e studiati assumendo ulteriori restrizioni. Naturalmente un protocollo definito sotto un gran numero di restrizioni è generalmente *debole*, nel senso che rimuovendo alcune di queste restrizioni il protocollo potrebbe non funzionare. Un protocollo definito su un sistema con poche restrizioni invece risulta più *generale*, e dunque più forte.
Si elencano alcune tipologie di restrizioni.
### Message ordering
Spesso si assumerà che in assenza di perdita di messaggi, i messaggi trasmessi mediante uno stesso link arrivano nello stesso ordine. Seguono quindi una coda di tipo **FIFO** ordinata con l'ordine di invio dei messaggi.
### Link bidirezionali
Questa restrizione assume che le connessioni nella rete sono *bidirezionali*, dunque ogni nodo è in grado di distinguere gli estremi degli archi. Formalmente
$$
\forall x \in V, N_{\text{out}}(x) = N_{\text{in}}(x) = N(x) \land \forall y \in N(x), \lambda_{x}(x,y) = \lambda_{x}(y,x)
$$
![](adrc_img04.png)
### Restrizioni sulla disponibilità
1. **Delivery garantita**. Ogni messaggio che viene inviato viene ricevuto correttamente, cioè non ci sono corruzioni di messaggi. Sotto questa restrizione, i protocolli non devono tenere conto
2. **Disponibilità parziale**. Non ci saranno perdite di messaggi (*failures*) dal momento in cui si osserva il sistema in poi. In altre parole, in passato il sistema potrebbe aver subito dei fallimenti;
3. **Disponibilità totale**. Non sono mai avvenuti fallimenti in passato e mai avverranno in futuro.
### Restrizioni sulla topologia
Sono tutte quelle assunzioni che si possono fare sulla struttura del grafo $G$ che descrive la rete di comunicazione del sistema. Ad esempio, spesso si assume che $G$ è fortemente connesso.
### Restrizioni sulla conoscenza
Sono tutte quelle assunzioni che riguardano la conoscenza da parte dei nodi di informazioni *globali*, cioè di informazioni che riguardano il sistema distribuito. Ad esempio, si può assumere che sia disponibile come informazione globale il numero dei nodi della rete, il numero dei links, il valore del diametro della rete, ecc.
### Restrizioni temporali
In generale si assumono due possibili restrizioni riguardo i tempi di comunicazione:
- **Bounded communication delay**. Esiste una costante $\Delta$ tale che, in assenza di fallimenti, il tempo di trasmissione dei messaggi tramite qualsiasi link è al più $\Delta$;
- **Clock sincronizzati**. Ogni clock delle entità è incrementato di una unità temporale costante in modo *simultaneo*.
## Misure di complessità
In generale in un sistema distribuito non ha sempre senso parlare di *complessità temporale*, in quanto non è detto che il tempo di trasmissione di un messaggio sia sempre lo stesso, e non è detto che il clock sia uguale per tutti i nodi. Per far sì che abbia senso parlare di _time complexity_ si necessitano le due restrizioni temporali: la *synchronized clocks* e la *Bounded Communication Delay*. Idealmente, tutti i messaggi dovrebbero viaggiare con uno stesso delay unitario.
Un'altro tipo di complessità che verrà sempre analizzata è la cosiddetta _message complexity_, ovvero una complessità che tiene conto del numero di messaggi trasmessi prima che un protocollo termini un suo _task_. Si osserva che, se un messaggio per arrivare dal nodo $a$ al nodo $b$ necessita di essere instradato 4 volte, allora verranno contante tutte e 4 le trasmissioni fatte prima di arrivare a $b$, anche se il messaggio è uno solo.