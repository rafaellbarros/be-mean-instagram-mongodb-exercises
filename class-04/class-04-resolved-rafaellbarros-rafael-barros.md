# MongoDB - Aula 04 - Exercício
Autor: Rafael Barros

## 1. **Adicionar** 2 ataques ao mesmo tempo para os seguintes pokemons: Pikachu, Squirtle, Bulbassauro e Charmander.


```
DEV(mongod-3.4.6) be-mean-pokemons> var query = {name: /pikachu/i}
DEV(mongod-3.4.6) be-mean-pokemons> var mod = {$pushAll: {moves: ['Raio que o Parta', 'Descarga']}}
DEV(mongod-3.4.6) be-mean-pokemons> db.pokemons.update(query, mod)
Updated 1 existing record(s) in 22ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

DEV(mongod-3.4.6) be-mean-pokemons> var query = {name: /squirtle/i}
DEV(mongod-3.4.6) be-mean-pokemons> var mod = {$pushAll: {moves: ['Bolhas', 'Jato Aqua']}}
DEV(mongod-3.4.6) be-mean-pokemons> db.pokemons.update(query, mod)
Updated 1 existing record(s) in 3ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

DEV(mongod-3.4.6) be-mean-pokemons> var query = {name: /bulbassauro/i}
DEV(mongod-3.4.6) be-mean-pokemons> var mod = {$pushAll: {moves: ['Folhas Navalhas', 'Planta Mortal']}}
DEV(mongod-3.4.6) be-mean-pokemons> db.pokemons.update(query, mod)
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

DEV(mongod-3.4.6) be-mean-pokemons> var query = {name: /charmander/i}
DEV(mongod-3.4.6) be-mean-pokemons> var mod = {$pushAll: {moves: ['Soco Flamejante', 'Fogo do Dragão']}}
DEV(mongod-3.4.6) be-mean-pokemons> db.pokemons.update(query, mod)
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

```

## 2. **Adicionar** 1 movimento em todos os pokemons: `desvio`.

```
DEV(mongod-3.4.6) be-mean-pokemons> var query = {}
DEV(mongod-3.4.6) be-mean-pokemons> var mod = {$push: {moves: 'desvio'}}
DEV(mongod-3.4.6) be-mean-pokemons> var options = {multi: true}
DEV(mongod-3.4.6) be-mean-pokemons> db.pokemons.update(query, mod, options)
Updated 5 existing record(s) in 7ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})

```

## 3. **Adicionar** o pokemon `AindaNaoExisteMon` caso ele não exista com todos os dados com o valor `null` e a descrição: "Sem maiores informações".

```
DEV(mongod-3.4.6) be-mean-pokemons> var query = {name: /AindaNaoExisteMon/i}
DEV(mongod-3.4.6) be-mean-pokemons> var mod = {$setOnInsert: {name: 'AindaNaoExisteMon', type: null, attack: null, defense: null, height: null, description: 'Sem maiores informações'}}
DEV(mongod-3.4.6) be-mean-pokemons> var options = { upsert: true }
DEV(mongod-3.4.6) be-mean-pokemons> db.pokemons.update(query, mod, options)
Updated 1 new record(s) in 1ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("59ac0fe9784108b5fededda4")
})
```

## 4. Pesquisar todos o pokemons que possuam o ataque `investida` e mais um que você adicionou, escolha seu pokemon favorito.

```
DEV(mongod-3.4.6) be-mean-pokemons> var query = { moves: { $in: [/descarga/i, /bolhas/i] }}
DEV(mongod-3.4.6) be-mean-pokemons> db.pokemons.find(query)
{
  "_id": ObjectId("5977f7b9a11aa5d85502aa74"),
  "name": "Pikachu",
  "description": "Rato elétrico bem fofinho",
  "type": "eletric",
  "attack": 55,
  "height": 0.4,
  "moves": [
    "Raio que o Parta",
    "Descarga",
    "desvio"
  ]
}
{
  "_id": ObjectId("5977f7b9a11aa5d85502aa77"),
  "name": "Squirtle",
  "description": "Ejeta água mais que uma Baleia",
  "type": "água",
  "attack": 48,
  "height": 0.5,
  "moves": [
    "Bolhas",
    "Jato Aqua",
    "desvio"
  ]
}
Fetched 2 record(s) in 9ms
```

## 5. Pesquisar **todos** os pokemons que possuam os ataques que você adicionou, escolha seu pokemon favorito.

```
DEV(mongod-3.4.6) be-mean-pokemons> var query = {moves: { $all: [/folhas navalhas/i, /planta mortal/i] }}
DEV(mongod-3.4.6) be-mean-pokemons> db.pokemons.find(query)
{
  "_id": ObjectId("5977f7b9a11aa5d85502aa75"),
  "name": "Bulbassauro",
  "description": "Chicote de trepadeira",
  "type": "grama",
  "attack": 49,
  "height": 0.4,
  "moves": [
    "Folhas Navalhas",
    "Planta Mortal",
    "desvio"
  ]
}
Fetched 1 record(s) in 5ms

```

## 6. Pesquisar **todos** os pokemons que não são do tipo `elétrico`.

```
DEV(mongod-3.4.6) be-mean-pokemons> var query = {type: {$not: /elétrico/i}}
DEV(mongod-3.4.6) be-mean-pokemons> db.pokemons.find(query)
{
  "_id": ObjectId("5977f7b9a11aa5d85502aa74"),
  "name": "Pikachu",
  "description": "Rato elétrico bem fofinho",
  "type": "eletric",
  "attack": 55,
  "height": 0.4,
  "moves": [
    "Raio que o Parta",
    "Descarga",
    "desvio"
  ]
}
{
  "_id": ObjectId("5977f7b9a11aa5d85502aa75"),
  "name": "Bulbassauro",
  "description": "Chicote de trepadeira",
  "type": "grama",
  "attack": 49,
  "height": 0.4,
  "moves": [
    "Folhas Navalhas",
    "Planta Mortal",
    "desvio"
  ]
}
{
  "_id": ObjectId("5977f7b9a11aa5d85502aa76"),
  "name": "Charmander",
  "description": "Esse é o cão chupando manga de fofinho",
  "type": "fogo",
  "attack": 52,
  "height": 0.6,
  "moves": [
    "Soco Flamejante",
    "Fogo do Dragão",
    "desvio"
  ]
}
{
  "_id": ObjectId("5977f7b9a11aa5d85502aa78"),
  "name": "Caterpie",
  "description": "Inseto bem fofinho",
  "type": "Bug",
  "attack": 30,
  "height": 0.3,
  "moves": [
    "desvio"
  ]
}
{
  "_id": ObjectId("59ac0fe9784108b5fededda4"),
  "name": "AindaNaoExisteMon",
  "type": null,
  "attack": null,
  "defense": null,
  "height": null,
  "description": "Sem maiores informações"
}
Fetched 5 record(s) in 20ms
```

## 7. Pesquisar **todos** os pokemons que tenham o ataque `investida` **E** tenham a defesa **não menor ou igual** a 49.

```
DEV(mongod-3.4.6) be-mean-pokemons> var query = {$and: [ {moves: {$in: ['investida']}}, {attack: {$not: {$lte: 49}}} ]}
DEV(mongod-3.4.6) be-mean-pokemons> db.pokemons.find(query)
Fetched 0 record(s) in 7ms
```

## 8. Remova **todos** os pokemons do tipo água e com attack menor que 50.
```
DEV(mongod-3.4.6) be-mean-pokemons> var query = {$and: [ {type: /água/i}, {attack: {$lt: 50}} ]}
DEV(mongod-3.4.6) be-mean-pokemons> db.pokemons.remove(query)
Removed 1 record(s) in 8ms
WriteResult({
  "nRemoved": 1
})
```
