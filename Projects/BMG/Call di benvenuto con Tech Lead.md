---
type: meeting-notes
date: 2026-05-11
created: 2026-05-11 10:30
project: BMG
attendees:
  - Tommaso Meledina (Tech Lead)
---

# Call di benvenuto con Tech Lead — 2026-05-11

## Attendees
- Tommaso Meledina (Tech Lead)

---

## Contesto

### Catena di lavoro
**Nearform** → ingaggiata per supportare **BMG** → BMG fa consulenza a **Roche** sul progetto Trial Simulator (Swiss Pharma)

### Stakeholder chiave
- **Wayne Vest** (McKinsey) — dirigente che gestisce la collaborazione con Nearform; viaggia molto, nessun timezone fisso

### Situazione alla data della call
- Q1 appena concluso; sera del 11/05 presentazione interna per decidere se si va avanti
- Settimana corrente (11–16 maggio): nessun lavoro operativo
- Settimana successiva (18 maggio~): si decide la roadmap del Q2

---

## Progetto: Trial Simulator (Roche) — AINE

Progetto **AI Native** basato su framework **BMAD**. Modalità di ingaggio: **Staff Augmentation**.

### Problema che risolve
Consentire ai decision maker di Roche di investire in R&D con decisioni **data-driven**, invece di affidarsi a valutazioni soggettive.

### Come funziona

Un **scenario** rappresenta la simulazione di un percorso R&D per una molecola specifica:

```
Input (molecola + parametri) → Macchina a stati finiti → Simulazione → Risultati
```

**Output di ogni scenario:**
- Probabilità che il ciclo R&D dia i risultati sperati
- Tempo stimato
- Costo stimato
- ROI calcolato sui 3 precedenti

**Feature aggiuntive:**
- Versioning degli scenari
- Confronto tra N scenari

### Stack tecnico

| Layer | Tecnologia |
|---|---|
| Frontend | Angular |
| Backend | FastAPI (BFF — Backend for Frontend) |
| AI / Analytics | Hemisphere separato, sviluppato da BMG |

> Noi gestiamo input e output. L'hemisphere AI (sviluppato da BMG) viene chiamato via API per eseguire le simulazioni.

---

## Stato attuale e focus Q2

- Q1 terminato; emersi miglioramenti da fare
- **Q2 focus**: miglioramenti **non funzionali**, rendere il prodotto enterprise-grade
- Applicazione sistematica del framework **BMAD** per la parte AI

---

## Onboarding e accessi

### Infrastruttura Roche (segregata)
- Account Roche separati — Tommaso se ne sta occupando
- Profili Chrome professionali dedicati
- Accesso tramite computer Roche o **VDI (Siteix)** → sessione Windows remota
- ⚠️ Senza account Roche non è possibile accedere al materiale del progetto

### Prima settimana
- Un **buddy** mi contatterà per rispondere a domande di onboarding

---

## Team e modo di lavorare

### Strumenti
- **GitLab Enterprise**: issue board + board agile integrato col cliente

### Divisione del lavoro (fase iniziale)
- Tommaso continua lo sviluppo mentre mi ambiento
- Progressivamente prendo autonomia sullo sviluppo

### Work-life balance
- Orario tipico: **9–18**, guidato dagli impegni del giorno
- L'obiettivo è il cliente contento con aspettative ben dimensionate
- Ciascuno gestisce come raggiunge l'obiettivo — non c'è microgestione

### Attenzione ai fusi orari
- Wayne Vest: nessun timezone fisso (viaggia)
- Team McKinsey: prevalentemente in Europa
- Cliente Roche: maggior parte in **America**, piccola parte in **India**
- Possiamo fare pushback sulle riunioni, ma è importante dare disponibilità

---

## Action items

- [ ] Aspettare account Roche (Tommaso se ne occupa)
- [ ] Essere contattato dal buddy nella prima settimana
- [ ] Partecipare alla decisione sulla roadmap Q2 (settimana del 18 maggio)
- [ ] Ambientarsi nel codebase prima di prendere autonomia sullo sviluppo

## Next steps

