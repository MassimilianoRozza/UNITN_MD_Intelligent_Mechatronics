---
tags:
  - BORTOLUZZI
data: 2026-09-23
handwriting-page-id: 3c64a1cb-3729-44fe-b99c-61d1a448e357
handwriting-paper: none
---
# Introduzione
**Dynamic**: qualcosa con evolizione nel tempo
**Vibration**: oscillazione, esistenza di una o più configurazioni privilegiate descritte da **ampiezza e frequenza**
	- immagazzina energia in forme differente (almeno due es. potenziale e cinetica)
	- non solo meccania ma anche elettrica, biologica, ...
Studio vibrazioni con il metodo del System engeneering:
![[System_input_and_output.webp]]
## Come analizzarememo la dinamica? 
Necessaria la costruzione di un modello matematico:
- sempre ideale
- più modelli con complessità differente, differenti dynamics in base al livello di dettaglio richiesto. 
Per proporre un modello vanno risptettate alcuni criteri:
- as simple as possible (rasoio di Hoccam) tralasciando paramentri non significativi.
	- ma come si definiscono i parametri significativi?
- keeping in mind that the purpose of the object must be granted by the analysis
### Ex.
To understand the different result of different DOF model here's an example of a ion motor (or something similar) where a mass, hanged on a rope, is hit with a laser beam and an interferometer measure the displacement.
![[Model_pendulum_with_laser_motor_and_interferometer.webp|305]]
The model will present a oscillating motion destribed in the following graph:
















Object dynamic described by **simple pendulum** simplify in mass and a rope:










l: lunghezza della fune
m: massa
$\theta$: angolo di spostamento
sistema a 1 grado di librertà: 1DOF
Se si misura il vero moto ci si accorge che presenza oscillazini a frequenza superiore a quella prevista -> devo aumentare i DOF considerati.








Allora considero il corpo come corpo rigido e non parametri concentrati:







Potrebbe essere ancora necessario aumenteare il numero di DOF considerando ad esempio la deformazione della fune:





dato che la fune è fina e flessibile non possiamo calcolare la sua dinamica con approcci discreti ma dobbiamo trattarla come un insieme di infinitesimi segmenti ognuno descritto dalla proprio scostamento angolare rispetto alla fune rigida con dipendenza anche dalla posizione del punto della fune che stiamo considerando portando ad avere infiniti gradi di libertà.
Se non raggiungiamo ancora un'accuratezza del modello sufficiente forse la il corpo non è rigido come ci stamo aspettando:





Quindi abbiamo:
- modello a parametri concentrati (lumped) DOF finiti,  ordinary differential equations
- distributed parameters systems, inf DOF, partial derivatives diff. equaztions
Il modello può essere:
- lineare: variabili dipendenti e le loro derivate compaiono nell'equazione differenziale solo alla prima potenza e senza prodotti misti
Risposta può essre:
- deterministic
- random










## Pogramma del corso
1. 1 DOF systems
2. n-DOF systems
3. continuous systems
4. non-linear systems
5. random













