proprietà degli algoritmi di mutua esclusione
assenza di starvasion: nessun processo rimane bloccato per sempre nella trying section. Nel caso dell'elezione vuol dire che tutti i processi hanno la possibilità di essere eletti come leader.
no deadlock: c'è sempre un processo che riesce ad entrare in sezione critica, elezione: viene eletto un leader.
ordering- le diverse richieste di accesso in sessione critica vengono servite in ordine di arrivo.
condizioni di ordering e assenza di starvation sono proprietà di fairness, nel senso che alg si comporta in modo fair con tutti i processi.

Sistemi distribuiti, la comunicazione è ossibile solo tramite scambio di messaggi. Si addatta algoritmo del panificio di lamport, trasformando le variabili condivise in operazioni di invio di messaggi.
sistema N processi asincrono e non soggetti a fallimenti, comunicazione affidabile (no messaggi persi o replicati) e FIFO ordered. inoltre si assume che i processi trascorrono tempo finito nella sezione critica.
panificio di lamport 6n messaggi.

classificazione algoritmi di mutua esclusione distribuiti in base a come viene presa la decisione su chi deve entrare in sezione critica:
- alg basati su autorizzazione. processo che vuole entrare in CS chiede autorizzazione: o a coordinatore centrale o a tutti i processi
- basati su token: tra i processi circola un token, chi detiene il token può accedere alla CS.
- basati su quorum o votazione: un processo che vuole accedere alla risorsa condivisa chiede il voto ad un sottoinsieme di processi.

metriche per valutare prestazioni degli algoritmi:
- verifica proprietà;
- calcolo efficienza in termini di messaggi che vengono scambiati: nro di messaggi scambiati per entrare in sezione critica (quanto tempo ciascun processo deve aspettare per entrare in CS dopo richiesta di accesso); nro complessivo di messaggi per entrare e uscire da SC (stima della banda di rete consumata).

1o alg: centralizzato con coordinatore centrale.
Processo $p$ manda richiesta di accesso a CS al coordinatore. se risorsa libera, il coordinatore informa $p$ che gli è consentito l'accesso. altrimenti, accoda la richiesta di $p$ all'interno di una coda con politica FIFO e lo informa che per il momento l'accesso non gli è consentito.
vantaggi:
- semplice
- garantisce safety e liveness (no starvation grazie a politica fifo della coda)
- efficiente perché solo 3 messaggi: richiesta, risposta e rilascio per entrare e uscire dalla sezione critica.

alg lamport distribuito
ciascun processo che vuole entrare in cs manda richiesta a tutti gli altri processi, allegando il valore del sul clock logico scalare e identificativo.
ciascun processo ha una coda locale in cui mantiene le richieste di accesso in sc che ha ricevuto dagli altri processi. mantiene le richieste ordinate in base al valore del timestamp (sua richiesta inclusa).