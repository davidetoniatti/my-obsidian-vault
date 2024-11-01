In questo capitolo, si studiano alcune tecniche utilizzate per comprimere un indice inverso. Nella prima sezione, sono descritti diversi metodi per la compressione del dizionario, mentre la seconda sezione si concentra sulla compressione delle posting list.
# Compressione in IR
La compressione porta una serie di vantaggi:
- Permette di risparmiare spazio su disco (e quindi DANARI);
- Permette un migliore uso della memoria centrale. In particolare, tramite la compressione sarà possibile memorizzare un maggior numero di posting list ricorrenti (cioè molto richieste) in memoria, rendendo l'elaborazione delle query frequenti molto rapida e dunque riducendo il tempo di risposta. A tale scopo è chiaro che la decompressione delle posting list deve essere più efficiente rispetto alla ricerca di quest'ultime, non compresse, su disco. Questo è il motivo principale per il quale si utilizza la compressione in IR;
- Permette una trasmissione dei dati da disco a memoria più veloce. Gli algoritmi di decompressione efficienti funzionano così velocemente sull'hardware moderno che il tempo totale di trasferimento di una porzione di dati compressi dal disco e la successiva decompressione è solitamente inferiore al tempo di trasferimento della stessa porzione di dati in forma non compressa.

In particolare, comprimere il dizionario di un indice inverso porta i seguenti vantaggi:
- Rende il dizionario sufficientemente piccolo in modo da poter risiedere in memoria centrale;
- Lo può rendere talmente piccolo da poter mantenere anche alcune posting list in memoria centrale;
La compressione delle posting list invece porta i seguenti vantaggi:
- Riduce lo spazio utilizzato su disco;
-  Riduce il tempo necessario per leggere le posting list su disco;
- I grandi motori di ricerca tengono un significativo numero di posting list in memoria: comprimendole, si possono mantenere un maggior numero di posting list in memoria centrale.
## Caratteristiche fondamentali
In particolare, uno schema di compressione per poter essere applicato nel campo dell'IR, deve rispettare almeno le seguenti caratteristiche:
- **Lossless compression.** La compressione non deve portare a perdita di informazione;
- **Decompressione efficiente.** Il tempo per leggere i dati compressi e decomprimerli deve essere minore del tempo necessario a leggere i dati non compressi.
### Lossless compression
Una compressione è detta **lossless** quando la fase di compressione non porta a perdita di informazione. In IR si utilizzano esclusivamente compressioni lossless.
Alcuni step nell'elaborazione dei documenti per la creazione dell'indice possono essere di tipo lossy (cioè con perdita di informazioni), come ad esempio la rimozione delle stop-words. Tra queste tecniche vi sono:
- **No numbers**: rimozione dei numeri.
- **Case folding**: non si considerano le maiuscole.
- **30 stopwords**: rimozione delle top 30 stopwords.
- **150 stopwords**: rimozione  delle top 150 stopwords.
- **stemming**: rimozione delle desinenze;
## Leggi empiriche
Si presentano ora un paio di leggi empiriche utili a stimare:
- la dimensione del dizionario in relazione alla dimensione della collezione, con la legge di **Heap**;
-  la dimensione delle posting list, stimando la frequenza dei termini nei vari documenti, con la legge di **Zipf**.
### Legge di Heap
La dimensione del dizionario cresce al crescere della dimensione della collezione. In generale, con l'assunzione che le parole contengano al più 20 caratteri, un possibile upper bound per il dizionario è $70^{20} \approx 10^{37}$, dove $70$  è la dimensione dell'alfabeto.
La **legge di Heap** mette in relazione la dimensione del dizionario alla dimensione della collezione. Sia $M$ la dimensione del dizionario e sia $T$ il numero di token nella collezione, allora
$$
M = k T^b
$$
con, tipicamente, $30\leqslant k \leqslant 100$ e $b \approx 0.5$.
Con questa legge, si può approssimare la dimensione del vocabolario dato il numero di tokens nella collezione.
#### Esempio
Si consideri il dataset RCV1 (Reuters dataset). La legge di Heap approssima molto bene la relazione tra la dimensione del dizionario e la dimensione della collezione. Il seguente grafico mostra come la retta tratteggiata, ottenuta dalla legge di Heap con i parametri $k=10^{1.64}$ e $b=0.49$, approssima sempre meglio la relazione tra la dimensione del dizionario e quella della collezione, al crescere del numero di tokens.
![center|600](0E70ECBA7BEAEDD4603B8238D5D4384E.png)
### Legge di Zipf
Lo spazio occupato dalle posting lists dipende anche dalla frequenza dei token nei vari documenti. In particolare, più frequente è un token, e più sarà presente in diversi documenti, quindi più $\text{docID}$ sono salvati nella postings list di quel token.
La **legge di Zipf** viene utilizzata per stimare la frequenza relativa di ogni termine. Essa esprime che nei linguaggi naturali ci sono, tendenzialmente, poche parole con una elevata frequenza, e tante parole con una frequenza molto più bassa. In particolare, la legge stima che, dati i tokens ordinati per frequenza (dal più frequente al meno frequente), l'$i$-esimo termine più frequente ha una frequenza proporzionale a
$$
\text{cf}_{i} \approx \frac{1}{i} = \frac{K}{i}
$$
dove $K$ è una costante di normalizzazione, mentre $\text{cf}_{i}$ indica la *collection frequency* dell'$i$-esimo termine più frequente nella collezione.
Una conseguenza della legge di Zipf è che se il termine più frequente appare $\text{cf}_{1}$ volte, allora il secondo termine più frequente apparirà più o meno $\frac{\text{cf}_{1}}{2}$, il terzo termine più frequente apparirà più o meno $\frac{\text{cf}_{1}}{3}$ volte, e cosi via.
# Compressione del dizionario
Si vedono alcune delle possibili tecniche utili alla compressione del dizionario di un indice inverso.
## Struttura dati dizionario - versione naive
La struttura dati più semplice da costruire per mantenere un dizionario consiste nel mantenere i termini in array con entrate di lunghezza fissata, ordinati secondo l'ordine lessicografico. In particolare per ogni array si allocano 20byte per il termine, 4byte per la document frequency e 4byte per il puntatore alla posting list.

```ad-note
title: Nota
Un puntatore da 4byte permette di risolvere 4GB di spazio degli indirizzi.
```

![center|600](D5CC5A947DFAC255C303435E139C4A46.png)
la struttura è tale da permettere una ricerca veloce, tramite ricerca binaria, dei termini all'interno contenuti.
Memorizzando in questa struttura dati il dataset di Reuters, la dimensione totale del dizionario risulta pari a 11.2MB.
## Dizionario come stringa
Memorizzare i termini in celle di lunghezza fissa porta ad un importante spreco di memoria. Infatti, i termini nella lingua inglese sono mediamente lunghi 8 caratteri (dunque 8 byte), dunque in media sono sprecati 12 byte di memoria per ogni termine nel dizionario. Inoltre, in tali celle non è possibile memorizzare quelle poche parole che risultano più lunghe di 20 caratteri.
Queste limitazioni vengono superate memorizzando i termini del dizionario come una lunga stringa di caratteri. Allora ogni array del dizionario al posto di un termine mantiene un puntatore che punta al primo carattere del termine che l'array rappresenta nella stringa dei termini del dizionario. Si osserva che, dato un puntatore alla prima lettera di un termine, il puntatore successivo indica la fine di quest'ultimo. 
![center|600](0804A55F9605FC8310CDCD8D1B787728.png)
Memorizzando RCV1 usando quest'ultima strategia, allora sono necessari, per ogni termine:
- 4 byte per memorizzare la frequenza del termine;
- 4 byte per memorizzare il puntatore alla posting list;
- 3 byte per memorizzare il puntatore al termine: infatti quest'ultimo deve poter indirizzare $400.000 \times 8 = 3.2 \times 10^6$ posizioni (400.000 è il numero di termini nella collezione, 8 è il numero di caratteri per temine in media), e per fare ciò sono necessari $\log_{2}3.2 \times 10^6 \approx 22 \text{ bits}$ cioè 4 byte.
Dunque per ogni termine si devono mantenere 19 byte (11 per l'array, 8 per il termine) contro i 28 byte nell'implementazione banale: in totale il dizionario occupa uno spazio pari a $7.6 \text{ MB}$.
## Blocking
La tecnica del **blocking** permette di comprimere ulteriormente il dizionario raggruppando i termini della stringa in blocchi di dimensione $k$ (numero di termini in ogni blocco) e mantenendo un puntatore solo per il primo termine di ciascun blocco: si memorizzano i puntatori solo per ogni $k$-esimo termine. Si osserva che è necessario memorizzare la lunghezza dei termini in modo da poterli recuperare: per ogni termine, viene memorizzata nella stringa come byte aggiuntivo posto all'inizio del termine. Con tale approccio si eliminano $k-1$ puntatori, ma sono necessari $k$ byte aggiuntivi per memorizzare la lunghezza di ogni termine.
![center|600](E8139DB3EC5A30E9CD62AFAFAC3EC737.png)
In generale, maggiore è $k$ e maggiore sarà lo spazio risparmiato. Al crescere di $k$ però aumenta anche il tempo necessario per trovare il termine: più è grande e più sarà grande la porzione di stringa da scorrere linearmente ad ogni ricerca.
![center|600](02797542FAC6CA26CEF313C88EFC805D.png)

Memorizzando RCV1 con questa tecnica con $k=4$, si ottiene che:
- per ogni blocco si utilizzando si risparmiano 9 byte per i tre puntatori a termine, dunque ogni blocco occupa $4 \times 4 + 4 \times 4 + 3 + 4 = 39$ byte; 
- considerando gli 8 byte per ogni termine, dato che un blocco mantiene 4 termini, si ottiene una dimensione totale pari a $39+32=71$ byte;
Dato che i termini sono $400.000$, con $k=4$ il dizionario occupa uno spazio pari a
$$
\frac{400.000}{4} \times 71 = 7.1 \text{ MB} 
$$
## Front coding
Un'ulteriore tecnica di compressione è il **front coding**, utilizzata per comprimere parole che hanno un lungo prefisso in comune. L'idea è quella di memorizzare solo le differenze rispetto al prefisso condiviso.
![center|600](01484F18153F19F258854CD4E0081AC0.png)
## Conclusione
Applicando tutte queste tecniche al dataset RCV1, si ottiene
![center|600](E01D47BAF623186BD1420848B77F7BE3.png)
# Compressione delle posting list
Le posting lists, che ricordiamo essere liste contenenti $\text{docID}$, sono molto più grandi del dizionario. La compressione delle posting lists si basa su tecniche che permettano di utilizzare il minor numero di bytes per rappresentare i $\text{docID}$.
Si consideri il dataset di Reuters, contenente 800.000 documenti. Con un $\text{docID}$ di 4 bytes, si possono identificare tutti i documenti. L'obiettivo è quello di utilizzare meno di 4 bytes per rappresentare ciascun documento.

In prima battuta, esistono due possibili modi per gestire le posting lists:
- Le posting lists associate a termini poco frequenti, sono gestite usando il numero di bits necessario per indicizzare ogni documento. Nel caso di reuters questo equivale a 20 bits.
- Le posting lists associate a termini molto frequenti sono invece rappresentate come bitmap vectors. Infatti, in questo caso, risulta estremamente inefficiente memorizzare ogni documento con 20 bits.
## Gap compression
Si osserva che per ogni termine, la lista di documenti contenenti quel termine è ordinata in ordine crescente rispetto al $\text{docID}$. Allora è possibile comprimere le posting lists cambiandone la rappresentazione: al posto di un sistema di riferimento assoluto per i vari $\text{docID}$, si adotta un sistema relativo, memorizzando i gaps tra gli identificativi, e non il valore assoluto.
Si consideri il termine $\text{computer}$; allora la differenza tra un riferimento assoluto e uno relativo è la seguente
$$
\begin{split}
   \text{absolute} &\longrightarrow 33, \,\,\, 47, \,\,\, 154, \,\,\, 159, \,\,\, 202 \\
   \text{relative} &\longrightarrow 33, \,\,\, 14, \,\,\, 107, \,\,\, 5, \,\,\, 43 \\   
   \end{split}
$$
Si osserva che con questa nuova rappresentazione non si perde informazione. Per individuare il $\text{docID}$ nella $i$-esima posizione nella nuova rappresentazione, è sufficiente sommare tutti i valori presenti nella lista fino all'$i$-esimo. Si adotta tale tecnica perché, intuitivamente, la maggior parte dei gap può essere memorizzata con meno di 20 bit. In particolare, i migliori risultati sono ottenuti quando viene applicata a posting list associate a termini che sono comuni. Ma la memorizzazione dei gap per termini molto rari implica l'utilizzo di un numero di bit decisamente maggiore rispetto ai termini frequenti. Allora, per mantenere i vantaggi di questa rappresentazione, è necessario utilizzare un metodo che codifica ogni gap con un numero di bit strettamente necessario per quest'ultimo. Dunque è necessaria una *codifica a lunghezza variabile*.
## Variable bit codes
Le codifiche a livello di bit riguardano i singoli bit. Teoricamente, con le codifiche a livello di bit si ottengono i migliori risultati in termini di compressione. Nella pratica, le macchine moderne operano a livello di byte, dunque tale codifiche spesso risultano inefficienti una volta implementate, dunque vengono raramente utilizzate.
Si presentano ora un paio di codifiche variabili che lavorano sui singoli bit.
### Unary code
La **codifica unaria**, rappresenta il numero $n$ come ripetizioni del bit $1$ seguito da un bit $0$ finale. Segue una tabella di conversione con qualche esempio.
$$
\begin{array}{c |l}
   n & \text{unary code for } n \\
   \hline
   3 & 1110 \\
   5 & 111110 \\
   7 & 11111110 \\
   10 & 11111111110 \\
   \end{array}
$$
### Codifica gamma
La **codifica gamma** rappresenta un valore di gap $G$ come una coppia $(\text{length, offset})$ dove:
- L'**offset** è rappresentato dal valore di $G$ in binario. Escludendo il caso $G=0$, il bit più significativo di $G$ è sempre $1$ per definizione, allora tale bit non viene memorizzato;
- La **lunghezza** è la lunghezza sintattica dell'offset, ovvero il numero di bit necessari per esprimere l'offset, escludendo il bit più significativo. Tale lunghezza viene espressa con una codifica unaria.
#### Esempio
 Il codice gamma per il numero $13$ si ottiene con i seguenti passaggi:
1. Offset per $13$:$$(13)_{10} \to (1101)_{2} \to (101)_{\gamma}$$
2. Lunghezza dell'offset per $13$ è $3$, dunque in unario $(1110)_{1}$
Segue che il codice gamma per $13$ è
$$
\Gamma(13) = (1110,101) = 1110101
$$
Altri esempi di conversione sono riportati nella seguente tabella.
$$
\begin{array}{c |l}
   n & \text{gamma code for } n \\
   \hline
   0 & \text{none} \\
   1 & 0 \\
   5 & 11001 \\
   9 & 1110001 \\
   13 & 1110101 \\
   \end{array}
$$

Si osserva che tramite la codifica gamma il valore $G$ viene codificato con un numero di bit pari a $2\lfloor \log_{2}G \rfloor +1$, in particolare:
- L'offset con $\lfloor \log_{2}G \rfloor$ bit;
- La lunghezza dell'offset con $\lfloor \log_{2}G \rfloor +1$ bit.
In conclusione, la codifica gamma permette di codificare il valore dei gap in modo estremamente efficiente: infatti per codificare un gap $G$ sono necessari $\log_{2}G$ bit, e la codifica gamma è molto vicina a questo lower bound.
## Variable byte codes
Le codifica variabile a livello di bit nelle macchine moderne non risultano efficienti. Infatti quest'ultime operano a livello di byte. Dunque si devono codificare i valori di gap usando il minor numero di bytes. A soluzione di tale problema si introduce il concetto di **continuation bit**, denotato con $c$. Tale bit si utilizza come segue:
- Se $G\leqslant 127$, allora si codifica $G$ con i primi $7$ bit di un byte. In questo caso quindi l'ultimo bit, ovvero quello più significativo, è il continuation bit $c$ e viene settato a $1$;
- Se $G>127$, allora si codificano i primi $7$ bits meno significativi di $G$ nel primo byte, e il continuation bit del byte viene posto a $0$. La restante parte di $G$ si codifica con altri bytes, seguendo lo stesso schema. L'ultimo byte ha il continuation bit posto a $1$.

Questa famiglia di codifica prende il nome di **_Variabile Bytes codes_** (VB codes). Utilizzando questa codifica si possono memorizzare le posting lists come sequenze di bytes. I VB codes, e alcune loro estensioni, sono i codici effettivamente utilizzati nella pratica.
![center|600](E463F85969AAA3AB8869EA76F661E191.png)
# RCV1: compressione finale
![center|600](B28CD0FE819C4C26DC18323F1B3C2908.png)