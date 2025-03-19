## Dodelo de datos en mongoDB

- Modelos   
    - Embebidos
    - Referenciados
- Modelo Embebidos

**Uno a Uno**

```json
db.deparatmentos.insertMany([{
    _id:1,
    nombre:'Tecnologias de informacion',
    responsable: {
        nombre:'Monico',
        apellidos:'Ramirez Perez'
    },
    descripcion:'Ejemplo de departamentos'
},
{
 _id:2,
    nombre:'Contabilidad',
    responsable: {
        nombre:'Raul',
        apellidos:'Lopez Hernandez'
    },
    descripcion:'Ejemplo de departamentos'   
}
])
```

- Modelos Referenciados

**Uno a uno**

```json
db.localidades.insertMany([
    {
        _id:'BA',
        ciudad:'Buenos Aires',
        pais: 'Argentina',
        poblacion: '16 millones',
        turismo:['edificios', 'Tango', 'gastronimia', 'museos', 'parques'],
        direccion:'Av Tortolos',
        cod_departamento:1
    },
    {
         _id:'BR',
        ciudad:'Brasilia',
        pais: 'Brasil',
        poblacion: '20 millones',
        turismo:['edificios', 'Tango', 'gastronimia', 'museos', 'parques', 'playas'],
        direccion:'Av Toretto',
        cod_departamento:2
    }
])
```

```json
db.departamentos.aggregate(
[
    {
        $lookup:{
            from:'localidades',
            localField:'_id', 
            foreignField:'cod_departamento',
            as: 'localidades'
        }
    }
])
```

```json
db.productos.insertMany([
    {
        _id:1,
        nombre:'Jabon',
        precio:20,
        existencia:10,
        cod_catego:1
    },
    {
        _id:2,
        nombre:'Pala',
        precio:20,
        existencia:10,
        cod_catego:2
    }
])
```

```json
db.categorias.insertMany([
    {
        _id:1,
        nombre:'Limpeza'
    },
    {
        _id:2,
        nombre:'Herramientas'
    }
])
```