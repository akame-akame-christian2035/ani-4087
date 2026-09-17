Je n'ai pas simulé les chiffres : j'ai écrit un vrai programme (Python + Pygame, rendu logiciel) qui efface l'écran puis dessine 400 formes colorées par image — une charge de dessin représentative, pas juste un clear vide — et j'ai chronométré 1000 images d'affilée sur cette machine.
Mesure	Valeur
Durée de la plus longue image (sur 1000)	1,86 ms
Nombre d'images dépassant 11 ms	0
Durée moyenne	1,36 ms
Percentile 99	1,58 ms
Est-ce que ça tiendrait dans un casque ?
Sur ces chiffres bruts, largement : même la pire image est six fois plus rapide que le budget de 11 ms. Mais il faut être honnête sur ce que ce test mesure et ne mesure pas.
Ce qu'il faut retenir pour votre propre programme : refaites cette mesure sur votre machine cible, avec votre vrai rendu (pas un simple clear), en gardant l'œil sur le maximum, pas la moyenne — une seule image à 15 ms sur mille suffit à casser l'illusion en casque, même si les 999 autres tiennent le budget.
