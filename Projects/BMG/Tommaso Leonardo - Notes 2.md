---
type: meeting-notes
date: 2026-05-12
created: 2026-05-12 10:30
project: BMG
attendees:
  - Tommaso Meledina (Tech Lead)
---
## Attendees
- Tommaso Meledina (Tech Lead)

---

## Context
## Autenticazione

QVR - quadrimestre di BMG

Programmata per QVR 2: 
- Roche ha un sistema di autenticazione interno (loro lo chiamano SSO, immagino che lo consenta) rispetta il protocollo OIDC
- Se rispetta il OIDC, si integra l'app frontend, redirect all'applicazione
- Quel token contiene dei claim , e da una volta ottenuto il token, vorrebbe utilizzare il patter di autenticazione OBO
	- A questa chiamata attacchiamo un token autorizzativo, dicendo anche qual è l'utente
	- Supponiamo di voler mandare indietro qualcosa di più complesso:
		- chiama altri servizi, per darmi altri dettagli
		- possiamo farlo in due modi:
			- OBO
			- Client Credentials - 
			- La nostra APP parlando con il provider OICD:
				- da chi arriva la richiesta
				- per cos'è la richiesta
			- condenso il token 
			- uso lo scope per capire se sono l'applicazione giusta
			- è più complicato ma chi gestisce il provider di autenticazione
-

nel QVR lavoreremo di più a requisiti non funzionali:
- Autenticazione
- Solamente un ambiente di sviluppo
	- Ambiente di test
	- Ambiente di produzione
- Monitoraggio 
	- le nostre applicazioni già emettono tracce e metriche secondo il protocollo Open Telemetry
	- Tommaso ha simulato un ambiente con un ambiente open telemetry e graphana
	- Adesso dobbiamo fare le cose come si aspetta il cliente
		- loro hanno le idee abbastanza confuse
		- non hanno un modo standard di fare osservabilità

OIDC Oauth2, OBO / CC

