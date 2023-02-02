
Evaluation:

filtre dataset: restaurant français végétarien

Cette reqeute a été utilisée pour importer la base de donnée dans Atlas:

```json
mongoimport --db EvalDB --collection resto --file restaurant.json --jsonArray mongodb+srv://Alex:VPvVFqNKEnxo4aJr@cluster0.vavbcfc.mongodb.net/?retryWrites=true
```

A. Rechercher les restaurant qui sont ouverts a partir de 18h00.

```json

db.resto.find({"close_hours": { $gt: "18" } }).projection({name: 1, close_hours: 1})

```

Cette requete va faire apparaitre tous les restaurant qui commence a partir de 18h00.

Résultat:
![[Pasted image 20230202100159.png]]

B.trié les restaurtant avec leur notes du plus petit au plus grand
```json

db.resto.find({}, {name: 1, rating: 1}).sort({rating: -1})

```

cette requette permet d'afficher les restaurant est de les classé par note par ordre décroissant

Résultat:
![[Pasted image 20230202095550.png]]

Index MongoDB:

B. Créé un index

```json
db.resto.createIndex( { name: 1, opening_hours: 1 } )
```

Cette requette a permis de créé un index. L'index contient les champs name et opening_hours

Pour voir ci l'index a bien été créé on utilise la commande suivante:
```json
db.runCommand({listIndexes: "resto"})
```

Résultat:

![[Pasted image 20230202101610.png]]

La requete nous a bien renvoyé l'index que nous avons créé.



Requêtes géospatiales

index féospatiale
```json
db.resto.createIndex({ geo_shape: "2dsphere" })
db.resto.createIndex({ "geo_shape.geometry": "2dsphere" })
```


```json
db.resto.find({ geo_shape: {{$nearSphere : {$geometry : {type : "Point", coordinates : [55.2883153, -21.169296000289428]},$maxDistance : 2000}}})

db.resto.find({ "geo_shape.geometry": {{ $nearSphere: { $geometry: { type: "Point", coordinates: [55.2883153, -21.169296000289428] }, $maxDistance: 2000 } } })
```
