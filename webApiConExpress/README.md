# Web api con express 


## Index.js 

```r
//Uso del modo estricto especificado en ecmascript5 
'use strict';
//Paquete para el envio de variables de sesion 
require('dotenv').config();
//Analice los cuerpos de las solicitudes entrantes en un middleware antes que sus controladores
var bodyParser = require('body-parser');
//Paquete npm para mostrar informacion de las solicitudes http 
var logger = require('morgan');
//Paquete npm para levantar el servidor 
const express = require('express');
//Puerto de coneccion usado en dockerfile
const port = process.env.PORT || 5000;
//Paquete para manejo de directorios internos del servidor 
const path = require('path');
const cors = require('cors');

//Inicializacion de la instancia de express 
const app = express();

//Lineas extras de configuracion 
app.use(logger('dev')); 
app.use(express.urlencoded({ extended: false }));
app.use(express.json());
app.use(cors({
    origin: "*" 
})); 
app.use(bodyParser.json({limit: '700mb'}));

//Direccion de las apis 
app.use('/prueba', require('./src/routes/ejemplo') );

//Levantado del servidor 
app.listen(port, function() {
    console.log('Servidor corriendo en el puerto ' + port);
});

```

## Conection.js 
Nos conectaremos a una base de datos en mongo db 
se debe de tener la contrasenña, el usuario en un archivo que este en el git ignore 
y la url por lo general es mas facil obtenerla desde donde esta la base de datos (o al menos en el caso de que esten usando mongo atlas) 
```r
const MongoClient = require('mongodb').MongoClient;
const uri = "mongodb+srv://usuario:contrasenia@nombreDelCluster.hdy5a.mongodb.net/SADB?retryWrites=true&w=majority";

const client = new MongoClient(uri, { useNewUrlParser: true, useUnifiedTopology: true });
client.connect(err => {
    if (err) {
        console.log(err)
    } else{
        console.log("¡Base de datos conectada!");
    }  
});

module.exports = client;
```

## Controladores 
```r
const mongoConnection = require('../connection/connection');

const eliminar_clientes = (req, res) => {
    const collection = mongoConnection.db('SADB').collection("usuario");
    collection.deleteMany({tipoUsuario : "cliente"}, function(err, obj){
        console.log(obj); 
        if(err){
            console.log(err); 
        }else{
            console.log("se eliminaron los usuarios de tipo cliente")
        }
    });
    collection.find({}).toArray(function (err, docs) {
        res.json(docs); 
    });
}


module.exports = {
    eliminar_clientes : eliminar_clientes
}

```

## Rutas 
```r
  var express = require('express');
  var router = express.Router();

  const ejemplo = require('../controllers/ejemplo'); 
  //endpoint
  router.get('/eliminarClientes',ejemplo.eliminar_clientes); 

  module.exports = router;

```


