---
tags:
  - BORTOLUZZI
data: 2026-09-23
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
![[Example_oscillation.webp|433]]
Object dynamic can be described by a **simple pendulum** composed by a mass and a rope:
![[Pendolo_parametri_concentrati.webp]]
The parametriameters are:
- l: lunghezza della fune
- m: massa
- $\theta$: angolo di spostamento
This system has 1 DOF: the $\theta$
Se si misura il vero moto ci si accorge che presenza oscillazini a frequenza superiore a quella prevista sovrapposti a quella semplice del pendolo
![[moto_pendolo+dimple.webp]]
devo aumentare i DOF considerati.
![[2DOF_pendulum.webp]]
Repeting the analysis, more unexpected oscillation can be noted, another DOF must be consider:
![[Inf_DOF_pendulum.webp]]
In this case the rope isn't consider as rigid but with her own ocsillating motion. This approach deny the possibility to study the motion with discrete approaches but requires to study the rope as a set of infinitesimal segment, each described by its own angular displacement compared to the rope. This case can have up to $\infty$ DOF.
If the required accuracy has not been reached yet, we can stop considering the body as rigid but with his own elasticity.
![[inf+1DOF_pendulum.webp]]
At this point  we have:
- **lumped parameter model:** has finite number of DOF, described by ordinary differential equations.
- **distributed parameter model**: has $\infty$ DOF, described by partial differential equations.
The model can be:
- linear: dependent variables and their derivatives appear only with exponential $1$ and without mixed product.
The response can be:
- deterministic: if described by a functoin $f(t)$
- random: can't be described by a function. But after the data collection we can create a function that describe the datas.
![[Graph I don't know what means.webp]]