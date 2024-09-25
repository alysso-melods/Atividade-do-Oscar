# Atividade-do-Oscar
1- Quantas vezes Natalie Portman foi indicada ao Oscar?

R: 3 vezes

Q: 
```js
db.oscarzinho.countDocuments({ nome_do_indicado: "Natalie Portman" })
```

2- Quantos Oscars Natalie Portman ganhou?

R: 1 vez

Q: 
```js
db.oscarzinho.countDocuments({nome_do_indicado: "Natalie Portman", vencedor: "true"})
```

3- Amy Adams já ganhou algum Oscar?

R: Nunca

Q: 
```js
db.oscarzinho.countDocuments({nome_do_indicado: "Amy Adams", vencedor: "true"})
```

4- A série de filmes Toy Story ganhou um Oscar em quais anos?

R: 2011 e 2020

Q: 
```js
db.oscarzinho.find({"nome_do_filme": {"$in": ["Toy Story", "Toy Story 2", "Toy Story 3", "Toy Story 4"]}, "vencedor": {"$eq": "true"}})
```
  
5 - A partir de que ano que a categoria "Actress" deixa de existir?

R:  "1976 foi o último ano da categoria"

Q: 
```js
db.oscarzinho.aggregate([{ $match: { categoria: "ACTRESS" } },{ $sort: { ano_cerimonia: -1 } },{ $project: { ano_cerimonia: 1 } }, { $limit: 1 } ]) 
```

6 - O primeiro Oscar para melhor Atriz foi para quem? Em que ano?

R: Janet Gaynor na cerimonia de 1928.

Q:
```js
db.oscarzinho.find({vencedor:"true", categoria: "ACTRESS"}, {nome_do_indicado: 1, ano_cerimonia: 1, _id: 0}).limit(1)
```

7- No campo "Vencedor", altere todos os valores com "Sim" para 1 e todos os valores "Não" para 0.

Q: 
```js
db.oscarzinho.updateMany(
  { "vencedor": "false" }, 
  { 
    $set: { "vencedor": 0 }
  }
);

db.oscarzinho.updateMany(
  { "vencedor": "true" }, 
  { 
    $set: { "vencedor": 1 }
  }
);
```

8- Em qual edição do Oscar "Crash" concorreu ao Oscar?

R: 2006

Q: 
```js
db.oscarzinho.find({ "nome_do_filme": "Crash"}, { "cerimonia": 1, "ano_cerimonia": 1, "_id": 0 }).limit(1)
```

9 - Bom... dê um Oscar para um filme que mereceu muito, mas não ganhou.

R: Oscar de Melhor Animação 2015 para O Conto Da Princesa Kaguya

Q:
```js
db.oscarzinho.updateOne(
  { "nome_do_filme": "The Tale of the Princess Kaguya" }, 
  { 
    $set: { "vencedor": "1" }
  }
);

db.oscarzinho.updateOne(
  { "nome_do_filme": "Big Hero 6" }, 
  { 
    $set: { "vencedor": "0" }
  }
);
```

10 - O filme Central do Brasil aparece no Oscar?

R: Sim, 2 vezes

Q:
```js
db.oscarzinho.find({ nome_do_filme: "Central Station" })
```

11 - Inclua no banco 3 filmes que nunca foram nem nomeados ao Oscar, mas que mereceiam ter sido.

R: The Iron Claw(2023), Twin Peaks: Fire Walk with Me (1992), Heat (1995)

Q:
```js
db.oscarzinho.insertMany([
    {
      "id_registro": 11900,
          "ano_filmagem": "2023",
          "ano_cerimonia": "2024",
          "cerimonia": "96",
          "categoria": "MUSIC (Original Song)",
          "nome_do_indicado": "Music and Lyric by Richard Reed Perry, Little Scream and The Barr Brothers",
          "nome_do_filme": "The Iron Claw",
          "vencedor": 1
    },
  {
    "id_registro": 11901,
        "ano_filmagem": 1992,
        "ano_cerimonia": 1993,
        "cerimonia": "65",
        "categoria": "ACTRESS IN A LEADING ROLE",
        "nome_do_indicado": "Sheryl Lee",
        "nome_do_filme": "Twin Peaks: Fire Walk with Me",
        "vencedor": 1
  },
   {
    "id_registro": 11902,
        "ano_filmagem": 1995,
        "ano_cerimonia": 1996,
        "cerimonia": "68",
        "categoria": "DIRECTING",
        "nome_do_indicado": "Michael Mann",
        "nome_do_filme": "Heat",
        "vencedor": 1
  }
])
```

12 - Pensando no ano em que você nasceu: Qual foi o Oscar de melhor filme, Melhor Atriz e Melhor Diretor?

R: Melhor Filme - Million Dollar Baby, Melhor Atriz - Hillary Swank, Melhor Diretor - Clint Eastwood

Q: 
```js
db.oscarzinho.find({"categoria": {"$in": ["DIRECTING", "ACTRESS IN A LEADING ROLE", "BEST PICTURE"]}, "ano_cerimonia":2005, "vencedor":1} )
```
