![alt text](img/top.png)



## Integrantes
<div>
<img align="left" src="img/bryan.png" width="45%"/>
<img align="left" src="img/brandon.png" width="45%"/>
<img align="left" src="img/ed.png" width="45%"/>
</div>
<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>

## Introducción

<img align="left" src="img/ii.png" height="220px"/>

```
----------------------------------------------------------------------
Los procesos de una empresa son fundamentales para lograr con éxito el 
resultado esperado de un producto o servicio dirigido al consumidor. 
    
Las empresas, su coordinación de sus departamentos, saber su estado s-
uele ser un punto fundamental para que se llegue el proceso adecuado 
para brindar con éxito lo que ofrecen cada empresa. 
    
Para saber que todos sus departamentos están en sincronía entre ellos 
y que estén en un mismo canal se suele aplicar auditorías a diversos 
departamentos de una empresa, pero estos procesos son algo lentos y e-
n ocasiones la misma empresa contrata a servicios externos que realic-
en esta función y correr el riesgo de no estar familiarizado con la e-
mpresa y se tome decisiones erroneas o el enfoque principal se pierda.
----------------------------------------------------------------------
```
<br>

## Propuesta Técnica

<img align="right" src="img/ft.png" height="220px"/>

```
------------------------------------------------------------
El posesos de las auditorias antes mencionado las cuales son 
aplicadas a las empresas para la mejora continua de estas m-
ismas, pensamos como equipos que sería viable hacer una API 
para agilizar los procesos de auditorías de cualquier índol-
e aplicada en cualquier departamento de una empresa, hacer 
que el proceso de planeación de la auditorias sea más agiles, 
ayudar a ver resultados de movimientos y su desempeño en un 
lapso de tiempo.
---------------------------------------------------------
```
<br>


## Modelo Enitdad - Relacion de la DB


Se muestra las entidades, metodos y  las relaciones de la **DB** 

![alt text](img/img.png)

## Modelo Relacional


![alt text](img/diaga.png)


###Dependencias
Esta API Rest esta construida con los siguientes paquetes/herramientas:
- .Net Core (3.1)
- Microsoft.AspNetCore.Authentication.JwtBearer (3.1.8)
- Microsoft.EntityFrameworkCore (3.1.7)
- Microsoft.EntityFrameworkCore.Tools(3.1.7)
- Pomelo.EntityFrameworkCore.MySql(3.1.2)

### Estructura del proyecto
Este proyecto funciona con el patron de diseÃ±o **repository**.
####Elementos del proyecto
- Controladores: Son clases que heredan de *ControllerBase*, en dÃ³nde tenemos la colecciÃ³n de nuestros endpoints.
- Modelos: Son las clases que contienen los atributos que posteriormente serÃ¡n convertidas a tablas y campos.
- IRepository e IService: Son interfaces las cuÃ¡les nos ayudan a que los servicios y los repositorios sigan un contrato y esten atados a implementar los prototipos de los mÃ©todos dentro de nuestras interfaces (Nota: *Ambas inerfaces van a tener los mismos mÃ©todos*)
- Services: En estas clases esta la lÃ³gica del negocio, implementan la interfaz IService que le corresponda a cada clase, sirven como middleware para que el controlador y los repositorios se comuniquen entre si.
- Repositories: Contiene una instancia del contexto de la base de datos, con la cual realizan operaciones.
- Context: Es una clase que hereda de DbContext, aquÃ­ establecemos las instancias de DbSet (tablas en la base de datos), le pasamos la clase que vamos a convertir en una entidad asÃ­ como el nombre que va a tener en la base de datos.
- DTO: Son objetos auxiliares que nos ayudan a ser peticiones en los endpoints a los cuales no tenemos un modelo que cumpla con la estructura que necesita el body de las peticiones
- Utils: Es la carpeta con las clases que nos ayudan a realizar procesos en nuestra aplicaciÃ³n, como encriptar contraseÃ±as u obtener los claims de un JWT.
####Diagrama secuencial del patrÃ³n de dsieÃ±o

```seq
Frontend->Controlador:PeticiÃ³n
Controlador->Service: Puente hacÃ­a repository
Service->Repository: OperaciÃ³n con la base de datos
Repository->Service: Devuelve una tarea
Service->Controlador: Puente hacÃ­a controlador
Controlador->Frontend: Respuesta de la peticiÃ³n
```
####Sistema de carpetas
La vista principal del proyecto es la siguiente


####DistribuciÃ³n de archivos
+ Controllers: Contiene los controladores del proyecto.
+ Domain:
    + IServices: Contiene las interfaces que van a implementar los service.
    + IRepositories: Contiene las interfaces que van a implementar los repository.
    + Models: Son las clases que van a ser convertidas en entidades a la base de datos.
+ DTO: Son los objetos auxiliares los cuales, los cuales tienen atritbutos que se adaptan a los parÃ¡metros del body en la peticiÃ³n.
+ Migrations: AquÃ­ se almacenan los procesos de las migraciones que realizemos.
+ Persistence: Es la carpeta en la que tenemos archivos relacionados a la BD.
    * Context: Es el contexto de la base de datos, en dÃ³nde definimos las entidades que vamos a agregar.
    * Repositories: Los repositorios que hacen peticiones directamente a la base de datos.
+ Services: La carpeta de nuestra lÃ³gica de negocio que funciona como middleware entre controladores y repositorios.
+ Utils: AquÃ­ se guardan todos las clases que nos puedan utilizar a lo largo del proyecto que nos ayudan a realizar diferentes operaciones. 

####Proceso de creaciÃ³n de un nuevo componente.
1. Se crea una interfaz para el servicio y el repositorio dentro de la carpeta correspondiente (Nota: Al momento de crearse, las interfaces deben ser pÃºblicas).
2. Ahora se tiene que crear una clase repository, con una instancia *readonly* de la base de datos para hacer las operaciones correspondientes, esta clase debe de implementar a su respectivo IRepository.
3. Se crea una clase services que va a tener una instancia *readonly* del repositorio, esta clase tiene que implementar a su respectivo IService.
4. Por Ãºltimo, se crea un controlador que herede de *ControllerBase*, este controlador tambiÃ©n va a tener una instancia *readonly* del service al que le corresponde.
5. Para poder utilizar los servicios en el controlador, necesitamos ir al archivo *Startup.cs*, buscar el mÃ©todo *ConfigureServices* e y agregar los servicios y el repositorio de la siguiente manera:

```cs
        public void ConfigureServices(IServiceCollection services)
        {

            //Servicos
            services.AddScoped<InterfazService, MiService>();

            //Repositorios
            services.AddScoped<InterfazRepository,MiRepository>();
		}
````
> NOTA: Si algÃºn servicio o repositorio no implementa una interfaz, el cÃ³digo marcarÃ¡ error.

Con esto ya podemos trabajar en nuestro nuevo controlador y programando los endpoints.

####Configurando otros service
Dentro de este mÃ©todo service ya tenemos nuestra base de datos configurada, asÃ­ como el CORS para recibir peticiones del frontend, si se desea agregar algÃºn otro servicio en el futuro, se tiene que inyectar en este mÃ©todo para que la aplicaciÃ³n lo pueda utilizar.
###Controladores y endpoints

------------


####UsuarioController
Dentro de este controlador vamos a tener los endpoints relacionados con las operaciones del usuario, como la alta y baja o cambio de contraseÃ±as
#####POST
*Nivel de acceso: PÃºblico*
*Requiere token: No*
*DescripciÃ³n: Agrega un usuario a la base de datos*
*ParÃ¡metros del body:  Modelo Usuario*
#####GET
*Nivel de acceso:* Solo usuarios RH
*Requiere token: *Si
*DescripciÃ³n:* Devuelve la lista de usuarios en la base de datos
*ParÃ¡metros del body*: Ninguno
#####PUT
*Ruta de acceso:* /CambiarPassword
*Nivel de acceso*: Usuarios dentro de la base
*Requiere token:*Si
*DescripciÃ³n:*Cambia la contraseÃ±a de un usuario en la base
*ParÃ¡metros del body:*Recibe un modelo cambiarPasswordDTO con la contraseÃ±a actual y la nueva
#####DELETE
*Nivel de acceso*: Solo usuarios RH
*Requiere token:* Si
*DescripciÃ³n:* Elimina un usuario de la base de datos con su id
*ParÃ¡metros de la peticiÃ³n:* Al final del link de la peticiÃ³n se agrega un /id=(id usuario a borrar)

------------

####LoginController
Contiene un Ãºnico endpoint que se utiliza para hacer el login al sistema.
#####POST
*Nivel de acceso*: PÃºblico
*Requiere token:* No
*DescripciÃ³n:*  Verifica si el usuario a ingresar es vÃ¡lido y le regresa un token
*ParÃ¡metros del body:* Recibe un objeto LoginDTO con la informaciÃ³n de inicio de sesiÃ³n del usuario

------------

####DepartamentoController
Contiene un Ãºnico endpoint que nos sirve para obtener la lista de departamentos dentro de la base.
#####GET
*Nivel de acceso*: Solo usuarios RH
*Requiere token:* Si
*DescripciÃ³n:* Devuelve la lista de los departamentos registrados en la base de datos
*ParÃ¡metros del body:* Ninguno


------------

####PregutnaController
Tiene endpoints para agregar y traer preguntas de la base de datos.
#####POST
*Nivel de acceso*: Solo usuarios RH
*Requiere token:* Si
*DescripciÃ³n:* Agrega una pregunta a la base de datos
*ParÃ¡metros del body:* Modelo Pregunta
#####GET
*Nivel de acceso*: Solo usuarios RH
*Requiere token:* Si
*DescripciÃ³n:* Devuelve la lista de preguntas que tenemos disponibles en la base de datos
*ParÃ¡metros del body:* Ninguno


------------

####AuditoriaController
AquÃ­ estan todos los endpoints que nos van a ayudar para el proceso de la adutorÃ­a
#####POST
*Nivel de acceso*: Solo usuarios RH
*Requiere token:* Si
*DescripciÃ³n:* Crea una auditoria vacÃ­a y devuelve el id (int) con el que fue registrada en la base de datos 
*ParÃ¡metros del body:* Modelo Auditoria
#####POST
*Ruta de accceso:* /CrearAuditoria 
*Nivel de acceso*: Solo usuarios RH
*Requiere token:* Si
*DescripciÃ³n:* Asigna un grupo de respuestas a una auditorÃ­a
*ParÃ¡metros del body:* Lista de modelos PreguntaRespuesta
####GET
*Nivel de acceso*: Usuarios dentro del sistema
*Requiere token:* Si
*DescripciÃ³n:* Obtiene la lista de las auditorias creadas
*ParÃ¡metros del body:* Ninguno
####GET
*Ruta de acceso:* /VerAuditoria
*Nivel de acceso*: Usuarios dentro del sistema
*Requiere token:* Si
*DescripciÃ³n:* Obtiene la lista de preguntas que fueron asignadas a una auditorÃ­a
*ParÃ¡metros del body:* Objeto IdAuditoria con el id de la auditoria que queremos ver.
####PUT
*Ruta de acceso:* /CambiarEstadoEP
*Nivel de acceso*: Solo usuarios auditores
*Requiere token:* Si
*DescripciÃ³n:* Se invoca cuÃ¡ndo se va a acceder a una auditoria, pregunta por su estado, si la auditoria esta "En Proceso", devuelve un -1, si la auditorÃ­a no estÃ¡ en proceso, cambia su estado a "En Proceso" y devuelve un 1, si el usuario no tiene permiso devuelve un 0.
*ParÃ¡metros del body:* Objeto IdAuditoria con el id de la auditoria a aplicar.
####PUT
*Ruta de acceso:* /CambiarEstadoAP
*Nivel de acceso*: Solo usuarios auditores
*Requiere token:* Si
*DescripciÃ³n:* Cambia el estado de una auditoria a "Aplicada", se debe de aplicar cuando se termina de contstar una auditorÃ­a (siguiente endpoint como referencia).
*ParÃ¡metros del body:* Objeto IdAuditoria con el id de la auditorÃ­a ya contestada.
####PUT
*Ruta de acceso:* /AplicarAuditoria
*Nivel de acceso*: Solo usuarios auditores
*Requiere token:* Si
*DescripciÃ³n:* De la lista de preguntas relacionadas a una auditoria, les cambia su respuesta inicial (la cual era nula) a una respuesta del catÃ¡logo de las respuestas vÃ¡lidas.
*ParÃ¡metros del body:* Recibe una lista con las preguntas asociadas a una auditorÃ­a, las cuÃ¡les van a ser contestadas.



![alt text](img/butt.png)
