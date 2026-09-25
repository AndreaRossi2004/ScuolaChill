# PRD di ScuolaChill · Team Andrea Rossi

## Informazioni sul documento

|              |                                                     |
| ------------ | --------------------------------------------------- |
| **Prodotto** | ScuolaChill                                         |
| **Team**     | Andrea Rossi — sviluppo individuale                 |
| **Autori**   | Andrea Rossi                                        |
| **Versione** | 0.1                                                 |
| **Data**     | 23/09/2026                                          |
| **Stato**    | Bozza                                               |

### Storico delle versioni

| Versione | Data       | Autore       | Cosa è cambiato e perché                   |
| -------- | ---------- | ------------ | ------------------------------------------ |
| 0.1      | 23/09/2026 | Andrea Rossi | Organizzato secondo il template ufficiale. |

---

### Perché esiste ScuolaChill

**Dal lato business.** ScuolaChill unifica tutte le attività necessarie per gestire classi, studenti, materiale scolastico e comunicazioni in un solo sistema, dando a ciascun ruolo solo ciò che gli serve e semplificando l'accessibilià
alla piattaforma.

**Dal lato tecnico.** Applicazione web full stack con tre ruoli (Direttore, Docente, Studente), API REST autenticate a token, dati relazionali persistiti su un database gestito, distribuita su cloud pubblico e raggiungibile per il collaudo reale con studenti, direttore e un docente.

### Cosa è incluso

- Creazione degli account di docenti e studenti da parte del Direttore
- Creazione delle classi e loro composizione (studenti, docenti per materia)
- Vista d'insieme della scuola per il Direttore (classi, docenti, studenti, andamento voti)
- Caricamento di materiale didattico da parte dei docenti, per classe e materia
- Creazione di verifiche e assegnazione voti da parte dei docenti
- Consultazione del materiale, svolgimento delle verifiche e consultazione dei propri voti da parte degli studenti
- Autenticazione e autorizzazione basata su ruolo, applicata sempre lato backend

### Cosa non è incluso

- Gestione di assenze e presenze
- Comunicazioni DIRETTE con le famiglie
- Gestione di pagamenti o rette
- Orario delle lezioni (timetabling settimanale)

## Stakeholder

| Stakeholder | Cosa fa | Cosa gli interessa | Come è coinvolto |
| --- | --- | --- | --- |
| Direttore | Dirige l'istituto; crea account e classi; ha visibilità completa. | Una vista d'insieme affidabile, senza dover entrare in ogni singola classe. | Intervista iniziale sulle assunzioni della scuola + collaudo finale. |
| Docenti | Insegnano una o più materie in una o più classi; caricano materiale; creano verifiche, valutano. | Uno strumento che non aggiunga lavoro rispetto a email/Excel; caricare materiale, assegnare, veloce. | Collaudo con un docente reale dell'istituto. |
| Studenti | Consultano il materiale, svolgono le verifiche, controllano i propri voti. | Un'interfaccia semplice, usabile da smartphone, senza ambiguità. | Collaudo con i ragazzi del primo anno. |
| Docente del corso | Valida il PRD. | Che ogni scelta sia motivata e coerente con le cinque aree architetturali richieste. | Presentazione del PRD e domande di validazione. |
| Collaudatori del primo anno | Usano ScuolaChill come utenti reali durante il collaudo. | Facilità d'uso, nessun errore bloccante, tempi di risposta ragionevoli. | Sessione di collaudo sull'ambiente pubblicato. |

---

## Destinatari e contesto d'uso

Scuola di riferimento: **CFP Don Bosco San Donà di Piave**, ente di formazione professionale (IeFP) con 10 percorsi (5 qualifiche triennali 3EQF + 5 diplomi quadriennali 4EQF) nei settori elettrico, energia/termoidraulico, informatico, meccanico, automotive. I numeri sotto non sono pubblicati dall'istituto (dato sensibile). DA VERIFICARE.

|  | Valore |
| --- | --- |
| Numero di studenti | ~380 (assunzione) |
| Numero di docenti | ~45, molti a orario ridotto su più settori/materie (assunzione) |
| Numero di classi | ~35, ~19 studenti/classe in media, più sezioni concentrate sui primi anni (assunzione) |
| Orario scolastico | 8:00–13:00 dal lunedì al sabato, con rientri pomeridiani per i laboratori pratici 2-3 volte a settimana  (assunzione) |
| Connettività | Wi-Fi scolastico condiviso in aule/laboratori (banda non garantita nei picchi); studenti e docenti usano anche rete dati mobile personale fuori dai laboratori |

Fascia critica identificata: più classi che aprono una verifica nello stesso orario di inizio lezioni (es. 8:00), con banda Wi-Fi condivisa — è il caso guida per la stima del carico nella Seconda parte.

### Gli archetipi

| ID | Archetipo | Contesto d'uso | Competenze digitali | Dispositivo principale | Frequenza d'uso |
| --- | --- | --- | --- | --- | --- |
| ARC-001 | Direttore | Ufficio di direzione, consulta spesso il sistema | Buone, uso quotidiano di email e strumenti gestionali | PC in ufficio, occasionalmente portatile | Quotidiana, sessioni brevi e frequenti orientate alla vista d'insieme |
| ARC-002 | Docente | Aula o laboratorio per fare l'appello e fra una lezione e l'altra, oppure da casa per preparare materiale e verifiche | Generalmente medie ed eterogenee | PC/laptop personale o della scuola; smartphone solo per controlli rapidi | Quotidiana con picchi quando carica materiale o quando controlla gli studenti |
| ARC-003 | Studente | Da casa o dal cellulare | Generalmente molto abituati allo smartphone, meno a form e interfacce gestionali articolate | Smartphone personale in prevalenza; PC nei laboratori durante le verifiche | Quotidiana per materiale/voti; Generalemnte di pochi minuti|

---

## User flow e User story

**Storia: DIR-01 · Creare account docente**

> Come **Direttore** voglio **creare gli account dei docenti** così da **dare al personale l'accesso al sistema con il ruolo corretto**. (DIR-01)

*User flow*
1. Il Direttore accede a ScuolaChill e apre la sezione "Docenti"
2. Seleziona "Nuovo docente" e compila nome, cognome, email, materie, classi
3. Conferma la creazione
4. Il sistema crea l'account, lo mostra nell'elenco docenti e invia una email con conferma di creazione account e le credenziali di primo accesso da cambaire.

*Scenario principale.* Il Direttore ha appena assunto un nuovo docente di elettrotecnica: in due minuti crea l'account, il docente riceve l'email e può accedere lo stesso giorno.

*Scenari alternativi.* Se l'email inserita esiste già, il sistema blocca la creazione e indica l'errore prima di salvare nulla. Se l'invio dell'email fallisce (servizio esterno non raggiungibile), l'account viene comunque creato con stato "email da rinviare", visibile al Direttore, che può farla reinviare senza dover ricreare l'account.

**Storia: DOC-01 · Caricare materiale didattico**

> Come **Docente** voglio **caricare il materiale didattico per le mie materie e classi** così da **metterlo a disposizione degli studenti in un posto solo**. (DOC-01)

*User flow*
1. Il Docente apre una classe/materia fra quelle assegnate
2. Carica un file (slide, PDF) con titolo e descrizione
3. Il materiale compare nell'elenco della materia, visibile agli studenti di quella classe

*Scenario principale.* Dopo una lezione di laboratorio, il docente carica le slide dal proprio laptop: gli studenti le trovano già la sera stessa.

*Scenari alternativi.* Se il docente prova a caricare materiale per una classe/materia a cui non è assegnato, l'operazione viene negata. Se il file supera la dimensione massima consentita, il sistema lo segnala prima di iniziare l'upload, non a metà caricamento.

**Storia: STU-01 · Consultare il materiale didattico**

> Come **Studente** voglio **consultare il materiale caricato da ogni docente delle mie materie** così da **avere tutto ciò che serve per studiare in un unico posto**. (STU-01)

*User flow*
1. Lo Studente apre la sua classe e seleziona una materia
2. Vede l'elenco del materiale caricato dal docente di quella materia, più recente per primo

*Scenario principale.* La sera prima di una verifica, uno studente riapre dallo smartphone le slide caricate dal docente in laboratorio.

*Scenari alternativi.* Se lo studente prova ad accedere (es. via link diretto) a materiale di una classe diversa dalla propria, l'accesso viene negato.

**Storia: STU-04 · Vedere la media per materia**

> Come **Studente** voglio **vedere la media dei miei voti in ciascuna materia** così da **capire subito dove sono in difficoltà senza fare i conti a mano**. (STU-04)

*Acceptance criteria*
- AC-01 · Dato che ho almeno un voto in una materia, Quando apro la pagina dei voti, Allora vedo la media di quella materia calcolata su tutti i suoi voti.
- AC-02 · Dato che non ho ancora voti in una materia, Quando apro la pagina dei voti, Allora la materia compare senza media (non "0").
- AC-03 · Dato che un docente registra o modifica un voto, Quando riapro la pagina, Allora la media è aggiornata.

*User flow*
1. Lo Studente apre la pagina "I miei voti"
2. Vede l'elenco delle materie, ciascuna con la propria media e i singoli voti
3. Apre una materia per vedere da quali verifiche provengono i voti

*Scenario principale.* Prima del colloquio con i genitori, uno studente controlla dallo smartphone che la media di matematica è sotto la sufficienza e decide di recuperare prima della prossima verifica.

*Scenari alternativi.* Se una materia non ha ancora voti, compare senza media. Se il docente corregge un voto, la media si aggiorna alla riapertura della pagina.

**Storia: UTE-01 · Cambiare la password al primo accesso**

> Come **utente (Direttore, Docente o Studente)** voglio **sostituire la password temporanea ricevuta via email con una mia** così da **essere l'unico a conoscere le mie credenziali**. (UTE-01)

*Acceptance criteria*
- AC-01 · Dato che accedo con le credenziali temporanee, Quando effettuo il primo login, Allora devo impostare una nuova password prima di usare il sistema.
- AC-02 · Dato che la nuova password non rispetta le regole minime (lunghezza, uguale alla temporanea), Quando confermo, Allora ricevo un errore chiaro e la password non cambia.
- AC-03 · Dato che non ho ancora cambiato la password, Quando provo a chiamare qualunque altra API, Allora l'operazione viene negata.

*User flow*
1. L'utente riceve l'email con le credenziali temporanee e accede a ScuolaChill
2. Il sistema lo porta subito alla schermata "Imposta la tua password"
3. Inserisce la nuova password due volte e conferma
4. Il sistema salva la password e lo porta alla sua pagina iniziale

*Scenario principale.* Uno studente del primo anno riceve l'email, accede dallo smartphone, sceglie una password personale e da quel momento entra solo con quella.

*Scenari alternativi.* Se la nuova password è troppo corta o uguale alla temporanea, il sistema la rifiuta con un messaggio chiaro. Se l'utente prova ad aprire altre pagine o API prima di averla cambiata, il backend nega l'operazione: il blocco non dipende solo dall'interfaccia.

---

# Seconda parte · Il come

> **Nota sul template**: la traccia ricevuta finora copre solo la "Prima parte · Il cosa"; il template ufficiale per la "Seconda parte · Il come" (architettura, API, persistenza, sicurezza, qualità, carico, stack, deployment) non è ancora stato fornito. I contenuti sotto sono organizzati con la struttura usata finora (numerata, coerente con i riferimenti incrociati già scritti nel documento) e vanno riversati nel template ufficiale non appena disponibile.

## 3. Requisiti non funzionali

- Disponibilità attesa: TODO
- Sicurezza (HTTPS, autenticazione, autorizzazione lato backend): TODO
- Prestazioni (tempi di risposta attesi per operazione critica): TODO
- Paginazione, formato errori uniforme: TODO
- Ambienti Development/Production: TODO
- Accessibilità/usabilità per utenti reali (primo anno): TODO

---

## 4. Le cinque aree architetturali

### 4.1 Fondamenti di architettura

- Architettura complessiva e suddivisione in componenti (diagramma): TODO
- Separazione delle responsabilità / architettura a livelli (presentation, application/business, data access) applicata al nostro gestionale: TODO
- Dipendenze fra livelli (chi conosce chi): TODO
- Come questo riduce accoppiamento e migliora testabilità: TODO

### 4.2 Progettazione e realizzazione delle API

- Risorse REST e route principali: TODO
- Uso di GET/POST/PUT/PATCH/DELETE, CRUD completo sulle entità principali: TODO
- Codici di stato HTTP e formato uniforme degli errori: TODO
- Validazione input e paginazione: TODO
- OpenAPI/Swagger + piano di verifica Postman: TODO
- **Contratto API principali** (route, verbi, payload esempio, risposte): TODO — tabella o sezione dedicata

### 4.3 Persistenza e modellazione

- Modello relazionale con diagramma ER (entità, relazioni, cardinalità): TODO
- Strategia di accesso ai dati, query parametrizzate (anti SQL injection): TODO
- Strategia di generazione degli identificatori: TODO
- Distinzione modello DB / modello di dominio / rappresentazione API: TODO
- Normalizzazione e viste denormalizzate (es. voti per materia, dashboard Direttore): TODO

### 4.4 Sicurezza e integrazione

- HTTPS end-to-end: TODO
- Autenticazione/autorizzazione: come si ottiene il token, cosa contiene, come viaggia il profilo utente: TODO
- Controllo dei tre ruoli, dove viene fatto rispettare (mai solo frontend): TODO
- Almeno una chiamata a un'API esterna dal backend (es. invio email credenziali) e gestione del fallimento: TODO
- Gestione configurazioni/segreti per ambiente: TODO

### 4.5 Qualità architetturale

- Organizzazione del codice (struttura progetti/moduli/cartelle) e motivazioni: TODO
- Design pattern applicati e a cosa servono: TODO
- Strategia di testabilità (cosa si testa, come si isolano DB/API esterne): TODO
- Differenze configurazione Development/Production: TODO
- Deployment applicazione + gestione migrazioni schema DB nel tempo: TODO

---

## 5. Stima del carico, dimensionamento e costi

> Base: ~380 studenti, ~45 docenti, 1 Direttore, ~35 classi (vedi "La scuola che avete immaginato", Prima parte).

### 5.1 Utenti concorrenti attesi

- **Condizioni normali** (durante l'orario scolastico, consultazione sparsa di materiale/voti): ~10-15% degli account attivi contemporaneamente → **~40-60 utenti concorrenti**.
- **Picco stimato**: 3 classi che iniziano una verifica alle 8:00 in punto → 3 × ~19 studenti ≈ 57 submission concentrate in pochi minuti, sommate al traffico di fondo (altre classi che consultano materiale, docenti che controllano la dashboard) → **~100-120 utenti concorrenti** nel picco.
- Il picco è **breve e prevedibile** (orario delle verifiche è noto in anticipo), non un carico sostenuto: il dimensionamento deve reggere burst brevi, non un throughput costante elevato.

### 5.2 Profilo di carico

| Tipo | Operazioni | Caratteristiche |
| --- | --- | --- |
| Frequenti, leggere | Login, consultazione materiale, consultazione voti | Prevalentemente letture (GET), payload piccoli, cacheable |
| Pesanti | Caricamento materiale didattico (upload file), dashboard aggregata del Direttore (query su tutta la scuola) | Upload di file di dimensioni non banali (PDF, slide); query di aggregazione su più tabelle — candidate per viste denormalizzate (cfr. 4.3) |
| Critiche | Consegna verifica entro la finestra d'esame, assegnazione voti | Bassa tolleranza a perdita dati o errore silente; scritture che devono essere atomiche e confermate all'utente; finestra temporale concentrata (il picco del 5.1) |

### 5.3 Dimensionamento macchine e costi stimati

> Dimensionamento provvisorio, indipendente dal provider — confermato/adattato in sez. 6 in base allo stack e al cloud scelti.

Con un picco di ~120 utenti concorrenti e operazioni in larga parte leggere, il carico rientra nella fascia "piccolo carico" per un singolo istituto:

- **App server**: 1 istanza, 2 vCPU / 4 GB RAM è sufficiente per il picco stimato con margine; non serve clustering fin da subito.
- **Database**: database relazionale gestito a capacità fissa (Azure SQL Standard S1, scalabile a S2/S3), storage iniziale ~10-20 GB (materiale didattico escluso, salvato su object storage separato).
- **Storage file** (materiale didattico): object storage dedicato (es. blob storage), dimensionato a crescita (partire da qualche decina di GB).
- **Stima costo mensile** (ordine di grandezza, da affinare col provider scelto in sez. 6): ~30-40 €/mese per app server + ~25-30 €/mese per DB gestito + storage file a consumo (pochi €/mese a questi volumi) → **totale indicativo ~60-80 €/mese** (cfr. sez. 6).

### 5.4 Strategia di scalabilità

- **Scelta iniziale**: scalabilità **verticale manuale** — il carico stimato (max ~120 utenti concorrenti, picchi brevi e prevedibili) non giustifica la complessità di un autoscaling orizzontale fin dal lancio.
- **Piano se la stima è sbagliata**: se il picco reale supera quanto dimensionato (es. l'istituto è più grande del previsto, o più classi si allineano sullo stesso orario di verifica), il passo successivo è passare a **scalabilità orizzontale** (più istanze app server dietro un load balancer, DB già gestito e separabile in read replica per le letture pesanti come la dashboard del Direttore). Il fatto che l'architettura sia già a livelli (sez. 4.1) e l'app server sia stateless (sessione/token, non stato in memoria) rende questo passaggio un'operazione di infrastruttura, non di riscrittura del codice.
- Monitoraggio da prevedere fin dal deployment (metriche CPU/RAM/tempo di risposta) per accorgersi per tempo se le assunzioni di questa sezione non reggono.

---

## 6. Scelte tecnologiche (con alternative considerate)

> **Vincolo guida: sviluppo da solo, livello medio-basso.** Ogni scelta privilegia (1) pochi componenti da imparare e gestire, (2) tanta documentazione/tutorial disponibili, (3) servizi gestiti (PaaS) invece di macchine da amministrare. Ho preferito una soluzione più semplice e "noiosa" anche quando esisteva un'alternativa più moderna o performante. Assunzione: conoscenza di base di C# e di web development; da correggere se diversa.

| Decisione | Scelta | Alternativa considerata | Motivazione (competenze / ecosistema / costi / NFR) |
| --- | --- | --- | --- |
| Backend | **ASP.NET Core Web API (C#, .NET LTS)** con Entity Framework Core | Node.js + Express/NestJS | Un solo linguaggio tipizzato e un framework "con le batterie incluse": Swagger/OpenAPI, autenticazione JWT con ASP.NET Identity, query parametrizzate via EF Core, dependency injection e configurazione Development/Production sono già integrati, quindi meno librerie da scegliere e incastrare. Node.js scartato: ottimo, ma per soddisfare gli stessi requisiti bisogna assemblare molte librerie diverse, più decisioni e più rischio di errori per chi è alle prime armi. |
| Frontend | **React con Vite (JavaScript) + libreria UI pronta (es. MUI o Tailwind)**, servito dallo stesso ASP.NET Core come file statici | Blazor WebAssembly / Angular / frontend su hosting separato | Per un solo sviluppatore conta poter scrivere poco codice UI: React ha la maggiore quantità di tutorial ed esempi e una libreria di componenti evita di disegnare l'interfaccia da zero (mobile-first, utenti su smartphone). JavaScript e non TypeScript per ridurre la curva di apprendimento (migrazione a TS possibile in seguito). Servire il build React dall'app ASP.NET Core dà **un solo deploy, un solo URL, niente problemi CORS**. Blazor scartato: bundle pesante su mobile; Angular scartato: troppo framework da imparare; hosting separato scartato: raddoppia deploy e configurazione. |
| Database | **Azure SQL Database, tier Standard (DTU) provisioned, partendo da S1** | Serverless con auto-pause / PostgreSQL | Dominio fortemente relazionale (studente-classe, docente-materia-classe, voto-verifica): serve un RDBMS con vincoli di integrità; Azure SQL si integra con EF Core, backup automatici inclusi, zero amministrazione. Tier fisso invece del serverless: con auto-pause il primo accesso dopo l'inattività può richiedere decine di secondi, inaccettabile per il collaudo con studenti e docenti; costo prevedibile. Scalabile a S2/S3 con un click se il picco lo richiede. PostgreSQL scartato: alternativa valida ma con meno integrazione nel portale Azure e negli strumenti .NET. |
| Cloud provider | **Microsoft Azure** | AWS | Coerenza con lo stack .NET (deploy diretto da Visual Studio/GitHub, diagnostica integrata), un solo portale per tutti i servizi, documentazione in italiano e percorsi di apprendimento gratuiti. AWS scartato: offerta equivalente ma più configurazione manuale (reti, permessi) e curva di apprendimento maggiore. |
| Hosting applicazione | **Azure App Service (Linux, piano B2)** — PaaS | VM dedicata / Container Apps / Kubernetes | Si carica il codice e Azure gestisce sistema operativo, HTTPS con certificato gestito, riavvii e log. Zero amministrazione di server: fondamentale se si è soli. Coerente col dimensionamento 5.3 (1 istanza, ~2 vCPU). Scalabile verticalmente con un click. VM scartata: obbliga a gestire patch e configurare TLS; Container Apps/Kubernetes scartati: complessità inutile per una sola app. |
| Storage file (materiale didattico) | **Azure Blob Storage**, con download passante dal backend | Salvataggio su disco dell'app server o nel DB | I file non vanno nel DB né sul disco dell'app server (si perderebbero o non permetterebbero di scalare); nel DB si salva solo il riferimento. Il download passa dall'API, che controlla l'autorizzazione e poi restituisce il file: più semplice da implementare e da testare rispetto a generare URL firmati (SAS), e mantiene il controllo permessi interamente lato server. Dimensione massima file limitata (es. 10-20 MB) per contenere costi e complessità. |
| API esterna (invio credenziali) | **Servizio email transazionale con piano gratuito (es. Brevo, Resend)** chiamato via HTTP dal backend | SMTP diretto / Azure Communication Services | Invio della email di creazione account (DIR-01/02) tramite una semplice chiamata HTTP; il fallimento non deve bloccare la creazione dell'account (stato "email da rinviare", dettagli in 4.4). Verificare limiti e condizioni del piano gratuito al momento della scelta. SMTP diretto scartato: deliverability e configurazione più difficili. |
| Deployment / CI-CD | **Prima release: pubblicazione manuale da Visual Studio/CLI verso App Service; poi GitHub Actions** generata dal Deployment Center di App Service | Pipeline complessa fin da subito | Un solo sviluppatore non ha bisogno di pipeline elaborate all'inizio; si parte dal deploy manuale (semplice da capire e correggere) e si automatizza quando l'app funziona. Il Deployment Center di Azure genera in automatico il file GitHub Actions, evitando di scriverlo da zero. |

**Cosa NON uso (di proposito):** microservizi, Docker/Kubernetes, code di messaggi, cache distribuite (Redis), autoscaling. Il carico stimato (~100-120 utenti concorrenti al picco) non li richiede, e ognuno aggiungerebbe un argomento da imparare e un punto di guasto da gestire da solo. Sono candidati per evoluzioni future solo se le stime di sez. 5 si rivelassero sbagliate.

> Costi: fasce di 5.3 aggiornate a questa scelta — App Service B2 (~30-40 €/mese) + Azure SQL Standard S1 (~25-30 €/mese) + Blob Storage (pochi €/mese) + email (piano gratuito) → **~60-80 €/mese** indicativi. Verificare i prezzi aggiornati con l'Azure Pricing Calculator prima della consegna.

---

## 7. Piano di deployment

- Pipeline CI/CD (se prevista): TODO
- Come applicazione e database arrivano sul cloud scelto: TODO
- Gestione delle migrazioni schema DB nel tempo: TODO
- Come avviene il collaudo con utenti reali (accesso pubblico, credenziali di test): TODO

---

## Appendice — Riferimenti

- Diagramma architetturale: TODO (link/immagine)
- Diagramma ER: TODO (link/immagine)
- Collezione Postman: TODO (link)
- Repository: TODO (link)
