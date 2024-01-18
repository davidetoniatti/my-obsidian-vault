berkley: alg di sincronizzazione interna, clock sincronizzati non rispetto a sorgente esterna. Viene eletto un processo leader, che chiede il valore del clock agli altri nodi. ottiene questi valori e stima la differenza tra il valore del suo clock con i valori ricevuto (in modo simile ad alg di cristian). fa la media di queste differenza e invia al nodo $i$ il valore correttivo dato da avg-valore stimato$_i$.
Nel caso di correzione negativa, la frequenza del clock viene rallentato in modo da colmare la differenza dopo un lasso di tempo.
per calcolare la media, non vengono presi in conto valori che si discostano troppo dal valor medio.
vantaggi.
l'alg è robusto, anche rispetto ad attacchi.
ha un costo contenuto (implementazione semplice).

punto b
per ogni evento, sia entreno che esterno, ciascun processo incrementa di un unità il proprio valore del clock logico, quando evento riceve msg che contiene clock logico del mittente, la sincronizzazione avviene prendendo il valore massimo tra clock corrente e clock ricevuto.

vettore che ha tante componenti quanti i processi coinvolti. per iesimo process, iesima componente rapppresenta il numero esatto di eventi interni che il processo interno a visto. componente j!=i indica gli eventi avvenuti nel proc j che il proc i ha "visto".
magewa non garantisce ordine totale: due richieste in due sottoinsiemi ma accade che il proc che ha generato dopo la richiesta ottiene prima i