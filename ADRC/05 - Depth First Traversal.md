A differenza del problema del broadcast in cui si deve condividere un'informazione con tutti i nodi della rete, si consideri la situazione in cui c'è una risorsa da distribuire in modo *mutualmente esclusivo* tra i nodi. In particolare esiste una sola copia della risorsa e in un qualunque istante temporale esiste un solo nodo che la possiede.
In particolare si studia una strategia di condivisione nota come **depth first traversal**. L'idea alla base prevede che ogni nodo cerca di inoltrare il token ai suoi vicini fino a quando tutti i nodi a lui adiacenti hanno già ricevuto il token. Quando un nodo non può inoltrare il token in "avanti", lo rimanda "indietro" al nodo che glielo ha inoltrato precedentemente.
# Il problema
Formalmente, il problema è definito dalla seguente tripla $P = \langle P_{init}, P_{final}, R \rangle$ dove
- $P_{init}$: solo l'initiator ha il token;
- $P_{final}$: tutti i nodi hanno ricevuto il token seguendo l'ordine di una DFS.
- $R = \begin{cases} \text{Total Reliability (TR)} \\ \text{Bidirectional Link (BL)} \\ \text{Connectivity (CN)} \\ \text{Unique Initiator (UI)}\end{cases}$
# Protocollo BACK
Il protocollo più semplice che risolve il problema DFT è il seguente:
1. Quando un nodo riceve il token per la *prima volta*, ricorda il nodo che gli ha inviato il token (il padre) e inoltra il token ad uno dei suoi vicini non visitati con il messaggio $\text{TOKEN}$, aspettando la sua risposta;
2. Quando il vicino riceve il token, se lo ha già ricevuto lo ritorna immediatamente con un messaggio $\text{BACK-EDGE}$, altrimenti lo inoltra a tutti i suoi vicini in modo sequenziale, per poi ritornarlo al nodo da cui lo ha ricevuto con un messaggio $\text{RETURN}$;
3. Quando il nodo riceve il messaggio $\text{RETURN}$ dal suo vicino, inoltra il token ad un altro vicino;
4. Infine, quando il nodo finisce tutti i vicini, invia il messaggio $\text{RETURN}$ al nodo da cui inizialmente aveva ricevuto il token, ossia il nodo che ha segnato come padre.

Si formalizza la seguente strategia. Il protocollo fa uso degli stati seguenti
- $S = \{ \text{initiator,idle,visited,done} \}$
- $S_{init} = \{ \text{initiator,idle} \}$
- $S_{term} = \{ \text{done} \}$
Il comportamento dei nodi è descritto dalle seguenti regole
```python
```
dove `VISIT` è la procedura utilizzata dai nodi per implementare la DFT ed è la seguente

## Message complexity
Si osserva che nel protocollo appena descritto esistono tre tipologie di messaggi, che sono:
- Messaggi TOKEN: usati per scambiare il token;
- Messaggi $\text{BACK-EDGE}$: usati per indicare che un nodo ha già ricevuto il token;
- Messaggi $\text{RETURN}$: usati per ritornare il token dopo averlo ricevuto per la prima volta.
Dato un arco $(x,y) \in E$, sono possibili le seguenti situazioni
![](adrc_img23.png)
allora su ogni arco passano esattamente due messaggi. Dunque la message complexity vale
$$
M(\text{BACK})= 2m
$$
### Lower bound alla message complexity
Si osserva che non si può progettare un protocollo per il problema DFT che utilizza $o(m)$ messaggi. Infatti risolvendo DFT si risolve anche il problema broadcast, dunque il lower bound del broadcast si riflette anche su questo problema.
Allora in termini di message complexity il protocollo BACK risulta essere asintoticamente ottimo.
## Ideal Time Complexity
Si osserva che nel protocolo BACK ogni nodo esplora in modo sequenziale tutti i suoi vicini. Questo significa la ideal time complexity è pari a
$$
T(\text{BACK})=\Theta(m)
$$
Notiamo però che in termini di ideal time complexity il problema DFT ha un lower bound di $\Omega(n)$. Quindi esiste un gap abbastanza significativo tra il lower bound e la ideal time complexity del protocollo discusso.
# Protocollo BACK+
Utilizzando il protocollo BACK in grafi *densi*, ovvero grafi con molti archi, la maggior parte del tempo viene speso dai nodi aspettando la ricezione dei messaggi $\text{BACK-EDGE}$. Per evitare questo spreco di tempo, si progetta un protocollo che non utilizza i messaggi $\text{BACK-EDGE}$. In particolare, si effettuano le seguenti modifiche al protocollo BACK:
1. Quando un nodo $x$ riceve il token, informa tutto il suo vicinato $N(x)$ che possiede il token;
2. Tutti i nodi in $N(x)$ inviano un messaggio di ACK al nodo $x$;
3. Dopo aver ricevuto tutti gli ACKs, il nodo $x$ invia il token ad uno dei suoi vicini che non sono ancora visitati.
Tale protocollo prende il nome di BACK+. La differenza sostanziale rispetto al protocollo BACK è che i "back-edges" sono scoperti in parallelo.
## Message complexity
Si osserva che tutti i nodi tranne l'initiator ricevono un solo messaggio TOKEN e inviano un solo messaggio $\text{RETURN}$. L'initiator non può ricevere nessun messaggio TOKEN e non invia nessun messaggio $\text{RETURN}$. Dunque si hanno esattamente $2(n-1)$ messaggi inviati di tipo $\text{RETURN}$ e TOKEN.
Ogni nodo invia un messaggio VISITED a tutti i nodi a lui adiacenti, tranne al nodo da cui ha ricevuto il messaggio. Allora il numero totale di messaggi VISITED è pari a
$$
|N(s)| + \sum_{x \in V \setminus \{ s \}} |N(x)|-1 = 2m - (n-1)
$$
Infine, si osserva che ogni messaggio VISITED implica un corrispondente messaggio di ACK. Dunque il numero totale di messaggi è pari a
$$
M(\text{BACK+}) = \underbrace{2(2m-(n-1))}_{\text{VISITED e ACK}} + \underbrace{2(n-1)}_{\text{TOKEN e $\text{RETURN}$}} = 4m
$$
## Ideal Time Complexity
Si assuma di eseguire il protocollo BACK+ in un sistema sincrono con u delay di trasmissione pari a $\Delta = 1$. Allora vale che:
- I messaggi TOKEN e $\text{RETURN}$ sono inviati in modo sequenziale, impiegando tempo $2(n-1)$;
- I messaggi ACK e VISITED sono inviati in modo parallelo. In particolare, ogni nodo impiega 2 unità di tempo per queste operazioni. Il tempo totale impiegato tra l'invio e la ricezione di questi messaggi è quindi pari a $2n$.
In totale si ottiene
$$
T(\text{BACK+}) = 2(n-1) + 2n = 4n-2
$$
il che risulta essere asintoticamente ottimale.
# Ulteriori modifiche
#TODO 