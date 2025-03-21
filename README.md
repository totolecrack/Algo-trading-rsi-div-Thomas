📌 Trading Algorithmique avec Pine Script sur TradingView Pour développer ces algorithmes de trading, j'ai utilisé l'éditeur Pine Script de TradingView, ainsi que son testeur de stratégie pour analyser et optimiser les performances de mes modèles.

Démarche générale : 

Mon objectif était de reproduire un algorithme basé sur ma manière habituelle de trader. Je trade généralement en repérant des divergences RSI sur des niveaux clés que j’identifie en amont, comme les retracements de Fibonacci, les zones de demande et d’offre, les order blocks ou les trendlines. Quand le prix atteint ces niveaux, j’analyse le RSI sur différentes timeframes : s’il y a une divergence, je prends un trade ; sinon, je passe.
Pour commencer, j’ai créé un indicateur de divergences haussières (bull div) quand le RSI est en survente (oversold). J’ai ensuite testé une stratégie simple basée uniquement sur ces divergences. Après backtesting, elle s’est révélée plutôt rentable avec un ratio risque/rendement (RR) de 1:1, utilisant un TP et un SL fixes de 2 %. Pour l’optimiser, j’ai cherché à ajouter des confluences et à améliorer la gestion des trades.
Pour identifier des niveaux clés automatiquement, j’ai intégré les Points of Interest (POI), un indicateur que j’avais utilisé auparavant. Les POI sont calculés comme le midpoint entre le plus haut et le plus bas de la session de New York (11h-13h30), une période de forte volatilité et d’intérêt pour le marché. J’ai aussi testé un filtre EMA, mais je l’ai retiré car il n’apportait pas de valeur ajoutée. À la place, j’ai ajouté un filtre de volatilité basé sur l’ATR (Average True Range) pour éviter les cascades de liquidation et les sorties prématurées dues à des mouvements brusques.
Enfin, j’ai ajusté la gestion des TP et SL. Pour les Longs, j’ai remarqué (notamment sur Bitcoin) que la volatilité influençait beaucoup mes résultats, alors j’ai utilisé un TP et un SL dynamiques basés sur 2x l’ATR. Pour les Shorts, la volatilité avait moins d’impact, donc j’ai opté pour un TP et un SL fixes à 5 %, ce qui restait efficace après tests.



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

