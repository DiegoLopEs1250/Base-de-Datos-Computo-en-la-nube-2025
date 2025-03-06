### Practica 3 Updates y Deletes ### 

1. Cambiar el salario del empleado Imogene Nolan y se leasigna 8,000

```json
db.empleados.updateOne({nombre: 'Imogene'},{$set:{salario:8000}})
```

2. Cambiar "Belgium" por "Belgica" en los empleados (debe haber 2)
```json
 db.empleados.updateMany({pais:'Belgium'},{$set: {pais:'Belgica'}})
```

3. Incrementar el salario de todos los empleados de Google en 1000
```json
db.empleados.updateMany({ empresa: 'Google' }, { $inc: { salario: 1000 } } )
```

4. Reemplazar el empleado Omar Gentry por el siguiente documento:
        ```json
            {
"nombre": "Omar",
"apellidos": "Gentry",
"correo": "sin correo",
"direccion": "Sin calle",
"region": "Sin region",
"pais": "Sin pais",
"empresa": "Sin empresa",
"ventas": 0,
"salario": 0,
"departamentos": "Este empleado ha sido anulado"
}
        ```
```json
db.empleados.replaceOne({nombre: 'Omar'},{nombre:'Omarr', apellidos: 'Gentry', correo: 'Sin correo', direccion: 'Sin direccion', region: 'Sin region', pais: 'Sin pais', empresa: 'Sin empresa', ventas: 0, salario: 0, departamentos: 'Este empleado a sido midificado'})
```



5. Con un fin dcoprobar que el empleado ha sido midificado
```json
 db.empleados.find({nombre: 'Omarr'})
```


6. Borrar todos los empleados que ganen mas de 8,500 (nota: deben ser borrados 3 documentos)

```json
db.empleados.deleteMany({salario: {$gt:8500}})
```

7. Visualizar con una expresión regular todos los empleados con apellidos que comienzen con "R"

```json
db.empleados.find({apellidos: /^R/})
```

8. Buscar todas las regiones que contengan una "V". Hacerlo con el operador $regex y que no distinga mayusculas y minusculas deben salir 2

```json
db.empleados.find({region: {$regex: 'V'}})
```

9. Visualizar los apellidos de los empleados ordenados por el propio apellido.

```json
db.empleados.find({},{_id:0, apellidos: 1}).sort({apellidos:-1})
```

10. Indicar el numero de empleados que trabajan en Google

```json
db.empleados.find({empresa: 'Google'}).size()
```

11. Borra la collecion empleados y la base de datos
```json
db.empleados.dropCollection() 

db.dropDatabase()
```