---
type: meeting-notes
date: 2026-05-10
created: 2026-05-11 10:30
project: BMG
attendees:
  - Tommaso Meledina (Tech Lead)
---

# Call di benvenuto con Tech Lead — 2026-05-11

## Attendees
- Tommaso Meledina (Tech Lead)

## Agenda

- Di cosa si tratta il progetto? Che problema stiamo risolvendo?
- Com'è il progetto oggi — siamo in produzione o ancora in sviluppo? Ci sono scadenze vicine?
- Quali sono, a grandi linee, le parti in movimento? Puoi farmi una panoramica dell'architettura? Ci sono aree critiche o debito tecnico di cui devo essere consapevole?
- Come lavoriamo insieme — c'è un ticketing system, come facciamo PR e review tra noi?
- Come preferisci comunicare e aggiornarti sul mio avanzamento, almeno all'inizio?
- Di cosa ho bisogno subito per iniziare? (accessi, doc, setup locale) E qual è la prima cosa su cui posso mettere le mani?
- Vorresti che approfondissi qualcosa in particolare?


## Notes
AINE - qualcosa -> richiede un po' di tempo

Questa settimana non faremo niente

Stasera presentiamo il progetto e vediamo se andiamo avanti o meno

Wayne Vest (stakeholder)- dirigente mckinsy, gestisce la collaborazione con nearform

Nella prima settimana mi dovrebbe contattare un certo buddy: risponde a domande

Tommaso si muove per farmi account Roche, sono segragati, non c'è modo di lavorare con il loro materiale

Devo aspettare l'account Roche, usano profili professionali di google (profile di chrome), computer roche, VDI -> Siteix, apre una sessione all'interno di un computer windows.

Da quello che capiamo la situaizione è:
Neafrom -> presi per aiutare BMG -> BMG fa consulenza a Roche sul progetto Traial Simulator di Swiss Pharma


## Progetto - Roche - Trial Simulator
Facciamo Staff Augmentation, piattaforma basata su AI e Analytics che consenta ai decision maker di investire i soldi in R&D invece che ai vecchi, con deciisioni data driven.

La nostra app deve consentire di eseguire simulazioni su come andrebbe il percorso di ricerca e sviluppo.

Questi vengono chiamati scenari:
voglio testare questa specifica molecoola, prendiamo degli input , macchina a stati finiti, esegue simulazione -> risultato qual'è la probabilita che questo giro di RD dia i risultati sperati ?


- Quanto probabile (fa quello che deve fare)?
- Quanto ci mettiamo?
- Quanto spendiamo?
- Calcola il ROI in base agli output dei 4
Questi sono i risultati che noi mostriamo

Angular - Backend For Frontend (FAST API) - Tutta la parte ai gira da un altra parte, ingaggiamo l'emisfero analytics e lo chiamiamo per triggherare queste simulazioni e recuperare le simulazioni (l'emisfero analytics viene sviluppato da BMG, noi prendiamo gli input e mostriamo l'output).

Sistema di versioning di uno scenario

Una volta che abbiamo N scneari diversi possiamo confrontarli tra di loro



Dove siamo?
Abbiamo finito di mettere insieme quello che abbiamo visto in call.
Facendolo ci siamo resi conto di una serie di cose che vorremmo fare diversamente.

La prossima settimana decidiamo che fare nel secondo quarter.

AGILE e stabilire le priorità.


# secondo quarter

- molto incentrato sulla parte non funzionale
- rendere enterprise grade quello che abbiamo fatto
## AI
- ho una consapevolezza non completa di quello che è la codebase
- enterprise grade -> BMAD

## Dimensioni (come ci dividiamo il lavoro io e tommaso)

Chi fa cosa ?
- Mentre io continuo ad ambientarmi, gradualmente prendo io la parte dello sviluppo.
Sul come?
- GitLab enterprise
	- Issue board su gitlab
	- mk agile con il cliente completamente intrecciato con il cliente finale

Worklife balance:
 - Lavoriamo nel tempo che siamo pagati per lavorare
 - Tommaso solitamente fa 9-18, quello che guida sono gli impegni che ci sono quel giorno
 - Dobbiamo metterci nelle condizioni del cliente contento:
	 - mettiamoci nelle condizioni in cui le aspettative sono dimensionate correttamente
	 - come nello specifico ognuno di noi ottiene quel risultato, all'azienda non interessa
- Punto di attenzione di questo progetto:
	- Wayne Vest, gira il mondo non ha timezione
	- Parte importante del team mckinsey è in europa
	- La maggiora part del cliente è in America, e una piccola parte è in india
	- Stiamo attenti agli orari, possiamo fare pushback per le riunioni, ma dare disponibilità

## Action items

- [ ] 

## Next steps

