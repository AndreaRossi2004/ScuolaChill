# PRD di ScuolaChill · Team Andrea Rossi

## Informazioni sul documento

|              |                                                     |
| ------------ | --------------------------------------------------- |
| **Prodotto** | ScuolaChill                                         |
| **Team**     | Andrea Rossi — sviluppo individuale                 |
| **Autori**   | Andrea Rossi                                        |
| **Versione** | 0.4                                                 |
| **Data**     | 12/10/2026                                          |
| **Stato**    | Bozza                                               |

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

Scuola di riferimento: **CFP Don Bosco San Donà di Piave**, ente di formazione professionale (IeFP) con 10 percorsi (5 qualifiche triennali 3EQF + 5 diplomi quadriennali 4EQF) nei settori elettrico, energia/termoidraulico, informatico, meccanico, automotive. I numeri su studenti, docenti, classi e orario sono stati forniti dall'istituto; connettività e picchi d'uso restano assunzioni.

|  | Valore |
| --- | --- |
| Numero di studenti | 412 |
| Numero di docenti | 50 |
| Numero di classi | 23, circa 18 studenti per classe in media (412 ÷ 23) |
| Orario scolastico | 8:10–13:20 e 14:00–16:00 |
| Connettività | Wi-Fi scolastico condiviso in aule/laboratori (banda non garantita nei picchi); studenti e docenti usano anche rete dati mobile personale fuori dai laboratori (assunzione) |

Fascia critica identificata: più classi che aprono una verifica nello stesso orario di inizio lezioni (es. 8:10, o alla ripresa delle 14:00), con banda Wi-Fi condivisa — è il caso guida per la stima del carico nella Seconda parte.

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
4. Il sistema crea l'account, lo mostra nell'elenco docenti come "in attesa di attivazione" e invia al docente una email con un link di attivazione personale. Il Direttore non sceglie né vede nessuna password.

*Scenario principale.* Il Direttore ha appena assunto un nuovo docente di elettrotecnica: in due minuti crea l'account, il docente riceve l'email, apre il link, sceglie la sua password e può accedere lo stesso giorno.

*Scenari alternativi.* Se l'email inserita esiste già, il sistema blocca la creazione e indica l'errore prima di salvare nulla. Se l'invio dell'email fallisce (servizio esterno non raggiungibile), l'account viene comunque creato con stato "email da rinviare", visibile al Direttore, che può farla reinviare senza dover ricreare l'account. Se il docente non apre il link entro il tempo previsto, il link scade e il Direttore può farne inviare uno nuovo.

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

**Storia: UTE-01 · Attivare l'account scegliendo la propria password**

> Come **utente (Direttore, Docente o Studente)** voglio **scegliere la mia password aprendo il link che ricevo via email** così da **essere l'unico a conoscere le mie credenziali**. (UTE-01)

*Acceptance criteria*
- AC-01 · Dato che ho ricevuto il link di attivazione, Quando lo apro, Allora posso scegliere la mia password e il mio account diventa attivo.
- AC-02 · Dato che ho già usato il link o è scaduto, Quando provo ad aprirlo di nuovo, Allora non funziona e ricevo un messaggio chiaro su come chiedere un nuovo link.
- AC-03 · Dato che la password scelta non rispetta le regole minime (es. troppo corta), Quando confermo, Allora ricevo un errore chiaro e l'account resta in attesa di attivazione.
- AC-04 · Dato che il mio account non è ancora attivo, Quando provo ad accedere, Allora l'accesso viene negato.

*User flow*
1. L'utente riceve l'email di ScuolaChill e apre il link di attivazione
2. Il sistema mostra la schermata "Scegli la tua password"
3. L'utente inserisce la password due volte e conferma
4. Il sistema attiva l'account e lo porta alla pagina di accesso

*Scenario principale.* Uno studente del primo anno riceve l'email, apre il link dallo smartphone, sceglie una password personale e da quel momento entra solo con quella: nessun altro l'ha mai vista.

*Scenari alternativi.* Se il link è scaduto o è già stato usato, non permette di scegliere la password e spiega di chiedere un nuovo link al Direttore. Se la password è troppo semplice, il sistema la rifiuta con un messaggio chiaro. Se l'utente prova ad accedere prima di aver attivato l'account, l'accesso viene negato anche se conosce la sua email.

---

## Requisiti non funzionali

| ID | Requisito | Soglia | Condizione | Storie collegate |
| --- | --- | --- | --- | --- |
| NFR-01 | Consegnare una verifica è veloce | Conferma entro 2 secondi per il 95% delle consegne | Con fino a 120 persone connesse insieme (picco delle 8:10) | STU-02 |
| NFR-02 | Le pagine di consultazione si aprono in fretta | Apertura entro 2 secondi per il 95% delle richieste | Da smartphone con rete mobile 4G | STU-01, STU-03, STU-04 |
| NFR-03 | La vista d'insieme del Direttore si carica in fretta | Caricamento entro 3 secondi | Con i dati di tutta la scuola (412 studenti, 23 classi) | DIR-04 |
| NFR-04 | Il servizio c'è quando serve | Disponibile almeno il 99% del tempo | Dalle 8:10 alle 16:00 nei giorni di lezione; gli aggiornamenti si fanno fuori da questa fascia | STU-02, STU-03, DOC-03 |
| NFR-05 | Nessun dato va perso | 0 consegne e 0 voti persi | Anche se la connessione cade durante l'invio: l'utente riceve sempre una conferma o un errore chiaro | STU-02, DOC-03 |
| NFR-06 | Ognuno accede solo a ciò che gli spetta | 0 operazioni riuscite a chi non ha il ruolo o il diritto richiesto | In ogni operazione, controllato dal sistema e non solo nascosto nell'interfaccia | DIR-01, DIR-03, DIR-04, DOC-01, DOC-02, DOC-03, STU-01, STU-02, STU-03 |
| NFR-07 | L'attivazione dell'account è sicura | Link valido 48 ore e utilizzabile una sola volta | Dal momento dell'invio dell'email o della richiesta di un nuovo link | DIR-01, DIR-02, UTE-01 |
| NFR-08 | Le password sono personali e non banali | Almeno 10 caratteri | Quando l'utente sceglie la password | UTE-01 |
| NFR-09 | Si usa bene da smartphone | Tutte le funzioni usabili su schermi larghi 360 pixel | Senza ingrandire e senza scorrere in orizzontale | STU-01, STU-02, STU-03, STU-04 |
| NFR-10 | È facile per i ragazzi del primo anno | Almeno il 90% completa una verifica senza aiuto | Durante il collaudo, alla prima volta | STU-02 |
| NFR-11 | Gli elenchi lunghi sono leggibili | 20 elementi per pagina | Quando un elenco ha più elementi | DIR-04, STU-01, STU-03 |
| NFR-12 | I dati dei minorenni sono protetti | Dati conservati nell'Unione Europea e comunicazioni sempre cifrate | Sempre | STU-03, DIR-04 |

---

## Assunzioni, vincoli e dipendenze

### Assunzioni

- La scuola ha 412 studenti, 50 docenti, 23 classi e lezioni dalle 8:10 alle 13:20 e dalle 14:00 alle 16:00 (dati forniti dalla scuola, vedi "La scuola che avete immaginato").
- Nello stesso momento è connesso circa il 10-15% degli utenti; nel picco delle verifiche iniziano insieme fino a 3 classi.
- Gli studenti usano in prevalenza lo smartphone, con Wi-Fi scolastico condiviso o rete mobile.
- Direttore, un docente e i ragazzi del primo anno sono disponibili per il collaudo.

### Vincoli

- Il progetto è sviluppato da una sola persona, con livello medio-basso: le soluzioni più semplici hanno la precedenza.
- Il PRD deve essere validato prima di scrivere codice.
- Tutto il traffico viaggia su HTTPS e il controllo dei ruoli sta nel backend.
- Il sistema deve essere online e raggiungibile pubblicamente per il collaudo.
- Budget ipotizzato: al massimo ~80 € al mese IVA inclusa.
- I dati sono di minorenni: devono restare nell'Unione Europea.

### Dipendenze

- Un servizio esterno per l'invio delle email (Brevo): se non risponde, gli inviti vanno rinviati.
- Il provider cloud (Azure) per applicazione, database e archivio dei file.
- GitHub per il codice e per la pubblicazione automatica.
- I dati reali dell'istituto (studenti, classi, orari) da confermare con la scuola.

### Requisiti impliciti emersi dalle interviste

Da compilare: finora non è stata fatta nessuna intervista, quindi qui non è stato scritto nulla. Da raccogliere con il Direttore, un docente e alcuni ragazzi del primo anno prima di chiudere il PRD.

---

# Seconda parte · Il come

## Stima del carico

Base: 412 studenti, 50 docenti, 1 Direttore (463 account), 23 classi, come dichiarato in "La scuola che avete immaginato".

### Utenti concorrenti

| Situazione | Utenti concorrenti | Da dove viene il numero |
| --- | --- | --- |
| Uso normale durante la giornata | ~45-70 | ~10-15% dei 463 account attivi nello stesso momento (consultazione sparsa di materiale e voti) |
| Picco delle 8:10 (verifiche) | ~100-120 | 3 classi che iniziano una verifica insieme (3 × ~18 studenti ≈ 54 consegne concentrate in pochi minuti) più il traffico di fondo delle altre classi e dei docenti |
| Fine quadrimestre (voti) | ~115-140 | Quando i voti vengono pubblicati, ~25-30% degli studenti (~105-125) li apre nella stessa mezz'ora, mentre ~10-15 docenti inseriscono gli ultimi voti. Sono quasi tutte letture |

Il picco è breve e prevedibile (l'orario delle verifiche è noto in anticipo): il sistema deve reggere raffiche brevi, non un carico costante alto. Il dimensionamento si basa sul caso peggiore, ~140 utenti concorrenti (quasi tutte letture); per le scritture il caso peggiore è il picco delle verifiche, ~100-120.

### Profilo di carico

| Operazione | Frequente? | Pesante? | Critica? | Note |
| --- | --- | --- | --- | --- |
| Login | Sì, concentrata all'inizio giornata e delle verifiche | No | Sì: senza login non si fa nulla | Ci sono ~54 login quasi simultanei alle 8:10; il controllo della password è l'operazione più costosa in CPU. Con Node va eseguito in modo asincrono, altrimenti blocca le altre richieste |
| Apertura verifica | Sì nel picco | No, è una lettura | Sì | Il contenuto è identico per tutta la classe, quindi si può riusare |
| Consegna verifica | No, una volta per studente | Media: scrittura delle risposte | Sì: non deve perdere dati né accettare doppie consegne | Scritture atomiche con conferma esplicita allo studente |
| Dashboard del Direttore | No, pochi accessi al giorno | Sì: aggregazioni su tutta la scuola | No | Elenchi paginati; candidata a una lettura già aggregata (vedi Persistenza e modello dei dati) |
| Caricamento materiale | No, alcune volte a settimana per docente | Sì: upload di file (limite 10-20 MB) | No | I file vanno su storage separato dal database |

---

## Scelte tecnologiche

| Area | Scelta | Alternativa considerata | Perché avete scelto così |
| --- | --- | --- | --- |
| Backend | **Node.js + Express** | ASP.NET Core Web API (C#) | Lo stesso linguaggio (JavaScript) di frontend e backend: meno cose da imparare per chi lavora da solo, moltissimi tutorial e librerie pronte per autenticazione con token, validazione degli input, documentazione OpenAPI/Swagger e accesso al database con query parametrizzate. Express è minimale: le librerie vanno scelte una per una, ma sono poche e molto documentate. |
| Frontend | **React con Vite (JavaScript)** | Angular / Vue / frontend su hosting separato | Moltissimi tutorial e componenti già pronti, mobile-first per gli studenti su smartphone. Servirlo da Express dà un solo deploy, un solo URL. Lo abbiamo iniziato in classe.|
| Database | **Azure SQL Database, Standard a capacità fissa** | Serverless con auto-pause / PostgreSQL | Dominio molto relazionale (studente-classe, docente-materia-classe, voto-verifica), con backup automatici e nessuna amministrazione. PostgreSQL è l'abbinamento più comune con Node e resta un'alternativa valida. |
| Provider cloud | **Microsoft Azure** | AWS | App Service supporta Node.js in modo nativo, il deploy si fa da GitHub o da riga di comando, un solo portale per tutti i servizi e documentazione in italiano. |
| Servizi cloud | **Azure App Service (Linux, B2)** per l'app e **Azure Blob Storage** per il materiale didattico | VM dedicata o Container Apps / file su disco o nel database | App Service gestisce sistema operativo, HTTPS e riavvii: zero amministrazione di server. I file non vanno nel database né sul disco dell'app (si perderebbero e impedirebbero di scalare); nel database resta solo il riferimento e il download passa dall'API, che controlla i permessi.|
| Regione | **Italy North (Milano)** | West Europe (Paesi Bassi) | Dati di minorenni che restano in Italia e latenza minima per gli utenti. Dal listino Azure (controllato online) App Service e Azure SQL costano in Italy North quanto in West Europe, e lo storage è praticamente uguale: restare in Italia non ha un costo extra |
| Servizio esterno | **Servizio email con piano gratuito (Brevo)**, chiamato via HTTP dal backend | Resend / SMTP diretto / Azure Communication Services | Serve per inviare il link di attivazione (DIR-01, DIR-02, UTE-01). Se l'invio fallisce, l'account viene creato comunque con stato "email da rinviare" e il Direttore può rinviarla. SMTP diretto scartato: più difficile da configurare e meno affidabile nella consegna. Limiti gratuiti (da recensioni, da confermare sui siti dei provider): Brevo ~300 email al giorno, Resend ~100 al giorno. Per creare 463 account servono quindi 2 giorni con Brevo o 5 con Resend: va pianificato o va scaglionata la creazione |
