

MDP :VPvVFqNKEnxo4aJr

Utiliser pour de la base de données

Optimiser temps d'exécution avec un Index

Mongo DB  est un système de base de données


AVANTAGES:

-pas de schéma

-stockage sur disques

-très facile de "scale"

-pas de jointure complexe


INCONVENIENT:

-On se retrouve très vite avec des informations dupliqué

-trop grande liberté pour un novice

Utilisé dans différents domaines:

-appli web

-data base

-data hub

Base de données orienté document

Document (def): est un ensemble de données clé-valeur

Stocker au format Json dans un conteneur est le doc n'a pas de schéma prédéfinis

Json : format universel, très répondu, n'impose pas de structure, les docs permettent de regrouper les informations

Schéma (def) :le schéma des documents est dit dynamiques

Deux documents peuvent contenir les mêmes champs

Champs : donné

Collection (def) : groupe de document équivalent d'une table dans un système de base de donnes. Les collections n'imposent pas de schémas précis.

Notation JSON

Les propriétés d'un objet JSON sont représenté par paires clefs/valeurs
```
json

{"cle": "valeur", "autre_cle" : ""}

{"nom": " ","prenom":" "}
```
Les types de données:

-boleen

-numériques

-chaine de caracteres

-tableau

-objet

-null

MongoDB apporteses propres types a ceux rencontrés au sein du format JSON standard:

-le type Date: entier signé de 8 octets represente le nombre de secondes depuis epoch (01/01/1970 00:00:00), le type date ne stocke pas la timezone.

-le type ObjectID stocke sur 10 octets, utile en interne afin de garantir l'unicité des identifiants auto-générés par la BDD.

-NumberLong et NumberInt : par défaut Mongo considére que toute valeur numérique est un nombre a virgule codé sur 8 octets.

-NombreDecimal : sur 16 octets, grande précision

-BinData : pour stocker des caractères qu'on ne peut pas encodé en Utf-8


#### Création Base de données:

On peut utiliser "use" pour se connecter a une BDD qui n'existe pas. l'insertion du premier document va entrainer la creation de la BDD

```
Bash

Use top
```



Vous pouvez effectuer la requete :

Vous obtenez un résultat du type:

Vous remarquez l'utilisation du mot clé  'db' il s'agit du mot clé quirenvoie vers la base de donné en cours d'utilisation.

La combinaison 'db.uneCollection' constitue ce qu'on appelle un namespace en mongoDB

Suppression d'une base de données :
```

usenaDB

Db.dropsDataBase()
```

Pour vérifier utiliser la commande :
```

show dbs
```

Afin de lister les base de données.

MongoDb met à disposition des commandes de type 'helper'

runCommand()
adminCommand()


pour créer une collection il suffit d'entrer 

```
db.creationCollection(
"maCollection",
{"collation": {
"locale": "fr",
} }
)
```


EXOBOOK
``` js

use sample_db//créé data base

db.createCollection("employees")/////créé la collection

 db.employees.insert({ "name": "John Doe", "age": 35, "job": "Manager", "salary": 80000 })////inseré dans collection

db.employees.find()////chercher dans collection

db.employees.find({age:{$gt: 33}})/// employé de plus de 33 ans ($lt pour moins de 33)

db.employees.find().sort({salary:-1})/// salaire par odre décroissant

db.employees.find({},{"name" :1,"job":1,"_id": 0})//cheercher seulement le nom et le job

db.employees.aggregate([{$group: {_id: "$job", count: { $sum: 1}}}])//nb employees par poste($ devant job pour variables)

db.employees.updateMany({job: "Developer"},{$set:{salary:80000}})// changer salaire dev
```

EXO
``` js

Exo 1:

db.salles.find({smac:true},{nom:1})// affiche les salles de type SMAC

Exo 2:

db.salles.find({capacite: {$gt:1000}},{nom:1})// affiche les salles avec une capacités de plus de 1000

Exo 3:

db.salles.find({"adresse.numero": {$exists:false}})// cherche dans les adresse ci le numéro est vide est affiche la salle ou il n'y a pas de numéro

Exo 4:

db.salles.find({avis: {$size:1}},{_id:1, nom:1})// affiche les salles avec leur identifiant qui ont 1 avis 

Exo 5:

db.salles.find({styles: "blues"}, {styles:1})// affiche tous les styles musicaux des salles qui ont du blues

Exo 6:

db.salles.find({"styles.0": "blues"}, {styles:1})//// affiche tous les styles musicaux des salles qui ont du blues en première position. premiere position= "styles.0"

Exo 7:

db.salles.find({ "adresse.codePostal": { $regex: /^84/ }, "capacite": { $lt: 500 } }, { "adresse.ville": 1,}) // ^84= commence par 84. affiche les salles avec un code postal commencent par 84 et qui on une capacité supérieur a 500 places.

Exo 8:

db.salles.find({$or: [{_id: {$mod: [2, 0]}}, {avis: {$exists: false}}]}, {_id: 1}) // Affiche les identifiants des salles qui ont un id pair et ou le champ avis est vide.

Exo 9:

db.salles.find({avis: {$elemMatch: {note: {$gte: 8, $lte: 10}}}}, {nom: 1})// affiche les salles dons au moins un des avis comporte une note entre 8 et 10

Exo 10:

db.salles.find({avis: {$elemMatch: {date: {$gt: new Date('2019-11-15')}}}}, {nom: 1})
//affiche les salles qui a un avais postérieure au 15/10/2019

Exo 11:

db.salles.find({$expr: {$gt: [{$multiply: [100, "$_id"]}, "$capacite"]}},{"nom": 1, "capacite": 1}) //affiche le nom est la capacité des salles dont le produit de la valeur de l'id par 100 est supérieur a la capacité. $expr va comparé les champs d'un meme document.

Exo 12 :




```


Index: un index répertorie les mots les plus souvent utilisés et important  pour les retrouver plus facilement au lieux d'aller les chercher dans toutes les pages. Mais l'index peut aussi ralentir la suppresion et la modification .

Exemple indexsimple:
``` js
db.collection.createIndex(<champ_et_type>), <options>)

db.personnes.createIndex({"age": -1})
```

il existe un moyen de consulter la liste des index d'une collection :
``` js
db.personnes.getIndexes()

```

pour supprimer un index il suffit d'effectuer la commande suivante:
``` js
db.personnes.dropIndex("age_1_")

db.personnes.createIndex({"age":-1}, {"name":"superNom"})

db.personnes.createIndex({"prenom": 1}, {"background": true})
```
