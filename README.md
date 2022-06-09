Universiad de San Carlos de Guatemala ![](/image/logousac.png)
Escuela de Ciencia y Sistemas
Facultad de Ingeniería
Laboratorio de Software Avanzado

# *** Proyecto de Laboratorio Fase 1 ***


### Hayrton Omar Ixpatá Coloch - 201313875

## yoVoto

yoVoto es un sistema de voto electrónico que permite a los ciudadanos del  país de manera segura y confiable votar a través de una página web o  teléfono inteligente. Este diseño facilita votar a personas localizadas en el  extranjero y en el territorio nacional utilizando el internet y dispositivos  móviles. Esta facilidad del voto sin embargo requiere mecanismos  especiales de seguridad y transparencia. Por lo que parte de los  requerimientos técnicos incluyen las garantías como: voto anónimo  (secreto) y voto único.

## Analisis de Solución propuesta
- Comunicacion entre Servicios
Para la construccion del proyecto se implementara una arquitectura SOA que tendra varios servicos que funcionaran de manera independiente aumentando la robustez del sistema, con esto evitamos de que un servicio falle afecte a todo nuestro sistema, para ello se propone el siguiente diagrama.

![](/image/arquitectura.png)


- Diagrama de actividades del microservicio de autenticación.

![](/image/diagramaActividadesAutenticacion.png)

- Diagrama de actividades de emisión de voto

![](/image/EmisionVoto.png)

- Diagrama de actividades del sistema de registro (enrollamiento) de ciudadanos.

![](/image/RegistroCiudadano.png)

- Descripción de la seguridad de la aplicación.
Para la seguridad de los datos se utilizara JWT, que define un mecanismo para poder programa entre dos partes y de forma segura, que genera un token que tiene tres partes codificadas en Base64.

Ademas de utilizar un token de autenticacion que validara que el usuario que se registro solo utilice un dispositivo, para ello se enviara un token al dispositivio que debera de ingresar el ciudadano con esto se validara que solo se utlice un unico dispositivo.


- Descripción del uso de la tecnología blockchain.
El blockchain, traducido del inglés como cadena de bloques, es una base de datos tecnológica y compartida que funciona como si fuera un libro en el que se registran las operaciones de compra-venta y otras transiciones.
Es un registro público donde se comparten todas las transacciones jamás realizadas sobre algo en concreto, impidiendo de esta manera que se produzcan falsificaciones.
Cada operacion que se realiza se registra en todos los ordenadores participantes en la cadena incluyendo datos como cantidad, fecha, operacion y participantes, al completarse la operacion y hacerse pubulica no se podra borrar, con esto se evita que se pueda falsificar la cadena

![](/image/funciona-blockchain.png)
Fuente:https://labeabogados.com/digital-tecnologia/el-funcionamiento-del-blockchain/

# Documentación de las Pipelines para los servicios.

## Microservicios
#### Login
Este microservico se encargara de validar el ingreso de los ciudadanos que emitaran su voto por medio de las credenciales que furon proporcionados al realizar su registro.

Ejemplo de entrada:

{
    "dpi":"1111222223333",
    "contrasenia":"123abc"
}

#### Registro de usuario
Este microsercicio se encargara realizar el registro del ciudadano para que pueda realizar su voto cuando las votaciones esten activas.

Ejemplo de entrada:

{
    "cui":"111122222333",
    "userAgent":"Android",
    "telefono":"12345678",
    "fotografia":"<base64>",
    "fotografia_dpi":"<base64>",
    "pin":"123456",
    "correo":"micorreo@correo.com",
    "fecha":"01/01/2022",
    "ip":"192.168.0.2",
    "Departamento":"Guatemala",
    "Municipio":"Guatemala",
    "Ubicacion":"-55555555,22222222",
    "token":"123456789"
}

#### Eleccion
Este microservicio se encragara de la creacion de una eleccion.

ejemplo:

{
    "titulo":"elecion_2022",
    "descripcion":"primera vuelta",
    "fecha_inico":"01/01/2022 07:00:00",
    "fecha_fin":"01/01/2022 22:00:00"
}

#### Ciudadano
En este microservicio servira para poder simular los datos almacenados en RENAP, donde podremos obtener si el ciudadano es apto para votar:

ejemplo:

{
    "cui":"1111222223333",
    "fecha":"01/01/1999"
}

#### Voto
En este microservicio podra ingresar el boto seleccionado por el usuario, este debera de ser secreto.

ejemplo:

{
    "tituloVotacion":"selecion",
    "imagen":"<base64>"
    "Votacion": {
        "clave":"FAS123AD",
        "partido":"partido",
        "tipo":"presidente"
    }
        
}

#### ResultadoVotacion
En este microservicio se podra obtener los resultados de la votacion, ya sea preseidente, viceprecidente o dependiendo de la entidad publica que se esta eligiendo.

ejemplo:

{
    "resultado":{
        "presidente":{
            "votosValidos":1000,
            "votosNulos":50,
            "partido":[{
                "nombre":"luchador",
                "total":100
            },
            {
                "nombre":"fuerte",
                "total":98
            },
            ...
            ]
        }
        vicepresidente:{
            "votosValidos":1000,
            "votosNulos":50,
            "partido":[{
                "nombre":"luchador",
                "total":100
            },
            {
                "nombre":"fuerte",
                "total":98
            },
            ...
            ]
        }
    }

}

#### Auditoria
En este microservicio se podran obtener los votos que fueron realizado por los ciudadanos con fecha y hora realizada.

ejemplo:

{
    "votosValidos":1000,
    "votosNulos":50,
    "votos":[
        {
            "titulo":"valido",
            "fecha":"01/01/2022 07:59:00"
            "localidad":"111111,1111111"
        },
        {
            "titulo":"nulo",
            "fecha":"01/01/2022 08:59:00"
            "localidad":"111111,1111111"
        }
        ...
    ]
}