# mongodb

Exercício 1- Aquecendo com os pets 

1. Adicione outro Peixe e um Hamster com nome Frodo.<br>
```
db.pets.insert({name: "Frodo", species: "Peixe"}) 
db.pets.insert({name: "Frodo", species: "Hamster"}) 
```
2. Faça uma contagem dos pets na coleção.<br>
```
db.pets.count()
```
3. Retorne apenas um elemento o método prático possível.<br>
```
db.pets.findOne()
```
4. Identifique o ID para o Gato Kilha.<br>
```
db.pets.find({name: "Kilha", species: "Gato"}, {ObjectId:1})
```
5. Faça uma busca pelo ID e traga o Hamster Mike.<br>
```
db.pets.findOne({ObjectId: db.pets.findOne({name: "Kilha", species: "Gato"}, {"id":1})._id})
```
6. Use o find para trazer todos os Hamsters.<br>
```
db.pets.find({species: "Hamster"})
```
7. Use o find para listar todos os pets com nome Mike.<br>
```
db.pets.find({name: "Mike"})
```
8. Liste apenas o documento que é um Cachorro chamado Mike.<br>
```
db.pets.findOne({name: "Mike", species: "Cachorro"})
```
