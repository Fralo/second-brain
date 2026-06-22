
### Domane sulla struttura attuale
- `secondary_endpoint_description` sembra venir popolato con `endpoint_type` invece che con `endpoint_description` (nel secondary endpoint si vede `"Continuous"` e non `"Change from baseline in IWQoL-Lite-CT physical function composite score at Week 80"`)

### Domande sulla struttura futura con endpoint multipli
- Come vogliamo memorizzare le informazioni per i vari endpoint dello step clinical success??
- Per ogni endpoint dobbiamo memorizzare tutte le informazioni che sono presenti all'interno della tab dell'endpoint nel design?
	- Nello step Configure Statistical Approach, possiamo aggiungere N endpoint
	- Per ogni endpoint dovremo salvare singolarmente i contenuti delle card`Endpoint structure`, `Win Conditions`,`Alpha & Power` , `Calculation Parameters`  ?
- I risultati dei vari endpoint finiranno in un unico punto dentro a PTS_RESULTS?
- Every endpoint will have a dedicated prefect flow? Or the same flow is gonna calculate every endpoint we pass as the input?
- We might need some work to provide better alpha management, if we use a specific alpha value for every endpoint, we should show to the user the combined ALPHA ? Or it will be an output from the module?


