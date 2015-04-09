---
layout: post
title: Divvy data challenge 2
plugins: highlight
---

Deuxième partie du Divvy data challenge. Nous continuons à étudier les
données du [TD précédent](../tutorial9)

**Attention :** ce TD compte pour le contrôle continu. Vous devez
rédiger vos réponses dans un notebook IPython, et envoyer votre
fichier `.ipynb` par mail à la fin de la séance de TD. Pour chaque
question, votre feuille doit contenir le code permettant de justifier
votre réponse.

## Le serveur IPython

Pour éviter les problèmes de la dernière fois, on va mettre à
dispostion plusieurs serveurs IPython.

### Pour les ordinateurs personnels

Vous pouvez installer IPython et pandas directement sur votre
ordinateur. La distribution [Anaconda](http://continuum.io/downloads)
est la plus populaire pour faire cette installation.

Alternativement, nous mettons à dispostion une VM Virtualbox contenant
IPython et les bibliothèques nécessaires. Cette installation nécessite
au moins 2GB de mémoire vive et deux cores.

1. Télécharger la bibliothèque ici :
   <http://proust.prism.uvsq.fr/~dfl/ipython-appliance.zip> (500Mo)

2. Décompresser.

3. Faire double click sur le fichier `ipython-appliance.vbox`, ou
   ajouter ce fichier à Virtualbox avec le menu *« Machine → Add »*.

4. Lancer la machine et diriger votre browser (le browser de votre
   système, pas celui de la VM) à l'adresse
   <http://localhost:8888>. Les données du data challenge sont déjà
   installées sur le serveur.

Quelque solution que vous choisissiez, testez-là avant le TD.

### Pour tous

Deux serveurs IPython dans le cloud sont disponibles aux adresses

- <http://cloud.sagemath.com>,
- <http://defeo2-8888.terminal.com/>.

Choisissez celle qui marche mieux pour vous.

### Pour les cartables numériques

À faire en dernier recours, lorsque les autres solutions ne marchent
pas. Cela peut demander quelques minutes, à faire impérativement avec
une connexion cablée.

Installer Ipython en local avec la commande

	curl https://gist.githubusercontent.com/defeo/ef11f2d04a78b68e357e/raw/ef9a0dc9c87d3c1052b2a3ade9b63c7dcf370092/apt.sh | bin bash

Il vous sera demandé le mot de passe utilisateur (rappel: user).

Ensuite, lancer ipython avec la commande

~~~
ipython notebook
~~~


## Les données

Les données sont les mêmes que la semaine dernière. Vous pouvez les
télécharger avec la commande

~~~
git clone https://bitbucket.org/defeo/datachallenge-in202.git
~~~
{:.bash}

Ensuite les jeux de données peuvent être importés dans un notebook
IPython avec les commandes

~~~
import pandas as pd
stations  = pd.read_csv('datachallenge-in202/divvy-stations.csv',
                        parse_dates=['dateCreated'])
distances = pd.read_csv('datachallenge-in202/divvy-distances.csv')
trajets   = pd.read_hdf('datachallenge-in202/divvy-trips.h5', 'fixed')
meteo     = pd.read_csv('datachallenge-in202/meteo.csv',
                        parse_dates=['CST'])
~~~


## Analyse

### Croiser avec les données géographiques <small>(3 points)</small>

Publié vendredi.

### Pas de vie privée pour les vieux (et les menteurs) <small>(4 points)</small>

Publié vendredi.

### Croiser avec les données météo <small>(bonus)</small>

Comme pour les trajets, les données météo comportent une colonne `CST`
correspondant à la date de l'observation météo. Cette colonne est au
format date+heure, alors que nous avons besoin uniquement de la
date. Comme auparavant, on ajoute une colonne `date` au DataFrame :

~~~
meteo.set_index('CST', inplace=True, drop=False)
meteo['date'] = meteo.index.date
~~~

1. Fusionner par date les données météo avec les données sur les
   trajets.

1. Dessiner le nombre de trajets en fonction de la température moyenne.

1. Le diagramme précédent, même s'il donne une idée des températures
   préférées par les cyclistes, est biaisé par le fait que certaines
   températures arrivent plus fréquemment que d'autres.
   
   Dessiner le nombre moyen de trajets par jour en fonction de la
   température moyenne.

1. Dessiner le nombre total de trajets en fonction du jour.

On voit clairement que le nombre de trajets diminue avec l'arrivée de
l'hiver. Par contre, il est plus difficile d'expliquer les variations
jour par jour, avec des différences qui peuvent atteindre le 30% des
trajets journaliers. Nous allons essayer de corréler cette donnée avec
les données météo.

Lisez comment
[utiliser directement matplotlib](http://pandas.pydata.org/pandas-docs/stable/visualization.html#plotting-directly-with-matplotlib)
sans passer par la méthode `.plot` de pandas. Ceci vous permettra de
superposer plusieurs graphes.

1. Reportez sur le même diagramme le nombre de trajets par jour et la
   température moyenne de la journée. Vous devrez normaliser ces
   valeurs (les rapporter à l'intervalle [-1,1], par exemple) pour
   pouvoir les comparer visuellement.

1. La colonne `Events` signale les jours où il a plu ou
   neigé. Reportez sur le même diagramme un point (utiliser un
   *scatter plot*) pour chaque jour où il y a eu un événement météo.

1. Dessiner la vitesse moyenne des cyclistes en fonction de la vitesse
   moyenne du vent. Comparer avec la déviation standard : observez
   vous une dépendance significative ?


### Autant emporte le vent (<small>bonus ultime, l'an prochain vous faites le cours à ma place</small>)

La colonne `WindDirDegrees` contient la direction du vent en degrés
(entre 0° et 359°), le nord étant à 0°, et l'est à 90°. On veut
déterminer comment la force du vent impacter la vitesse des
cyclistes.

- Pour chaque jour, utiliser les données géographiques pour diviser
  les trajets en trois catégories : ceux qui ont le vent derrière,
  ceux qui ont le vent de face, et ceux qui ont le vent de côté.

- Pour chacune de ces trois catégories, dessiner la vitesse moyenne en
  fonction de la vitesse du vent.

{: start="5" }