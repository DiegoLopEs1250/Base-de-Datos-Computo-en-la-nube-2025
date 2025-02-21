## CONSULTAS ##

1. Cargar el fichero empleados.json
-En local:
  comando: 
    mongoimport --db curso --collection empleados --file empleados.json

- Docker:
  mongoimport --db curso --collection empleados --file empleados.json --port 27018

2. Utilizar la base de datos curso
```json
 use curso
```

3. Buscar todos los epleados que trabajen en Google

```json
db.empleados.find({empresa:'Google'})
```

4. Empleados que vivan en Peru

```json
db.empleados.find({pais: 'Peru'})
```


5. Empleados que ganen mas de 8,000 dolares

```json
db.empleados.find({ventas: {$gt:8000}})
```

6. Empleados con ventas inferiores a 10,0000

```json
db.empleados.find({ventas: {$lt:8000}})
```

7. Realizar la consulta anterior pero devolviendo una sola fila

```json
 db.empleados.findOne({})
```

8. Empleados que trabajan en Google o en yahoo con el operador $in

```json
db.empleados.find({empresa: {$in:['Google','Yahoo']}})
```

9. Empleados de Amazon que ganen mas de 9,000 dolares

```json
 db.empleados.find({empresa: 'Amazon', salario: {$gt:9000}})
```

10. Empleados que trabajen en yahoo que ganen mas de 6,000 o empleados que trabajen en Google que tengan ventas inferiores a 20,000

```json
db.empleados.find(
{
    $or:[
        {$and:[{empresa:"Yahoo"},{salario:{$gt:6000}}]},
        {$and:[{empresa:"Google"},{ventas:{$lt:20000}}]}
    ]
}
)
```

11. Visualizar el nombre, apellido y el paais de cada empleado

```json
 db.empleados.find({},{nombre:1,apellidos:1,pais:1})

```

12. Empleados que yrabajen en Google o Yahoo con el operador $or

```json
db.empleados.find({
    $or: [{empresa:'Amazon'}, {empresa:{$eq:'Yahoo'}}]
})
```