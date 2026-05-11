---
type: meeting-notes
date: 2026-05-11
created: 2026-05-11 10:30
project: BMG
attendees:
  - Tommaso Meledina (Tech Lead)
---

# Welcome Call with Tech Lead — 2026-05-11

## Attendees
- Tommaso Meledina (Tech Lead)

---

## Context

### Work chain
**Nearform** → engaged to support **BMG** → BMG consults for **Roche** on the Trial Simulator project (Swiss Pharma)

### Key stakeholders
- **Wayne Vest** (McKinsey) — executive managing the collaboration with Nearform; travels a lot, no fixed timezone

### Situation as of the call date
- Q1 just concluded; internal presentation on the evening of 05/11 to decide whether to move forward
- Current week (May 11–16): no operational work
- Following week (~May 18): Q2 roadmap will be decided

---

## Project: Trial Simulator (Roche) — AINE

**AI Native** project based on the **BMAD** framework. Engagement model: **Staff Augmentation**.

### Problem it solves
Enable Roche decision makers to invest in R&D with **data-driven** decisions, instead of relying on subjective evaluations.

### How it works

A **scenario** represents the simulation of an R&D path for a specific molecule:

```
Input (molecule + parameters) → Finite state machine → Simulation → Results
```

**Output of each scenario:**
- Probability that the R&D cycle yields the expected results
- Estimated time
- Estimated cost
- ROI calculated on the 3 above

**Additional features:**
- Scenario versioning
- Comparison between N scenarios

### Tech stack

| Layer | Technology |
|---|---|
| Frontend | Angular |
| Backend | FastAPI (BFF — Backend for Frontend) |
| AI / Analytics | Separate hemisphere, developed by BMG |

> We handle input and output. The AI hemisphere (developed by BMG) is called via API to run the simulations.

---

## Current status and Q2 focus

- Q1 finished; improvements identified
- **Q2 focus**: **non-functional** improvements, making the product enterprise-grade
- Systematic application of the **BMAD** framework for the AI part

---

## Onboarding and access

### Roche infrastructure (segregated)
- Separate Roche accounts — Tommaso is handling this
- Dedicated professional Chrome profiles
- Access via Roche computer or **VDI (Siteix)** → remote Windows session
- ⚠️ Without a Roche account it's not possible to access project material

### First week
- A **buddy** will contact me to answer onboarding questions

---

## Team and way of working

### Tools
- **GitLab Enterprise**: issue board + agile board integrated with the client

### Work division (initial phase)
- Tommaso continues development while I get up to speed
- I progressively take ownership of development

### Work-life balance
- Typical hours: **9–18**, driven by the day's commitments
- The goal is a happy client with well-calibrated expectations
- Everyone manages how they reach the goal — there's no micromanagement

### Timezone awareness
- Wayne Vest: no fixed timezone (travels)
- McKinsey team: mostly in Europe
- Roche client: most in **America**, a small part in **India**
- We can push back on meetings, but it's important to give availability

---

## Action items

- [ ] Wait for Roche account (Tommaso is handling it)
- [ ] Be contacted by the buddy during the first week
- [ ] Participate in the Q2 roadmap decision (week of May 18)
- [ ] Get familiar with the codebase before taking ownership of development

## Next steps

