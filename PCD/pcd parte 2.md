nov 2008: protocollo annunciato;
permissionless:
- non si sa chi sono i nodi che partecipano al protocollo
- in ogni momento nuovi nodi si aggiungono e potenzialmente vecchi nodi escono;
- partecipare al protocollo: scaricare un sw ed eseguirlo
- in tale scenario, i canali NON sono autenticati

sybil attack: agente simula tanti altri entità distinte
nel mondo permissionless, maggioranza non ha senso perché si aggiungono e escono nodi senza controllo
sybil resistance: resistenza a tale fatto; bitcon: proof of work.
CAP theorem (consistency, avalibility, partition tollerance): tutte e tre insieme non si possono ottenere.
pow: per mandare

meccanismo di consenso: longest pow chain

ogni nodo:
- mantiene un vettore BLOCK: BLOCK(0),BLOCK(1),...
- riceve in input periodicamente delle transazioni $tx$;
goal:
- consistency: tutti i nodi onesti hanno la stessa sequenza di blocchi BLOCK;
- liveness: se un nodo riceve in input $tx$. rima o poi $tx$ finisce in BLOCK.

si rilassa la consistency: **eventual consistency**.
$\exists k \in \mathbb{N}: BLOCK[0:t-k]$ è uguale per tutti i nodi onesti per ogni $t\geqslant k$.
rispetta a state machine replication, ultimi k blocchi possono essere diversi tra nodi onesti; in smr, era tollerato che ultimi k blocchi mancano.

idea nakamoto:
$BLOCK[i] = (\text{header},\text{data})$;
header=(versione: 4byte, prev_hash: 32byte, data_hash: 32byte, timestamp: 4byte, target: 4byte, nonce: 4byte)

prev_hash: sha256(BLOCK$[i-1]$.header) hash dell'header del blocco precedente;

BLOCK$[0]$ sta dentro al codice: detto genesis block.
pseudo protocollo nakamoto:
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