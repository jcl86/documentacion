# Preparación del proyecto

Vamos a crear la solución y los proyectos necesarios utilizando cli de .NET directamente desde la consola. Yo estoy utilizando como emulador de consola (en windows) [cmder](https://cmder.net/), que tiene soporte para bash.

Para trabajar sobre un proyecto real, y haciendo un alarde de originalidad, vamos a construir una tienda online.

Primero creamos una solución vacía:

````
dotnet new sln -o Ecommerce
````

Esto nos crea una carpeta llamada Ecommerce, con un fichero Ecommerce.sln vacío. Ahora creamos do carpetas mas:

````
mkdir src
mkdir test
````

Y creamos los proyectos necesarios. Si queremos conocer los diferentes tipos de proyectos que podemos crear, haciendo dotnet new nos ofrece la lista de opciones disponibles.

````
dotnet new webapi -o src/Ecommerce.Host
dotnet new xunit -o test/Ecommerce.FunctionalTests
dotnet new classlib -o src/Ecommerce.Infraestructure
dotnet new classlib -o src/Ecommerce.Domain
````
Hemos creado un proyecto tipo webapi (Host) que alojará la Api rest, un proyecto para implemetar pruebas funcionales, y un par de librerías de clases para alojar la infraestructura y la lógica de negocio. Como podéis deducir por la división de los proyectos, intentaremos seguir un enfoque hexagonal. 

Adicionalmente, separaremos los módelos en otra librería de clases (Ecommerce.Model). Así, podremos consumir la api, tanto desde las pruebas, como desde un cliente que agregaremos un el futuro, agregando una referencia.

Con el parametro -o (de output), le indicamos la ruta donde queremos que se cree el proyecto. Recordad que con la tecla tab nos autocompleta, no es necesario escribir la ruta letra por letra.

Ahora, queda agregar en la solución los proyectos:
````
dotnet sln Ecommerce.sln add src\Ecommerce.Host\
dotnet sln Ecommerce.sln add src\Ecommerce.Domain\
dotnet sln Ecommerce.sln add src\Ecommerce.Infraestructure\
dotnet sln Ecommerce.sln add test\Ecommerce.FunctionalTests\
dotnet sln Ecommerce.sln add src\Ecommerce.Model\
````

Y agregar las referencias entre proyectos:
````
dotnet add src\Ecommerce.Domain reference src\Ecommerce.Model\
dotnet add src\Ecommerce.Host reference src\Ecommerce.Model\
dotnet add src\Ecommerce.Host reference src\Ecommerce.Domain\
dotnet add src\Ecommerce.Infraestructure reference src\Ecommerce.Domain\
dotnet add src\Ecommerce.Host reference src\Ecommerce.Infraestructure\
dotnet add test\Ecommerce.FunctionalTests\ reference src\Ecommerce.Model\
````


### Configuración adicional

Como veíamos en [Montar una librería de clases](https://github.com/jcl86/documentacion/blob/master/class-library-nuget.md), podemos hacer una serie de ajustes adicionales en la solución que, más adelante, nos facilitarán la vida. Lo más interesante sería:

- Agregar un fichero build/dependencies.props con variables comunes a todos los proyectos
- Agregar un fichero Directory.Build.targets con el compendio de versiones de paquetes nuget
- Hacer dotnet new global.json
- Hacer dotnet new tool-manifest, para que cualquiera que se baje el repo y haga dotnet tool restore, tenga disponibles las herramientas requeridas por el proyecto
- Implementar en .github/workflows un par de pipelines, de integración continua y de despliegue continuo.

Por último, ya podemos abrir visual studio y empezar a trabajar sobre esta solución. 

````
Ecommerce.sln
````

Cabe decir que todos estos pasos se pueden realizar desde el propio IDE, pero desde la consola, una vez que conoces los comandos necesarios, acaba resultando más rápido.


