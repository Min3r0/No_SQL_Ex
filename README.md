# No_SQL_Ex
# Part 01_basic-queries.md

## Exercice 3 : Lecture de Données

**Trouvez tous les Pokémon de type "Feu".**
```js
db["Pokemons"].find({ $or: [{ "Type 1": "Fire" }, { "Type 2": "Fire" }] })
```

**Récupérez les informations du Pokémon nommé "Pikachu".**
```js
db["Pokemons"].findOne({ "Name": "Pikachu" })
```
```json
{
  _id: ObjectId('685961f5bf9d083ac0cf3447'),
  "Pokemon No": {
    "": 25
  },
  "Name": "Pikachu",
  "Type 1": "Electric",
  "Max CP": 894,
  "Max HP": 67,
  "Image URL": "http://cdn.bulbagarden.net/upload/thumb/0/0d/025Pikachu.png/250px-025Pikachu.png"
}
```

---

## Exercice 4 : Mise à Jour de Données

**Mettez à jour les points de combat (CP) de "Pikachu" pour qu'ils soient de 900.**
```js
db["Pokemons"].updateOne({ "Name": "Pikachu" }, { "$set": { "Max CP": 900 } })
```
```json
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
```

---

## Exercice 5 : Suppression d'Éléments

**Supprimez le Pokémon "Bulbasaur" de la collection Pokemons.**
```js
db["Pokemons"].deleteOne({ "Name": "Bulbasaur" })
```
```json
{
  acknowledged: true,
  deletedCount: 1
}
```
