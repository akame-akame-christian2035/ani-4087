
Le déplacement apparent (la parallaxe) suit à peu près :

écart ≈ écart pupillaire × (1/d_doigt − 1/d_mur)

Avec un écart pupillaire de 65 mm (votre propre mesure du chapitre précédent) et un mur à 2 m :

Distance du doigt	Écart attendu sur le mur
30 cm	≈ 65 × (1/0,30 − 1/2) ≈ 55 cm
1 m	≈ 65 × (1/1 − 1/2) ≈ 3,3 cm
3 m	doigt plus loin que le mur : parallaxe s'inverse, mais reste petite (≈ −2 cm)

Rendez vos trois valeurs mesurées, elles ne colleront pas exactement à ce calcul — votre mur n'est pas à 2 m, votre œil directeur biaise la mesure, et l'estimation à l'œil d'un déplacement sur un mur texturé est grossière. C'est la forme du résultat qui compte, pas le chiffre : le déplacement doit s'effondrer très vite entre 30 cm et 1 m, puis devenir presque imperceptible à 3 m.

Ce que ça annonce pour le chapitre 9

La parallaxe binoculaire varie en 1/distance, pas linéairement. Elle est énorme tout près du visage et s'écrase déjà aux alentours du bras tendu. Concrètement, pour tout système qui simule ou exploite la vision stéréoscopique (rendu VR à deux caméras, calcul de convergence, effets de profondeur par disparité) :

Le signal de profondeur par disparité est utile seulement à courte distance (grosso modo jusqu'à 1-2 m). Au-delà, la disparité entre les deux yeux est trop faible pour porter de l'information — le cerveau, et donc votre moteur de rendu, doit s'appuyer sur d'autres indices (occlusion, taille relative, parallaxe de mouvement) pour juger la profondeur des objets lointains.
Séparer les deux caméras virtuelles de l'écart pupillaire n'a d'effet visible que sur le premier plan. Si le chapitre 9 porte sur le rendu stéréo ou la VR, c'est la justification chiffrée de pourquoi on optimise l'effort de calcul de disparité sur les objets proches et pourquoi le confort visuel se dégrade surtout dans les scènes à faible distance de convergence (armes en vue subjective, objets tenus en main), pas dans les paysages lointains.
