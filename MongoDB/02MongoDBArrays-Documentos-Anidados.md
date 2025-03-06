### Busquedas en Arrays y documentos Anidados ###

1. Buscar dentro de un documento anidado

```json
db.Ciudades.find({ alcalde: {nombre:'Crystal', apellidos: 'Long', edad:76}})

db.Ciudades.find({'alcalde.nombre': 'Crystal'})

```
2. Seleccionar las ciudades donde los alcaldes tengan mas de 70 años
```json
db.Ciudades.find({ 'alcalde.edad':{$gt:70}})
```

3. Realizar la consulta anterior usando una proyeccion
```json
db.Ciudades.find({ 'alcalde.edad':{$gt:70}},{_id:0,nombre:1,'alcalde.edad':1})
```
4. Seleccionar las ciudades donde el alcalde tenga 76 años o 41 
db.Ciudades.find({$or:[{"alcalde.edad":76},{"alcalde.edad":41}] })

### Consulta con Arrays ###

1. Crear una coleccion cazas

2. Cargar los datos de las colecciones

## Ejemplos con cosnultas con Arrays

1. Buscar los documentos que tengan usa

```json
db.cazas.find({paises:'usa'})
```

2. Buscar los documentos donde cualquier elemento del Array dimensiones sea mayor a 5

```json
db.cazas.find({dimensiones: {$gt:5}})
```

### Buscar por Posicion
1. Buscar los documentos donde las dimensiones del avión por ancho que es la posicion 1
sea superior a 14

```json
db.cazas.find({"dimensiones.1": {$gt:14}},{_id:0,modelo:1, dimensiones:1})
```

### Arrays operador $all ###
1. Buscar los aviones que estan sirviendo en egipto e israel

```json
db.cazas.find({paises: {$all: ['egipto', 'israel']}})
```

2. Seleccionar los cazas que sirven en inglaterra y españa

```json
db.cazas.find({$and:[{paises:'Inglaterra'},{paises: 'españa'}]})
```

### Arrays Operador $elemMatch -> Permite hacer busquedas dentro de arrays, este busca la lista condiciones que estan dentro del operador

1. Buscar que aviones tiene uso dentro de inglaterra

```json
db.cazas.find({paises: {$elemMatch:{$eq:'inglaterra'}}})
```

2. BBuscar los aviones donde las dimensiones sean menores a 20 o mayores a 15

```json
db.cazas.find({dimensiones: {$elemMatch: {$eq:20, $lt:15}}})
```

### Buscar en arrays de documentos anidados

1. Utilizar la coleccion Ciudades

2. Buscar los consejeros de nombre Jery Flower de edad 78

```json
db.Ciudades.find({identificador:1, nombre:'Jeri Flowers', edad: 78})
```
3. Buscar en consejeros donde tengan una edad mayor a 70

```json
db.Ciudades.find({"consejeros.edad": {$gt:70}})
```

