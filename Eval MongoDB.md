
Evaluation:

filtre dataset resto: restaurant français végétarien

Cette requête a été utilisée pour importer la base de données dans Atlas:

```
mongoimport --db EvalDB --collection resto --file restaurant.json --jsonArray mongodb+srv://Alex:VPvVFqNKEnxo4aJr@cluster0.vavbcfc.mongodb.net/?retryWrites=true
```

A. Rechercher les restaurants qui sont ouverts à partir de 18h00.

```js
db.resto.find({"close_hours": { $gt: "18" } }).projection({name: 1, close_hours: 1})
```

Cette requête va faire apparaitre tous les restaurants qui commencent à partir de 18h00.

Résultat:
![[Pasted image 20230202100159.png]]

B.Trié les restaurants avec leurs notes du plus petit au plus grand
```js
db.resto.find({}, {name: 1, rating: 1}).sort({rating: -1})
```

Cette requête permet d'afficher les restaurant est de les classer par notes par ordre décroissant.

Résultat:
![[Pasted image 20230202095550.png]]

Index MongoDB:

B. Créé un index

```js
db.resto.createIndex( { name: 1, opening_hours: 1 } )
```

Cette requête a permis de créer un index. L'index contient les champs name et opening_hours

Pour voir si l'index a bien été créé on utilise la commande suivante:
```js
db.runCommand({listIndexes: "resto"})
```

Résultat:

![[Pasted image 20230202101610.png]]

La requête nous a bien renvoyé l'index que nous avons créé.



Requêtes géospatiales:

dataset utilisé : locations

index féospatiale
```js
db.locations.createIndex({ location: "2dsphere" })
```
 Permet de créé un index pour pouvoir utilisé $near

```js
db.locations.find({ location: { $near: { $geometry: { type: "Point", coordinates: [48.865770, 2.341780] }, $maxDistance: 2000 } } })
```

Cette requete va montrer tous ce qui est a moi de 2km des coordonnées choisi.

Résultat:
![[Pasted image 20230202122824.png]]

Agrégation :
vg vbv
```js
db.locations.aggregate([ { $group: { _id: null, averageRating: { $avg: "$rating" } } } ])
```
Cette requête permet de calculer la moyenne des restaurants 

Résultat
![[Pasted image 20230202123623.png]]


```js
db.locations.aggregate([ { $group: { _id: "$restaurantName", totalComments: { $sum: "$numberOfComments" } } }, { $sort: { totalComments: -1 } }, { $limit: 1 } ])
```

Résultat:
![[Pasted image 20230202124930.png]]

Export:

export en Json
```
mongoexport --uri=mongodb+srv://Alex:VPvVFqNKEnxo4aJr@cluster0.vavbcfc.mongodb.net/EvalDB --collection=resto  --out=resto.json
```
Ceci ma permis d'exporter le dataset en json et de faire des modifications et ensuite de le re-importer.

export en CSV

export collection locations:
```

mongoexport --uri mongodb+srv://Alex:VPvVFqNKEnxo4aJr@cluster0.vavbcfc.mongodb.net/?retryWrites=true --db EvalDB --collection locations --csv --out locations.csv --fields restaurantName,location,type,coordinates,rating,numberOfRatings,numberOfComments

```


export colelction resto:
```
mongoexport --uri mongodb+srv://Alex:VPvVFqNKEnxo4aJr@cluster0.vavbcfc.mongodb.net/?retryWrites=true --db EvalDB --collection resto --csv --out resto.csv --fields   
name,opening_hours,close_hours,close_min,rating,_id
```

C'est deux requêtes m'ont permis d'exporter mes datasets en fichier Csv et aussi de choisir les colonnes à exporter

