# Exercício 1 - Aquecendo com os pets 

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
db.pets.find({name: "Kilha", species: "Gato"}, {_id:1})
```
5. Faça uma busca pelo ID e traga o Hamster Mike.<br>
```
db.pets.findOne({_id: db.pets.findOne({name: "Kilha", species: "Gato"}, {"_id":1})._id})
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

# Exercício 2 - Mama mia!

1. Liste/Conte todas as pessoas que tem exatamente 99 anos. Você pode usar um count para indicar a quantidade.
```
db.italians.find({age:99}).count()
```
2. Identifique quantas pessoas são elegíveis atendimento prioritário (pessoas com mais de 65 anos).
```
db.italians.find({"age":{"$gt":65}}).count()
```
3. Identifique todos os jovens (pessoas entre 12 a 18 anos).
```
db.italians.find({"age":{"$gte":12,"$lte":18}}).count()
```
4. Identifique quantas pessoas tem gatos, quantas tem cachorro e quantas não tem nenhum dos dois.
```
db.italians.find({cat:{$exists:true}}).count()
db.italians.find({dog:{$exists:true}}).count()
db.italians.find({"$and":[{cat:{$exists:false}},{dog:{$exists:false}}]}).count()
```
5. Liste/Conte todas as pessoas acima de 60 anos que tenham gato.
```
db.italians.find({"$and":[{age:{$gt:60}},{cat:{$exists:true}}]}).count()
```
6. Liste/Conte todos os jovens com cachorro.
```
db.italians.find({"$and":[{age:{$gte:12,$lte:18}},{dog:{$exists:true}}]}).count()
```
7. Utilizando o $where, liste todas as pessoas que tem gato e cachorro.
```
db.italians.find({$where:function(){return this.cat!=null && this.dog!=null}}).count()
```
8. Liste todas as pessoas mais novas que seus respectivos gatos.
```
db.italians.find({$where:"(this.cat!=null)&&(this.age<this.cat.age)"})
```
9. Liste as pessoas que tem o mesmo nome que seu bichano (gato ou cachorro).
```
db.italians.find({$where:"((this.cat!=null)&&(this.firstname==this.cat.name))||((this.dog!=null)&&(this.firstname==this.dog.name))"})
```
10. Projete apenas o nome e sobrenome das pessoas com tipo de sangue de fator RH negativo.
```
db.italians.find({"bloodType":/-/},{firstname:1,surname:1})
```
11. Projete apenas os animais dos italianos. Devem ser listados os animais com nome e idade. Não mostre o identificado do mongo (ObjectId).
```
db.italians.find({"$or":[{cat:{$exists:true}},{dog:{$exists:true}}]},{cat:1,dog:1,_id:0})
```
12. Quais são as 5 pessoas mais velhas com sobrenome Rossi?
```
db.italians.
```
13. Crie um italiano que tenha um leão como animal de estimação. Associe um nome e idade ao bichano.
```
db.italians.insert({"firstname":"Daniel","lion":{"name":"Simba","age":20}})
```
14. Infelizmente o Leão comeu o italiano. Remova essa pessoa usando o Id.
```
db.italians.remove({_id: db.italians.findOne({lion:{$exists:true}})._id})
```
15. Passou um ano. Atualize a idade de todos os italianos e dos bichanos em 1.
```
db.italians.update({},{"$inc":{"age":1,"cat.age":1,"dog.age":1}})
```
16. O Corona Vírus chegou na Itália e misteriosamente atingiu pessoas somente com gatos e de 66 anos. Remova esses italianos.
```
db.italians.remove({"$and":[{cat:{$exists:true}},{age:66}]})
```
17. Utilizando o framework agregate, liste apenas as pessoas com nomes iguais a sua respectiva mãe e que tenha gato ou cachorro.
```
db.italians.
```
18. Utilizando aggregate framework, faça uma lista de nomes única de nomes. Faça isso usando apenas o primeiro nome.
```
db.italians.
```
19. Agora faça a mesma lista do item acima, considerando nome completo.
```
db.italians.
```
20. Procure pessoas que gosta de Banana ou Maçã, tenham cachorro ou gato, mais de 20 e menos de 60 anos.
```
db.italians.find({favFruits:{$in:["Banana","Maçã"]},$or:[{cat:{$exists:true}},{dog:{$exists:true}}],age:{$gt:20,$lt:60}})
```

# Exercício 3 - Stockbrokers

1. Liste as ações com profit acima de 0.5 (limite a 10 o resultado)
```
db.stocks.find({"Profit Margin":{"$gt":0.5}}).limit(10)
```
2. Liste as ações com perdas (limite a 10 novamente)
```
db.stocks.find({"Profit Margin":{"$lt":0.0}}).limit(10)
```
3. Liste as 10 ações mais rentáveis
```
db.stocks.find({"Profit Margin":{$exists:true}}).sort({"Profit Margin":-1}).limit(10)
```
4. Qual foi o setor mais rentável?
```
db.stocks.aggregate([{$group:{_id:"$Sector",total:{$sum:"$Profit Margin"}}},{$sort:{total:-1}},{$limit:1}])
```
5. Ordene as ações pelo profit e usando um cursor, liste as ações.
```
var cursor = db.stocks.find({"Profit Margin":{$exists:true}}).sort({"Profit Margin":-1});
cursor.forEach(function(x){print(x.Ticker));
```
6. Renomeie o campo “Profit Margin” para apenas “profit”.
```
db.stocks.updateMany({},{$rename:{"Profit Margin":"profit"}})
```
7. Agora liste apenas a empresa e seu respectivo resultado
```
db.stocks.
```
8. Analise as ações. É uma bola de cristal na sua mão... Quais as três ações você investiria?
```
db.stocks.find({"Performance (YTD)":{$exists:true}},{"Performance (YTD)":1,"Ticker":1,"_id":0}).sort({"Performance (YTD)":-1}).limit(3)
```
9. Liste as ações agrupadas por setor
```
db.stocks.
```
