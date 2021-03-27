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
Con el parametro -o (de output), le indicamos la ruta donde queremos que se cree el proyecto. Recordad que con la tecla tab nos autocompleta, no es necesario escribir la ruta letra por letra.

Ahora, queda agregar en la solución las referencias a los proyectos:
````
dotnet sln Ecommerce.sln add src\Ecommerce.Host\
dotnet sln Ecommerce.sln add src\Ecommerce.Domain\
dotnet sln Ecommerce.sln add src\Ecommerce.Infraestructure\
dotnet sln Ecommerce.sln add test\Ecommerce.FunctionalTests\
````

Por último, ya podemos abrir visual studio y empezar a atrabajar sobre esta solución. 

````
Ecommerce.sln
````

Cabe decir que todos estos pasos se pueden realizar desde el propio IDE, pero desde la consola, una vez que conoces los comandos necesarios, acaba resultando más rápido.
