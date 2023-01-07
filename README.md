# Curso Avanzado de Django

Sitios increíbles como Platzi, Instagram, Pinterest o el portal del New York Times tienen algo en común: todos usan Django. Aprovecha su versatilidad, potencia y rapidez para mostrar tus ideas y dar rienda suelta a tu creatividad. Aprende Django con Platzi y ¡haz de tu próximo sitio web el mejor que el mundo haya visto!

- Diseñar y construir APIs rest
- Implementar Test Driven Development
- Aplicar y resolver problemas reales de la industria
- Implementar procesos de testing

Lo que verás a continuación son mis apuntes sobre el [curso](https://platzi.com/cursos/django-avanzado/) 🚀 Si ves algún error o punto de mejora no dudes en hacer tu aporte 💚

Este curso es la continuación del [curso intermedio de Django]()

## Cimientos

### Arquitectura de una aplicación

Un backend developer es un diseñador, las decisiones son costosas y dificiles de cambiar

**Arquitectura:** Tomar decisiones sobre la estructura fundamental que van a implicar costos y van a ser dificiles de cambiar una vez implementadas.

La arquitectura orientada en servicios consiste en crear una API que se comunica entre el backend y el front-end
Para el consumidor lo que pasa en la aplicación es una caja negra. Estas son autocontenidas.

**Rest:** Depende del protocolo http usando status code.

### The twelve factor app

Principios:

- Formas declarativas de configuración
- Un contrato claro con el OS
- Listas para lanzar
- Minimizar la diferencia entre entornos
- Fácil de escalar

**Codebase**: Se refiere a que nuestra app siempre debe estar trackeada por un sistema de control de versiones como Git, Mercurial, etc. Una sola fuente de verdad.

**Dependencias**: Una 12 factor app nunca debe depender de la existencia implícita de nuestro OS, siempre se declaran explícitamente qué dependencias usa el proyecto y se encarga de que estas no se filtren. Dependency Isolation.

**Configuración**: Acá nos referimos a algo que va a cambiar durante entornos.

**Backing services**: Estos pueden ser conectados y desconectados a voluntad. Es cualquier servicio que nuestra aplicación puede consumir a través de la red como Base de Datos, Mensajería y Cola, Envío de Emails o Caché.

**Build, release, run**: Separa estrictamente las etapas de construcción y las de ejecución. Build es convertir nuestro código fuente en un paquete.Release es la etapa donde agregamos a nuestro paquete cosas de configuración como variables de entorno y Run donde corremos la aplicación en el entorno correspondiente. Las etapas no se pueden mezclar.

**Procesos**: En el caso más complejo tenemos muchos procesos corriendo como Celery y Redis, en esta parte los procesos son stateless y no comparten nada. Cualquier dato que necesite persistir en memoria o en disco duro tiene que almacenarse en un backing services,

**Dev/prod parity**: Reducir la diferencia entre entornos para reducir tiempo entre deploys y las personas involucradas sean las mismas que puedan hacer el deploy

**Admin processes**: Tratar los procesos administrativos como una cosa diferente, no deben estar con la app.

### Codebase

Todo corriendo en **docker**

**Django** 

**PostgreSQL**

**Redis**

**Celery** Está compuesto de tres servicios: broker beat y flower, una interfaz gráfica a saber lo que pasa en celery


### Docker

Para evitar estar usando el argumento `-f local.yml` en cada comando `docker-compose` podemos usar el siguiente comando:
```zsh
export COMPOSE_FILE=local.yml
```
Así, ya podemos ejecutar:
```zsh
docker-compose build
docker-compose up
docker-compose ps
docker-compose down
```

Para construir el contenedor del proyecto con todas sus imagenes ejecutamos:  
```zsh
docker-compose -f local.yml build
```

Para ejecutar el contenedor:
```zsh
docker-compose -f local.yml up
```
Si queremos información sobre docker:
```zsh
docker-compose ps
# Nos ayuda a ver las imagenes que corren en docker

docker images
# Tendremos una lista de las imagenes creadas
```

Para ejecutar algo en el proyecto se debe ejecutar dentro de docker, por lo que cada comando debe ir acompañado de `docker-compose`

El argumento --rm elimina el contenedor después de ejecutar el comando

Para correr el contenedor de django en otra terminal se debe eliminar y luego correr en una nueva.

Cuado ejecutamos `docker-compose -f local.yml up` debemos revisar con qué nombre esta el contenedor django:
`docker-compose ps`
Luego lo eliminamos y lo volvemos a correr
```zsh
docker-compose -f local.yml ps
docker rm -f django
docker-compose -f local.yml run --rm --service-ports django
```
Ahora tendremos una consola donde corre django y otra donde corren los demás servicios

Para hacer operaciones debemos abrir una tercer consola.

Una de las operaciones más utiles es abrir la shell de django:
```
docker-compose run --rm django python manage.py shell_plus
```

### Comandos de administración


```
docker-compose run --rm django COMMAND

docker-compose run --rm django python manage.py createsuperuser

```
### Más comandos

```zsh
docker container
docker images
docker volume
docker network
```

## Modelos  

Desde los archivos `admin.py` se configura qué se ve en el administrador de archivos de DJango, se debe tener en cuenta que el super usuario se debe crear cada vez que se ejecuta un contenedor ya que la base de datos vive en el contenedor y no en el proyecto.

Los datos se deben importar en el contenedor ya que la base de datos vive en docker.

Las dependencias hacen parte de las imagnes por eso se deben reconstruir cada vez que hayan cambios.





## Introducción a Django REST Framework

Rest es un tip de arquitectura.

Una api devuelve respuestas http a solicitudes de cada usuario.

Decorator [`@api_view`](https://www.django-rest-framework.org/api-guide/views/#api_view)

The core of this functionality is the api_view decorator, which takes a list of HTTP methods that your view should respond to. For example, this is how you would write a very simple view that just manually returns some data

Va encima de una vista y el objeto request se convierte de REST así como el objeto response.


### Serializers

Los serializers son contenedores que nos permiten tomar tipos de datos complejos, convertirlos en datos nativos de python para después poderlos usar como JSON o XML. Son contenedores que amoldan datos para que cumplan con las condiciones de los serializers y sean llevados a un tipo de estos y después estos puedan ser transformados en otra cosa.

Un buen rendimiento de serializer puede ayudar a evitar la escritura de código innecesario.

Se declaran en `selializers.py` en la carpeta de cada aplicación.

Un serializer se encarga de el trabajo de comunicación de datos en la api por las respuestas http.


### Buenas prácticas para el diseño de una API REST

Uno de los prerequisitos para crear APIs es conocer el protocolo HTTP. Verbos, métodos, estados y las cabeceras.

Van a estar diseñando una interfaz para programadores para que otros programadores puedan interactuar, nos olvidaremos de los templates para que un equipo de Frontend se encargue de eso. Debemos tener la perspectiva de un usuario de API y no la de un diseñador de API.

El objetivo es algo que siempre se deben preguntar qué problema deben de resolverle al usuario final nuestra API. El éxito de nuestra API se mide por qué tan rápido nuestros compañeros pueden usarla.

Es importante usar plurales en vez de singulares.

**REST**: Es una serie de principio de cómo diseñar una web service. Un estilo de arquitectura.

**HTTP** Status Code:

- 201: Creado
- 304: No modificado
- 404: No encontrado
- 401: No autorizado
- 403: Prohibido o restringido.

Pro tips:

- SSL
- Caché
- Valida
- CSRF o Cross-Site Request Forgery
- Limita los requests
- Complementa tu API con un SDK


### Request, response, renderers y parsers

**Parsers:** Tipo de formato para las respuestas o solicitudes

**Renderers**: La forma en la que se visualizan los datos.

Es una ayuda importante al momento de consultar la API a través del navegador, ya que se pueden configurar distintas formas de representación de datos o ingreso de los mismos. Incluso podemos visualizar formatos como Latex o PNG.

## Real DRF

### Autenticación y tipos de autenticación

La autenticación es la parte de asociar una petición a un usuario y después al objeto request se le asigna dos propiedades como request.user y request.auth





- **O Auth** (short for "Open Authorization") is an open standard for access delegation, commonly used as a way for internet users to grant websites or applications access to their information on other websites but without giving them the passwords.This mechanism is used by companies such as Amazon, Google, Facebook, Microsoft, and Twitter to permit the users to share information about their accounts with third-party applications or websites.


- **Json Web Tokens** 



### API View




### Creando token de autorización





## Tareas Asíncronas





## Testing





## Django Admin




## Deployment

















# Helpful tips

- Para crear un entorno virtual ejecuta:  
`python3 -m venv .venv`  

- Para evitar estar usando el argumento `-f local.yml` en cada comando `docker-compose` podemos usar el siguiente comando:
    ```zsh
    export COMPOSE_FILE=local.yml
    ```

# Helpful Links

- [.gitignore](https://www.toptal.com/developers/gitignore)

- [Basic writing and formatting syntax](https://docs.github.com/es/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
  - ----
- [Django documentation](https://docs.djangoproject.com/en/3.2/)

- [The Twelve-Factor App](https://12factor.net/)

- [Django REST framework](https://www.django-rest-framework.org/)

- [Serializers](https://platzi.com/clases/1461-django-avanzado/17198-serializers/)

- [Web API Design](https://pages.apigee.com/rs/apigee/images/api-design-ebook-2012-03.pdf)

- [Buenas prácticas al crear una API](https://platzi.com/clases/1461-django-avanzado/17199-buenas-practicas-para-el-diseno-de-un-api-rest/)

- [Authentication](https://www.django-rest-framework.org/api-guide/authentication/)

- [Permissions](https://www.django-rest-framework.org/api-guide/permissions/)
