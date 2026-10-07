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


## Informazioni sul documento ##
Prodotto: ScuolaChill
Team: _Romanov Industries_
Autori: _Romano Cappelletto_
Versione: 1.1.2
Data: 23.09.2026
Stato: Bozza


## Storico delle versioni ##
1.0 | 23.09.2026 | Romano | Prima stesura


## Scopo e perimetro ##
- _Perchè esiste?_

| Lato business: 
    ScuolaChill è un gestionale scolastico pensato per semplificare le attività quotidiane di direzione, docenti e studenti. 
    Riunisce in un’unica piattaforma la gestione degli utenti e delle classi, la distribuzione del materiale didattico, lo svolgimento delle verifiche e la consultazione dei voti

| Lato tecnico: 
    Il sistema comprende un’applicazione web accessibile da computer e smartphone, attraverso la quale gli utenti autenticati possono utilizzare funzionalità differenti in base al proprio ruolo. 
    Il sistema gestisce utenti, classi, materiali didattici, verifiche e voti, garantendo sicurezza, semplicità d’uso e affidabilità
    L’interfaccia viene progettata per essere semplice e comprensibile anche per utenti con competenze digitali limitate.

- _Cosa è incluso?_ 
    Creazione e gestione degli account di docenti e studenti da parte del Direttore.
    Creazione delle classi e assegnazione degli studenti alle classi.
    Consultazione complessiva dei dati scolastici da parte del Direttore.
    Caricamento e consultazione del materiale didattico.
    Creazione e gestione delle verifiche.
    Svolgimento online delle verifiche da parte degli studenti.
    Assegnazione dei voti da parte dei docenti.
    Consultazione dei propri voti da parte degli studenti.
    Gestione dell’autenticazione e delle autorizzazioni in base al ruolo.
    Interfaccia web utilizzabile da computer e smartphone.

- _Cosa non è incluso?_
    Gestione delle assenze, dei ritardi e delle giustificazioni. (da vedere in corso d'opera)
    Comunicazioni con le famiglie.
    Pagelle e documenti scolastici ufficiali.
    Gestione di pagamenti, tasse o rette scolastiche.
    Gestione dell’orario delle lezioni. (da vedere in corso d'opera)
    Chatbot di assistenza. (da vedere in corso d'opera)
    Analisi automatica dell’efficacia delle lezioni e delle verifiche.
    Applicazioni native per Android e iOS.


## Stakeholder ##
| Stakeholder          | Cosa fa                            | Cosa gli interessa                  | Come lo coinvolgo                         |
|----------------------|------------------------------------|-------------------------------------|-------------------------------------------|
|  _Direttore_         | Crea utenti per docenti e studenti | Avere dati corretti e aggiornati,   | Intervista iniziale                       |
|                      | Crea classi                        | controllare l’organizzazione        | Revisione dei requisiti                   |
|                      | Compone classi                     | scolastica e ridurre il lavoro      | Collaudo                                  |
|                      | Vede tutto                         | manuale                             |                                           |
|----------------------|------------------------------------|-------------------------------------|-------------------------------------------|
|  _Docenti_           | Carica materiale didattico         | Utilizzare rapidamente le funzioni  | Interviste                                |
|                      | Crea le proprie verifiche          | didattiche e non perdere dati       | Prove delle funzionalità                  |
|                      | Assegna i voti                     | durante il lavoro                   | Raccolta di feedback                      |
|----------------------|------------------------------------|-------------------------------------|-------------------------------------------|
|  _Studenti_          | Consultare il materiale scolastico | Trovare facilmente i contenuti,     | Interviste                                |
|                      | Svolgere le verifiche              | usare il sistema anche da tel.,     | Collaudo con studenti del primo anno      |
|                      | Consultare i voti                  | completare le verifiche e poter     |                                           |
|----------------------|------------------------------------|-------------------------------------|-------------------------------------------|
|  _Docente del corso_ | Validare il PRD e il progetto      |                                     | Raccolta di feedback durante lo sviluppo  |
|                      |                                    |                                     | Revisione finale alla presentazione       |


## Destinatari e contesto d'uso ##
Numero di studenti: 500
Numero di docenti: 25
Numero di classi: 25
Orario scolastico: 8:00-13:00 (lunedì-venerdì)
Connettività: Wi-Fi scolastico condiviso, rete mobile degli studenti


## Scenario	Utenti contemporanei da considerare
Bassa attività:   100-150
Uso normale:	  150-250
Periodo intenso:  250-350
Picco importante: 350–450
Stress test	300+: 500+


## Archetipi ##
|ID            | Archetipo   | Contesto d'uso  | Competenze digitali  | Dispositivo principale  | Frequenza d'uso          |
|--------------|-------------|-----------------|----------------------|-------------------------|--------------------------|
|  _ARC-001_   | Direttore | Usa il registro per consultare, creare, modificare o eliminare dati e informazioni relativi a classi, docenti e studenti | Competenze base nell'utilizzo di un gestionale, competenze avanzate nella comprensione dei dati e informazioni                 | Computer scolastico     | Quotidianamente durante l'orario scolastico |
|  _ARC-002_   | Docente | Usa il registro per monitorare l'andamento degli studenti, gestire le verifiche e i vari momenti relativi alle lezioni    | Competenze base nell'utilizzo di un gestionale,  | Computer scolastico     | Più volte durante la giornata scolastica    |
|              |    |     | competenze avanzate nella comprensione dei dati e informazioni, |                         |       |
|              |    |                                                                | competenze base nella creazione e gestione di verifiche         |                         |                          |
|  _ARC-003_   | Studente    | Usa il registro per monitorare il proprio andamento, consultare i dati relativi a orari scolastici, visualizzare e fare le verifiche          | Competenze base nell'utilizzo di un gestionale,                 | Computer scolastico,    | Quotidiana, durante le lezioni e da casa  |
|              |             |     | competenze base nella comprensione dei dati e informazioni      | Telefono personale      |         |


## Panoramica e casi d'uso ##
_ScuolaChill in poche righe_

ScuolaChill è una piattaforma scolastica che raccoglie in un unico ambiente le attività principali di direzione, docenti e studenti. Il Direttore può creare gli account, organizzare le classi 
e consultare i dati dell’istituto. I docenti possono distribuire materiale didattico, preparare verifiche e assegnare voti. Gli studenti possono consultare i materiali, svolgere le verifiche e 
controllare i propri risultati. Ogni utente visualizza solamente le informazioni e le funzionalità consentite dal proprio ruolo. 
L’interfaccia è progettata per essere semplice da utilizzare sia da computer sia da smartphone.

_User flow e scenari_

1. _DIR-03 · Creare e comporre una classe_ 
  - User flow
    1. Il Direttore accede a ScuolaChill.
    2. Apre la sezione dedicata alle classi.
    3. Seleziona la funzione per creare una nuova classe.
    4. Inserisce il nome della classe e l’anno scolastico.
    5. Seleziona gli studenti da inserire nella classe.
    6. Associa alla classe i docenti e le rispettive materie.
    7. Controlla i dati inseriti.
    8. Conferma la creazione.
    9. Il sistema salva la classe e mostra un messaggio di conferma.
  - Scenario principale. 
    Prima dell’inizio dell’anno scolastico, il Direttore deve creare la classe 1A. Inserisce il nome e l’anno scolastico, seleziona gli studenti iscritti e associa i docenti alle rispettive materie. Dopo aver controllato i dati, conferma l’operazione. Il sistema crea la classe e la rende visibile agli utenti interessati.
  - Scenari alternativi.
    | Se esiste già una classe con lo stesso nome nello stesso anno scolastico, il sistema impedisce la creazione e segnala il problema.
    | Se uno studente appartiene già a un’altra classe, il sistema chiede al Direttore se desidera trasferirlo.
    | Se mancano dati obbligatori, il sistema indica i campi da completare.
    | Se il salvataggio non riesce, nessuna modifica parziale viene applicata e il Direttore può riprovare.

2. _DOC-02 · Creare e pubblicare una verifica_ 
  - User flow
    1. Il docente accede a ScuolaChill.
    2. Apre la sezione dedicata alle verifiche.
    3. Seleziona la funzione per creare una nuova verifica.
    4. Inserisce il titolo, le istruzioni e le domande.
    5. Seleziona la classe destinataria.
    6. Imposta la data di apertura e la scadenza.
    7. Salva la verifica come bozza.
    8. Controlla il contenuto e pubblica la verifica.
    9. Il sistema rende la verifica disponibile agli studenti nel periodo stabilito.
  - Scenario principale. 
    Il docente deve preparare una verifica per la propria classe. Inserisce le domande, seleziona la classe destinataria e stabilisce il periodo nel quale la verifica potrà essere svolta. Dopo aver controllato il contenuto, pubblica la verifica. Gli studenti potranno visualizzarla e svolgerla dalla data di apertura fino alla scadenza.
  - Scenari alternativi.
    | Se mancano il titolo, le domande o la classe destinataria, il sistema impedisce la pubblicazione e indica i dati mancanti.
    | Se la scadenza è precedente alla data di apertura, il sistema richiede di correggere le date.
    | Se il docente non insegna nella classe selezionata, il sistema non consente di assegnarle la verifica.
    | Se almeno uno studente ha già iniziato la verifica, il docente non può modificare le domande o il punteggio, ma può correggere solamente le informazioni che non alterano lo svolgimento.

3. _STU-02 · Svolgere e consegnare una verifica_ 
  - User flow
    1. Lo studente accede a ScuolaChill.
    2. Apre la sezione dedicata alle verifiche.
    3. Seleziona una verifica disponibile.
    4. Legge le istruzioni e avvia il tentativo.
    5. Compila le risposte.
    6. Controlla le risposte inserite.
    7. Seleziona la funzione per consegnare.
    8. Conferma la consegna.
    9. Il sistema registra la verifica e mostra una conferma allo studente.
  - Scenario principale. 
  Durante una lezione, lo studente apre la verifica assegnata dal docente, legge le istruzioni e risponde alle domande. Dopo aver controllato le risposte, conferma la consegna entro il tempo disponibile. Il sistema registra la verifica e mostra data e ora dell’avvenuta consegna.
  - Scenari alternativi.
    | Se la connessione si interrompe, le risposte già salvate vengono conservate e lo studente può riprendere la verifica quando la connessione torna disponibile.
    | Se la scadenza viene raggiunta durante lo svolgimento, il sistema impedisce ulteriori modifiche e informa lo studente che il tempo disponibile è terminato.
    | Se lo studente prova ad aprire la verifica prima della data prevista o dopo la scadenza, il sistema non consente di iniziarla.
    | Dopo la consegna, lo studente non può modificare le risposte né iniziare un secondo tentativo.


## Le user story della traccia ## 
| ID     | Storia                  | AC aggiunti dal team                                                                     | Note                                       |
|--------|-------------------------|------------------------------------------------------------------------------------------|--------------------------------------------|
|DIR-01  | Creare account docente  | Dato che il Direttore sta creando un docente, quando inserisce un’email già registrata,  | Nome, cognome ed email sono obbligatori,   |
|        |                         | allora il sistema impedisce la creazione del duplicato.                                  | L’email deve essere univoca                | 
|        |                         | Dato che l’account è stato creato, quando l’invio delle credenziali fallisce, allora     |                                            |
|        |                         | il Direttore può ripetere l’invio senza ricreare l’account.                              |                                            |
| - - - -| - - - - - - - - - - - - | - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -| - - - - - - - - - - - - - - - - - - - - - -|
|DIR-02  | Creare account studente | Dato che il Direttore sta creando uno studente, quando inserisce un’email già registrata,| L’assegnazione alla classe può essere      |
|        |                         | allora il sistema impedisce la creazione del duplicato.                                  | completata in un secondo momento           |
|        |                         | Dato che la classe non è ancora stata stabilita, quando il Direttore crea l’account,     |                                            |
|        |                         | allora può lasciare temporaneamente lo studente senza classe.                            |                                            |
| - - - -| - - - - - - - - - - - - | - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -| - - - - - - - - - - - - - - - - - - - - - -|
|DIR-03  | Creare classi e comporle| Dato che esiste già una classe con lo stesso nome nello stesso anno scolastico, quando il| Ogni studente può appartenere a una sola   |
|        |                         | Direttore prova a crearne un’altra, allora il sistema blocca l’operazione.               | classe nello stesso anno scolastico        | 
|        |                         | Dato che uno studente appartiene già a una classe, quando viene inserito in una nuova    |                                            |
|        |                         | classe, allora il sistema richiede la conferma del trasferimento.                        |                                            |
| - - - -| - - - - - - - - - - - - | - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -| - - - - - - - - - - - - - - - - - - - - - -|
|DIR-04  | Vedere tutto            | Dato che il Direttore ha effettuato l’accesso, quando consulta la dashboard, allora può  | Il Direttore può consultare tutti i dati,  |
|        |                         | visualizzare classi, docenti, studenti, verifiche e voti.                                | ma le informazioni sensibili devono essere |
|        |                         | Dato che sono presenti molti risultati, quando apre un elenco, allora può filtrarlo      | mostrate solo quando necessarie            |
|        |                         | e visualizzarlo in pagine.                                                               |                                            |
|--------|-------------------------|------------------------------------------------------------------------------------------|--------------------------------------------|
|DOC-01  | Caricare materiale      | Dato che il docente insegna in una classe, quando carica un materiale valido, allora     | Il docente può gestire materiali solamente |
|        | didattico               | questo diventa visibile agli studenti della classe.                                      | per le proprie classi e materie.           | 
|        |                         | Dato che il file supera la dimensione massima o ha un formato non consentito, quando     |                                            |
|        |                         | il docente tenta il caricamento, allora il sistema rifiuta il file e mostra il motivo.   |                                            |
| - - - -| - - - - - - - - - - - - | - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -| - - - - - - - - - - - - - - - - - - - - - -|
|DOC-02  | Creare le proprie       | Dato che il docente sta preparando una verifica, quando mancano titolo, domande o classe | La data di scadenza deve essere successiva |
|        | verifiche               | destinataria, allora il sistema permette di salvarla come bozza ma non di pubblicarla.   | alla data di apertura.                     |
|        |                         | Dato che almeno uno studente ha iniziato la verifica, quando il docente tenta di         |                                            |
|        |                         | modificare domande o punteggi, allora il sistema impedisce la modifica.                  |                                            |
| - - - -| - - - - - - - - - - - - | - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -| - - - - - - - - - - - - - - - - - - - - - -|
|DOC-03  | Assegnare i voti        | Dato che il docente sta correggendo una verifica, quando inserisce un voto non valido,   | Il voto segue la scala definita nei        |
|        |                         | allora il sistema impedisce il salvataggio.                                              | requisiti funzionali,                      | 
|        |                         | Dato che un voto è già stato pubblicato, quando il docente lo modifica, allora il        | Un voto non pubblicato non è visibile allo |
|        |                         | sistema registra la nuova valutazione e la data della modifica.                          | studente                                   |
|--------|-------------------------|------------------------------------------------------------------------------------------|--------------------------------------------|
|STU-01  | Consultare il materiale | Dato che lo studente appartiene a una classe, quando apre la sezione dei materiali,      | I materiali possono essere filtrati per    |
|        | didattico               | allora visualizza solamente quelli destinati alla propria classe.                        | materia e ordinati per data di             | 
|        |                         | Dato che un materiale non è più disponibile, quando lo studente tenta di aprirlo,        | pubblicazione                              |
|        |                         | allora il sistema mostra un messaggio comprensibile.                                     |                                            |
| - - - -| - - - - - - - - - - - - | - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -| - - - - - - - - - - - - - - - - - - - - - -|
|STU-02  | Svolgere una verifica   | Dato che lo studente sta svolgendo una verifica, quando la connessione si interrompe,    | È consentito un solo tentativo,            |
|        |                         | allora le risposte già salvate vengono conservate.                                       | Durante lo svolgimento le risposte vengono |
|        |                         | Dato che viene raggiunta la scadenza, quando la verifica è ancora aperta, allora il      | salvate automaticamente                    |
|        |                         | sistema impedisce allo studente di inserire o modificare altre risposte.                 |                                            |
|        |                         | Dato che la verifica è già stata consegnata, quando lo studente prova a riaprirla,       |                                            |
|        |                         | allora non può modificare le risposte.                                                   |                                            |
| - - - -| - - - - - - - - - - - - | - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -| - - - - - - - - - - - - - - - - - - - - - -|
|STU-03  | Consultare i propri voti| Dato che il docente ha pubblicato un voto, quando lo studente apre la sezione delle      | Lo studente può consultare esclusivamente i|
|        |                         | valutazioni, allora può visualizzare voto, materia, verifica, data ed eventuale commento.| propri voti,                               | 
|        |                         | Dato che un voto non è ancora stato pubblicato, quando lo studente consulta le           | I risultati possono essere filtrati per    |
|        |                         | valutazioni, allora il voto non viene mostrato.                                          | materia.                                   |
|--------|-------------------------|------------------------------------------------------------------------------------------|--------------------------------------------|


## Le decisioni lasciate aperte dalla traccia ##

_FR-VOT-01 - scala dei voti_ (collegato a DOC-03 e STU-03)

I voti sono numerici e vanno da 1 a 10, estremi compresi. 
Sono ammessi voti interi e mezzi voti, per esempio 6, 6,5 e 7. Non sono ammessi simboli come “+” e “−”. 
Il sistema impedisce il salvataggio di valori non validi.
Motivazione: una scala semplice e uniforme facilita l’inserimento e la consultazione dei voti.

_FR-CLA-01 - Trasferimento di uno studente_ (collegato a DIR-03)

Il Direttore può trasferire uno studente da una classe a un’altra. 
Lo studente viene rimosso dalla classe precedente e assegnato a quella nuova. 
I voti già ricevuti vengono conservati e rimangono consultabili dallo studente e dal Direttore.
Motivazione: il trasferimento deve essere possibile senza cancellare i risultati già ottenuti.

_FR-VER-01 - Modifica di una verifica_ (collegato a DOC-02)
Il docente può modificare liberamente una verifica finché nessuno studente l’ha iniziata. 
Dopo l’inizio della prima compilazione, domande, risposte e punteggi non possono più essere modificati.
Motivazione: tutti gli studenti devono svolgere la stessa verifica nelle stesse condizioni.

_FR-VER-02 - Perdita della connessione durante una verifica_ (collegato a STU-02)
Le risposte vengono salvate durante lo svolgimento della verifica. 
Se la connessione si interrompe, le risposte già salvate vengono conservate. 
Lo studente deve ristabilire la connessione e riaprire la verifica prima della scadenza per continuare. Le risposte non ancora salvate potrebbero dover essere inserite nuovamente.
Motivazione: questa soluzione limita la perdita di dati senza richiedere una modalità offline completa.

_FR-EMAIL-01 - Fallimento dell’invio delle credenziali_ (collegato a DIR-01 e DIR-02)
Se il servizio email non risponde, l’account viene comunque creato. 
Il sistema informa il Direttore che l’invio non è riuscito e mette a disposizione un comando per riprovare manualmente.
Motivazione: un problema del servizio email non deve obbligare il Direttore a creare nuovamente l’account.

_FR-VER-03 - Numero di tentativi_ (collegato a STU-02)
Ogni studente può effettuare un solo tentativo per ciascuna verifica. 
Dopo la consegna, le risposte non possono più essere modificate.
Motivazione: la regola è semplice da comprendere e garantisce le stesse condizioni a tutti gli studenti.


## Requisiti non funzionali ##
| ID       | Famiglia     | Requisito                             | Soglia e condizione                                                     | Come si verifica             | Storie collegate |
| _NFR-01_ | Prestazioni  | Apertura della verifica nel picco     | Meno di 2s per il 95% delle richieste, 75 utenti nello stesso minuto    | Test di carico               | STU-02           |
| _NFR-02_ | Sicurezza    | Protezione degli accessi              | il 100% delle richieste fatte senza aver fatto il login viene bloccato  | Test del login               | DIR-01, DOC-03   |
| _NFR-03_ | Usabilità    | Facilità di consultazione dei voti    | Almeno 4 studenti su 5 trovano i propri voti entro 30s, senza aiuti     | Test con utenti              | STU-03           |
| _NFR-04_ | Disponibilità| Accessibilità durante le lezioni      | Dispobibilità mensile del 100% circa nei feriali, dalle 8 alle 13       | Monitoraggio del servizio    | DOC-02, STU-02   |
| _NFR-05_ | Ambientale   | Separazione sviluppo-produzione       | Dev e Prod usano database e credenziali differenti                      | Controllo configurazioni     | DIR-01, DIR-02   |
| _NFR-06_ | Supporto     | Tracciabilità degli errori            | il 100% degli errori del server vengono identificati e registrati       | Simulazione errori e log     | DOC-01, STU-02   |
| _NFR-07_ | Interazione  | Uniformità degli errori API           | il 100% degli errori hanno campi comuni (codice e messaggio)            | Test delle API               | DOC-01, DOC-03   |
| _NFR-08_ | Conformità   | Documentazione delle API              | Ogni API è documentata nell'apposito documento                          | Confronto API-documentazione | DOC-01, DOC-03   |


## Requisiti impliciti ##


## Assunzioni, vincoli e dipendenze ##

_Assunzioni_

| ID       | Assunzione | Cosa succede se è falsa |
|----------|------------|-------------------------|
| ASS-01   | Non più di 75 studenti aprono una verifica nello stesso minuto | Vanno rivisti il dimensionamento e il carico |
| ASS-02   | Gli utenti hanno a disposizione dispositivi aggiornati e funzionanti | Alcune funzioni potrebbero essere inutilizzabili |
| ASS-03   | La connessione a internet è garantita durante lo svolgimento delle verifiche | Gli studenti potrebbero non finire la verifica entro i tempi previsti |
| ASS-04   | Docenti e Studenti dispongono di email valide a cui mandare le credenziali | La consegna delle credenziali potrebbe non essere possibile |