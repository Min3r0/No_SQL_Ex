# No_SQL_Ex
# Part 01_basic-queries

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
Voici la transformation de ton contenu en Markdown clair et structuré :

````markdown
# Exercice 2 : Analyse des Données

- **Comptez le nombre total de passagers**  
  ```js
  db["Titanic"].countDocuments()
  // 418
````

* **Trouvez combien de passagers ont survécu**

  ```js
  db["Titanic"].countDocuments({ "Survived": 1 })
  // 152
  ```

* **Trouvez le nombre de passagers femmes**

  ```js
  db["Titanic"].countDocuments({ "Sex": "female" })
  // 152
  ```

* **Trouvez le nombre de passagers avec au moins 3 enfants**

  ```js
  db["Titanic"].countDocuments({ Parch: { $gte: 3 } })
  // 9
  ```

---

# Part 02_advanced-queries
## Exercice 3 : Mise à Jour de Données

* **Mettez à jour les documents pour lesquels le port d'embarquement est manquant, en supposant qu'ils sont montés à bord à Southampton**

  ```js
  db["Titanic"].updateMany(
    { Embarked: { $in: [null, ""] } },
    { $set: { Embarked: "S" } }
  )
  ```

* **Ajoutez un champ `rescued` avec la valeur `true` pour tous les passagers qui ont survécu**

  ```js
  db["Titanic"].updateMany(
    { Survived: 1 },
    { $set: { rescued: true } }
  )
  /*
  {
    acknowledged: true,
    insertedId: null,
    matchedCount: 152,
    modifiedCount: 152,
    upsertedCount: 0
  }
  */
  ```

---

## Exercice 4 : Requêtes Complexes

* **Sélectionnez les noms des 10 passagers les plus jeunes**

  ```js
  db["Titanic"].find(
    { Age: { $ne: null } },
    { Name: 1, _id: 0, Age: 1 }
  )
  .sort({ Age: 1 })
  .limit(10)
  ```

* **Identifiez les passagers qui n'ont pas survécu et qui étaient dans la 2e classe**

  ```js
  db["Titanic"].find(
    { Survived: 0, Pclass: 2 }
  )
  ```

---

## Exercice 5 : Suppression de Données

* **Supprimez les enregistrements des passagers qui n'ont pas survécu et dont l'âge est inconnu**

  ```js
  db["Titanic"].deleteMany({
    Survived: 0,
    $or: [
      { Age: null },
      { Age: { $exists: false } }
    ]
  })
  /*
  {
    acknowledged: true,
    deletedCount: 61
  }
  */
  ```

---

## Exercice 6 : Mise à Jour en Masse

* **Utilisez une opération de mise à jour pour augmenter la valeur du champ Age de 1 pour tous les documents**

  ```js
  db["Titanic"].updateMany(
    { Age: { $exists: true, $ne: null } },  // filtre : Age existe et n’est pas null
    { $inc: { Age: 1 } }                    // incrémente Age de 1
  )
  /*
  {
    acknowledged: true,
    insertedId: null,
    matchedCount: 332,
    modifiedCount: 332,
    upsertedCount: 0
  }
  */
  ```

---

## Exercice 7 : Suppression Conditionnelle

* **Supprimez tous les documents où le champ Ticket est absent ou vide**

  ```js
  db["Titanic"].deleteMany({
    $or: [
      { Ticket: { $exists: false } },
      { Ticket: "" }
    ]
  })
  /*
  {
    acknowledged: true,
    deletedCount: 0
  }
  */
  ```

* **Autre tentative de suppression où Ticket vaut "null" (string)**

  ```js
  db["Titanic"].deleteMany({
    $or: [
      { Ticket: { $exists: false } },
      { Ticket: "null" }
    ]
  })
  /*
  {
    acknowledged: true,
    deletedCount: 0
  }
  */
  ```

---

## Bonus : Utiliser les REGEX

* **Utiliser une regex pour trouver tous les passagers qui portent le titre de Dr.**

  ```js
  db["Titanic"].find(
    { Name: { $regex: /Dr\./, $options: "i" } }
  )
  /*
  {
    _id: ObjectId('685a64735813de5ce46e839a'),
    PassengerId: 1185,
    Survived: 0,
    Pclass: 1,
    Name: 'Dodge, Dr. Washington',
    Sex: 'male',
    Age: 54,
    SibSp: 1,
    Parch: 1,
    Ticket: 33638,
    Fare: 81.8583,
    Cabin: 'A34',
    Embarked: 'S'
  }
  */
  ```


