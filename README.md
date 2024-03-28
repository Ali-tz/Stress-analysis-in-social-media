### Introduction

Le stress est indissociable du quotidien de chacun.
Il peut être ressenti différemment chez chacun et est à l'origine de problèmes de santé mentale ou physique.
Il peut augmenter la pression artérielle, le risque de maladie cardiaque, des problèmes gastriques ou encore des maux de tête.
La numérisation du monde actuel donne accès à un flot d'informations continues et est un facteur de stress chez l'être humain.
Nous allons, dans notre cas, traiter de l'expression du stress dans les réseaux sociaux, en particulier nous allons nous pencher sur le réseau social Reddit, où les inscrits peuvent poster leurs pensées par catégories et sans limite de caractères.
Nous allons pour cela utiliser le dataset Dreaddit, tiré de Kaggle et issu de la recherche "[*Dreaddit: A Reddit Dataset for Stress Analysis in Social Media*]{.underline}" par *Elsbeth Turcan et Kathleen McKeown* à *Columbia University*.

Dans un premier temps, nous récupérerons ces données, puis vérifierons que notre problème est bien équilibré.
Autrement dit, nous regarderons la répartition des personnes stressées ou non dans notre jeu de données.
Une fois cela fait, nous décrirons les variables qui composent ce dataset avant de les regarder plus en détail à travers une étude statistique.
L'objectif de cette étude est de comprendre le rôle de chacune des variables, leur corrélation et leur importance face au stress.
Cela nous permettra par la suite de mettre en place des modèles prédictifs nous permettant de prévoir le stress chez une personne et les facteurs importants face à cette prédiction.

```R
# Set the working directory to where your CSV file is located
setwd("C:/Users/alito/OneDrive/Documents/Moi/IMTAtlantique/A3/DataSante/Projet")

# Load the CSV file into a data frame
dreaddit_train <- read.csv("data/dreaddit-train.csv")
dreaddit_test <- read.csv("data/dreaddit-test.csv")

# View the first few rows of the data frame
head(dreaddit_train)

```
