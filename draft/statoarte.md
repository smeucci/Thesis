## STATO DELL'ARTE

- utilizzo di immagini e video come prove
- definizione multimedia Forensics.
- fingerprints
- procedure standard
- chi è l'analista forense
- applicazioni (autenticità, integrità, identificazione)
- strumenti utilizzati (flusso dati, metadati)
- metadati exif per immagini, debolezze
- metadati file container per video, vantaggi e descrizione
- applicazioni precedenti usando container

##### 1.1. Introduction to Multimedia Forensics

Con la sempre maggior diffusione di contenuti digitali sia audio che video, l'analisi su questi oggetti multimediali sta sempre più assumendo di importanza nel contesto delle investigazioni digitali, che considerano sia dati digitali che dispositivi digitali.

Nelle investigazioni digitali i contenuti multimediali come immagini, tracce audio e video sono sempre più utilizzati come prove giudiziarie. Risulta quindi molto importante riuscire ad estrarre informazioni da tali contenuti in maniera affidabile.

La multimedia forensics ha l'obbiettivo di acquisire conoscenza sul ciclo di vita di un contenuto multimediale sfruttando le tracce che i vari step di processing lasciano sui dati. Infatti, l'idea alla base del multimedia forensics è che ogni dispositivi di acquisizione e ogni operazione di processing sui dati multimediali lascia sui dati del contenuto multimediale delle tracce, note come fingerprints, che quindi caratterizzano la sua storia.

Molti algoritmi e tecniche sono state sviluppate dalla comunità scientifica basandosi sull'estrazione di features dal flusso di dati audio-visuali del contenuto multimediale e sfruttando tali features cerca di inferire alcune informazioni sul dispositivo d'acquisizione sorgente e sui processi di modifica e codifica a cui il contenuto multimediale è stato sottoposto durante il suo ciclo di vita. In particolare riguardo contenuti come immagini e video, le tecniche principali si pongono l'obbiettivo di identificare la sorgente del video/immagine e/o determinare se il contenuto è autentico oppure è stato modificato senza nessuna informazione a priori sul contenuto sotto analisi. Ciò è possibile sfruttando appunto tali features e degli strumenti che permettono di verificare la presenza o l'assenza di tali features, ovvero delle impronte che sono intrinsicamente legate ai dati multimediali dal dispositivo sorgente o dagli step di processing di programmi di modifica ecc. Infatti si possono distinguer tre tipi di tracce lasciate su un contenuto multimediale: tracce di acquisizione, tracce di codifica e tracce di modifica.

Come spiegato, da un punto di vista scientifico, la ricerca ha prodotto un grande numero di tecniche per l'analisi di contenuti multimediali, ma dal punto di vista dell'applicazione di tale tecniche nell'aule di tribunale c'è un gap importante che è probabilmente dovuto ad una mancanza di comunicazione fra la parte legale e quella scientifica, così come una non completa maturità delle tecniche che spesso si basano su risultati ottenuti in contesti di laboratorio e non in scenari del mondo reale.

Inoltre c'è necessità di una maggiore condivisione di standard nel contesto delle investigazioni digitali che aiuti la multimedia forensics a cresce ed a raggiungere la maturità.
Diverse comunità si sono adoperate per mettere insieme delle linee guida e degli standard su aspetti importanti dell'investigazioni digitali come catena di custodia, autenticazione dei dati, applicazione del metodo scientifico, documentazione e reporting.
The ISO/IEC JTC1 Working Group 4 è uno di tali gruppi che cercano di dare standard internazionali (lista) il cui scopo principali è quello di promuovere delle buone procedure e metodi per l'investigazione di prove digitali e di incoraggiare l'adozione di approcci per l'analisi forense di contenuti digitali più simili a livello internazionale, in modo da rendere più facile il confronto e la combinazione di risultati provenienti da entità e organizzazione differenti e anche attraverso giurisdizioni diverse, in modo tale da aumentare l'affidabilità delle tecniche e dei risultati.

Un'altro gruppo che si occupa di dare standard e linee guida è lo Scientific Working Group on Digital Evidence (SWGDE) che si occupa di mettere in contatto organizzazioni diverse che sono concentrate nel campo del multimedia forensics in modo da promuovere comunicazione e cooperazione e assicurare una maggiore qualità e consistenza all'interno della comunità forense.

Lo Scientific Working Group on Imaging Technologies (SWGIT) invece concentra il suo lavoro sulle tecnologie di analisi delle immagini e ha l'obbiettivo di facilitare l'integrazione di tali sistemi di analisi delle immagini nel contesto del sistema giudiziario fornendo best practises e linee guida per l'acquisizione, storage, processing, analisi, trasmissione, output image e archivio delle prove digitali.
Riguardo l'analisi forense di immagini e/o video, il processo è definito essere composto da tre task principali: preparazione tecnica, esaminazione, interpretazione. La preparazione tecnica si occupa di tutti quelle operazioni necessarie a preparare i video e le immagini per gli altri task. L'esaminazione è la parte principali dell'analisi forense e si occupa dell'applicazione delle tecniche  per estrarre informazioni dai video/immagini. L'interpretazione riguarda l'analisi del contenuto digitale da parte di esperti in modo da fornire conclusioni sulle features estratte dai video/immagini in esame.

In questo contesto diventa fondamentale la figura dell'analista forense, ovvero coli che è in grado di applicare tali tecniche di analisi di contenuti digitali, interpretare i risultati e fare una sintesi di risultati provenienti da tecniche diverse in modo da aumentare l'affidabilità delle conclusioni. É inoltre in grado di eseguire tutti i tasks dell'analisi in accordo con gli standard condivisi dalla comunità forense.
L'analista forense è in grado di trovare le tracce lasciate sui dati multimediali ed acquisire numerose informazioni sull'oggetto multimediali in esame come il dispositivo d'origine, se il contenuto è autentico, se l'oggetto digitale è stato sottoposto a particolare fasi di processing, che informazioni semantiche possono essere estratte da esso, se esso è integro, ecc.

Applicazioni.
Le principali applicazione per l'analisi forense sono: la source idenfitication, l'autenticazione e l'integrità della risorsa multimediale.

Il processo di identificazione della sorgente ha come obbiettivo quello di recuperare informazioni riguardo il dispositivo sorgente del contenuto multimediali in esame. É possibile identificare la sorgente a vari livelli di dettaglio. Ad esempio distinguere fra tipi di sorgenti, distinguere fra differenti modelli di sorgente dello stesso tipo e distinguere fra dispositivi diverse ma dello stesso tipo e modello. Informazioni possono essere ottenuti dai metadati.

Il problema dell'autenticazione riguarda il compito di stabilire se il contenuto multimediale è un accurato rendiconto di un evento originale. Il processo di analisi si basa sulla ricerca di inconsistenze nelle features del segnale audio-visivo.

Il problema dell'integrità riguarda il compito di stabilire se un contenuto multimediale è stato modificato o meno da quando è stato acquisito dal dispositivo sorgente. L'analisi si basa sulla ricerca di tracce lasciate dai tools di editing e processing durante il ciclo di vita che non sono compatibili con i contenuti provenienti dal dispositivo sorgente che è noto.

Un file multimediali può essere visto come un pacco composto da due parti principali: l'header, che contiene i metadati ovvero informazioni sul contenuto del file; il contenuto stesso, ovvero il flusso di dati che forma il segnale audio-visivo.
In generale, l'estrazione di features si basa sull'analisi delle tracce lasciate sia sui dati veri e proprio che sui metadati del file multimediale.

Per quanto riguarda l'ispezione del flusso di dati, l'esaminazione consiste di due aspetti principali: l'interpretazione del contenuto, che riguarda l'analisi del contesto come comprendere cosa sta succedendo, chi sono i soggetti e gli oggetti coinvolti, informazioni sull'ambiente e tutte le informazione che derivano dall'osservazione umana; identificazione di dettagli rivelanti nella scena rappresentata, come anomalie audio-visive, features interessanti come direzione della luce, ombre, prospettiva incoerenze, segni di smudge ecc. Sempre nel contesto dell'ispezione del segnale audio-visivo, è l'analisi e l'enhancement dei contenuti, come migliorare il segnale per rilevare dettagli o oggetti rilevanti, estrazione di relazioni dimensionali come dimensioni di oggetti o soggetti, comparazione fotografica fra oggetti noti e oggetti rappresentati nella scena.

L'altro strumento riguarda l'estrazione e l'analisi di metadati. I metadati possono essere estratti facilmente e possono contenere molte inforamzioni riguardo il segnale audio-visivo come ad esempio (video) il dispositivo sorgente, lo spazio colore, la risoluzione, i parametri di compressione, la data, dati gps, frame rate, (audio) format tags, bit rate, sample rate, numero di canali. Ovviamente il tipo e il numero di metadati dipende dal tipo del file in esame e quali processi ha subito durante il suo ciclo di vita.Ad esempio, per quando riguarda le immagini i metadati sono estratti dall'header Image file format (Exif). Una volta ottenuti i metadati è compito dell'analista forense verificare la compatibilità, la completezza e la coerenza delle inforamzioni estratte rispetto allo scenario da cui il contenuto si suppone provenire: metadati saranno differenti se una risorsa proviene da un social network piuttosto che direttamente dal dispositivo di acquisizione.



##### 1.2. Forensic Analysis of Video File Format


###### 1.2.1. Container

Gli standard dei formati video prescrivono un numero limitato di features, lasciando quindi molta libertà alla casa produttrice. Ciò può essere sfruttato come risorsa per identificare e autenticare, dato un video, la sorgente di provenienza. In particolare lo studio si concentra su MP4-like video format (mp4 e mov).

In un mondo sempre più digitale c'è sempre più la necessità di metodi per l'autenticazione dei dati multimediali, del riconoscimento della loro sorgente di provenienza e della loro storia.

L'anilisi forense si è principalmente concentrata su l'analisi di immagini sviluppando tecniche basate su due principali approcci: analisi del rumore (PRNU); analisi dei metadati JPEG e EXIF.

Anche l'analisi forense di video digitale è sempre più rilevente. I primi lavori hanno seguito un procedimento simile a quello delle immagini, basandosi rumore e artefatti presenti nel video ed anche l'utilizzo di metadati JPEG e EXIF.
Tuttavia, riguardo all'utilizzo di metadati per l'autenticazione di sorgenti digitali, sono state espresse preoccupazioni rispetto falsicabilità. Infatti, è solo questione di trovare il giusto strumento, spesso disponibile pubblicamente, per modificare facilmente informazioni di alto livello come appunto i metadati EXIF.

Contrariamente, gli strumenti noti non permettono di modificare strutture core dei file multimediali, come appunto i video file containers, ed altri strumenti pubblicamente disponibili che permettano di falsificare sistematicamente i containers non sono noti. Quindi queste caratteristiche di basso livello riducono le preoccupazioni riguardo la falsificabilità di tali metadati in quanto solo in possesso di skills avanzate di programmazione sarebbe possibile modificare la struttura interna dei file video rendendo la falsificabilità un'operazione non triviale ed altamente tecnica.

Il il formato container MOV è stato introdotto da Apple nel 1991. Utilizzando questo formato come base, sono stati introdotti anche i formati MP4 e 3GP.

I file containers di questo formato sono composti da atomi (o boxes) che sono identificati caratteri unici di 4 byte, preceduti dalla dimensione dell'atomo. Tali atomi possono avere o meno dei campi e possono essere o meno nidificati, ovvero possono contenere altri atomi; quindi si incontrano 4 tipi di atomi:
- atomi senza campi né che contengono altri atomi;
- atomi senza campi che contengono altri atomi;
- atomi con campi ma che non contengono altri atomi;
- atomi con campi che contengono altri atomi;

La struttura più comune dei MP4-like containers è la seguente:
- atomo *ftyp*: è semi-obbligatorio, ovvero il più recente standard ISO prevede che sia presente e che venga esplicitato il prima possibile nel file container. Fa riferimento alla specifica del tipo di file con la quale il file video, a cui il container appartiene, è compatibile. I campi principali sono *major_brand*, che indica quale specifica sia la migliore, e vari campi numerati chiamati *compatible_brand*, che indica una specifica che è compatibile con il file video. Non contiene altri atomi.
- atomo *mdat*: contiene lo stream dei dati ed è accompagnato dal campo *size* che ne indica la dimensione. Non contiene altri atomi.
- atomo *moov*: è l'atomo con la struttura più complessa del container. É un atomo nificati, ovvero contiene numerosi altri atomi. In questo atomo, e nella sua sotto-struttura, sono contenuti i metadati necessari per la decodifica del flusso dati contenuto nell'atomo *mdat*.

Tale struttura non è sempre rispetta. I file container dei formati MP4-like sono molto complessi e presentano numerosi elementi e differenze dipendenti dalla sorgente e dalla casa manifatturiera; quindi costituiscono uno strumento prezioso per un'analista forense.
Le differenze dei containers di diversi file video si possono presentare nei seguenti modi:
- posizione relativi degli atomi: nonostante lo standard dia specifiche riguardo la posizione di determinati atomi (in particolare quelli principali), ciò non è sempre rispettato. Soprattutto, si ha variazione di posizione per quegli atomi la cui posizione non è specificata dallo standard. Questo discorso è valido sia al primo livello, ovvero quello contenente gli atomi *ftyp*, *mdat*, *moov*, sia per quanto riguarda gli atomi della sotto-struttura contenuta nell'atomo *moov*.
- presenza di atomi aggiuntivi non standard ai vari livelli del container.
- differenze nei valori dei campi contenuti negli atomi.

Tutte queste possibili differenze e tipi di differenze danno vita ad un numeroso insieme di combinazioni che l'analista forense può sfruttare per autenticare una risorsa video.

PARLARE ANCHE DEL FORMATO AVI.

###### 1.2.2. Applications

Il problema dell'autenticazione si basa sull'analisi del segnale audio-visivo, quindi non è nel nostro dominio visto che usiamo i file container che sono metadati. Con i metadati non si può stabilire se il contenuto multimediale è una rappresentazione autentica di un evento originale.

Il problema dell'integrativa invece viene affrontato. Infatti, come descritto prima, i file container di un video è il file container creato dall'ultimo strumento del ciclo di vita. Se un video è modificato anche leggermente da un software avrà un suo particolare container. Questo container conterrà atomi aggiuntivi o in posizione diversa e valori degli attributi differenti rispetto ad un video proveniente direttamente dalla sorgente. É quindi possibile confrontare un container reference, preveniente da un dispositivo noto, ed un container query, che si suppone provenire da quel dispositivo noto, per verificare se il video è stato o meno modificato.

Il problema della source classification ha senso perchè siccome gli standard definiscono solo alcune features obbligatorie, lasciano molta libertà alle case manifatturiere. Significa che ogni classe di dispositivi avrà il suo container diverso. Ciò può essere usato per distinguere fra dispositivi, nel caso di video provenienti da smarthphone, dello stesso brand, dallo stesso brand e model, dallo stesso brand, model e sistema operativo.
Per fare ciò occorre rappresentare in qualche modo le varie classi presenti in un training set. Inoltre occorre una misura di compatibilità che stimi la probabilità di un container query di appartenere ad una determinata classe di dispositivi.
