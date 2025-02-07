
#CRUD y consultas en MongoDB



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