## Endpoint
Si definisce endpoint un risultato misurabile che possa determinare se il farmaco ha *funzionato*.
Per esempio:
- "Percent change in body weight at Week 80" -> Continuous
- "Pacient achieves >5% wight loss" -> yes/no - Binary
- "Time until disease progress" (time-to-event/survival)
#### Hierarchical vs Co-primary Endpoints

Hierarchichal:
- endpoints are tested in a fixed orderL E1, E2, ... (E2 only if E1 succeedes)
- Each endpoint gets full alpha
- Trial success = E1 passes as minimum, additional are bonus claims
- DDCP can be run for every endpoint

Co-primary:
- All endpoints must pass for trial to succeed


## Alpha
Acceptable false positive rate (percentuale di falsi positivi accettabili)


## DDCP - Drug Development Confidence Profile
- Core PTS simulation output
- È una distribuzione di probabilità
- Risponde alla domanda: Date queste ipotesi, quel'è la possibilità che il trial abbia successo?