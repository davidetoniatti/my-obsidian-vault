I problemi e i protocolli considerati finora erano formulati per un ambiente *permissioned*, nel quale i nodi che partecipano al protocollo sono noti a tutti e non cambiano durante l'esecuzione del protocollo.
In un ambiente *permissionless* invece i nodi che partecipano al protocollo non sono noti a priori e possono variare durante l'esecuzione del protocollo; in particolare:
- non sono noti a priori i nodi che partecipano al protocollo;
- in ogni momento nuovi nodi possono partecipare al protocollo. Inoltre, i nodi che già eseguono il protocollo possono abbandonarlo;
- Partecipare al protocollo significa, essenzialmente, scaricare un *software* ed eseguirlo, entrando in comunicazione con gli altri nodi che eseguono tale software;
- In tale scenario, i canali **non** sono autenticati.

Progettare un protocollo di consenso in un ambiente *permissionless* significa introdurre delle misure atte a mitigare la possibilità di **sybil attack**, ossia limitare la capacità di un singolo nodo *corrotto* di generare più identità e quindi *simulare* più nodi. In particolare, si osserva che sicuramente, in un ambiente permissionless, non si può usare la nozione di maggioranza per raggiungere il consenso: i nodi si aggiungono ed escono dal protocollo senza controllo, in particolare un attore corrotto può mettere in atto un **sybil attack** per ottenere la maggioranza.
Inoltre, nei primi anni 2000 nel mondo dei database distribuiti, viene formulato e dimostrato il cosiddetto **CAP Theorem** (Consistency, Availability, Partition tolerance), dove
- **Consistency**: tutti i client che accedono al database vedono gli stessi dati nello stesso momento, indipendentemente dal nodo a cui si connettono.
- **Availability**: Qualsiasi client deve poter accedere al sistema (database), anche quando uno o più nodi non sono disponibili;
- **Partition tolerance**: Una partizione in un sistema distribuito è una interruzione nella comunicazione all'interno del sistema. Partition tolerance significa che il sistema deve continuare a funzionare nonostante qualsiasi numero di interruzioni nella comunicazione tra i nodi nel sistema.
Il teorema CAP dice che queste tre proprietà **non** possono essere soddisfatte contemporaneamente. Ad esempio, per avere partition tolerance in un sistema distribuito, non si possono avere consistency e availability al massimo livello.
Nel mondo permissioned, i protocolli studiati privilegiano consistency a discapito di partition tolerance: se non c'è maggioranza di nodi onesti, il protocollo non termina.

Nel novembre del 2008, un tizio (o più) sotto il nome di *Satoshi Nakamoto* [annuncia](https://satoshi.nakamotoinstitute.org/emails/cryptography/threads/1), per mezzo di un [Whitepaper](https://bitcoin.org/bitcoin.pdf), un nuovo protocollo di consenso, detto **Nakamoto consensus**. Questo protocollo utilizza la *proof-of-work* per:
- impedire che un singolo nodo corrotto possa agire come più nodi onesti, cioè come meccanismo di **sybil resistance**;
- fare in modo che tutti i nodi onesti concordino su quali sono i dati *corretti* da tenere in memoria, cioè come meccanismo di **consenso distribuito**. 
In particolare, non è garantita la consistenza ad ogni round, ma il sistema diventa consistente col tempo: il meccanismo di consenso è la **longest proof-of-work chain**.
Con il termine *proof-of-work* si intende una breve "dimostrazione", che può essere facilmente verificata (da un punto di vista computazionale), che una certa quantità di lavoro computazionale è stata eseguita.

Nel sistema Bitcoin, ogni nodo onesto deve:
- mantenere una copia locale di un vettore $BLOCK$, a cui vengono periodicamente aggiunti nuovi elementi detti blocchi. In particolare, $BLOCK$: $BLOCK[0],BLOCK[1],\dots$, dove $BLOCK[t]$ è il $t$-esimo blocco aggiunto al vettore. Il vettore di blocchi è la cosiddetta **blockchain**.
- ricevere in input periodicamente delle transazioni $tx$, da registrare all'interno di $BLOCK$;
Gli obiettivi del sistema sono:
- **Eventual Consistency**: le copie locali del vettore di blocchi $BLOCK$ dei singoli nodi onesti devono essere concordi, eccetto al più per un numero limitato di blocchi alla fine del vettore, cioè $\exists k \in \mathbb{N}$ tale che $BLOCK[0:t-k]$ è uguale per tutti i nodi onesti per ogni $t\geqslant k$;
- **Liveness**: se un nodo onesto riceve in input una transazione $tx$, questa prima o poi viene inserita in un blocco;
- Un nodo onesto che entri a far parte della rete in qualunque momento e chieda ad altri nodi nella rete di fornirgli il vettore $BLOCK$ condiviso dagli altri, deve avere un modo di discernere quale sia il vettore *corretto*, in caso riceva informazioni contrastanti da nodi diversi.

Si osserva che rispetto a state machine replication, gli ultimi $k$ blocchi possono essere diversi tra nodi onesti; in state machine replication, era solamente tollerata la mancanza degli ultimi $k$ blocchi.

idea nakamoto:
Ogni blocco è diviso in due parti: **header** e **data**, formalmente $BLOCK[i] = (\text{header},\text{data}) \forall i$.
L'**header** è formato esattamente da $80$ byte: $32$ byte ciascuno per `prev_hash` e `data_hash` che sono rispettivamente lo `SHA256` dell'$\text{header}$ del blocco precedente (`SHA256`$(BLOCK[i-1]\text{.header})$) e lo `SHA256` della parte $\text{data}$, opportunamente serializzati, e $4$ byte ciascuno per `ver`,`timestamp`,`target`,`nonce`:
- `ver` è un numero di versione che in genere non ha nessun ruolo;
- `timestamp` è la data e ora attuale rappresentata in Unix time, ossia un numero intero che indica il numero di secondi trascorsi dalla mezzanotte del primo gennaio 1940;
- `target` è un elemento cruciale, che viene utilizzato per fare in modo che il numero di blocchi creati si mantenga, in media, a un ritmo di 1 blocco ogni 10 minuti circa, indipendentemente da quanti siano i nodi della rete e quale sia il loro potere computazionale;
- `nonce` è un numero intero qualunque a $32$ bit tale che, dati gli altri 5 campi, faccia in modo che lo `SHA256` dello $\text{header}$ sia minore del target.
$\text{header}=$`(ver, prev_hash, data_hash, timestamp, target, nonce)`
Si osserva che il primo blocco, il BLOCK$[0]$, è scritto dentro al codice. Questo blocco è detto **genesis block**.
pseudo protocollo nakamoto:
```pseudo
	\begin{algorithm}
	\caption{Nakamoto Consensus}
	\begin{algorithmic}
	\State \texttt{ver} = 1.0
	\State \texttt{block}[0] = (GENESIS\texttt{.header}, GENESIS\texttt{.data})
	\State t = 0
	\State \texttt{data} = $\emptyset$
	\While{True}
    \EndWhile
	
	\end{algorithmic}
	\end{algorithm}
```
inizializzazione:
	block 0 genesis block
t = 0
data = emptyset
while true
  prev_hash = sha256(block t.header)
  timestamp = now
  target = update_target(BLOCK)
  while true:
    data_hash = sha256(data)
	header(...)
	if sha256(header) < target:
	  t+=1
	  block(t) = (header,data)
	  invia block(t) a tutti
	  break
	 else
	  nonce+=1
	 if recieve "tx"
	   if validate(tx):
	     aggiungi tx a data
	 if receive block'(t+1) da nodo u:
	   if verifica_blk(block'(t+1)):
	     t+=1
	     block(t) = block'(t)
	     break
	   else
	     chiede a nodo u (BLOCK')
	     if Pow(BLOCK') > Pow(BLOCK):
		    BLOCK = BLOCK'
		     t = len(BLOCK)
		     break  

target:
obiettivo -> un blocco ogni 10min
target0 = 32bit a 0 e 224bit a 1
target aggiornato ogni 2016 blocchi: se timestamp del blocco 2016- blocco 0 < 2 settimane: si riduce il target della quantità opportuna.