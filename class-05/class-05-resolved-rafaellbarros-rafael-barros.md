# MongoDB - Aula 05 - Exercício
Autor: Rafael Barros

## 1. Importar as collections restaurantes e pokemons.
```js

mongoimport --db be-mean-restaurantes --collection restaurantes --drop --file restaurantes.json
2017-09-19T22:35:41.140-0300    connected to: localhost
2017-09-19T22:35:41.151-0300    dropping: be-mean-restaurantes.restaurantes
2017-09-19T22:35:41.871-0300    imported 25359 documents

mongoimport --db be-mean-pokemons --collection pokemons --drop --file pokemons.json
2017-09-19T22:35:00.125-0300    connected to: localhost
2017-09-19T22:35:00.137-0300    dropping: be-mean-pokemons.pokemons
2017-09-19T22:35:00.167-0300    imported 610 documents

```

## 2. Distinct por cuisine na restaurantes.
```js
DEV(mongod-3.4.6) be-mean-restaurantes> db.restaurantes.distinct('cuisine')
[
  "American ",
  "Delicatessen",
  "Hamburgers",
  "Ice Cream, Gelato, Yogurt, Ices",
  "Irish",
  "Chinese",
  "Jewish/Kosher",
  "Bakery",
  "Chicken",
  "Turkish",
  "Donuts",
  "Caribbean",
  "Sandwiches/Salads/Mixed Buffet",
  "Bagels/Pretzels",
  "Continental",
  "Pizza",
  "Italian",
  "Steak",
  "Polish",
  "Latin (Cuban, Dominican, Puerto Rican, South & Central American)",
  "German",
  "French",
  "Pizza/Italian",
  "Mexican",
  "Spanish",
  "Café/Coffee/Tea",
  "Tex-Mex",
  "Pancakes/Waffles",
  "Soul Food",
  "Hotdogs",
  "Seafood",
  "Greek",
  "Not Listed/Not Applicable",
  "African",
  "Japanese",
  "Indian",
  "Armenian",
  "Thai",
  "Chinese/Cuban",
  "Mediterranean",
  "Korean",
  "Bottled beverages, including water, sodas, juices, etc.",
  "Russian",
  "Eastern European",
  "Middle Eastern",
  "Asian",
  "Ethiopian",
  "Vegetarian",
  "Barbecue",
  "Egyptian",
  "English",
  "Other",
  "Sandwiches",
  "Portuguese",
  "Indonesian",
  "Chinese/Japanese",
  "Filipino",
  "Juice, Smoothies, Fruit Salads",
  "Brazilian",
  "Afghan",
  "Vietnamese/Cambodian/Malaysia",
  "CafÃÃ©/Coffee/Tea",
  "Soups & Sandwiches",
  "Tapas",
  "Moroccan",
  "Pakistani",
  "Peruvian",
  "Bangladeshi",
  "Czech",
  "Salads",
  "Creole",
  "Fruits/Vegetables",
  "Iranian",
  "Cajun",
  "Scandinavian",
  "Polynesian",
  "Soups",
  "Australian",
  "Hotdogs/Pretzels",
  "Southwestern",
  "Nuts/Confectionary",
  "Hawaiian",
  "Creole/Cajun",
  "Californian",
  "Chilean"
]

```

## 3. Distinct por types na pokemons.
```js
DEV(mongod-3.4.6) be-mean-pokemons> db.pokemons.distinct('types')
[
  "bug",
  "flying",
  "normal",
  "poison",
  "electric",
  "steel",
  "water",
  "fire",
  "ice",
  "ghost",
  "fighting",
  "psychic",
  "grass",
  "ground",
  "fairy",
  "rock",
  "dark",
  "dragon"
]
```

## 4. As primeiras 3 pág. com .limit() e .skip() de pokemons (5 em 5).
```js
DEV(mongod-3.4.6) be-mean-pokemons> db.pokemons.find({}, {_id: 0, name: 1}).limit(5).skip(5 * 0)
{
  "name": "Butterfree"
}
{
  "name": "Spearow"
}
{
  "name": "Kakuna"
}
{
  "name": "Farfetchd"
}
{
  "name": "Magnemite"
}
Fetched 5 record(s) in 5ms

DEV(mongod-3.4.6) be-mean-pokemons> db.pokemons.find({}, {_id: 0, name: 1}).limit(5).skip(5 * 1)
{
  "name": "Magneton"
}
{
  "name": "Doduo"
}
{
  "name": "Seel"
}
{
  "name": "Dodrio"
}
{
  "name": "Charmeleon"
}
Fetched 5 record(s) in 4ms

DEV(mongod-3.4.6) be-mean-pokemons> db.pokemons.find({}, {_id: 0, name: 1}).limit(5).skip(5 * 2)
{
  "name": "Charmander"
}
{
  "name": "Wartortle"
}
{
  "name": "Dewgong"
}
{
  "name": "Caterpie"
}
{
  "name": "Blastoise"
}
Fetched 5 record(s) in 4ms

```

## 5. Group ou Aggregate contando a quantidade de pokemons de cada tipo
```js
DEV(mongod-3.4.6) be-mean-pokemons> db.pokemons.aggregate([
...     {$unwind: "$types"},
...     {
...          $group : {
...                 _id: '$types',
...                 total: { $sum: 1 }
...             }
...         }
... ]);
{
  "result": [
    {
      "_id": "dragon",
      "total": 20
    },
    {
      "_id": "dark",
      "total": 38
    },
    {
      "_id": "psychic",
      "total": 62
    },
    {
      "_id": "fighting",
      "total": 38
    },
    {
      "_id": "ice",
      "total": 24
    },
    {
      "_id": "rock",
      "total": 46
    },
    {
      "_id": "water",
      "total": 105
    },
    {
      "_id": "electric",
      "total": 40
    },
    {
      "_id": "steel",
      "total": 37
    },
    {
      "_id": "normal",
      "total": 78
    },
    {
      "_id": "fairy",
      "total": 28
    },
    {
      "_id": "poison",
      "total": 54
    },
    {
      "_id": "fire",
      "total": 47
    },
    {
      "_id": "ghost",
      "total": 34
    },
    {
      "_id": "ground",
      "total": 51
    },
    {
      "_id": "bug",
      "total": 61
    },
    {
      "_id": "grass",
      "total": 75
    },
    {
      "_id": "flying",
      "total": 77
    }
  ],
  "ok": 1
}

```

## 6. Realizar 3 counts na pokemons.
1) count -- todos
```js
DEV(mongod-3.4.6) be-mean-pokemons> db.pokemons.count()
610

```
2) count -- só tipo fogo
```js
DEV(mongod-3.4.6) be-mean-pokemons> db.pokemons.count({ types: 'fire' })
47

```

3) count -- só de quantos tem a defesa maior que 70
```js
DEV(mongod-3.4.6) be-mean-pokemons> db.pokemons.count({ defense: { $gt : 70 } })
250
```
