## VIDEO FILE CONTAINERS

### Paper

#### Abstract

Gli standard dei formati video prescrivono un numero limitato di features, lasciando quindi molta libertà alla casa produttrice. Ciò può essere sfruttato come risorsa per identificare e autenticare, dato un video, la sorgente di provenienza. In particolare lo studio si concentra su MP4-like video format (mp4 e mov).

#### Introduzione

In un mondo sempre più digitale c'è sempre più la necessità di metodi per l'autenticazione dei dati multimediali, del riconoscimento della loro sorgente di provenienza e della loro storia.

L'anilisi forense si è principalmente concentrata su l'analisi di immagini sviluppando tecniche basate su due principali approcci: analisi del rumore (PRNU); analisi dei metadati JPEG e EXIF.

Anche l'analisi forense di video digitale è sempre più rilevente. I primi lavori hanno seguito un procedimento simile a quello delle immagini, basandosi rumore e artefatti presenti nel video ed anche l'utilizzo di metadati JPEG e EXIF.
Tuttavia, riguardo all'utilizzo di metadati per l'autenticazione di sorgenti digitali, sono state espresse preoccupazioni rispetto falsicabilità. Infatti, è solo questione di trovare il giusto strumento, spesso disponibile pubblicamente, per modificare facilmente informazioni di alto livello come appunto i metadati EXIF.

Contrariamente, gli strumenti noti non permettono di modificare strutture core dei file multimediali, come appunto i video file containers, ed altri strumenti pubblicamente disponibili che permettano di falsificare sistematicamente i containers non sono noti. Quindi queste caratteristiche di basso livello riducono le preoccupazioni riguardo la falsificabilità di tali metadati in quanto solo in possesso di skills avanzate di programmazione sarebbe possibile modificare la struttura interna dei file video rendendo la falsificabilità un'operazione non triviale ed altamente tecnica.

#### MP4-like containers

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
