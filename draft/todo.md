# Forensic analysis of video file format

## INDEX

#### Abstract

Descrivere lo scope della tesi e i suoi obbiettivi

#### Introduzione

- parlare di related works, etif e l'utilizzo dei metadati, le debolezze etc. introdurre brevemente i container e spiegare come questi metadati siano differenti e più utili.

- Spiegare obbiettivi tesi, introdurre fase di training e testing.

- ho un video e il dispositivo sorgente -> faccio compare
- ho un video ma non so sorgente -> faccio source classification (manual se ho un'idea, auto se non ce l'ho)

#### Stato dell'arte

- introduzione dettagliata ai video file container, usare articolo e tesi triennale

#### Implementazione

- vft, video format tool restyle, spiegare nel dettaglio come funziona, le funzioni principali, come funziona il programma da command line, spiegare dettagli implementativi (numeri ai tag, attributi decisi da libreria)

- foa, format origin analysis, spiegare come funziona la fase di training e testing (teoria) e poi spiegare come è implementata e come funziona il programma da command line. Spiegare dettagli implementativi (non considero alcuni tag e alcuni attributi, entropia, come calcolo ratio, numA e numB con 0/1 etc)

- webapp, spiegare con che tecnologia è sviluppato e quale è il suo scopo, classify, compare, test, vft-parse, spiegare manual e auto come selezione la classe avversaria, spiegare problema del parlante

#### Test

- spiegare i tipi di test fatti, che dataset è stato usato e come è stato creato, risultati, commento ai risultati

#### Conclusione

- problemi, cose su cui si può migliorare o indagare, possibili applicazioni e sviluppi etc.
