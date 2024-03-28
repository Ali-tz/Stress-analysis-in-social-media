### Introduction

Le stress est indissociable du quotidien de chacun.
Il peut être ressenti différemment chez chacun et est à l'origine de problèmes de santé mentale ou physique.
Il peut augmenter la pression artérielle, le risque de maladie cardiaque, des problèmes gastriques ou encore des maux de tête.
La numérisation du monde actuel donne accès à un flot d'informations continues et est un facteur de stress chez l'être humain.
Nous allons, dans notre cas, traiter de l'expression du stress dans les réseaux sociaux, en particulier nous allons nous pencher sur le réseau social Reddit, où les inscrits peuvent poster leurs pensées par catégories et sans limite de caractères.
Nous allons pour cela utiliser le dataset Dreaddit, tiré de Kaggle et issu de la recherche "[*Dreaddit: A Reddit Dataset for Stress Analysis in Social Media*]" par *Elsbeth Turcan et Kathleen McKeown* à *Columbia University*.

Dans un premier temps, nous récupérerons ces données, puis vérifierons que notre problème est bien équilibré.
Autrement dit, nous regarderons la répartition des personnes stressées ou non dans notre jeu de données.
Une fois cela fait, nous décrirons les variables qui composent ce dataset avant de les regarder plus en détail à travers une étude statistique.
L'objectif de cette étude est de comprendre le rôle de chacune des variables, leur corrélation et leur importance face au stress.
Cela nous permettra par la suite de mettre en place des modèles prédictifs nous permettant de prévoir le stress chez une personne et les facteurs importants face à cette prédiction.

#### Mise en place


### Description du dataset:

Notre dataset contient 116 colonnes.
Nous allons préciser la signification des données qui le nécessitent :

-   **Subreddit** : donne la nature du post.
    Les natures de posts possibles sont ptsd, assistance, finance, sans domicile, quasiment sans domicile, violence domestique, repas sociaux, stress, rescapé d'un abus.

-   **Post_id** : donne l'identifiant du post en question.

-   **Identifiant** : identifiant de la personne qui a posté.

-   **Label** : label du post, indique si oui ou non le post vient d'une situation de stress.
    Le label vaut 1 si oui, 0 dans le cas contraire.

-   **Confidence** : donne le pourcentage de personnes qui ont étudié le test qui sont d'accord avec le label attribué.

-   **Socialtimestamp** : donne le nombre de personnes ayant vu le post.

-   **Karma** : notion de Reddit qui donne plus ou moins d'importance à un utilisateur selon si ses posts sont appréciés de la communauté.

Ensuite, nous trouvons les variables contenant dans leur noms LIWC, Linguistic Inquiry and Word Count.
Le LIWC est une métrique permettant de donner un score pour des catégories psychologiques telles que la tristesse ou la négativité mentale.
Ce score est donné par un ratio ou un pourcentage.
Nous rééquilibrerons tout cela par la suite.

Nous avons également des variables contenant FK qui tient pour le Flesch-Kincaid Grade Level qui donne la lisibilité du texte considéreré.
Cette variables est exprimée en grade américain, autrement dit le niveau de scolarité des U.S.A. Plus le score est bas, plus le texte est facile à lire.

Nous trouvons également les notes moyennes, maximales et minimales pour l'agrément, l'activation et l'imagerie tirées du Dictionnaire de l'affect dans le langage (DAL) .
Ce dernier correspond à un ensemble de mots classés selon leur association avec des concept affectif.
Il permet d'analyser le contenu textuel en terme d'emotions et d'affect.

Enfin, l'horodatage UTC du message ; le ratio des votes positifs (upvotes) et négatifs (downvotes) sur le message, où un upvote correspond approximativement à une réaction de "like" et un downvote à une réaction de "dislike".

#### Problème équilibré ?

Nous allons à présent vérifier que notre problème est bien équilibré.

Dans un premier temps, regardons la répartion global de personne stressées et non stressées.



Autant de gens stressés que de gens non stressés.

Vérifions maintenant l'équilibre dans chaque sous catégories:



Nous savons que les subreddits sont des sous-catégories des quatre domaines suivants :

-   **Abus**, qui contient des survivants d'abus et de violence domestique

-   **Anxiété**, qui contient de l'anxiété et du stress

-   **Financier**, qui contient presque sans-abri, assistance, garde-manger alimentaire et sans-abri

-   **PTSD**, qui est le trouble de stress post-traumatique

-   **Social**, qui concerne les relations

Regardons les proportions des différents domaines.


Observons maintenant la proportions de gens stressés dans ces différents domaines.

**Proportion de personnes stressées parmi celles ayant subi des abus.**


**Proportion de personnes stressées parmi celles ayant déjà été anxieuse.**


**Proportion de personne stressées parmi celles ayant eu des problèmes financiers**


**Proportion de personnes stressées parmi celles ayant eu un trouble de stress post-traumatique**


**Proportion de personnes stressées parmi celles ayant eu une relation**


### Préparation des données



Très simplement, on enlève les colonnes "id", "subreddit", "post_id", "sentence_range" et "text" car elles n'ont aucun impact sur le fait d'être stressé ou non.
Les textes ayant déjà été traités via des variables quantitatives, nous n'avons plus besoin du texte brut qui est plus compliqué à analyser pour la machine.


### Exploration, statistiques descriptives

Nous allons maintenant réaliser une étude statistique pour mieux comprendre l'importance des variables de Dreaddit.
Cela nous permettra, d'une part, une meilleure analyse de nos résultats lors des prédictions, mais aussi d'éliminer toutes les variables inutiles et de limiter le surapprentissage de nos futurs modèles.

Donner trop d'informations à notre modèle peut entraîner du surapprentissage.
Or, nous voulons que nos modèles soient suffisamment souples pour fournir des prédictions précises, mais pas trop souples pour limiter les erreurs.

Cette analyse nous permet donc, dans cette optique, d'anticiper le problème du biais/variance d'un modèle de machine learning.
Pour déterminer l'importance de nos variables, nous allons réaliser une analyse factorielle, puis des tests de Student sur chacune de nos variables par rapport à la présence de stress ou non.
Enfin, nous réaliserons un modèle de forêt aléatoire et lui demanderons quelles variables lui ont semblé importantes.



Nous allons à présent réaliser une analyse PCA pour mieux comprendre l'impact relatif des variables sur le stress.
Avant d'appliquer la PCA, les données seront standardisées pour assurer que toutes les variables ont une influence égale sur les résultats.
L'analyse PCA, ou Analyse en Composantes Principales en français, cherche à projeter les données dans un sous-espace de dimensions réduites, généralement de 1 à 4.
Les composantes principales seront sélectionnées en se basant sur des critères tels que la variance expliquée cumulative, afin de garantir une représentation optimale des données.
Une fois cette projection effectuée, nous interpréterons les résultats en identifiant les variables qui contribuent le plus à chaque composante principale.
Cette analyse nous permettra de mieux comprendre comment les différentes variables influent sur le stress.
Les informations obtenues guideront nos décisions dans l'analyse des données et dans la construction de modèles prédictifs.


On remarque qu'en dimension 2, nous observons les niveaux de corrélation les plus élevés avec la variable cible 'label'.
Comparativement aux autres dimensions, ce sont les corrélations les plus fortes.
On peut en déduire que c'est dans cet espace que notre problème est le mieux représenté, et les variables qui décrivent le mieux notre situation sont : - lex_liwc_social (corrélation = 0.76007971) - lex_liwc_Clout (corrélation = 0.61209181) - lex_liwc_Tone (corrélation = 0.53214829) - lex_liwc_affiliation (corrélation = 0.50418845).


#### Test de student

Le test de Student permet de comparer les moyennes observées d'un échantillon à une valeur fixée, souvent une moyenne théorique ou une référence établie.
Il est aussi utilisé pour comparer les moyennes de deux échantillons distincts, nous permettant de déterminer si ces groupes diffèrent significativement.

Pour ces raisons, nous utiliserons le test de Student ici pour évaluer si les différences observées dans nos données sont statistiquement significatives par rapport à nos hypothèses ou à des valeurs de référence préétablies.


Notre test montre que 104 variables sur un total de 110 ont une p-value\<= 0.01 face à notre variable cible.On peut en déduire que ces variables ont un lien significatif avec la variable cible "label" et peuvent être des prédicteurs importants du stress.
Cela suggère que ces variables pourraient être utilisées efficacement dans la modélisation et la prédiction du stress chez les individus. Mais ce résultat est aussi prévisible en raison du grand nombre de données à notre disposition, faussant le test de student.

### Approche lasso

Nous allons ici utiliser la méthode de régression de Lasso.
Cette dernière permet de régulariser les coefficients beta du modèle de régression en introduisant un terme de pénalisation.
Ce terme de pénalisation contrôle la complexité du modèle en forçant certains coefficients à devenir exactement zéro, ce qui élimine ainsi certaines variables moins importantes.
En ajustant ce terme de pénalisation, nous pourrons déterminer les variables les plus significatives par rapport à notre problème.

Df (degrés de liberté) indique le nombre de coefficients non nuls dans le modèle pour chaque valeur de lambda.
Lorsque lambda augmente, le nombre de coefficients non nuls diminue généralement, ce qui indique que le modèle devient plus clairsemé.

Dev (Déviance expliquée) indique le pourcentage de déviance expliqué par le modèle pour chaque valeur lambda.
À mesure que lambda augmente, le pourcentage de déviance expliquée diminue généralement, ce qui suggère un compromis entre la complexité du modèle et la qualité de l'ajustement.

Lambda est le paramètre de pénalité qui contrôle le degré de régularisation dans le modèle Lasso.
Plus lambda augmente, plus la force de régularisation augmente, ce qui conduit à une plus grande réduction des coefficients et à des modèles potentiellement plus simples avec moins de prédicteurs.

On observe dans notre cas que la deviance et la liberté du modèle diminuent fortement avec l'augmentation du paramètre lambda.
On a un degré de liberté égal à 0 pour lambda valant 0.217700.
Nous 108 variables sur 110 avec un lambda de 0.000042.

Ces courbes tracent le chemin de régularisation du modèle Lasso.
Elles montrent comment les coefficients des variables changent lorsque le paramètre de pénalité (lambda) augmente.

### Approche random forest et importance des variables

Dans cette partie, nous allons lancer un modèle de forêt aléatoire.
Une fois ce dernier exécuté, nous récupérerons dans ses paramètres les variables qui, de son point de vue, ont été jugées importantes.



On souhaite ajuster de manière optimale les paramètres de complexité de la méthode considérée pour pouvoir éviter le surapprentissage lorss de l'entrainement.
Pour cela nous allons utiliser la validation croisée V-folds sur l’échantillon d’apprentissage.
On constate que les trois variables les plus importantes sont les variables 'negemo', 'clout' et 'i', bien que leur importance relative soit faible.
En revanche, pour les autres variables, on observe une importance relative très forte.

Une importance positive indique que des valeurs plus élevées de la variable sont associées à des résultats de prédiction plus élevés, tandis qu'une importance négative indique l'inverse.
Dans ce cas, nous avons une importance positive.
L'importance d'une variable peut être influencée par sa corrélation avec d'autres variables.

#### Conclusion de l'exploration statistique

Conclusions de l'exploration statistique : On a vu lors de l'analyse factorielle qu'en dimension 2, quatre variables se dégageaientt avec un taux de corrélation supérieur à 0.5.
A l'inverse l'analyse avec le test de student nous révèle que quasiment toutes les variables sont très liées à notre variable cible, donnant ainsi un avis contradictoir avec notre précédente analyse.
L'étude à l'aide du random forest vient préciser notre analyse et montre que dans le cas de ce modèle les variables sont toutes fortement corrélées entre elles.
Ainsi indépendemment elles donnent des résultats pauvres mais regroupés ils permettent d'obtenir des résultats plus précis.
Cela semble logique dans la mesure ou nous étudions un texte et ou certains mots, même s'ils ont une significations plus forte que d'autres, ont toujours besoin des autres pour pouvoir exprimer clairement leurs rôles et ce qu'ils décrivent.
Par la suite, nous utiliserons donc toutes les variables décritent précédement pour entraîner nos modèles.

### Comparaison de modèles

#### Random Forest

On reprend le résultat précédent du random forest pour voir ses performances.

Prédiction sur l'échantillon test:
"Matrice de confusion"
             
prediction_rf   0   1
            0 228 108
            1 118 261


Erreur de prédiction du modèle =  0.3160839


Matrice de confusion après le tunning
"Matrice de confusion"
                    
prediction_rf_tunned   0   1
                   0 236  67
                   1 110 302
                   
Erreur du modèle après le tunning = 0.2475524


Précision: 0.8184282 
Rappel: 0.7330097 
F1-score: 0.7733675 

#### Arbre de décision



#### Régression logistique
Matrice de confusion
"Matrice de confusion"
                     
prediction_log_binary   0   1
                    0 249  83
                    1  97 286

Erreur de prediction = 0.2517483

#### Tunning de la régression logistique
Matrice de confusion
"Matrice de confusion"
                           
prediction_log_tuned_binary   0   1
                          0 346 369

Erreur de prediction = 0.5160839
Précision: 0.4500745 
Rappel: 0.7330097 
F1-score: 0.5577101 


#### SVM

Par la suite, nous allons réaliser le tunning directement.



