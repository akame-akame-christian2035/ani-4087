
La mesure : rendu seul, sans la logique
	Rendu seul
Durée moyenne	0,72 ms
Plus longue image	1,34 ms
Percentile 99	0,90 ms
Estimation : ce rendu fait deux fois (un par œil)
	Rendu ×2 (stéréo)
Moyenne	1,45 ms
Pire cas	2,68 ms
Ce qu'il resterait pour le reste
En reprenant le budget de 11 ms retenu au chapitre 8 (cas médian autour de 90 Hz, chaîne fixe de 8 ms déjà soustraite) :
Sur la moyenne : 11 − 1,45 = 9,55 ms restants pour la logique de jeu, la physique, l'IA, le réseau, l'audio, etc.
Sur le pire cas mesuré : 11 − 2,68 = 8,32 ms restants.
Conclusion : 
Sur cette machine, dans ces conditions synthétiques (pas de vrai GPU, pas de charge concurrente), le rendu seul — même doublé pour la stéréo — ne mange qu'une petite fraction du budget de 11 ms. Le goulot n'est pas le rendu, c'est visiblement tout ce qui n'est pas le rendu : dans ce petit programme, la logique (la génération des 400 formes) coûtait presque autant que le rendu lui-même, alors qu'elle devrait normalement être l'un des postes les moins chers d'une frame.

