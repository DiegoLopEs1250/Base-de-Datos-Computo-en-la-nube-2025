# AGREGACIONES #

## Metodos para hacer agregaciones simples 
- Distinct (): Deuelve valores no duplicados
- countDocuments(): Cuenta los documentos en una collecion 
- estimateDocumentCount(): Cuenta de manera aproximada durante un periodo de tiempo

## Una agrgacion (Aggregation pipeline) consta de una o mas estapas (stage) que porcesan documentos

1. Cada estapa realiza una operación en los documentos de entrada. Por ejemplo, una fase piede filtar documentosy calcular valores

2. Los documentos que se generan en una fase pasan a la siguiente fase

3. Puede devolver resultados para grupos de documentos como totales, maximo, minimo, etc.

### Se utiliza la clausula aggregate

- Existen una serie de operadores que se pueden utilizar para realizar operaciones
. Se tiene distintos tipos: etapa, de comparacion, booleanos, aritmeticos, de cadena, etc.

## Metodos Simples: CountDocuments() y distinct()

```json
db.libros.countDocuments()
```

1. Contar los documentos de la editorial terra
```json
db.libros.countDocuments({editorial: 'Terra'})
```

2. Mostrar todos los libros mostrando solamente la editorial

```json
db.libros.find({},{editorial:1, _id:0})
```

3. Seleccionar todos las distintas editoriales

```json
db.libros.distinct('editorial')
```

[Documentacion de Agregaciones](https://www.mongodb.com/docs/manual/aggregation/)

## $match. Una pipeline basica

## Tiene funciones de etapa
```json
db.libros.aggregate({$match: {editorial: 'Terra'}})
```

## $Projetc -> Incluir y remombrar campos
```json
db.libros.aggregate([
    {$match: {editorial: 'Terra'}},{$project:{_id:0, titulo:1, precio:1,NombreEditorial:'$editorial', editorial:1}}
])



```
- Generado por mongo compass
```json
[
  {
    $match:
      
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      {
        _id: 0,
        precio: 1,
        cantidad: 1,
        NombreEditorial: "$editorial",
        editorial: 1
      }
  }
]
```

# Crear un campo calculado con $project
```json
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        NombreEditorial: "$editorial",
        totalGanancias: {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  }
]
```
## Sort -> Ordenaciones
```json
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        NombreEditorial: "$editorial",
        totalGanancias: {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  },
  {
    $sort:
      /**
       * Provide any number of field/order pairs.
       */
      {
        precio: 1
      }
  }
]
```

## Group. Agrupaciones

[Agrupaciones](https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/)

- Cuantos documentos hay por cada una de las editoriales (count)

```json
[
  {
    $group:
      {
        _id: "$editorial",
        NumeroDocumentos: {
          $count: {}
        }
      }
  }
]
```

- Cuentos documentos hay por cada una de las editoriales por numero de documentos de manera descendente

```json
[
  {
    $group:
      {
        _id: "$editorial",
        NumeroDocumentos: {
          $count: {}
        }
      }
  },
  {
    $sort:
      {
        NumeroDocumento: -1
      }
  }
]
```

- Agrupar por tipo de propiedad mostrando el numero de propiedades y el promedio de sus precios

```json
[
  {
    $group:
      /**
       * _id: The id of the group.
       * fieldN: The first field name.
       */
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  }
]
```

# Operador $set y $unset

- Operador $set
```json
[
  {
    $group:
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  },
  {
    $set:
      {
        Media_total: {
          $trunc: "$Media"
        }
      }
  }
]
```

- Operador $unset

```json
[
  {
    $group:
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  },
  {
    $set:
      
      {
        Media_total: {
          $trunc: "$Media"
        }
      }
  },
  {
    $unset:
      ["Media", "Media_total"]
  }
]
```

- Creandoo nuevas colecciones utilizando el operador $out
-- Nota: debe ser el ultimo de la agragación 

- Ejemplos con operadores de comparación y lógicos

```json
[
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        price: 1,
        name: 1,
        room_type: 1,
        caro: {
          $gt: ["$price", 300]
        },
        medio: {
          $and: [
            {
              $gte: ["$price", 100]
            },
            {
              $lte: ["$price", 300]
            }
          ]
        },
        baratito: {
          $lt: ["$price", 100]
        }
      }
  }
]
```

- $cond -> Devuelve valores según una condición (es parecido a un operador ternario de un lenguje de programación)

```json
[
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        price: 1,
        name: 1,
        room_type: 1,
        caro: {
          $cond: [
            {
              $gt: ["$price", 300]
            },
            "Si",
            "No"
          ]
        },
        medio: {
          $cond: [
            {
              $and: [
                {
                  $gte: ["$price", 100]
                },
                {
                  $lte: ["$price", 300]
                }
              ]
            },
            "Si",
            "No"
          ]
        },
        baratito: {
          $cond: [
            {
              $lt: ["$price", 100]
            },
            "No",
            "Si"
          ]
        }
      }
  }
]
```

- Views

```json
db.createView("ganancias_Libros", "libros",
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        editorial: "Biblio"
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre editorial": "$editorial",
        "Total ganacias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  },
  {
    $sort:
      /**
       * Provide any number of field/order pairs.
       */
      {
        "Total ganacias": -1
      }
  }
]
)
```

```json
db.ganancias_Libros.find({'Total ganacias':{$lte:240}}, {titulo:1, 'Total ganancias':1}).sort({titulo:-1})
```