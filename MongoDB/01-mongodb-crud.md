
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
