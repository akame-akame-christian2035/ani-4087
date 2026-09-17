Durée d'une image

72 Hz → 1000 / 72 = 13,9 ms
90 Hz → 1000 / 90 = 11,1 ms
120 Hz → 1000 / 120 = 8,3 ms

Ce qu'il reste après les 8 ms de chaîne (capteurs, transmission, composition, affichage)

72 Hz → 13,9 − 8 = 5,9 ms
90 Hz → 11,1 − 8 = 3,1 ms
120 Hz → 8,3 − 8 = 0,3 ms

Les trois nombres à retenir pour le chapitre 10 : 5,9 / 3,1 / 0,3 ms.

À 120 Hz, il ne reste pratiquement rien : 0,3 ms de budget pour tout votre code par image. Autrement dit, la chaîne fixe de 8 ms consomme à elle seule 96 % de la trame, et le moindre dépassement fait sauter l'image.
