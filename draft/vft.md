## VFT

##### Requisiti Funzionali
Per poter utilizzare i container dei video come strumenti di analisi, abbiamo implementato un programma che permetta di estrarli da in input un video.
Tale programma presenta inoltre varia altre funzionalità che sono state rese disponibili sia come libreria Java che come programma da terminale, implementando sia funzioni di basso livello che di alto livello che fungono da interfaccia per il programma da terminale.
Le features implementate sono:
- Parse: dato in input un file video di formato mp4-like (mp4 o mov), estrae il file container sfruttando la libreria Mp4Parser e lo salva in un file xml.
- Batch parse: dato in input una folder, fa il parse di tutti i video presenti nella folder e nelle sottofolder, ricreato la stessa struttura delle cartelle.
- Draw: permette di visualizzare tramite interfaccia un albero, dato in input un file xml.
- Merge: dati in input due file xml, permette di unire tali file xml in un unico xml. Prendendo uno come base, vengono aggiunti atomi gli atomi che l'altro ha in più; inoltre per gli atomi in comune, vengono considerati gli attributi, ovvero vengono aggiunti i valori nuovi.
- Update: come merge, ma prende in input una cartella che contiene file xml e fa il merge di tutti i file; considera anche le sottocartelle.
- Compare: dati due file xml, li compara ovvero dà una misura di quanto sono simili.


##### Implementazione

Durante lo sviluppo dell'applicazione sono state fatte varie scelte implementative. Di seguito verrano spiegati tali scelte in aggiunta ad ulteriori dettagli. Le feature spiegate saranno solo quelle principali, ovvero parse, merge e compare:

- Parse:
 la feature parse sfrutta la libreria Mp4Parser, a Java API to read, write and create MP4-like file.
 La funzione di alto livello è parse(), che viene utilizzata da interfaccia. Prende in ingresso due stringhe, l'url del file video da parsare e il percorso della cartella di output dove salvare il file xml. Si occupa di fare il setup per l'operazione, di chiamare la funzione di basso livello parser() e di salvare il container estratto in un file xml.

 La funzione parser() è ciò che estrae effettivamente il file container. Prende in ingresso un file iso del video da parsare, estratto dalla libreria Mp4Parser. Questa funzione crea la base (root) del file xml (org.dom) e instanzia un oggetto di classe BoxParser. Tale classe ha il compito di leggere il file iso ricorsivamente per estrarre tutti gli atomi e i relativi attributi. Infatti, come spiegato in precedenza, gli atomi hanno una struttura nidificata. BoxParser impementa una funzione ricorsiva chiamata getBoxes(). Tale funziona prende in ingresso un AbstractContainerBox e un Element. Il primo parametro è passato come null nella prima chiamata da parser(); il secondo è il root element creato in parser().

 La funzione getBoxes() legge il file iso (passato nel costruttore di BoxParser) ed estrae ricorsivamente gli atomi. Inizialmente il AbstractContainerBox è inizializato a null, quindi la prima volta vengono estratti gli atomi direttamente dal file iso; questi sono gli atomi di primo livello (ftyp, moov, mdat, ecc). In generale, per ogni atomo vengono estratti i figli. Viene ciclato sui figli e, in base al loro codice di 4byte rappresentato da una stringa di 4 caratteri, viene chiamato il giusto parser per quell'atomo che è in grado di estrarre i suoi attributi. Tali "parser" implementano l'interfaccia Wrapper che ha un solo metodo toString(). I wrapper implementati chiamano infatti le funzioni get degli atomi specifici e costruiscono una stringa che costituirà la lista degli attributi e dei loro valor dell'atomo. Un attributo che è stato inserito artificiosamente nell'implementazione a tutti gli atomi è l'attributo *count* con valore fisso a 0, che servirà successivamente solo per verificare se un atomo è presente o meno.

 Gli atomi non riconosciuti, ovvero per il quale non è riconosciuto il nome di 4 caratteri, vengono gestiti dal un wrapper di default; tale wrapper si limita a inserire solo l'attributo *count*. É sempre possibile estendere il numero di atomi supportati, semplicemente implementando un nuovo wrapper dedicato.

 Una volta estratti gli attributi viene creato l'elemento xml col nome uguale al codice e vengono settati gli attributi dell'atomo come attributi dell'elemento xml corrispondente. A questo punto, se l'atomo ha figli viene passato alla funzione parser(), continuando la ricorsione. Quando la ricorsione per un certo atomo arriva alla fine, l'elemento xml viene aggiunto al root, che ogni volta sara l'elemento xml che era stato passato nella ricorsione, fino ad arrivare al root vero e proprio. A questo punto, il parsing del file iso del file video è terminato e la funzione parser() ha un Element root che contiene il file container rappresentato come file xml. La funzione parse() si occuperà di salvarlo su disco.

 Alcuni dettagli implementativi:
 - per considerare la posizione degli atomi nel container, viene utilizzata una variabile che da assegna un numero ad ogni atomo figlio, in modo da assegnare un ordinamento a tali figli. Ciò fa in modo che possa essere riconosciuta la differenza di posizione fra atomi di due video, utile successivamente. L'atomo *moov* nell'xml non corrisponderà più al tag xml *<moov>* ma ad esempio *<moov-2>*. Due atomi *moov* in posizione diversa nel container sono considerati completamente diversi e aiutano notevolmente nella discriminazione dei file video usando i container.
 - la posizione degli attributi degli atomi non è considerata. Sia che venga sfruttata la funzione toString della libreria Mp4Parser che quella implementata nei wrapper, l'ordine degli attributi è comunque arbitrariamente decisi da uno di questi due proxy e non ha niente a che vedere con la scelta della casa produttrice come invece avviene per l'ordine degli atomi.


- Merge:
 tale feature, unisce due file xml che rappresentato file container. Verrà utilizzata in seguito nella fase di training.

 Dato in input due file xml, viene preso il primo come base. Viene esplorato ricorsivamente il secondo e per ogni atomo viene verificato se è presente o meno nel file base:
 - se è presente in entrambi, si procede a confrontare i valori degli attributi. Se nel secondo file, dato un attributo, viene trovato un valore diverso, questo viene aggiunto al file base, creando quindi un vettore di valori per tale attributo. Al "valore" dell'attributo, che è una stringa, viene anche inserito un vettore di pesi, inizializzati a zero, di dimensione pari al primo vettore.
 - se non è presente, l'atomo del secondo file, viene aggiunto al file base insieme ai suoi attributi.

 La procedura procede poi ricorsivamente, in maniera simile alla feature parse.
 Dato che il merge produce un file xml come output, il merge può essere fatto tra un file xml di un video e il file xml dato in output ad un merge precedente. L'unica differeza è che quando verranno confrontanti i valori deli attributi, non dovrò più verificare se il valore è diverso, ma se il valore dell'attributo del secondo file è presente nel vettore di valori per quell'attributo nel file base.


- Compare:
 la feaure compare server per confrontare due file xml e dare un  misura della differenza dei container che rappresentanto. L'applicazione pratica è l'Integrity analysis, ovvero verificare che il video è integro, ovvero il suo file container non ha subito modifiche dopo essere uscito dalla camere del dispositivo.

 Per confrontare i due file xml, viene scelto uno come reference e l'altro come query. In teoria, per il reference viene costruito un vettore di dimensioni pari a tutti gli elementi del tag che possono essere diversi, quindi per tutti gli attributi di tutti gli atomi (per gli atomi senza attributi abbiamo precedentemente inserito l'attributo count). Gli elementi di tale vettore sono idealmente inizializzati a 1. Per il query viene inizializzato un vettore delle stesse dimensioni. Procedendo come in precedenza, il query viene esplorato ricorsivamente cercando gli atomi del reference nel query e si hanno due casi:
 - atomo presente: vengono confrontati gli attributi, ogni attributo diverso corrisponde ad uno zero nel vettore di query ad indicare una differenza.
 - atomo non presente: vengono messi tanti zero quanti sono gli attributi dell'atomo del reference.

 Alla fine, otterremo due vettori della stessa dimensioni, uno di uni e l'altro di uni e zeri con gli zeri che indicano le differenze.
 Siccome fare la distanza euclidea fra due vettori di valori {0, 1} in cui uno è tutto inizializzato a uno corrisponde semplicemente a contare il numero di zeri presenti nel secondo vettore e poi fare la radice quadrata, invece di creare questi vettori, semplicemente viene incrementata una variable per ogni differenza trovata.
 Oltra al numero di differenze, viene calcolato anche il numero totale degli attributi, quindi delle potenziali differenze, del file xml reference.

 La feature compare restitutisce infine un json con i seguenti campi: reference, query, tot, diff. (spiegare nomi).


- Albero:
 serve per rappresentare i file xml come oggetti da manipolare con più facilità. Ogni tag del file xml è un Node o un Leaf, classi che implementano l'interfaccia Tree. Ogni Tree ha delle variabili per idenficarla (code), un padre (per il root è null), una lista di altri tree (i figli) e una lista di fields (gli attributi dell'atomo corrispondente).

##### CMD Line Tool

`usage: vft [-b | -c | -d | -h | -m | -p | -u]    [-i <file|folder>] [-i2
       <file>]  [-o <folder>]   [-wa]
 -b,--batch                 batch parse a directory of video files; it
                            recreates the same folder structure
 -c,--compare               compare two xml files
 -d,--draw                  draw a tree from an xml file
 -h,--help                  print help message
 -i,--input <file|folder>   video file for --parse, xml file for --draw
                            and --compare, a folder for --batch and
                            --update-config
 -i2,--input2 <file>        second xml file for --merge and --compare
 -m,--merge                 merge two xml file into one
 -o,--output <folder>       output folder for the xml file,for --parse,
                            --merge and --update-config
 -p,--parse                 parse a video file container into a xml file
 -u,--update-config         merge all files in the dataset folder into a
                            config.xml file
 -wa,--with-attributes      whether to consider attributes in --merge and
                            --update-config or not. Default is false
`
