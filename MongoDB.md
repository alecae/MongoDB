

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

```json
Bash

Use top
```



Vous pouvez effectuer la requete :

Vous obtenez un résultat du type:

Vous remarquez l'utilisation du mot clé  'db' il s'agit du mot clé quirenvoie vers la base de donné en cours d'utilisation.

La combinaison 'db.uneCollection' constitue ce qu'on appelle un namespace en mongoDB

Suppression d'une base de données :
```json
Db.dropsDataBase()
```

Pour vérifier utiliser la commande :
```json
show dbs
```

Afin de lister les base de données.

MongoDb met à disposition des commandes de type 'helper'

runCommand()
adminCommand()


pour créer une collection il suffit d'entrer 

```json
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

db.salles.find({ smac: true, $where: "this.styles.length > 2"}, { nom: 1, _id: 0 })//affiche le noms des salles de tyeps smac qui programment plus de 2 types de musiques différents

Exo 13 :  

db.salles.distinct("adresse.codePostal")//affiche les différents codes postaux. distincts sert a ne pas mettre plusieurs fois le meme code postal

Exo 14 :

db.salles.updateMany({}, {$inc: {capacite: 100}})//incrémente 100 a la capacité de chaque salles

Exo 15 :

db.salles.updateMany({styles: {$exists: false}}, {$push: {styles: "jazz"}})//ajoute style jazz au salles qui n'en font pas 


Exo 16 :

db.salles.updateMany({_id: {$nin: [2, 3]}}, {$pull: {"styles": "funk"}});// retire le style funk aus salles qui ont un identifiant different a 2 et 3. $nin= diférent

Exo 17 : 

db.salles.update( { _id: 3 }, { $push: { styles: { $each: ["techno", "reggae"] } } } )//trouve la salle avec comment identifiant 3 est ajoute les styles techno et reaggae

Exo 18 :

db.salles.updateMany({nom: /^P/i}, {$inc: {capacite: 150}, $set: {contact: [{telephone: "04 11 94 00 10"}]}})//LEs salles qui commence par P (i= majuscule ou minuscules) alors augmenter capacite de 150 et on ajoute un champ de typa tableau(contact) dans lequel on met le champ telephone avec une valeur predefinis

Exo 19 :

db.salles.update({nom: {$regex: '[^aeiou]+$'}}, {$push: {avis: {date: new Date(), note: NumberInt(10)}}})//Met a jour les salles qui commence par une voyelle est rajoute dans le tableau avis un doc composé du champ date est note qui vaux 10. elle cherche aussi une chaine de caracterequi débute par une voyelle suivi de [^aeiou]+$

Exo 20 :

db.salles.updateMany({nom: {$regex: /^[zZ]/ } }, {$set: {nom: "Pub Z", capacite: 50, smac: false}}, {upsert: true})

//upsert= met a jour ci il existe et le créé ci il existe pas. Met a jour les docs dont le nom commence par z ou Z et leur affecte Pub Z, une capacité de 50 et le champ smac la valeur false.

Exo 21 :



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

les opérateurs de tableaux
``` js
{$push:{ <champ>: valeur,...}}
```

L'opérateur "push" permet d'ajouter une ou plusieurs valeurs au sein d'un tableau

```js
db.hobbies.updateOne({"_id": 1}, {$push : {"passions": "le roller"}})

db.hobbies.updateOne({"_id": 2}, {$addToSet: {"passions":{$each: ["Minecreaft", "Rise of Kingdom"]}}})


db.personnes.find({"interets": {$all: ["bridge","jardinage" ]}})// rechercher tous ceux qui ont jardinage et bridge 

db.personnes.find({interets: {$size:2}})//affiche les personnes avec 2 hobbies differents

db.personnes.find({"interets.1": {$exists: 1}})//affiche les personnes avec 2 interets
```

Exo Index
```js
db.salles.createIndex({"adresse.codePostal":1 , "capacite":1})// on créé l'index pour les champs "adresse.codePostal" et "capacite" 

db.salles.dropIndex({"adresse.codePostal":1, "capacite":1})// Ceci va détruire l'index que nous avons fait juste au dessus 
```

Exo validation
```js
Exo 1:

db.runCommand({collMod: "salles", validator: { $jsonSchema: { bsonType: "object", required: ["nom", "capacite", "adresse.ville", "adresse.codePostal"], properties: { nom: { bsonType: "string", description: "Doit être de type chaine de caractères et il est obligatoire" }, capacite: { bsonType: "int", description: "Doit être un int et est obligatoire" }, adresse: { bsonType: "object", required: ["ville", "codePostal"], properties: { ville: { bsonType: "string", description: "Doit être de type chaine de caractères et il est obligatoire" }, codePostal: { bsonType: "string", description: "Doit être de type chaine de caractères et il est obligatoire" } } } } } } })

//La tentative d'insertion ne marche pas parce qu'il n'y a pas le code postal dedans.il faudrait faire cela pour que sa marche :

db.salles.insertOne({"nom": "Super salle", "capacite": 1500, "adresse":{"ville": "Musiqueville", "codePostal": "01600"}})

Exo 2:

//Si on tente de le mettre à jour il va y avoir une erreur de validation car le champs verifie ne fait pas parti du schéma de validation.

db.runCommand({ collMod: "salles", validationLevel: "off" });//Ceci va supprimer les criteres ajoutés

Exo 3:

db.runCommand({ collMod: "salles", validationLevel: "strict", validator: { $or: [ {smac: {$exists: true}}, {stylesMusicaux: {$in: ["jazz", "soul", "funk", "blues"]}} ] } })// permet de rajouter au critere de validation : champs smac présent OU les styles musicaux doivent figurer parmi les suivants 




```


Les requetes geospatiales
```json
{type <type d'objet GeoJSON>, coordinates: <coordonnes>}
```

Le type Point
```json
{"type": "Point","coordinates": [18.0, 1.0]}
```

le type Multipoint
```json
{"type": "Multipoint", "coordinate": [13.0, 1.0], [13.0, 3.0]}
```

Le type LineString
```json
{"type": "LineString", "coordinate": [13.0, 1.0], [13.0, 3.0]}
```

Le type Polyon
```json
{"type": "Polyon", "coordinate": [[[13.0, 1.0] ,[13.0, 3.0]], [[13.0, 1.0], [13.0, 3.0]]]
```

Création d'index :

```json
db.avignon.createIndex({"localisation" : "2dsphere"})

db.avignon.createIndex({"localisation" : "2d"})
```

L'opérateur $nearSphere :

```json
{
    $nearSphere : {
        $geometry : {
            type : "Point",
            coordinates : [<longitude>, <latitude>]
        },
        $minDistance : <distance en mètres>
        $maxDistance : <distance en mètre>
    }
}

var opera = { type : "Point", coordinates : [43.949749, 4.805325]}
```

Effectuer une requete sur la collection avignon

```json
var opera = { type : "Point", coordinates : [43.949749, 4.805325]}
    db.avignon.find({"localisation" : {$nearSphere : { $geometry : opera}}}, {"_id" : 0, "nom" : 1}).explain()
```

Exo GEO:

```json
var requete = { "adresses.ville": "Nîmes", "musiquesProgrammees": { $in: ["Blues", "Soul"] }, "adresses.location": { $geoWithin: { $centerSphere: [ [salles.adresses.location.coordinates[0], salles.adresses.location.coordinates[1]], KilometresEnRadians(60) ] } } }; db.salles.find(requete, { "nom": 1 });

```

Le framework d'agregation : Mongodb met a disposition un puissant outil d'analyse et de traitement de l'information : le pipeline d'agregation(ou framework) Metaphore du tapis roulant d'usine / traitement du document telle le tapis roulant Methode utilisée :

```
db.collection.aggregate(pipeline, options)
```

pipeline = designe un tableau d'etapes options = designe un document Parmis les options, nous retiendrons : -collation, permet d'affecter une collation a l'operation d'agregation -bypassDocumentValidation : fonctionne avec un opérateur appelé $out et permet de passer au travers de la validation des docs. -allowDiskuse : donne la possibilité de faire deborder les operations d'ecriture sur le disque. Vous pouvez appeler aggregate sans argument :

```
db.personnes.aggregate()
```

Au sein du shell, nous allons créer une variable pipeline :

```js
var pipeline = {}
db.personnes.aggregate(pipeline)
db.personnes.aggregate(
pipeline,
{
    "collation" : {
        "locale" : "fr"
    }
}
)
```

Le filtrage avec $match : Cela permet d'obtenr des pipelines performants avec des temps d'execution courts. Normalement $match doit intervenir le plus en amont possible dans le pipeline car $match agit comme un filtre en reduisant le nombre de document a traiter plus en aval dans le pipeline. (Dans l'ideal on devrait le trouver comme premier operateur) La syntaxe est la suivante :

```
    {$match : {<requete>}}
```

```
var pipeline = [{
    $match : {
        "interets" : "jardinage"
    }
}
]
db.personnes.aggregate(pipeline)
```

Cela correspond à la requete

```
db.personnes.find({"interets" : "jardinage"})
```


![[Pasted image 20230201151506.png]]