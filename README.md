# Curso Avanzado de Django

Sitios increíbles como Platzi, Instagram, Pinterest o el portal del New York Times tienen algo en común: todos usan Django. Aprovecha su versatilidad, potencia y rapidez para mostrar tus ideas y dar rienda suelta a tu creatividad. Aprende Django con Platzi y ¡haz de tu próximo sitio web el mejor que el mundo haya visto!

- Diseñar y construir APIs rest
- Implementar Test Driven Development
- Aplicar y resolver problemas reales de la industria
- Implementar procesos de testing

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





# Helpful Links

- [.gitignore](https://www.toptal.com/developers/gitignore)

- [Basic writing and formatting syntax](https://docs.github.com/es/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)

- [Django documentation](https://docs.djangoproject.com/en/3.2/)

- [The Twelve-Factor App](https://12factor.net/)