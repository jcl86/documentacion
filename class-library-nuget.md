# Montar una librería de clases

Este documento describe los pasos seguidos [este video](https://www.youtube.com/watch?v=hilY0lLxaOs&t=1544s)

### 1. Creación del proyecto

Primero, creamos un repo en github y lo clonamos, o iniciamos un repositorio en local y le ponemos como remote el repositorio de github creado.

Una vez tenemos git funcionando, montamos una solución en visual studio, con al menos dos proyectos:

1. La librería de clases 

2. Un proyecto con tests unitarios, que tendrá una referencia a la librería de clases


### 2. Definir ficheros de configuración

Podemos definir un fichero *global.json*, que guardará la versión necesaria para que la solución funcione. Así, si te descargas el código y no tienes el sdk necesario, te avisará de que necesitas esa versión concreta

````
dotnet new global.json
````

También podemos definir otro fichero, *dotnet-tools.json*. Con el siguiente comando: 
````
dotnet new tool-manifest
````
Este fichero guarda la relación de herramientas que hemos instalado en la solución. Así, si alguien se descarga el repositorio, solo tiene que hacer *dotnet tool restore* para que se restauren y tener disponibles las herramientas especificadas.


Ahora, procemos a crear 3 ficheros:

##### build/dependencies.props

Creamos una carpeta en la raiz del repositorio llamada *build*, y dentro un fichero llamado *dependencies.props*. Este fichero contendrá algunas variables, que despues referenciaremos en los ficheros csproj
````
<Project>
    <PropertyGroup Label="Default SDK Versions">
        <NetCoreAppVersion>netcoreapp3.1</NetCoreAppVersion>
        <NetStandardVersion>netstandard2.0</NetStandardVersion>
    </PropertyGroup>
</Project>
````

En el ejemplo, me he definido dos variables, una con la versión de netocre que voy a usar, y otra con la versión de netstandar. De esta manera, cuando tenga multiples proyectos en mi solución, en vez de que cada uno referencia el sdk concreto, hara referencia a la variable definida. En el momento que quiera subir o bajar alguna versión, lo actualizaré en el fichero que acabo de crear.

