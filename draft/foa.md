## FOA

##### Teoria
L'applicazione è la source identification ovvero dato un video vogliamo determinarne la sua origine. Da che dispositivi, servizio, piattaforma, programma viene? Siccome il file container è molto arbitraria, lascia tracce che possono essere usata per identificarne l'origine.
Per ciascuno delle origini, o classi, di interesse viene posta una domanda binaria, ovvero: tale video proviene dall'origine X? La risposta dovrà essere una likelihood, che mi esprimerà quanto il video appartiene a quella classe; sogliando poi opportunamente la likelihood sarà possibile rispondere alla domanda binaria.
Per ogni domanda, vengono fatte due query nel dataset di training: vengono presi un insieme di video che sappiamo rispondo true alla domanda, ovvero appartengono alla classe in oggetto, e un insieme di video complementare che corrisponde ad una classe avversaria, composta cioè da video che rispondono false alla domanda binaria.

Il groundtruth viene quindi diviso in questi due set. Viene poi fatto un merge di tutti i file container (come spiegato in precedenza) in un unico file xml in modo che contenga tutti gli atomi e tutti i possibili valori degli attributi dei due set. Durante il merge, viene inizializzato il vettore dei pesi nella stringa degli attributi; ogni volta che un valore viene aggiunto si inizializza a zero il suo peso, quando troviamo il solito peso più volte il suo peso incrementa, modulato dal numero totale di video del set. Ciò serve a calcolare il potere discriminante di ciascun attributo rispetto alle due classi.

A questo punto, dato in ingresso un video query di cui vogliamo determinare la classe, guardiamo i valori dei suoi attributi e per ciascuno di questi viene calcolato il rapporto fra i pesi associati a quel valore nei due set. Il numerato è il set che risponde true e il denominatore il set che risponde false. Tale rapporto ha un massimo di 1 e un minimo di zero escluso (vedi dettagli). I rapporti di tutti gli attributi vengono poi moltiplicati (ecco perchè non può essere zero); più tale prodo è maggiore di 1, più il video è sicuro appartenere al set che risponde true; più è minore di 1 e più il video è sicuro appartenere al set che risponde true, ovvero appartiene al set che risponde false.


##### Requisiti Funzionali
L'implementazione della teoria è stata realizzata con un'applicazione java da utilizzare da terminale. Le funzionalità principale sono:
- Training: si occupa prendere i due set considerati e di fare il merge di tutti i file xml. Inoltre calcola il potere discriminante di ciascun attributo relativamente ad entrambi i set.
- Testing: è la fase che si occupa di rispondere alla domanda. Dato un video in ingresso, calcola la likelihood usando i config file costrutiti dalla fase di training. La likelihood viene modulata usando il logaritmo in base 10 e tale score viene sogliato sullo zero. >0 true, <0 false.

In foa sono state poi implementate una serie di operazione su database sqlite che serviranno successivamente per la web application che serve da interfaccia di foa e verranno quindi descritte in seguito.

##### Implementazione
Sono state fatte alcune scelte implementative per migliorare la capacità del programa di identificare correttamente la classe:
- quando cerco un atomo per calcolare i rapporti, prendo l'atomo corrispondente nel config di training cercando il nome intero es. *<moov-2>*. Questo vale per tutti gli atomi tranne per *<trak>*. Il codice trak è usato sia per l'atomo delle traccia audio che quello della traccia video. Cercare *<trak-2>* potrebbe restituire si l'atomo con lo stesso nome ma che invece riguarda l'audio e non il video, dovuto al diverso posizionamente nel file container.
- alcuni attributi che sono noti essere unici per il video e non per la classe a cui appartiene non vengono considerati nella fase di testing. Tali attributi sono ad esempio la dimensione del file video, o la data di creazione e modifica. Potremmo considerarli e poi sogliare la likelihood in accordo dopo aver fatto una statistica di quanto sposta tale modifica per tutte le classi. (come in compare).
- alcuni atomi non vengono considerati, in particolare xyz e udta. L'atomo xyz è dove sono salvate le informazioni relative alla posizione del dispositivo nel momento dell'acquisizione. Sebbene molto utile per certi scopi, non è utile come discriminante per l'appartenza ad una classe di dispositivi. Infatti basta filmare due video dallo stesso dispositivo una volta col gps accesso e l'altra spento per ottenere due container diversi. Tale diversità è accentuata dal fatto che considerando la posizione degli atomi (numerazione dei figli), l'aggiunta di un tag fa incrementare il valore del numero a tutti gli atomi figli che vengono dopo. Questo significa che i container verrà visto come completamente diverso. Per porsi rimedia basta attivare una flag quando ho trovato ad un certo livello dell'albero creato dal file xml. Quando la flag è attiva, per quel livello del file xml, cerco i tag in base al codice vero senza considerare il numero della posizione. (da rivedere).
- dato un atomo ho tanti attributi; nella fase di testing per ogni attributo di un atomo calcolo il rapporto e poi ne faccio il prodotto. Ciò rappresenta la likelihood di quell'atomo. Se però i rapporti degli attributi di un atomo sono tutti esattamente uguali, praticamente sto moltiplicando più volte la stessa cosa; è come se stessi contanto più volte la stessa uguaglianza. Per ovviare è ciò la likelihood di ciascuno atomo è modulata in base ad un entropia data da una formula (da mettere). Quando l'entropia è massima, ovvero tutti i rapporti diversi fa il prodotto normale; quando è minima ovvero zero praticamente considera solo un rapport; nei casi medi fa una specie di media per quei gruppetti che hanno i rapporti uguali. Altrimenti alcuni atomi poco discriminanti ma con tanti attributi pesano tantissimo.
- rapporto: quando vado a calcolare il rapporto fra i pesi di un attributo di un atomo, vado a prendere i pesi nel set A e i pesi nel set B. Si possono verificare fra casi diversi:
 - numeratore e denominatore = 0: faccio rapporto normale.
 - numeratore = 0 e denominatore != 0: siccome non posso avere un rapporto pari a zero altrimenti col prodotto mi manda a zero tutta la likelihood, devo modificare il numeratore. Siccome tale caso in teoria è favorevole alla classe del denominatore (B), mi basta scegliere come numeratore un valore per il quale il rapporto è sempre minore di 1. Quindi scelgo 1 / il numero di video presenti nella classe B + 1. In tal modo è sempre minore di 1, con il rapporto che però varia in base al valore del denominatore.
 - numeratore != 0 e denominatore = 0: il denominatore non può essere zero. Come prima: mi basta che il rapporto sia sempre in favore di numeratore, quindi che sia sempre maggiore di 1. Sceglo il denominatore pari a 1 / il numero di video della classe A + 1. Anche in questo caso l'intensità di tale rapporto varia in base al valore del numeratore.
 - numeratore e denominatore != 0: questo caso si verifica quando il video di query presenta atomi o valori di attributi che non sono presenti nel config di training. Tale case viene considerato a favore della classe B. Infatti tale problema, seppure binario, non è simmetrico: nel dubbio dico che non è della classe A (meglio lasciare libero un colpevole che mettere condannare un innocente). Questo caso viene considerato come un 0/1, in cui il peso per la classe A è zero e quello per la classe B è pari a 1 massimo. Modifichiamo il numeratore con i principi del secondo caso, ma invece di considerare il numero di video della classe del denominatore, lo scegliamo di quello del denominatore. Infatti, seppure deciso in favore di B, tale caso è comunque dubbio quindi moduliamo il rapporto in base al numero di video di A.
 (Spiegare meglio discorso dei rapporti e dei numA ecc)


##### CMD Line Tool
`usage: foa [-cA <xml file>] [-cB <xml file>] [-h | -init | -trn | -tst |
       -ute | -utr] [-i <xml/txt file or folder>]  [-lA <txt/json file>]
       [-lB <txt/json file>] [-o <folder>]     [-v]
 -cA,--configA <xml file>              xml config file for class A, only
                                       for --test
 -cB,--configB <xml file>              xml config file for class B, only
                                       for --test
 -h,--help                             print help message
 -i,--input <xml/txt file or folder>   xml file or txt file with list of
                                       xml paths for which compute the
                                       likelihood for class A and B, only
                                       for --test, dataset folder path for
                                       --update
 -init,--initialize                    initialize database
 -lA,--listA <txt/json file>           text/json file containing a list of
                                       xml file for class A, only for
                                       --train
 -lB,--listB <txt/json file>           text/json file containing a list of
                                       xml file for class B, only for
                                       --train
 -o,--output <folder>                  output folder for the training
                                       config files, only for --train
 -trn,--train                          train a binary classification
                                       problem
 -tst,--test                           predict the class of a xml file
 -ute,--update-testing                 update testing database
 -utr,--update-training                update trainingdatabase
 -v,--verbose                          whether or not display information,
                                       only for --test
`
