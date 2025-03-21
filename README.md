📌 Trading Algorithmique avec Pine Script sur TradingView Pour développer ces algorithmes de trading, j'ai utilisé l'éditeur Pine Script de TradingView, ainsi que son testeur de stratégie pour analyser et optimiser les performances de mes modèles.

Mes algorithmes reposent sur un indicateur personnalisé que j'ai conçu, en complément d'autres indicateurs techniques afin d'améliorer la prise de décision en trading.

Objectifs du projet : ✅ Détecter des opportunités d'achat et de vente basées sur des conditions précises. ✅ Tester et valider les stratégies grâce à l'historique des prix. ✅ Optimiser les paramètres pour maximiser le rendement et minimiser les risques.

Résumé rapide de la stratégie : 

RSI : Cherche une divergence (hausse RSI + baisse prix pour Long ; baisse RSI + hausse prix pour Short).

POI : Confirme avec un niveau retouché récemment.

ATR : Filtre la volatilité et fixe SL/TP pour Long.

Trade activé : Si divergence + POI + volatilité OK + pas de position.

SL/TP : 2x ATR pour Long, 5% pour Short.

Démarche globale : Comment tout fonctionne
1. Indicateurs utilisés
RSI (Relative Strength Index) : Mesure le momentum sur 14 périodes.
Sous 30 = survente (signal Long possible).

Au-dessus de 70 = surachat (signal Short possible).

ATR (Average True Range) : Mesure la volatilité sur 14 périodes.
Utilisé pour définir SL/TP des Longs et filtrer les marchés trop agités.

POI (Point of Interest) : Niveau clé calculé comme la moyenne entre le plus haut et le plus bas dans une fenêtre horaire (11h-13h30).
Sert à confirmer les signaux RSI.

2. Paramètres clés
RSI :
Seuil Long : 30.

Seuil Short : 70.

Différence minimale : 2% (RSI) et 0.5% (prix) pour valider une divergence.

POI :
Fenêtre : 11h-13h30 (UTC-5).

Délai activation : 870 min après la fin.

Fenêtre retouche : 10 barres.

Trades :
Long : SL/TP = 2x ATR.

Short : SL/TP = 5% au-dessus/en dessous.

Volatilité max : 3x moyenne ATR.

3. Conditions pour activer un trade
Un trade s’active si toutes ces étapes sont validées :
Pour un Long (achat) :
Divergence haussière RSI :
RSI sous 30 (survente).

Premier creux : RSI bas + prix bas.

Deuxième creux : RSI plus haut mais prix plus bas (différence > 2% RSI, 0.5% prix).

RSI croise à la hausse (signal haussier).

Confirmation POI :
Un POI (niveau clé) a été retouché dans les 10 dernières barres.

Filtre volatilité :
Volatilité actuelle < 3x moyenne ATR (marché pas trop agité).

Pas de position :
Aucune position ouverte.

Pour un Short (vente) :
Divergence baissière RSI :
RSI au-dessus de 70 (surachat).

Premier sommet : RSI haut + prix haut.

Deuxième sommet : RSI plus bas mais prix plus haut (différence > 2% RSI, 0.5% prix).

RSI croise à la baisse (signal baissier).

Confirmation POI :
Un POI retouché dans les 10 dernières barres.

Filtre volatilité :
Volatilité < 3x moyenne ATR.

Pas de position :
Aucune position ouverte.

4. Gestion SL/TP (Stop Loss / Take Profit)
Une fois le trade activé :
Long :
SL : Prix d’entrée - (2 x ATR).

TP : Prix d’entrée + (2 x ATR).

Exemple : Si entrée à 100 et ATR = 1, SL = 98, TP = 102.

Short :
SL : Prix d’entrée + 5%.

TP : Prix d’entrée - 5%.

Exemple : Si entrée à 100, SL = 105, TP = 95.

Le trade se ferme automatiquement si le prix touche le SL ou le TP.

