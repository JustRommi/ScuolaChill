Stack di sviluppo:

Infrastruttura Cloud:

Dimensionamento:



### Fondamenti di architettura

- L'**architettura software** complessiva del sistema e la **suddivisione in componenti**, con un diagramma.
- La **separazione delle responsabilità** e l'**architettura a livelli**: cosa fa il presentation/API layer, cosa l'application/business layer, cosa il data access layer — riferito al *tuo* gestionale, non in astratto.
- Le **dipendenze fra i livelli**: chi può conoscere chi, e in quale direzione.
- Come la struttura scelta riduce l'**accoppiamento** e rende il sistema **testabile**.

### Progettazione e realizzazione delle API

- Le **risorse** esposte dall'API secondo i principi **REST** e la **progettazione delle route**.
- L'uso corretto di **GET, POST, PUT, PATCH e DELETE**, con **CRUD completo** sulle entità principali.
- I **codici di stato HTTP** previsti e una strategia **coerente di gestione degli errori** (formato uniforme delle risposte di errore).
- La **validazione degli input** e la **paginazione** delle collezioni.
- La documentazione tramite **OpenAPI/Swagger** e il piano di verifica con **Postman** o strumento equivalente.

Nel PRD è sufficiente il **contratto** delle API principali (route, verbi, payload di esempio, codici di risposta): è il contratto che il frontend e i test useranno, quindi scriverlo prima conviene a te.

### Persistenza e modellazione

- Il **modello relazionale**: entità, **relazioni** e cardinalità, con diagramma ER. Il dominio contiene già casi interessanti: uno studente appartiene a una classe, un docente insegna più materie in più classi, un voto lega studente, verifica e docente.
- La strategia di **accesso ai dati** e l'uso di **query parametrizzate** per prevenire la **SQL injection**.
- Il **ruolo e la strategia di generazione degli identificatori**.
- La distinzione fra **modello del database, modello di dominio e rappresentazione esposta dall'API**: non sono la stessa cosa, e il PRD deve mostrare dove differiscono e perché.
- La **normalizzazione** dei dati e, dove serve, l'**aggregazione di più modelli** in letture denormalizzate — la vista "tutti i voti dello studente per materia" e la dashboard del Direttore sono i candidati naturali.

### Sicurezza e integrazione

- **HTTPS** e la messa in sicurezza del dialogo fra frontend e backend.
- **Autenticazione e autorizzazione**: come si ottiene un **token**, cosa contiene, come viaggia il **profilo utente**.
- I tre **ruoli** e il **controllo delle autorizzazioni**: chi può fare cosa, e — soprattutto — dove viene fatto rispettare il controllo (spoiler: mai solo nel frontend).
- Almeno una **chiamata a un'API esterna dal backend** (per esempio un servizio di invio email per le credenziali, o un servizio di generazione documenti): quale, e come viene gestito il suo fallimento.
- La **gestione delle configurazioni e degli ambienti**: dove vivono connection string e segreti, e come cambiano fra un ambiente e l'altro.

### Qualità architetturale

- L'**organizzazione del codice**: struttura di progetti/moduli/cartelle e sue motivazioni.
- **Design Pattern**: dove vengono applicati e a cosa servono nel tuo sistema.
- La strategia di **testabilità**: cosa verrà testato, come le dipendenze infrastrutturali (database, API esterne) vengono **separate** per poterle sostituire nei test.
- La configurazione **Development/Production** e le differenze fra i due ambienti.
- I **principi di deployment dell'applicazione e del database**: come il sistema arriva sul cloud scelto, e come vengono gestite le modifiche allo schema del database nel tempo.


## Requisiti trasversali (validi qualunque stack tu scelga)

- Tutto il traffico fra frontend e backend viaggia su **HTTPS**.
- Ogni operazione delle user story passa da un'**API autenticata**: il controllo dei ruoli sta **nel backend**; il frontend può al massimo nascondere ciò che non è permesso, mai essere l'unica barriera.
- Le API principali sono **documentate con OpenAPI/Swagger** e coperte da una collezione **Postman** (o equivalente) usata come verifica.
- Gli elenchi (studenti, materiali, voti) sono **paginati**.
- Gli errori hanno un **formato uniforme** e codici HTTP coerenti su tutta l'API.
- L'applicazione gira in almeno due configurazioni, **Development e Production**, senza segreti nel codice sorgente.
- Il sistema è **deployato sull'infrastruttura cloud scelta** e raggiungibile pubblicamente per il collaudo con i ragazzi del primo anno.


## DIMENSIONAMENTO:

| Tipologia                                           | Studenti | Professori / formatori | Altro personale |
| --------------------------------------------------- | -------: | ---------------------: | --------------: |
| **Scuola statale italiana media**                   |     ~947 |     ~119 posti docente |            n.d. |
| **Secondaria di II grado media**                    |   ~1.014 |                   n.d. |            n.d. |
| **SFP Don Bosco San Donà – 2024/25**                |  413–423 |                     21 |              12 |
| **ITS Digital Academy – singola classe**            |   max 25 |                   n.d. |            n.d. |
| **ITS Digital Academy – 2 annualità contemporanee** |  max ~50 |                   n.d. |            n.d. |

|Tipologia per il progetto                            |     1050 |                    120 |               10|

Per il progetto userei **due scenari** distinti, così il dimensionamento resta leggibile:

Scenario	Studenti	Professori	Personale non docente	Utenti totali stimati
Don Bosco – utilizzo realistico	400–600	15–30	10–15	425–645
Target di progetto – con margine di crescita	1.050	120	10	1.180

Il secondo scenario è utile come riferimento tecnico perché ti permette di progettare il sistema per una scuola più grande del Don Bosco attuale, senza sovradimensionarlo in modo assurdo.

Per il Don Bosco, come valori centrali da usare nei calcoli, potremmo fissare:

Voce	Valore di riferimento
Studenti	500
Professori	25
Personale	15
Totale utenti	540

E tenere ~1.200 utenti registrati come requisito massimo iniziale di progetto.

## Scenario	Utenti contemporanei da considerare
Uso normale	30–80
Periodo intenso	80–150
Picco importante	150–250
Stress test	300+

considerarare il gdpr