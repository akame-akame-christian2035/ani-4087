
Tableau des cinq étapes, avec valeurs mesurées et sources
#	Étape	Valeur mesurée	Source	Qualité de la source
1	Capteur (interrupteur + antirebond + MCU)	0,2 ms d'actuation, antirebond nul (interrupteurs optiques)	Fiche produit Razer Viper V3 Pro, relayée par mousedpianalyzer.com	Constructeur, valeur isolée mais non vérifiée par un tiers
2	Transmission (USB / radio)	1000 Hz → 1 ms entre deux rapports ; 8000 Hz → 0,125 ms	Un taux de 1000 Hz signifie un rapport par milliseconde, soit 1 ms de délai ; à 8000 Hz on descend à 0,125 ms (Attack Shark)	Vendeur, mais c'est de l'arithmétique vérifiable
2b	idem, cas dégradé	Fréquence de scrutation USB : 125 Hz par défaut en low speed, 1000 Hz en full speed, modélisable par une loi uniforme sur [0, 8 ms] ou [0, 1 ms]	Brevet US 8 725 443 (modélisation de latence)	Solide, mais c'est un modèle, pas une mesure
2c	Bluetooth	8 à 20 ms annoncés	Attack Shark, qui attribue le chiffre aux tests RTINGS	Faible : citation de seconde main, je n'ai pas retrouvé la page RTINGS d'origine
3	Votre code (moteur : lecture de l'entrée → état du monde)	Introuvable isolément	NVIDIA définit la Game Latency mais ne publie aucun chiffre	—
4	Composition (GPU + compositeur)	Le compositeur coûte une trame entière : le DWM rend impossible de descendre sous une trame de latence ; avec une fenêtre plein écran sans fenêtre superposée, DWM saute la composition et supprime cette trame	James Darpinian, Input Latency: Platform-specific Considerations	Bonne : ingénieur graphique, explique le mécanisme
4b	idem, chiffré	1 trame ≈ 16,7 ms à 60 Hz, 6,9 ms à 144 Hz	Forum Blur Busters, mesure « faite il y a des années »	Faible : forum, sans protocole ni date
5	Affichage (scan-out + dalle)	Période de trame : une dalle 240 Hz se redessine toutes les 4,17 ms, une dalle 144 Hz toutes les 6,94 ms	KTC	Arithmétique, vérifiable
5b	Traitement interne du moniteur	Le retard d'affichage est le temps que met le moniteur à traiter le signal envoyé par la machine et à commencer à afficher l'image, mesuré à la photodiode	RTINGS, méthodologie Monitor Input Tests: Input Lag	Solide : labo, protocole publié
5c	Seuil de perception	RTINGS estime que le retard peut commencer à se remarquer vers 30 ms, et que 15–20 ms peuvent compter dans les jeux de réaction	itechguides, résumant RTINGS	Moyenne : relais, mais fidèle
Ce que je n'ai pas trouvé
La latence du moteur de jeu isolée (étape 3). Aucun labo ne la publie séparément : les instruments mesurent du bout en bout. NVIDIA nomme la catégorie sans la chiffrer.
Un chiffre récent et sourcé pour le compositeur. Tout le monde répète « une trame » ; les seuls nombres que j'ai trouvés (1 à 5 ms, 2 à 3 ms) viennent de blogs commerciaux sans protocole. Je ne les mets pas dans le tableau principal.
La latence du capteur optique seule. RTINGS mesure la latence de clic avec un analyseur de protocole USB, en moyennant 205 clics — ce qui fusionne le capteur et la transmission. On ne peut pas les séparer depuis l'extérieur. 
RTINGS
Le point de calibrage utile

Une mesure de bout en bout existe : avec le LDAT V2 de NVIDIA, qui mesure du clic à la réaction visible à l'écran, la Razer Viper 8K atteint une latence moyenne de 9,5 ms. 
igor´sLAB

Cela vaut la peine d'être rapproché du chapitre précédent. Les 8 ms qu'on soustrayait sont une chaîne optimale : périphérique 8000 Hz, plein écran exclusif, dalle rapide. 9,5 ms mesurés de bout en bout, cela confirme l'ordre de grandeur — mais uniquement dans ce cas idéal. Dès qu'on tombe sur du Bluetooth, du plein écran fenêtré et un moniteur bureautique, la chaîne fixe dépasse largement la trame et le budget de code devient négatif : l'image saute, quoi que fasse votre boucle.
