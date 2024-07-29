# Byzantine broadcast
Si considera il seguente sistema distribuito:
- Si ha un insieme di $n$ nodi o agenti $[n] = \{ 1,\dots,n \}$ che formano la rete distribuita;
- Ogni nodo può eseguire computazioni locali arbitrarie e inviare messaggi di lunghezza arbitraria ad ogni altro nodo.
Nel **Byzantine Broadcast problem**, introdotto negli anni '80 da sir [Leslie Lamport](https://en.wikipedia.org/wiki/Leslie_Lamport), un nodo designato come *sorgente* riceve in input un messaggio $b$ (ad esempio, un bit), che deve diffondere nella rete. Si vorrebbe che tutti i nodi raggiungano il consenso sul bit $b$. Per raggiungere tale scopo, si deve definire un **protocollo**.
## Scenario operativo
Per analizzare il problema, si deve definire bene lo scenario operativo, ossia le .condizioni in cui lavorano gli agenti della rete. In particolare
### 1. Non tutti i nodi seguono il protocollo
I nodi **non** seguono necessariamente il protocollo, cioè ci sono degli agenti *onesti* che rispettano il protocollo e degli agenti *corrotti* che non lo seguono. Si osserva che se cosi non fosse, risulterebbe banale progettare un protocollo che risolve il problema.
In particolare, sia $f$ il numero di nodi corrotti (o *faulty*) con $0 \leq f \leq n$ e, naturalmente, sia $n-f$ il numero di nodi onesti. Inoltre, sia $1 \in [n]$ il nodo sorgente al quale viene affidato l'input $b$ da propagare all'interno della rete. Si vuole progettare un protocollo al termine del quale ogni nodo *onesto* $i$ dia in output un valore $y_i$ tale che rispetti le seguenti proprietà:
1. **Terminazione**: tutti i nodi onesti restituiscono un output rispettivo $y_i$ entro un tempo finito;
2. **Validità**:  se la sorgente è onesta, allora tutti i nodi onesti restituiscono in output il bit $b$ in input alla sorgente. Formalmente, se la sorgente è un nodo onesto, allora $$y_{i} = b \quad \forall i \text{ nodo onesto.}$$
3. **Consistenza**: tutti i nodi onesti devono restituire in output lo stesso valore. Formalmente, se $i$ e $j$ sono due nodi onesti e $y_i,y_j$ sono i loro rispettivi output, allora $y_i = y_j$.
I $n-f$ nodi onesti applicheranno il protocollo in modo rigoroso, mentre gli $f$ nodi corrotti possono comportarsi come vogliono: si può fare l'assunzione *worst-case* per la quale tutti i nodi corrotti si coalizzano al fine di far fallire il protocollo. Inoltre, non si è a conoscenza di quali siano i nodi corrotti.
### 2. I messaggi inviati arrivano ai destinatari
Si assume di trovarsi in un sistema sincrono, in cui:
- Esiste un **clock globale** noto a tutti i nodi che scandisce il tempo in round discreti: 0,1,2,...
- Ogni messaggio inviato in un round $t$ arriva al destinatario <u>prima</u> del round $t+1$.
### 3. Si conosce il numero di nodi
In particolare, la rete è composta da $n$ nodi $\{ 1,\dots,n \}$ e la sorgente è il nodo $1$ (**permissioned model**).
### 4. Public Key Infrastructure (PKI)
Si assume che i nodi sono identificati. Inoltre, si assume che per un messaggio che risulta essere proveniente da un nodo $i$, si può avere la certezza che questo sia veramente stato spedito dal nodo $i$.
In particolare, si assume che ci sia nel sistema un **Public Key Infrastructure (PKI)** setup, ossia:
- Ogni nodo $i$ dispone di una coppia di chiavi $(sk_{i},pk_{i})$ dove $sk_i$ è la *chiave privata (secret key)* di $i$, mentre $pk_i$ è la *chiave pubblica (public key)* di $i$;
- L'insieme delle chiavi pubbliche $\{ pk_{i}: i \in [n] \}$ è noto a tutti a priori.
In pratica, dato un messaggio $m$, si indica con $\langle m \rangle_i$ il messaggio $m$ con l'aggiunta di una firma valida eseguita dal nodo $i$ con la sua chiave privata $sk_i$.
## Un protocollo per BB
Si vuole definire un protocollo per la risoluzione del problema **BB**. Si assume che la topologia del grafo con cui viene modellata la rete sia quella del grafo completo: il nodo sorgente può comunicare direttamente con tutti gli agenti restanti.
In particolare, il protocollo deve rispettare le tre condizioni: **Consistenza**, **Validità** e **Terminazione**.
Un primo tentativo di risoluzione è il seguente.
### Tentativo 0.1
#### Il protocollo
- **Round 0**. La sorgente (il nodo $1$) riceve $b \in \{0,1\}$ in input;
- **Round 1**. La sorgente invia $\langle b \rangle_{1}$ a tutti i nodi;
- **Round 2**. Ogni nodo $i$: se ha ricevuto nel round 1 un unico messaggio $\langle b \rangle_1$, restituisce in output $b$. Altrimenti, restituisce in output $0$.
#### Analisi
- Il protocollo termina sempre: **terminazione soddisfatta**.
- **La validità è soddisfatta**. Infatti, se la sorgente è onesta, allora questa invierà al round 1 il messaggio $\langle b \rangle_1$ a tutti i nodi. Dunque ogni nodo onesto $i$ al round 2 restituisce in output $y_i = b$ come da protocollo;
- **La consistenza non è soddisfatta**. Infatti, se la sorgente è disonesta, questa può trasmettere a due nodi onesti $i$ e $j$ distinti rispettivamente due messaggi $b_i$ e $b_j$ tali che $b_i \neq b_j$. Quindi al round 2 i due nodi onesti $i,j$ restituiscono $y_i \neq y_j$.
#### Variante: insieme dei nodi corrotti noto
Come esercizio, si vuole mostrare un protocollo banale che rispetta validità e consistenza nel caso in cui l'insieme dei nodi corrotti è noto.
Se l'insieme dei nodi corrotti è noto, allora i nodi onesti possono eseguire un azione al round 2 in base all'onestà o meno della sorgente. In particolare, al round 2 ogni nodo $i$:
- se ha ricevuto nel round 1 un unico messaggio $\langle b \rangle_1$ e il nodo $1$ non è corrotto, restituisce in output $b$. Altrimenti, restituisce output $0$.
Si osserva che modificando il round 2 in questo modo, il protocollo rispetta validità e consistenza. In particolare, la consistenza risulta soddisfatta perché quando la sorgente è corrotta, tutti i nodi onesti restituiscono in output $0$ per definizione del protocollo.
### Tentativo 0.2
A titolo di esempio, si mostra ora un protocollo banale che soddisfa la consistenza ma non la validità.
#### Il protocollo
- **Round 0**. La sorgente (il nodo $1$) riceve $b \in \{0,1\}$ in input;
- **Round 1**. La sorgente invia $\langle b \rangle_{1}$ a tutti i nodi;
- **Round 2**. Ogni nodo $i$ restituisce in output $0$.
#### Analisi
- Il protocollo termina sempre: **terminazione soddisfatta**.
- **La validità non è soddisfatta**. Infatti, ogni nodo onesto $i$ restituisce in output $y_{i}=0$ anche nel caso in cui $b = 1$;
- **La consistenza è soddisfatta**. Infatti, se la sorgente è disonesta, questa può trasmettere a due nodi onesti $i$ e $j$ distinti rispettivamente due messaggi $b_i$ e $b_j$ tali che $b_i \neq b_j$. Ma per definizione del protocollo, i due nodi onesti $i,j$ restituiscono $y_i=y_j=0$. Questo è vero per ogni coppia $i,j$ con $i\neq j$.
### Tentativo 1
#### Il protocollo
- **Round 0**. La sorgente (il nodo $1$) riceve $b \in \{0,1\}$ in input;
- **Round 1**. La sorgente invia $\langle b \rangle_{1}$ a tutti i nodi;
- **Round 2**. Ogni nodo $i$: se nel round 1 ha ricevuto $\langle \hat{b} \rangle_1$, invia $\langle \hat{b} \rangle_i$ a tutti i nodi. Altrimenti, invia $\langle 0 \rangle_i$ a tutti i nodi;
- **Round 3**. Ogni nodo $i$: se c'è un unico valore $\hat{b}$ per cui nel round 2 ha ricevuto più di $n/2$ messaggi $\langle \hat{b} \rangle_{j}$, restituisce in output $\hat{b}$. Altrimenti restituisce in output $0$.
#### Analisi
- Il protocollo termina sempre: **terminazione soddisfatta**.
- **La validità non è soddisfatta**. Infatti, nel caso in cui il numero dei nodi corrotti è maggiore di $n/2$, questi possono mettersi d'accordo e inviare ad uno (o più) nodi onesti $n/2 + 1$ messaggi contenenti $\hat{b} \neq b$.
- **La consistenza non è soddisfatta**. Stessa motivazione della validità.
### Tentativo 2
#### Il protocollo
- **Round 0**. La sorgente (il nodo $1$) riceve $b \in \{0,1\}$ in input;
- **Round 1**. La sorgente invia $\langle b \rangle_{1}$ a tutti i nodi;
- **Round 2**. Ogni nodo $i$: se nel round 1 ha ricevuto $\langle b \rangle_1$, invia $\langle b \rangle_i$ a tutti i nodi. Altrimenti, invia $\langle 0 \rangle_i$ a tutti i nodi;
- **Round 3**. Ogni nodo $i$: se nel round 2 ha ricevuto il messaggio $\langle \hat{b} \rangle_{j}$ da ogni altro nodo $j$, restituisce in output $\hat{b}$. Altrimenti restituisce in output $0$.
#### Analisi
- Il protocollo termina sempre: **terminazione soddisfatta**.
- **La validità non è soddisfatta**. Infatti, è sufficiente che un nodo corrotto qualsiasi (diverso dalla sorgente) mandi un messaggio diverso da $b$ ad uno dei nodi onesti per fargli restituire in output $0$.
- **La consistenza non è soddisfatta**. Stessa motivazione della validità.

## Protocollo di Dolev-Strong
Si mostra ora un protocollo che rispetta le condizioni di consistenza e validità per il problema del Byzantine Broadcast.
### Il protocollo
Sia $\mathcal{E}_i$ un'insieme che nel protocollo prende il nome di **extracted set** e sia $f$ il numero di nodi corrotti. Il protocollo di Dolev-Strong è il seguente.
____
- **Round 0**. Ogni nodo $i$ inizializza un insieme vuoto $\mathcal{E}_i = \emptyset$; La sorgente $1$ riceve in input $b$ e invia a tutti i nodi $\langle b \rangle_1$.
- <u>for each round</u> $r=1,\dots,f$. Ogni nodo $i$:
	- <u>for each</u> messaggio $\langle \hat{b} \rangle_1,j_{1},\dots,j_{r-1}$ con $r$ firme distinte (inclusa quella della sorgente) ricevuto nel Round $r-1$:
		- <u>if</u> $b \not\in E_i$: aggiungi
- **Round 2**. Ogni nodo $i$: se nel round 1 ha ricevuto $\langle b \rangle_1$, invia $\langle b \rangle_i$ a tutti i nodi. Altrimenti, invia $\langle 0 \rangle_i$ a tutti i nodi;
- **Round 3**. Ogni nodo $i$: se nel round 2 ha ricevuto il messaggio $\langle \hat{b} \rangle_{j}$ da ogni altro nodo $j$, restituisce in output $\hat{b}$. Altrimenti restituisce in output $0$.