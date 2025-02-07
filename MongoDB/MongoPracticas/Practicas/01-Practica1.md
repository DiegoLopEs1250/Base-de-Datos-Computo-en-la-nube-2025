# Practica 1
1. Conectarnos com mongosh a MongoDB
1. Cramso la bases de datos cursos
1. Comporobamos que no exista
1. Crear una coleccion 'faturas'

```json
db.createCollection('Facturas')
show collections
```
1. Creamos la tabla

| Codigo   | Valor   |
|-------------|-------------|
| Cod_Factura | 10 |
| Ciente | Frutas Ramirez |
| Total | 223 |

1. crear una nueva coleccion usando directamente el insertOne
insertar un documento en la coleccion productos con los siguientes datos:

| Codigo   | Valor   |
|-------------|-------------|
| Cod_producto | 20 |
| Nombre | Tornillo x1p |
| Precio | 2 |
|Unidades | 1500 |


db.productos.insertOne( { cod_producto: 1, nombre: 'Tornillo x 1', precio: 2, unidades:1500 } )

1. Craer un nuevo documento de producto que contenga un array con los siguientes datos:

| Codigo   | Valor   |
|-------------|-------------|
| Cod_producto | 55 |
| Nombre | Martillo |
| Precio | 20 |
|Unidades | 50 |
|Fabricantes | fa1, fab2, fab3, fab3, fab4 |

```json

db.productos.insertOne(
 {
 cod_productos : 55,
nombre : 'Martillo',
precio : 20,
unidades : 50,
fabricantes : [
               'fab1', 'fab2','fab3', 'fab4'
              ]
 }
 )

```

1. Borrar la coleccion facturas y comprobar que se borro

```json
db.facturas.drop()

show collections
```

1. Insertar un documento en una colección denominada **fabricantes** 
  para probar los subdocumentos y la clave _id personalizada

| Codigo   | Valor   |
|-------------|-------------|
| id | 1 |
| Nombre | fab1 |
| Localidad | ciudad: buenos aires, pais: Portugal, Calle : tiburon, Cod_postal: 29000 |

```json
db.fabricantes.insertOne( { _id: 2, Nombre: 'fa1', Localidad: { ciudad: 'buenos aires', pais: 'Portugal', Calle: 'Tiburon', cod_postal: 27000 } } )
```

1. Realizar iuna insercion de varios documentos en la coleccion productos

| Codigo   | Valor   |
|-------------|-------------|
| Cod_producto | 3 |
| Nombre | Alicates |
| Precio | 10 |
|Unidades | 25 |
|Fabricantes | fa1, fab2, fab3, fab5 |

| Codigo   | Valor   |
|-------------|-------------|
| Cod_producto | 4 |
| Nombre | Arandelas |
| Precio | 1 |
|Unidades | 500 |
|Fabricantes | fa, fab3, fab4 |

```json
  db.productos.insertMany(
  [
    {
        Cod_producto: 3,
        Nombre : 'Alicates',
        Precio : 10,
        Unidades : 25,
        Fabricantes : fa1, fa2, fa5
    },
    {
        Cod_producto: 4,
        Nombre : 'Arandelas',
        Precio : 1,
        Unidades : 500,
        Fabricantes : fa2, fa3, fa4
    }
  ]
  )
```

