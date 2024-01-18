un algoritmo di elezione ha lo scopo di eleggere un coordinatore o leader tramite un processo di elezione: ad esempio scegliere un processo leader rispetto a certe operazioni. L'elezione di un cordinatore a runtime è necessaria perché il coordinatore corrente potrebbe avere un fallimento, dunque scelta del coordinatore utile anche a runtime.
Elezione richiede il raggiungimento di un consenso distribuito.
modello di sd:
sistema con n processi; i processi possono crashare; la comunicazione è affidabile (msg non si perdono); ogni processo avvia una sola elezione alla volta; ciascun processo ha ID univoco e lo scopo dell'elezione è quello di eleggere il processo non guasto con ID massimo.
proprieta alg di elezione:
safety: soltanto un processo viene eletto come leader, ed è quello no guasto con id più alto. Il risultato dell'elezione non dipende dal processo che avvia l'elezione; quando più processi avviano elezione, si arriva allo stesso risultato: eletto processo non guasto con id più alto.
liveness: prima o poi, viene eletto un nuovo leader
## alg bullo
idea: il nodo con id più alto cerca di fare il prepotente per ottenere la leadership.
un processo $p_i$ si accorge che leader corrente è guasto. $p_i$ cerca di farsi eleggere come leader: manda un messaggio di elezione a tutti i processi $p_k$ con id $k>i$. Se nessun $p_k$ risponde, allora $p_i$ vince elezione e diventa nuovo leader e comunica agli altri processi che diventa nuovo leader. Se invece qualche processo $p_k$ risponde, dato che $k>i$ , lo blocca e prova a diventare leader avviando una nuova elezione.
Supporta proprietà di liveness

importante- assunzione di avere sistema sincrono: se asincrono, non si ha la certezza se un nodo non risponde xk è crashato (essendo sincrono, deve rispondere prima di andare avanti, dunque se non risponde dopo un tot vuol dire che il nodo è crashato).
Costo di comunicazione dipende dall'id del processo che inizia l'elezione: caso migliore quando se ne accorge p_n-1 quando il leader è p_n. caso peggiore quando se ne accorge il processo con id più piccoloallora

partizione di rete: situazione di guasto in cui la rete viene suddivisa in due sottoinsieme di processi disgiunti: processi tra i due insiemi non riescono a comunicare.