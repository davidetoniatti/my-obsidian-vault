La consistenza nasce per via della replicazione nei sistemi distribuiti. La replicazione è utile a incrementare la disponibilità del sistema distribuito. Se serivizio replicato su $n$ nodi, la probabilità che fallisca, cioè che falliscano i due nodi è p^n. Inoltre migliora la tolleranza ai guasti del sistema. Infine, migliora le performance e la scalabilità del sd.
Il problema della replicazione è mantenere le repliche consistenti tra di loro: alla copia di un dato su un server, tutte le repliche devono avere il dato copiato.

per avere consistenta, bisogna eseguire le operazioni in conflitto nello stesso ordine su tutte le repliche. ad esempio operazioni di lettura e scrittura concorrenti sullo stesso dato o scrittura scrittua.

si rilassa requisit di consistenza, in modo da avere un sistema più efficiente.

un data store distribuito è un insieme di nodi su cui avviene la memorizzazione di dati. sono fisicamente distribuiti e possono essere replicati. rientrano database replicati, filesystem distribuiti ecc.
un modello di consistenza è un "contratto" tra data store distribuito e i processi in cui il datastore specifica le condizioni sotto le quali i risultati di operazioni di lettura e scrittura concorrenti vengono visti dai vari processi.

modelli data-centrici: forniscono una vista consistente a livello dell'intero sistema: tutti gli utenti vedono la stessa costa.
client centrici: hanno uno scope più limitato: fornire un datastore consistente a livello del singolo utente.

non c'è una soluzione generale e unica per la consistenza, ma molteplici soluzioni adatte a differenti requisiti di consistenza. diverse soluzioni hanno diverse facilità di utilizzo e danno diverse garanzie rispetto alla disponibilità del sistema.
piu si rilassano i requisiti di consistenza, sistema piu efficiente e veloce ma maggiori costi per lo sviluppatore.