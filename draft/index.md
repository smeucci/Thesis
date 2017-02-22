## INDEX

## 1. Introduction

##### 1.1. Introduction to Multimedia Forensics

- Diffusione dei video digitali e loro utilizzo come potenziale fonte di prova.
- Autenticità e integrità (Piva Survey, 2014-ICCT.pdf).

##### 1.2. Forensic Analysis of Video File Format

- Tracce nel container (VideoFormantAn.pdf).
- Applicabilità per valutare l'integrità di un filmato o più in generale la sua provenienza (ultimo container).


## 2. Implementation and Applications

##### 2.1. Forensic Applications of Video Container analysis

- Introduzione sui vari formati video (ISO?).
- Spiegare come hai implementato l'analisi del container, quali peculiarità e limiti ha (è estendibile?, come gestisce i contenitor inattesi?, ...)
- Applications:
 - Integrity Verification.
 - Source Identification (Brand/Model).
- Introduzione all'interfaccia.

## 3. Experiments

##### 3.1. Dataset

- Spiegare come è stato acquisito il dataset e da quali video è composto.

##### 3.2. Experimental Validation

1. Integrity Analysis based on File Container: applicazione confronto diretto tra due alberi (Query VS Ref)
 - TEST: Matching case (video nativi) VS minimal processed video (e.g. avidemux, ffmpeg, Adobe Premiere).
2. Model Identification based onf File Container: tua applicazione per identificare il modello del device sorgente.
 - TEST 1. Available Ref Device: comparison between matching and mismatching.
 - TEST 2. Unavailable Ref Device: recursive (iterative) search with adaptive training.
3. Application on Social Network: applicabilità della tua tecnica fuori dall'ambiente di laboratorio.
 - TEST 1: Facebook VS NON-Facebook (native + other social media).
 - TEST 2: YouTube VS NON-YouTube (native + other social media).
