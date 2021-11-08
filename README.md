# Manual moderno de aplicaciones .Net (En preparación)

Esta es una aproximacíon para hacer aplicaciones mantenibles en .Net. Es la consecuencia del camino que he seguido a lo largo de algunos años, creando y manteniendo aplicaciones de tamaño mediano, unido a todas las enseñanzas que he tomado prestadas de los links en la bibliografía. Las arquitecturas aquí expuestas tratan de seguir buenas prácticas,   con bastante influencia de arquitectura hexagonal, implementar tests funcionales y de integración útiles, integración y despliegue continuo con github actions.

Por supuesto, el esfuerzo que supone montar una solución con esta separación solo merecerá la pena para proyectos de larga duración y de cierta complejidad. Para webs de un solo uso, que no haya que mantener nunca, o que sean muy sencillas, lo mejor es tirar de las plantillas prediseñadas y no liarse la manta a la cabeza.

Intentaré mantener esta documentación, de manera que se mantenga lo más actualizada posible al respecto de las últimas versiones del framework.

# Backend

Librería de clases para compartir código
- Para publicar en nuget
- Para publicar en github packages

[Librerías de clase](nuget.md)

[Montar una librería de clases que se despliege en Nuget](class-library-nuget.md)

Arquitectura hexagonal con:
- Domain, con Servicios de aplicación y dominio
- Infraestructure -> Ef core, y demás tipos de infraestructura
- Model -> Viewmodels o dtos (Dependiendo de si la aplicación es ssr o api)
- Domain Tests

Aplicación web api
- Arquitectura hexagonal
- Api para Test Server y para Host.

[Montar una api rest](api-rest/index.md)

# Frontend

Aplicación cliente Html + css + js

[Webpack world](frontend/index.md)

Aplicación cliente Blazor
- Parte cliente (instalar webpack e integración con tailwind)
- Scripts
- Librería cliente con repositorios
- Testing (Pendiente)

[Script despliege manual app blazor](deploy-blazor-app-manually.md)

# Fullstack

Aplicacion web mvc ssr
- Arquitectura hexagonal
- Parte cliente (instalar webpack y compilar js)
- Test funcionales (Dejar las vistas en razor class lib y hacer del proyecto de xUnit tipo web)

[Serilog](serilog.md)


### GIT

[Gestión basica](git/basic-git.md)

[Gestión avanzada](git/advanced-git.md)


### Otros tutoriales de interés

[Azure webapps](azure-webapps.md)

[Linux](linux.md)

[Old IIS things](old-iis.md)

[Tips y consejos varios](tips.md)

##### Bibliografía general:

- [Github metup setup](https://github.com/madriddotnet/meetup-github-setup)
- [Xabaril](https://github.com/Xabaril/ManualEffectiveTestingHttpAPI)
- [Codely](https://pro.codely.tv/library/?category=Arquitectura+de+Software)
