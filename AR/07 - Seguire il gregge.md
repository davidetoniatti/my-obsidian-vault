Con questa serie di lezioni si tratta la parte finale del corso nella quale si studia un diverso aspetto delle reti, ovvero **il contenuto informativo** di una rete, il quale:
- può indurre gli individui che la compongono a modificare il proprio comportamento (*questa lezione*);
- deve essere sintetizzato per derivare una informazione cumulativa che tenga conto, in qualche modo, dei pezzi di informazione derivanti dai singoli individui ([08 - Sistemi di voto](08%20-%20Sistemi%20di%20voto.md));
- dal quale deve essere individuato ed estratto quello rilevante ad una certa richiesta ([09 - Web Search](09%20-%20Web%20Search.md)).
# Reti e Informazioni
Si è già mostrato che l'informazione presente in una rete può indurre gli individui che la compongono a modificare il proprio comportamento, parlando di *processi di diffusione*. Nello studio dei [processi di diffusione](06%20-%20Processi%20di%20diffusione.md) si è visto come il comportamento dei singoli individui veniva influenzato dalla _diretta comunicazione_ col proprio vicinato.
Ora si vedrà come le decisioni dei singoli individui possono essere influenzate dalle *osservazioni* fatte verso il comportamento degli altri, senza lo scambio diretto di alcuna informazione. In sostanza come è possibile _estrapolare informazioni_ dal comportamento della massa.

```ad-important
title: Importante
L'informazione presente in una rete può indurre gli individui che la compongono a modificare il proprio comportamento.
```

Si vede qualche esempio di tale fenomeno.
### Esempio 1: cenare in una città sconosciuta
Si supponga di essere in vacanza in un nuovo posto e di dover scegliere un ristorante nel quale mangiare. In base alle ricerche personali risulta che il ristorante **A** è il migliore che ci sia in zona, e quindi si sceglie tale ristorante. Poco prima di entrare però si osserva che nel ristorante accanto, il ristorante **B**, c'è moltissima clientela, mentre il ristorante **A** è praticamente vuoto. Non è del tutto irrazionale pensare che tutte le persone che vanno al ristorante **B** abbiano **informazioni private** che non si conoscono, e supporre che in realtà il ristorante **B** è il migliore. In effetti, il fatto che il ristorante **A** sia quasi vuoto non aiuta: verrebbe da pensare che si è in possesso di informazioni non del tutto esatte o complete. Dunque si decide di andare al ristorante **B**.

In questo caso si dirà che è avvenuta una **cascata informativa** (o **herding**). Una cascata informativa occorre quando gli individui prendono decisioni in maniera sequenziale, osservando ciò che hanno fatto gli altri prima e cercando di inferire qualche informazione aggiuntiva che ha spinto gli altri ad agire in tale maniera.
### Esempio 2: a naso in su
La cosa più interessante è che gli individui _"imitano"_ gli altri per via di una serie di ragionamenti ed inferenze più che sensate. Naturalmente, l'imitazione può verificarsi anche a causa della pressione sociale, all'esigenza di volersi conformare, senza alcuna causa informativa sottostante, e non è sempre facile distinguere questi due fenomeni. 

Si consideri l'esperimento di _Milgram_, _Bickman_ e _Berkowitz_ del 1960, nel quale vengono poste agli angoli delle strade dei gruppi di persone (da un minimo di 1 a un massimo di 15) a guardare il cielo senza alcun motivo. Si è osservato quanti passanti si sono fermati e hanno volto lo sguardo al cielo. Si è visto che con una sola persona, pochissimi passanti si fermavano; se 5 persone fissavano il cielo, alcuni passanti si fermavano a imitare, ma la maggior parte li ignorava; infine, con un gruppo di 15 persone che guardano verso l'alto, il 45% dei passanti imitava, fermandosi a guardare il cielo cercando di capire.  

Quest'ultimo esperimento lascia intendere che esiste una **soglia critica** oltre la quale si scatena l'_herding_ su una considerevole porzione di popolazione. Infatti, vedendo due persone che fissano il cielo, è naturale pensare che i due individui non siano proprio sani mentalmente. Invece vedendone 15, si potrebbe inferire l’esistenza di una motivazione razionale che induce queste persone a farlo.

Alcune domande sorgono spontanee:
- esiste sempre una soglia critica che scatena l'herding di massa?
- e se esiste, come trovarla?
## Il gioco delle due urne
Per capire il valore di questa soglia critica, si considera e analizza in profondità un terzo esperimento: il **gioco delle due urne**.
Questo gioco appartiene ad un genere di giochi con la seguente tipologia di regole:
1. C'è una decisione da prendere;
2. I giocatori prendono le proprie decisioni in *sequenza* (uno dopo l'altro) e ogni giocatore può osservare le scelte fatte da coloro che hanno agito prima;
3. Ogni giocatore mantiene alcune *informazioni private* che guidano la propria decisione;
4. Un giocatore non può conoscere *direttamente* le informazioni private di altri giocatori, ma può *dedurre* tali informazioni osservando ciò che fanno.
Seguendo queste indicazioni, il gioco delle due urne è il seguente:
- C'è uno **scommettitore** chiuso in una stanza e una serie di **giocatori** che attendono fuori;
- Lo scommettitore inserisce due palline rosse e una pallina blu in un’urna (denominata con **MR**, a *Maggioranza Rossa*), e due palline blu e una pallina rossa in un'altra urna (denominata con **MB**, a *Maggioranza Blu*);
- Poi, lo scommettitore “mischia” le due urne e ne sceglie una a caso;
- Un giocatore alla volta entra nella stanza, estrae una pallina dall'urna (scelta a caso nel passo precedente), e poi la reinserisce;
- Il giocatore comunica a tutti quanti se, sulla base delle informazioni in suo possesso, ritiene che l'urna sia **MR** o **MB**. **Nota bene**: il giocatore non deve comunicare il colore della pallina estratta;
- Al termine vinceranno solo i giocatori che hanno indovinato quale urna è stata scelta inizialmente.
Per definizione di questo gioco, l'*informazione privata* dei singoli giocatori è l'esito dell'estrazione (pallina blu o pallina rossa), e questa non viene mai condivisa con gli altri. Un giocatore però può inferire quale urna è meglio scegliere in base alle scelte fatte dagli altri prima. Dunque si cerca di capire in che modo, e in base a quali motivazioni, i giocatori prenderanno le loro decisioni.
##### Giocatore 1:
Prima di estrarre la pallina, il giocatore detiene una sola informazione, ossia *"l'urna è MR o MB"*, dunque la probabilità che l'urna sia MR o MB è la stessa. Dopo aver estratto una pallina, la probabilità che l'urna sia di un certo tipo verrà modificata: se estrae una pallina blu (rossa), il giocatore penserà che è più probabile che l'urna sia MB (MR). Perciò, gli conviene scommettere in accordo alla pallina che ha estratto.
##### Giocatore 2:
Prima di estrarre la pallina, può dedurre l'esito dell'estrazione del primo giocatore sulla base della sua scommessa. Si supponga senza perdere di generalità che il primo giocatore abbia estratto una pallina blu, e dunque abbia scommesso che l'urna è di tipo MB.
- Se il Giocatore 2 estrae una pallina rossa, sa che sono state estratte una pallina blu e una pallina rossa: quindi, si trova nella stessa situazione in cui si trovava il Giocatore 1 prima dell'estrazione, perciò gli conviene scommettere MR in accordo alla sua estrazione.
- Se il Giocatore 2 estrae una pallina blu, allora sa che sono state estratte due palline blu e questo rinforza l'ipotesi che l'urna sia MB, dunque scommette che l'urna è MB.
##### Giocatore 3:
Prima di estrarre la pallina, il giocatore deduce ciò che hanno estratto i giocatori precedenti sulla base delle loro scommesse: se sono discordi sulla scelta allora sono avvenute due estrazioni discorde, se sono concordi sulla scelta allora sono avvenute due estrazioni di palline dello stesso colore. Si consideri il caso in cui le prime due estrazioni siano discordi, in questo caso al terzo giocatore conviene rispondere in accordo a ciò che pesca (se pesca una pallina blu gli conviene votare MB, se ne pesca una rossa gli conviene votare MR).
Si consideri ora il caso in cui i primi due giocatori abbiamo voti concordi, e senza perdita di generalità si supponga che abbiano entrambi votato MB. Se il terzo giocatore pesca una pallina blu, allora vuol dire che sono state estratte tre palline blu di seguito, rafforzando la probabilità che MB sia la risposta esatta. Se invece estrae una pallina rossa, comunque è più probabile che la risposta esatta sia MB, perché sono state estratte due palline blu di seguito e poi una rossa. Quindi in ogni caso al terzo giocatore conviene votare **MB**.
##### Giocatore 4:
A questo punto il quarto giocatore può dedurre ciò che hanno fatto tutti i giocatori precedenti solamente se ci sono 2 voti concordi ed uno discorde. Infatti se ci fossero 3 voti concordi, il terzo giocatore avrebbe votato in accordo ai primi due in ogni caso a prescindere dall'esito della sua estrazione. Si supponga di essere nella situazione di _"3 a 0"_ per **MB**. Al quarto giocatore conviene, esattamente come per il terzo giocatore, votare **MB** in ogni caso.

Per ogni Giocatore $x$ con $x \geqslant 5$, valgono le considerazioni del Giocatore 4: si è innescato un fenomeno a cascata, ossia una **cascata imitativa** in favore di **MB**. Qualunque saranno le estrazioni, ogni giocatore risponderà **MB**.
### Analisi del gioco
Per formalizzare meglio la meccanica precedentemente descritta è necessario enunciare il **Teorema di Bayes**.
#### Teorema di Bayes
Siano $A,B \in \Omega$ due eventi di probabilità non nulla, allora
$$
\mathbf{Pr}(A|B) = \frac{\mathbf{Pr}(B|A)\mathbf{Pr}(A)}{\mathbf{Pr}(B|A)\mathbf{Pr}(A) + \mathbf{Pr}(B|A^{\mathcal{C}}) \mathbf{Pr}(A^{\mathcal{C}})  }
$$
##### Dimostrazione
Per definizione di probabilità condizionata, vale che
$$
\mathbf{Pr}(A|B) = \frac{\mathbf{Pr}(A\cap B)}{\mathbf{Pr}(B)}
$$
$$
\mathbf{Pr}(B|A) = \frac{\mathbf{Pr}(A\cap B)}{\mathbf{Pr}(A)}
$$
questo implica che
$$
\mathbf{Pr}(A \cap B) = \mathbf{Pr}(A | B)\mathbf{Pr}(B) = \mathbf{Pr}(B | A)\mathbf{Pr}(A)
$$
Sostituendo opportunamente, si ottiene
$$
\mathbf{Pr}(A|B) = \frac{\mathbf{Pr}(A\cap B)}{\mathbf{Pr}(B)} = \frac{\mathbf{Pr}(B | A)\mathbf{Pr}(A)}{\mathbf{Pr}(B)}
$$

Facendo delle opportune sostituzioni, si riscrive $\mathbf{Pr}(B)$ come
$$
\begin{align}
\mathbf{Pr}(B) &= \mathbf{Pr}(B \cap \Omega) \\
&= \mathbf{Pr}(B \cap (A \cup A^{\mathcal{C}}))  \\
&= \mathbf{Pr}((B \cap A) \cup (B \cap A^{\mathcal{C}}))  \\
&= \mathbf{Pr}(B \cap A) + \mathbf{Pr}(B \cap A^{\mathcal{C}}) \\
&= \mathbf{Pr}(B | A)\mathbf{Pr}(A) + \mathbf{Pr}(B | A^{\mathcal{C}})\mathbf{Pr}(A^{\mathcal{C}})
\end{align}
$$
dunque segue il teorema.
#### Analisi
Si utilizza quest'ultima legge per analizzare il gioco delle due urne. Si considerino gli eventi:
- $MR$: *"l'urna è a maggioranza rossa"*;
- $MB$: *"l'urna è a maggioranza blu"*;
- $r$: *"è stata estratta una pallina rossa"*;
- $b$: *"è stata estratta una pallina blu"*;
si osserva inoltre che
- $MR = MB^{\mathcal{C}}$;
- $MB = MR^{\mathcal{C}}$;
- $b = r^{\mathcal{C}}$;
- $r = b^{\mathcal{C}}$;
Secondo le regole del gioco, vale che
$$
\mathbf{Pr}(MR) = \mathbf{Pr}(MB) = \frac{1}{2}
$$
$$
\mathbf{Pr}(r|MR) = \frac{2}{3}; \mathbf{Pr}(r|MB) = \frac{1}{3}
$$
$$
\mathbf{Pr}(b|MR) = \frac{1}{3}; \mathbf{Pr}(b|MB) = \frac{2}{3}
$$
**Giocatore 1**: supponendo senza perdita di generalità che il primo giocatore estrae una pallina blu, si calcola usando il teorema di Bayes la probabilità che l'urna sia MR o MB
$$
\begin{align}
\mathbf{Pr}(MB|b) &=  \frac{\mathbf{Pr}(b|MB)\mathbf{Pr}(MB)}{\mathbf{Pr}(b|MB)\mathbf{Pr}(MB) + \mathbf{Pr}(b|MR)\mathbf{Pr}(MR)} = \frac{2}{3}  \\
\mathbf{Pr}(MR|b) &=  \frac{\mathbf{Pr}(b|MR)\mathbf{Pr}(MR)}{\mathbf{Pr}(b|MR)\mathbf{Pr}(MR) + \mathbf{Pr}(b|MB)\mathbf{Pr}(MB)} = \frac{1}{3}
\end{align}
$$
dunque al primo giocatore conviene votare l'urna MB, cioè in accordo alla sua estrazione.
**Giocatore 2**: per il secondo giocatore, si considerino prima la situazione in cui estrae una pallina rosse e poi la situazione in cui estrae una pallina blu.
- Sia $br$ l'evento *"sono state estratte in ordine una pallina blu e una rossa"*. Dato che il secondo giocatore può inferire con certezza, assumendo che giochi correttamente, cosa ha estratto il primo giocatore, allora per il secondo giocatore si calcolano le seguenti probabilità $$\begin{align} \mathbf{Pr}(MB|br) &=  \frac{\mathbf{Pr}(br|MB)\mathbf{Pr}(MB)}{\mathbf{Pr}(br|MB)\mathbf{Pr}(MB) + \mathbf{Pr}(br|MR)\mathbf{Pr}(MR)} = \frac{1}{2}  \\
\mathbf{Pr}(MR|br) &=  \frac{\mathbf{Pr}(br|MR)\mathbf{Pr}(MR)}{\mathbf{Pr}(br|MR)\mathbf{Pr}(MR) + \mathbf{Pr}(br|MB)\mathbf{Pr}(MB)} = \frac{1}{2} \end{align}$$ perciò il secondo giocatore non può ottenere alcuna informazione utile dall'estrazione del primo, dunque gli conviene votare l'urna in accordo a ciò che estrae.
- Sia $bb$ l'evento *"sono state estratte (in ordine) due pallina blu"*. Per il secondo giocatore si calcolano le seguenti probabilità $$\begin{align} \mathbf{Pr}(MB|bb) &=  \frac{\mathbf{Pr}(bb|MB)\mathbf{Pr}(MB)}{\mathbf{Pr}(bb|MB)\mathbf{Pr}(MB) + \mathbf{Pr}(bb|MR)\mathbf{Pr}(MR)} = \frac{4}{5}  \\
\mathbf{Pr}(MR|bb) &=  \frac{\mathbf{Pr}(bb|MR)\mathbf{Pr}(MR)}{\mathbf{Pr}(bb|MR)\mathbf{Pr}(MR) + \mathbf{Pr}(bb|MB)\mathbf{Pr}(MB)} = \frac{1}{5} \end{align}$$ dunque l'informazione riguardo la prima estrazione è realmente utile, in quanto la probabilità è nettamente più sbilanciata verso $MB$.

**Giocatore 3**: sempre supponendo che la prima estrazione sia stata una pallina blu, ci si può trovare nelle situazioni $br$ o $bb$. Se i primi due giocatori hanno voti discordi, vale
- caso $brb$, cioè viene estratta dal terzo giocatore una pallina blu: $$\begin{align} \mathbf{Pr}(MB|brb) &=  \frac{\mathbf{Pr}(brb|MB)\mathbf{Pr}(MB)}{\mathbf{Pr}(brb|MB)\mathbf{Pr}(MB) + \mathbf{Pr}(brb|MR)\mathbf{Pr}(MR)} = \frac{2}{3}  \\
\mathbf{Pr}(MR|brbb) &=  \frac{\mathbf{Pr}(brb|MR)\mathbf{Pr}(MR)}{\mathbf{Pr}(brb|MR)\mathbf{Pr}(MR) + \mathbf{Pr}(brb|MB)\mathbf{Pr}(MB)} = \frac{1}{3} \end{align}$$
- caso $brr$, cioè viene estratta dal terzo giocatore una pallina rossa$$\begin{align} \mathbf{Pr}(MB|brr) &=  \frac{\mathbf{Pr}(brr|MB)\mathbf{Pr}(MB)}{\mathbf{Pr}(brr|MB)\mathbf{Pr}(MB) + \mathbf{Pr}(brr|MR)\mathbf{Pr}(MR)} = \frac{1}{3}  \\
\mathbf{Pr}(MR|brr) &=  \frac{\mathbf{Pr}(brr|MR)\mathbf{Pr}(MR)}{\mathbf{Pr}(brr|MR)\mathbf{Pr}(MR) + \mathbf{Pr}(brr|MB)\mathbf{Pr}(MB)} = \frac{2}{3} \end{align}$$
Perciò se i primi due voti sono discordi al terzo giocatore conviene votare in accordo alla sua estrazione (come già intuito in precedenza).
Se i primi due giocatori hanno voti concordi, vale
- caso $bbb$: $$\begin{align} \mathbf{Pr}(MB|bbb) &=  \frac{\mathbf{Pr}(bbb|MB)\mathbf{Pr}(MB)}{\mathbf{Pr}(bbb|MB)\mathbf{Pr}(MB) + \mathbf{Pr}(bbb|MR)\mathbf{Pr}(MR)} = \frac{8}{9}  \\
\mathbf{Pr}(MR|bbb) &=  \frac{\mathbf{Pr}(bbb|MR)\mathbf{Pr}(MR)}{\mathbf{Pr}(bbb|MR)\mathbf{Pr}(MR) + \mathbf{Pr}(bbb|MB)\mathbf{Pr}(MB)} = \frac{1}{9} \end{align}$$
- caso $bbr$: $$\begin{align} \mathbf{Pr}(MB|bbr) &=  \frac{\mathbf{Pr}(bbr|MB)\mathbf{Pr}(MB)}{\mathbf{Pr}(bbr|MB)\mathbf{Pr}(MB) + \mathbf{Pr}(bbr|MR)\mathbf{Pr}(MR)} = \frac{2}{3}  \\
\mathbf{Pr}(MR|bbr) &=  \frac{\mathbf{Pr}(bbr|MR)\mathbf{Pr}(MR)}{\mathbf{Pr}(bbr|MR)\mathbf{Pr}(MR) + \mathbf{Pr}(bbr|MB)\mathbf{Pr}(MB)} = \frac{1}{3} \end{align}$$
In questo caso al giocatore 3 conviene votare in accordo a ciò che hanno votato i primi due giocatori, a prescindere da quale sarà l'esito della sua estrazione.
**Giocatore 4**: Senza perdita di generalità, si supponga che le prime due estrazioni siano entrambe di palline blu. Il giocatore 4 non può essere certo di cosa ha estratto il giocatore 3, perché sceglie l'urna in accordo con i voti dei primi due giocatori. Dunque si calcola la probabilità nel caso in cui il terzo giocatore ha estratto una pallina rossa e quella in cui ne ha estratta una blu.
$$
\begin{align}
\mathbf{Pr}(MB|bbbb) = \frac{16}{17}; \ &\mathbf{Pr}(MR|bbbb) = \frac{1}{17} \\
\mathbf{Pr}(MB|bbbr) = \frac{8}{9}; \ &\mathbf{Pr}(MR|bbbr) = \frac{1}{9} \\
\mathbf{Pr}(MB|bbrb) = \frac{8}{9}; \ &\mathbf{Pr}(MR|bbrb) = \frac{1}{9} \\
\mathbf{Pr}(MB|bbrr) = \frac{1}{2}; \ &\mathbf{Pr}(MR|bbrr) = \frac{1}{2} \\
\end{align}
$$
Perciò votando **MB** il giocatore 4 non ricade mai nella strategia che minimizza la probabilità di successo. Al più può ritrovarsi nel caso in cui le probabilità di successo e insuccesso equivalgono, ovvero quando giocatori 3 e 4 estraggono entrambi una pallina _rossa_. Purtroppo però il giocatore 4 non può sapere cosa ha estratto il terzo giocatore, perciò gli conviene votare comunque **MB** a prescindere dalla sua estrazione.

**Giocatore 5**: sapendo che le prime due estrazioni sono state entrambe blu, il giocatore 5 deve calcolare tutte le probabilità rispetto alle possibili combinazioni di estrazioni dei giocatori 3 e 4.
Caso $b b b b$ :
$$
\begin{aligned}
& \mathbf{Pr}(M B \mid b b b b b)=\frac{32}{33} ; \mathbf{Pr}(M R \mid b b b b b)=\frac{1}{33} \\
& \mathbf{Pr}(M B \mid b b b b r)=\frac{8}{9} ; \mathbf{Pr}(M R \mid b b b b r)=\frac{1}{9}
\end{aligned}
$$
Caso $bbbr$:
$$
\begin{aligned}
& \mathbf{Pr}(M B \mid b b b r b)=\frac{8}{9} ; \mathbf{Pr}(M R \mid b b b r b)=\frac{1}{9} \\
& \mathbf{Pr}(M B \mid b b b r r)=\frac{2}{3} ; \mathbf{Pr}(M R \mid b b b r r)=\frac{1}{3}
\end{aligned}
$$
Caso $bbrb$:
$$
\begin{aligned}
& \mathbf{Pr}(M B \mid b b r b b)=\frac{8}{9} ; \mathbf{Pr}(M R \mid b b r b b)=\frac{1}{9} \\
& \mathbf{Pr}(M B \mid b b r b r)=\frac{2}{3} ; \mathbf{Pr}(M R \mid b b r b r)=\frac{1}{3}
\end{aligned}
$$
Caso $bbrr$:
$$
\begin{aligned}
\mathbf{Pr}(M B \mid b b r r b) & =\frac{2}{3} ; \mathbf{Pr}(M R \mid b b r r b)=\frac{1}{3} \\
\star \mathbf{Pr}(M B \mid b b r r r) & =\frac{1}{3} ; \mathbf{Pr}(M R \mid b b r r r)=\frac{2}{3}
\end{aligned}
$$
Al giocatore 5 conviene sempre scommettere su **MB** a meno che il terzo e il quarto giocatore non abbiano estratto due palline _rosse_. Questo purtroppo il giocatore 5 non può saperlo, in quanto i giocatori 3 e 4 votano **MB** a prescindere dall'esito delle proprie estrazioni.  

Perciò se le prime due estrazioni sono _blu-blu_ si genera una cascata imitativa in cui tutti i giocatori voteranno **MB**, a prescindere dalle proprie informazioni private.
### Modello generale di Sequential Decision Making
Come visto negli esperimenti precedenti o nel gioco delle due urne, il fenomeno dell'_Herding_ si presenta in situazioni che hanno delle caratteristiche in comune, ovvero:
1. Ogni individuo deve prendere una decisione.
2. Ogni individuo ha una propria informazione **privata**.
3. Ogni individuo riceve dalla rete un'informazione incompleta, ovvero sa le scelte degli altri individui ma non l'informazione che li ha spinti a prendere tali decisioni.
4. Le decisioni vengono prese in sequenza, dopo aver osservato quello che hanno fatto gli altri in precedenza.
5. Ogni individuo prende la propria decisione in maniera **puramente razionale**: inferisce dalle proprie osservazioni quale è la strategia migliore da adottare, senza alcuna influenza o pressione sociale.
6. Si scatena una **cascata informativa** (**herding**), quando una certa **massa critica** prende una medesima decisione.

Si descrive in maniera formale un modello che cattura tutte queste caratteristiche.
#### Punto 1: decisione individuale
Per semplicità si assuma che le decisioni da prendere sono **binarie**, dove le due uniche alternative sono:
- $Y$: individuo **accetta** una proposta;
- $N$: individuo **rifiuta** una proposta;
Solo una una delle due alternativa è quella *giusta*. Sia $\mathbf{Pr}(Y) = p$ la probabilità che accettare la proposta sia la scelta giusta, e dunque sia $\mathbf{Pr}(N) = 1-p$ la probabilità che rifiutare la proposta sia la scelta giusta.
Se un individuo accetta una proposta può avere un profitto o una perdita: se accettare è la scelta giusta, allora ottiene un profitto $v_g > 0$; se accettare **non** è la scelta giusta, allora ottiene una perdita $v_b < 0$. Se un individuo rifiuta una proposta, allora non ottiene alcun profitto o perdita.
Affinché sia equivalente per un individuo accettare o non accettare la proposta in assenza di informazioni che permettano di guadagnare evidenza in favore di una delle due alternative deve essere:
$$
v_{g}p + v_{b}(1-p) = 0
$$
cioè il valore atteso del profitto nel caso di accettazione o rifiuto è lo stesso.
#### Punto 2: informazioni private
Ogni individuo mantiene un'**informazione privata**, ricevuta sotto forma di **segnale privato**. Il segnale può avere valore $A$ (accetta) o $R$ (rifiuta).
Se la scelta giusta è $Y$, allora la probabilità di ricevere come segnale privato $A$ è pari a $q > \frac{1}{2}$. Viceversa se la scelta giusta è $N$, allora la probabilità di ricevere come segnale privato $A$ è pari a $1 - q$. Simmetricamente, se la scelta giusta è $Y$ la probabilità di ricevere $R$ è $1-q$, se la scelta giusta è $N$ la probabilità di ricevere $R$ è $q$. Formalmente
$$
\begin{align}
\mathbf{Pr}(A|Y) = q; \ &\mathbf{Pr}(A|N) = 1-q  \\
\mathbf{Pr}(R|Y) = 1-q; \ &\mathbf{Pr}(R|N) = q  \\
\end{align}
$$
#### Punto 3: osservazioni
L'individuo $i$-esimo che deve prendere una decisione ricava dalla rete la sequenza $(X_1,...,X_{i−1})\in\{Y,N\}^{i−1}$ delle decisioni prese dai precedenti $i−1$ individui.
#### Punto 4: decisioni sequenziali
L'individuo $i$-esimo prende una decisione dopo che i precedenti $i-1$ individui hanno preso la loro.
#### Punto 5: decisioni razionali
Supponendo che per l'individuo $i$, inizialmente, $Y$ e $N$ sono alternative equivalenti, ovvero vale $v_{g}p+v_{b}(1-p) = 0$. Se dopo le decisioni dei primi $i-1$ individui la probabilità che $Y$ sia corretta diventa una certa quantità $p'$, all'individuo $i$ converrà accettare se e soltanto se
$$
v_{g}p' + v_{b}(1-p') \geqslant  0
$$
e questo accade se $p'\geqslant p$, ovvero se non peggiora la probabilità che accettare sia la strategia corretta.
#### Punto 6: massa critica
Si studia ora quanto deve essere grande una massa critica per scatenare l'*herding*.
___
Si indica con $S \in \{A,R\}^*$ la sequenza dei *segnali privati* ricevuti dagli individui. Si analizza come varia la probabilità che $Y$ sia la scelta migliore rispetto alla sequenza $S$.
Sia $|S|=1$, cioè viene ricevuto un solo segnale. Se questo segnale è $A$, allora la probabilità che $Y$ sia la scelta corretta è
$$
\begin{align}
\mathbf{Pr}(Y|A) &= \frac{\mathbf{Pr}(A|Y)\mathbf{Pr}(Y)}{\mathbf{Pr}(A|Y) \mathbf{Pr}(Y) + \mathbf{Pr}(A|N) \mathbf{Pr}(N) }  \\
&= \frac{qp}{qp + (1-q)(1-p)} \\
\left( q > \frac{1}{2} \right)&>\frac{qp}{qp + q(1-p)} = \frac{qp}{qp + q-qp}=p
\end{align}
$$
viceversa la probabilità che la scelta corretta sia $N$ ricevendo il segnale $A$ è
$$
\mathbf{Pr}(N|A)=1-\mathbf{Pr}(Y|A) < p  
$$
Simmetricamente, se il primo segnale privato è $R$ allora
$$
\begin{align}
\mathbf{Pr}(Y|R) &< p \\ 
\mathbf{Pr}(N|R) &> p 
\end{align}
$$
Segue che un individuo, in assenza di ulteriori informazioni rispetto alla propria informazione privata, ha convenienza nel rispondere in accordo ad essa: se riceve $A$ gli conviene rispondere $Y$, se riceve $R$ gli conviene rispondere $N$.

Sia $|S|>1$. Si supponga che, in qualche modo, l'individuo che deve fare la scelta sia riuscito ad inferire tutta la sequenza $S$. Sia in $S$ presente $a$ volte l'elemento $A$ e $r$ volte l'elemento $R$. Allora la probabilità che $Y$ sia la risposta corretta conoscendo la sequenza $S$ dei segnali privati è pari a
$$
\begin{align}
\mathbf{Pr}(Y|S) &= \frac{\mathbf{Pr}(S|Y)\mathbf{Pr}(Y)}{\mathbf{Pr}(S|Y) \mathbf{Pr}(Y) + \mathbf{Pr}(S|N) \mathbf{Pr}(N) }  \\
&= \frac{q^a(1-q)^rp}{q^a(1-q)^rp + (1-q)^aq^r(1-p)}
\end{align}
$$
Si consideri il <u>secondo addendo al denominatore</u>. Se $a>r$, poiché $q> \frac{1}{2}$, allora
$$
(1 − q)^a q^r = (1 − q)^{a−r} (1 − q)^r q^r < q^{a−r} (1 − q)^r q^r = q^a (1 − q)^r
$$
quindi
$$
\begin{align}
\mathbf{Pr}(Y|S) &= \frac{q^a(1-q)^rp}{q^a(1-q)^rp + (1-q)^aq^r(1-p)} \\
&> \frac{q^a(1-q)^rp}{q^a(1-q)^rp + q^a (1 − q)^r(1-p)}=p
\end{align}
$$
Viceversa, se $a<r$, allora
$$
(1 − q)^a q^r = (1 − q)^{a} q^a q^{r-a} > (1-q)^{a}  q^a (1-q)^{r-a} = q^a (1 − q)^r
$$
e quindi
$$
\begin{align}
\mathbf{Pr}(Y|S) &= \frac{q^a(1-q)^rp}{q^a(1-q)^rp + (1-q)^aq^r(1-p)} \\
&< \frac{q^a(1-q)^rp}{q^a(1-q)^rp + q^a (1 − q)^r(1-p)}=p
\end{align}
$$
Infine, se $a=r$ allora $q^a(1-q)^r = (1-q)^aq^r$ e quindi
$$
\mathbf{Pr}(Y|S) = p
$$
In conclusione
$$
\mathbf{Pr}(Y|S) = \begin{cases}
>p \ &\text{se } a>r \\
<p \ &\text{se } a<r \\
=p \ &\text{se } a=r
\end{cases} 
$$
Perciò se un individuo conosce l'intera sequenza di segnali privati dei giocatori precedenti, riesce a prendere la scelta che massimizza la probabilità di vincita in base alla maggioranza: se ci sono stati più segnali $A$ conviene votare $Y$, se ce ne sono stati più $R$ conviene votare $N$. Si osserva che il caso in cui $a=r$ equivale sostanzialmente al caso in cui un'individuo non ha alcuna informazione aggiuntiva riguardo la decisione da prendere, e quindi deve rifarsi unicamente al suo segnale privato.

Senza perdita di generalità si supponga che l'$n$-esimo individuo riesca ad inferire tutta la sequenza $S$ degli $n-1$ segnali privati precedenti, e che $a=r$. Sia $\sigma_{n} \in \{ A,R \}$ il segnale privato che riceve l'$n$-esimo individuo. Dato che $a=r$ per ipotesi, vale
$$
\begin{align}
\sigma_{n} = A \implies \mathbf{Pr}(Y|S \cup \{ \sigma_{n} \}) = \mathbf{Pr}(Y|\sigma_{n})  > p \\
\sigma_{n} = R \implies \mathbf{Pr}(Y|S \cup \{ \sigma_{n} \}) = \mathbf{Pr}(Y|\sigma_{n})  < p 
\end{align}
$$
perciò l'individuo segue il suo segnale privato, qualunque esso sia.
Se invece $a=r+1$, vale che
$$
\begin{align}
\sigma_{n} = A \implies \mathbf{Pr}(Y|S \cup \{ \sigma_{n} \}) > p \\
\sigma_{n} = R \implies \mathbf{Pr}(Y|S \cup \{ \sigma_{n} \}) = p 
\end{align}
$$
e anche in questo caso l'individuo $n$ segue il suo segnale privato.
Invece se $a \geqslant r+2$, allora all'$n$-esimo individuo conviene sempre scegliere $Y$ qualunque sia il suo segnale, scatenando così una cascata imitativa. Questo fatto vale simmetricamente per $N$ quando $r \geqslant a+2$.

Individuata la condizione per la quale scatta la cascata imitativa, si individua la situazione in cui si verifica $a \geqslant r+2$ (o $r\geqslant a+2$).
Fin quando i due segnali $A$ e $R$ si alternano non può scattare la cascata, infatti $|a-r| \leqslant 1$. Quando si hanno due segnali consecutivi, allora può scattare la cascata, ma non sicuramente. Infatti in una sequenza del tipo $ARAR \dots ARAA$, due $A$ consecutive bastano per scatenare la cascata imitativa per $Y$, ovvero bastano per avere $a \geqslant r+2;$ ma nella stessa sequenza, due $R$ consecutive non sono sufficienti a far scattare una cascata imitativa di $N$. Infatti nella sequenza $ARAR \dots ARR$ vale che $a-r=1$.
Perciò per scatenare con certezza una cascata imitativa sono necessari **almeno 3 segnali identici consecutivi**.

Si calcola ora la probabilità che una cascata si scateni entro il passo $n$. Sia $\mathcal{H}_{n}$ l'evento
$$
\mathcal{H}_{n} = \text{la cascata imitativa si innesca entro il passo }n 
$$
Sia $\sigma_i \in \{A,R\}$ il segnale privato dell'$i$-esimo individuo, e siano $\mathcal{A}$ e $\mathcal{B}$ gli eventi
$$
\begin{align}
\mathcal{A} &= (\sigma_{1}=\sigma_{2}=\sigma_{3}) \lor (\sigma_{2}=\sigma_{3}=\sigma_{4}) \lor \dots \lor (\sigma_{n-3}=\sigma_{n-2}=\sigma_{n-1})  \\
&\qquad \lor (\sigma_{n-2}=\sigma_{n-1}=\sigma_{n}) \\
&= \bigvee_{1 \leqslant i \leqslant n-2} (\sigma_{i} = \sigma_{i+1} = \sigma_{i+2})
\end{align}
$$
$$
\begin{align}
\mathcal{B} &= (\sigma_{1}=\sigma_{2}=\sigma_{3}) \lor (\sigma_{4}=\sigma_{5}=\sigma_{6}) \lor \dots \lor (\sigma_{n-5}=\sigma_{n-4}=\sigma_{n-3})  \\
&\qquad\lor (\sigma_{n-3}=\sigma_{n-2}=\sigma_{n}) \\
&= \bigvee_{1 \leqslant i \leqslant \frac{n}{3}} (\sigma_{3i-2} = \sigma_{3i-1} = \sigma_{3i})
\end{align}
$$
Osserviamo che $\mathcal{B} \subseteq \mathcal{A}$, perciò $\mathbf{Pr}(\mathbf{B}) \leq \mathbf{Pr}(\mathcal{A})$.
Allora vale che
$$
\mathbf{Pr}(\mathcal{H}_{n}) \geqslant \mathbf{Pr}(\mathcal{A})   \geqslant \mathbf{Pr}(\mathcal{B}) 
$$
Per la *legge di De Morgan*, vale che
$$
\mathbf{Pr}(\lnot\mathcal{H}_{n}) \leqslant \mathbf{Pr}(\lnot \mathcal{B}) = \mathbf{Pr}\left( \bigwedge_{1 \leqslant i \leqslant \frac{n}{3}} \lnot(\sigma_{3i-2} = \sigma_{3i-1} = \sigma_{3i}) \right)  
$$
Dato che gli eventi in $\mathcal{B}$ sono tutti indipendenti tra di loro, vale che
$$
\mathbf{Pr}(\lnot \mathcal{H}_{n}) \leqslant \prod_{1 \leqslant i \leqslant \frac{n}{3}} \mathbf{Pr}(\lnot (\sigma_{3i-2} = \sigma_{31-1} = \sigma_{3i})) 
$$
Per ogni $i \leqslant n/3$, la probabilità dell'evento $(\sigma_{3i-2} = \sigma_{31-1} = \sigma_{3i})$ è pari a
$$
\begin{align}
\mathbf{Pr}(\sigma_{3i-2} = \sigma_{31-1} = \sigma_{3i}) &= \mathbf{Pr}( (\sigma_{3i-2} = \sigma_{31-1} = \sigma_{3i}=A) \lor (\sigma_{3i-2} = \sigma_{31-1} = \sigma_{3i} = R))  \\
&= \mathbf{Pr}(\sigma_{3i-2} = \sigma_{31-1} = \sigma_{3i}=A) + \mathbf{Pr}(\sigma_{3i-2} = \sigma_{31-1} = \sigma_{3i}=R) \\
&= \mathbf{Pr}(AAA) + \mathbf{Pr}(BBB) \\
&= [ \mathbf{Pr}(AAA|Y)\mathbf{Pr}(Y)  + \mathbf{Pr}(AAA|N)\mathbf{Pr}(N)  ]  \\
&\quad+ [ \mathbf{Pr}(RRR|Y)\mathbf{Pr}(Y)  + \mathbf{Pr}(RRR|N)\mathbf{Pr}(N)  ] \\
&= [q^3p + (1-q)^3(1-p)] + [(1-q)^3p + q^3(1-p)] \\
&= 1 - 3q + 3q^2
\end{align}
$$
Allora il complemento è pari a
$$
\mathbf{Pr}(\lnot(\sigma_{3i-2} = \sigma_{31-1} = \sigma_{3i})) = 1- \mathbf{Pr}(\sigma_{3i-2} = \sigma_{31-1} = \sigma_{3i}) = 3q-3q^2
$$
Applicando tale uguaglianza per ogni $i$ si ottiene
$$
\mathbf{Pr}(\lnot \mathcal{H}_{n}) \leqslant (3q-3q^2)^{n/3}
$$
Si osserva infine che $3q-3q^2 < 1$ per ogni $q \in [0,1]$
![](Pasted%20image%2020240301164006.png)
Dunque si conclude che la cascata imitativa si innesca **quasi sicuramente** al crescere degli individui
$$
\mathbf{Pr}(\mathcal{H}_{n}) \geqslant 1-(3q-3q^2)^{n/3} \implies \lim_{ n \to \infty } \mathbf{Pr}(\mathcal{H}_{n}) = 1
$$
## Considerazioni finali
Come appena visto, le cascate informative sono fenomeni abbastanza semplici da innescare, in quanto richiedono lo scambio di pochissime informazioni (solo le decisioni prese). In realtà le uniche informazioni realmente rilevanti sono quelle _precedenti_ all'innesco della cascata in quanto, per definizione, una volta iniziata la cascata informativa, tutti gli individui prenderanno la stessa decisione **indipendentemente** dalle informazioni in possesso. Le informazioni che vengono ricevute dopo che la cascata si innesca sono _inutili_.  

Le cascata informative non sono affidabili e possono portare facilmente a prendere decisioni inesatte, proprio perché non vengono usate tutte le informazioni presenti nella rete, ma bensì un piccolissima parte di esse.  

Inoltre le cascate informative sono anche molto instabili: è possibile *"rompere"* una cascata semplicemente rendendo pubblica l'informazione privata di un individuo oppure che qualcuno voti in disaccordo alla cascata, e questo è più probabile che accada tanto più la rete è grande.  

Tuttavia nel libro _[The Wisdom of Crowds](https://www.amazon.it/Wisdom-Crowds-James-Surowiecki/dp/0385721706)_, James Surowiecki sostiene la tesi secondo la quale

> _il comportamento aggregato di un numero elevato di persone in possesso di informazione molto limitata può produrre risultati molto accurati_

Però l'assunzione alla base di questa tesi è che ogni individuo prende la propria decisione solamente sulla base delle proprie informazioni private, **indipendentemente** da ciò che fanno gli altri.