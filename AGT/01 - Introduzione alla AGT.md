# Nascita della Algorithmic Game Theory
L'**algorithmic game theory (AGT)** nasce dalla combinazione della *teoria dei giochi* con la *teoria degli algoritmi*.

Gli studenti di Informatica conoscono già la **teoria degli algoritmi**: è l'area di ricerca che cerca di rispondere a problemi di natura *computazionale*; in particolare:
- Cosa può essere calcolato? (Teoria della calcolabilità, macchine di Turing ecc)
- Quante risorse sono necessarie per calcolare una soluzione? (Teoria della complessità, NP-Completezza ecc)
- Qual'è la qualità della soluzione calcolata rispetto alla soluzione ottima? (Algoritmi approssimanti ecc)

La **teoria dei giochi** invece è un'area di studio che nasce dopo la pubblicazione del libro "Games and Economic Behavior", co-scritto da John Von Neumann nel 1944: il libro, per la prima volta, analizza le interazioni che avvengono tra *individui egoistici*. Tra le problematiche affrontate nella teoria dei giochi si trovano:
- Qual'è il risultato (**outcome**) dell'interazione tra agenti egoistici?
- Quali *obiettivi sociali* sono compatibili con l'egoismo dei giocatori?

Prese singolarmente le due teorie fanno ciascuna delle particolari assunzioni:
- **Teoria degli algoritmi in sistemi distribuiti**, progettazione di protocolli distribuiti dove:
	- I processori sono *obbedienti* al protocollo, sono *faulty* cioè possono fallire (progettazione di protocolli *tolleranti ai guasti*) e *avversariali*, cioè ci sono alcuni processori che cercano di far fallire il protocollo (processori *bizantini*);
	- I sistemi sono *grandi* e dispongono di risorse computazionali *limitate*: i protocolli e algoritmi progettati devono ottimizzare l'utilizzo di queste risorse;
- **Teoria dei giochi**, studio dell'interazione tra giocatori o agenti dove:
	- I giocatori sono strategici o **egoistici**, ossia ogni giocatore massimizza la sua funzione obiettivo indipendentemente da ciò che fanno gli altri agenti;
	- I sistemi studiati sono *piccoli* e dispongono di risorse *illimitate*.

Con l'avvento di Internet si sono venuti a creare molti sistemi contenti tanti utenti, ciascuno dei quali dispone di risorse limitate che deve utilizzare al fine di soddisfare determinati obiettivi egoistici. Sono quindi nati una serie di problemi in cui è di fondamentale importanza gestire bene sia gli aspetti _strategici_ degli utenti e sia quelli _computazionali_ delle risorse utilizzate. La **Algorithmic Game Theory** studia proprio questa tipologia di problemi.

L'AGT presenta quindi un ponte che connette queste due teorie tra loro. Mentre la **GT** (Game Theory), offre una serie di concetti e strumenti utili per risolvere problemi computazionali in scenari non-cooperativi, la **TA** (Theory of Algorithms) permette di interpretare e dare senso ad alcuni risultati della **GT** classica.
# Elementi di base della Teoria dei Giochi
Informalmente, un **gioco** nello scenario della teoria di giochi consiste in:
- Un insieme di *giocatori* o *agenti* egoistici;
- Un insieme di *strategie* dei giocatori che specifica cosa possono fare i possibili giocatori: chi deve agire quando e quali azioni si possono eseguire;
- Per ogni possibile combinazione di strategie dei giocatori bisogna specificare i **payoff** (guadagni) per ogni giocatori: il payoff è un valore numerico specifico per ogni giocatore che dipende dalle strategie e che indica o un guadagno o un pagamento per ogni giocatore.
La teoria dei giochi classica cerca di fare una previsione sull'**outcome** del gioco, cioè una previsione sul risultato dell'interazione tra i giocatori.
## Esempi di gioco
Per capire meglio il concetto di gioco, si vede ora un paio di esempi classici
### Esempio 1: The Prisoner's Dilemma