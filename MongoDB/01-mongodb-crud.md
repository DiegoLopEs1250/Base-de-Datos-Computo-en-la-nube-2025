
#CRUD y consultas en MongoDB


```json
db.Alumnos.insertOne(
{
    _id : 3,
    nombre : 'Sergio',
    Apellidos : 'Ramos',
    equipo : 'Monterrey',
    aficiones: ['Dinero', 'Hombres', 'Fiesta'],
    talentos: {
        futbol : true,
        bañarse : false
    }
}
)
```



# Practica 1

## Cargar datos

[Libros.json](./data/libros.json)

## Busqueda. Cindiciones simples de igualdad. Metodo Find() 

1. Seleccionar todos los documentos de la coleccion libros

```json
db.libros.find()
```
1. Mostrar todos los ducumentos que sean de la editoria 'Biblio'

```json
 db.libros.find({editorial: 'Biblio'})
```

1. Mostrar todos los documentod que el precio sea 25
```json
db.libros.find({precio: 25})
```

1. Seleccionar todos los documentos donde el titulo sea json para todos
```json
db.libros.find({titulo: 'JSON para todos'})
```

## Operadores de Comparacio

https://www.mongodb.com/docs/manual/reference/operator/query/


![Operadores de Comparación](../IMG/Operadores_Imagenes.png)

1. Mostrar todos los documentos donde el precio sea mayorn a 25

```json
db.libros.find({ precio: { $gt: 25 } } )
```

1. Mostrar los documentos donde el precio sea 25

```json
db.libros.find({ precio: { $eq: 25 } } )
```

1. Mostrar los documentos cuya cantidad sea menor a 5

```json
db.libros.find({ cantidad: { $lt: 5 } } )
```

1. Mostrar los documentos que pertenezcan a la editorial biblio o planeta

```json
db.libros.find({ editorial: { $in:['Biblio','Planeta'] } } )
```

1. Mostrar todos los documentos de libros que cuesten 20 o 25
```json
db.libros.find({ precio: { $in:[20,25] } } )
```

1. Mostrar todos los documentos de libros que no cuesten 20 o 25

```json
db.libros.find({ precio: { $nin:[20,25] } } )
```

1. Mostrar el primer documento de libros que cuesten 20 o 25

```json
 db.libros.findOne( { precio: { $in: [20, 25] } } )
```

## Operadores Lógicos

[Operadores Logicos](https://www.mongodb.com/docs/manual/reference/operator/query/)

![Operadores Logicos](../IMG/Operadores_Logicos.png)

## Operador AND

Dos posibles opciones de AND

1. La simple, mediante condiciones separadas por comas (,)

**Sintaxis** 

db.coleccion.find({condicione1, condicion2}) -> Con esto asume que es un AND

2. Usando el operador $and 

**Sintaxis**

db.coleccion.find({$and:[{condicion1},{condicion2}]})

### Ejercicios 

1. Mostrar todos aquellos libros que cuesten mas de 25 y cuya cantidad sea inferior a 15

*** Forma Simple ***

```json
 db.libros.find({precio: {$gt:25}, cantidad:{$lt:15}})
```

*** Forma Operador $and ***

```json
db.libros.find( { $and: [{ precio: { $gt: 25 }, cantidad: { $lt: 15 } }] } )

```

2. Mostrar todos aquellos libros con id 4

```json
db.libros.find( { $and: [{ precio: { $gt: 25 }, cantidad: { $lt: 15 }, _id:{$eq:4} }] } )
```

## Operador OR ##
1. Mostrar todos aquellos libros que cuesten mas de 25 o cuya cantidad sea inferior a 15

```json

db.libros.find({ $or: [{ precio: { $gt: 25 } }, { cantidad: { $lt: 15 } }] } )

```

1. ## AND y OR combinadas ##

2. Mostrar los libros de la editorial Biblio con precio mayor a 40 o libros con la editorial Planeta con precio mayor 
   mayor a 30.

```json

db.libros.find(
{
    $or: [
        {$and: [{editorial:'Biblio'}, {precio:{$gt:40}}]},
        {$and:[{editorial:{$eq:'Planeta'}},{precio:{$gt:30}}]}
    ]
}
)

```

## Forma Simple

db.libros.find(
{
    $or: [
        {editorial:'Biblio', precio:{$gt:30}},
        {editorial:{$eq:'Planeta'},precio:{$gt:20}}
        ]
}
)

## Proyeccion de Columnas

*** Sintaxis ***

db.coleccion.find(filtro, columna)

```json
db.libros.find({},{titulo:1})
```

1. Seleccionar todos los documentos mostrando el titulo y la aeditorial

```json
 db.libros.find({},{titulo:1, editorial:1})

 db.libros.find({},{titulo:1, editorial:1, _id:0})
```

1. Seleccionar todos los documentos de la editorial Planeta, mostrando el titulo y la editorial

```json
 db.libros.find({ editorial: 'Planeta'},{titulo:1, editorial:1, _id:0})
```

## Operador exists (Permite saber si un campo se encuentra o no en un documento)

```json
db.libros.find( { editorial: { $exists: true } } )
```

3. Mostrar todos los documentos que no contengan el campo cantidad

```json
db.libros.find( { cantidad: { $exists: false } } )
```

## Operador type (Permite preguntar si un determinado campo corresponde con un tipo) ##

[Operador Type](https://www.mongodb.com/docs/manual/reference/operator/query/type/#mongodb-query-op.-type)

1. Mostrar todos los documentos donde el precio sea doubles

```json
db.libros.find({
    precio: {$type:1}
})

```

```JSON
db.libros.find({ precio: { $type: 1 } }, {_id:0})
db.libros.find({ precio: { $type: 1 } }, {_id:0, cantidad:0})

```

1. Seleccionar los documentos donde la editorial se del tipo entero 
```json
db.libros.find({ editorial:{$type:16} })
db.libros.find({ editorial:{$type:'int'} })
```


2. Seleccionar todos los documentos la editorial sea string

```json
db.libros.find({ editorial:{$type:'string'} })
db.libros.find({ editorial:{$type:2} })
```

## Practica de consultas ##
1. Instalar las tools de mongodb
[DatabaseTools](https://www.mongodb.com/try/download/database-tools)

2. Cargar el json empleados (Debemos estar ubicados en la carpeta donde se encuentra el JSON empleados)

- En local:
  comando: 
    mongoimport --db curso --collection empleados --file empleados.json

- Docker:
  mongoimport --db curso --collection empleados --file empleados.json --port 27018

## Modificando Docmentos ##

## Comandos Importantes ##

1. updateOne -> Modificar un solo documento
2. updateMany -> Modificar multiples documentos
3. remplaceOne -> Sustituir el contenido completo de un documento

```json
db.collection.updateOne(
  {filtro},
  {operador: }
)
```

[Operadores Update] ()

### Operador Set ###

1. Modificar in documento


```json
db.libros.updateOne({titulo: 'Python para todos'},{$set:{titulo: 'Java para todos'}})
```

2. Actualizar el precio a 100 y la cantidad a 50 para el id 10

```json
db.libros.updateOne({_id:10},{$set:{precio:100, cantidad:50}})
```
#### Modificar Documentos ####

1. Modificar todos los documentos donde el precio sea mayor a 100 con el precio de 150

```json
db.libros.updateMany({precio: {$gt:100}}, {$set: {precio:150}})
```

2. ### Operador $inc y $mul

```json
1. db.libros.updateMany(
    {},
    {$inc: {precio:5}}
    )
```

- Actualizar con multiplicacion de 2 todos los documentos que la cantidad sean mayores a 20
```json
db.libros.updateOne({cantidad:{$gt:20}, {$nul:{cantidad:2}}})
```

- Actualizar todos los documentos donde el precio sea mayor a 20 y se multiplique por 2 la cantidad y el precio

db.libros.updateOne({precio:{$gt:20}}, {$mul:{cantidad:2, precio:2}})

- Remplazar documentos completos (RemplazeOne)
```json
db.libros.remplaceOne({_id:2}, {titulo:'De la tierra a la luna', autor:'Julio Verne', precio: 500})
```

### Borra Documentos ###

1. deleteOne -> Elimina un solo documento
2. deleteMany -> Elimina multiples documentos

1. Eliminar el docuemnto con id 2
```json
db.libros.deleteOne({_id:2})
```

2. Eliminar los documentos donde la cantidad sea mayor o igual a 150
```json
db.libros.deleteMany({
    cantidad: {$gte:150}
})
```

### Expresiones Regulares ###

1. Buscar los libros que contengan el titulo la letra t

db.libros.find({titulo: /t/})

2. Buscar los libros que en el titulo con la palabra json


db.libros.find({titulo: /json/})

3. Todos lo documentos que en titulo terminen en tos 

```json
db.libros.find({titulo: /tos$/})

```

4. Todos los documentois que comiencen con J


db.libros.find({titulo: /^J/})


### Operadores $regex

[Operador Regex] (https://www.mongodb.com/docs/manual/reference/operator/query/regex/)

- Seleccionar los libros que contengan la palabra para en titulo
```json
db.libros.find({tutulo :{$regex:'para'}})

```

```json
db.libros.find({titulo: {$regex:'JSON'}})
```

db.libros.find({titulo: {$regex:/JSON/}})

1. Distinguir entre mayusculas y minusculas

db.libros.find({titulo:{$regex:/json/, $options:"i"}})

db.libros.find({titulo:{$regex:/json/i}})

-- Seleccinar todos los libros que comiencen con j o J

db.libros.find({titulo: /^J/})

-- Seleccionar todos los libros que terminen con es

db.libros.find({titulo: /es$/})

### Metodo Sort ###

1. Ordenar los libros de manera ascendente
```json
db.libros.find({}, {titulo:1, precio:1, _id:0}).sort({precio:1})
```

2. Ordenar los libros de manera descendente por el precio
```Json
db.libros.find({}, {titulo:1, precio:1, _id:0}).sort({precio:-1})
```

- Ordenar los libros de manera ascendente por la editorial y de manera descendente por el precio, mostrando el titulo. el precio y la editorial 
```json
db.libros.find({}, {titulo:1, precio:1,editorial:1, _id:0}).sort({precio:-1,editorial:1})
```

### Otros metodos skip, limit, size ###

db.libros.find({}, {titulo:1, precio:1, _id:0, editorial:1}).size()

db.libros.find({titulo: {$regex:/Java/i}}).size()

- Buscar todos los docuemntos pero mostrando los 2 primeros
db.libros.find({},{titulo:1, editorial:1, precio:1, _id:0}).limit(2)

- Mostrar los 3 ultimos libros
db.libros.find({},{titulo:1, editorial:1, precio:1, _id:0}).sort({precio:-1}).limit(2)

- Seleccionar todos los libros de forma descendente, saltando los 2 primeros y el tamaño

db.libros.find({}, {titulo:1, _id:0}).sort({titulo:-1}).skip(2).size()

#### Como borrar colecciones y base de datos ###



db.Ejemplos.insertOne( { nombre: 'Chapuin' } )


db.Ejemplos.drop()

db.dropDatabase()

