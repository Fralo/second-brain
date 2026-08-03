________________
note che non c'entrano nula


### ALB Transport security
WI (Harden product ALB transport security: HTTPS redirect and TLS policy)[Harden product ALB transport security: HTTPS redirect and TLS policy]  è a livello di infra, giusto?
### e2e test suite for AuthN
Qua mi si aprono un paio di domande:
1. Per avere dei *veri* test e2e, non dovremmo avere sempre AuthN attivo?
2. Nel caso volessimo separare la parte con Auth attiva e keycloack che gira
	1. facciamo un nuovo progetto playwright solo con la parte di auth che testa login/refresh/accesso con credenziali valide/accesso con credenziali invalide
	2. Aggiungiamo un nuovo job `e2e:flow-run-auth` 





___________________

`~/trial-simulator-apps/docs/CICD.md `

`ENGINEERING_ASSESMENT.md`


### Steps
1. [prima volta] Cancellare release da gitlab e creare branch release da GitLab (use the Clementoni interface)

#### per hotfix
1. banch hotfix
2. merge fi hotfix -> release
3. deploy
4. staccare nuovo branch su dev (ex b1)
5. mergiare release dentro a b1
6. mergiare b1 -> develop

### la pezza
per portare a manella dei changes in AWS
1. chiedere a riccardo
2. a