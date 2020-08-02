# Montar una librería de clases

Este documento describe los pasos seguidos [este video](https://www.youtube.com/watch?v=hilY0lLxaOs&t=1544s)

### 1. Creación del proyecto

Primero, creamos un repo en github y lo clonamos, o iniciamos un repositorio en local y le ponemos como remote el repositorio de github creado.

Una vez tenemos git funcionando, montamos una solución en visual studio, con al menos dos proyectos:

1. La librería de clases 

2. Un proyecto con tests unitarios, que tendrá una referencia a la librería de clases


### 2. Definir ficheros de configuración

Podemos definir un fichero *global.json*, que guardará la versión necesaria del dsk de netcore para que la solución funcione. Así, si te descargas el código y no tienes el sdk necesario, te avisará de que necesitas esa versión concreta

````
dotnet new global.json
````

También podemos definir otro fichero, *dotnet-tools.json*. Con el siguiente comando: 
````
dotnet new tool-manifest
````
Este fichero guarda la relación de herramientas que hemos instalado en la solución. Así, si alguien se descarga el repositorio, solo tiene que hacer *dotnet tool restore* para que se restauren y tener disponibles las herramientas especificadas.


Ahora, procedemos a crear 3 ficheros:

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

En el ejemplo, me he definido dos variables, una con la versión de netocre que voy a usar, y otra con la versión de netstandard. De esta manera, cuando tenga multiples proyectos en mi solución, en vez de que cada uno referencia el sdk concreto, hara referencia a la variable definida. En el momento que quiera subir o bajar alguna versión, lo actualizaré en el fichero que acabo de crear.

El fichero csproj de la librería de clases haciendo referencia a la variable quedaría así:
````
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>$(NetStandardVersion)</TargetFramework>
  </PropertyGroup>
</Project>
````

#### Directory.Build-props

En la raiz del repositorio, creamos un fichero llamado Directory.Build.props
````
<Project>
    <Import Project="build/dependencies.props"/>
    <PropertyGroup Label="Default Metadata">
        <Authors>Authors name</Authors>
        <Company>Company name</Company>
        <RepositoryUrl>repo url</RepositoryUrl>
    </PropertyGroup>
</Project>
````
Este fichero refernecia al que hemos creado anteriormente, *build/dependencies.props*, y establece una serie de propiedades que serán comunes a todos los proyectos. En este caso hemos definido el autor, la empresa que realiza la aplicación y la url del repositorio donde se aloja el código. De esta manera, tendremos estas propiedades centralizadas en un solo fichero, en lugar de estar desperdigadas por los múltiples ficheros csproj que contenga la solución.


#### Directory.Build.targets

En la raiz del repositorio, creamos un fichero llamado Directory.Build.targets
````
<Project> 
    <ItemGroup Label="Test">
        <PackageReference Update="Microsoft.NET.Test.Sdk" Version="16.5.0" />
        <PackageReference Update="xunit" Version="2.4.0" />
        <PackageReference Update="xunit.runner.visualstudio" Version="2.4.0" />
        <PackageReference Update="coverlet.collector" Version="1.2.0" />
    </ItemGroup>
</Project>
````
En este fichero definiremos todas las referencias a paquetes nuget que tengamos en cualquiera de los proyectos de la solución. Esto nos permitirá eliminar la referencia a la versión en cada uno de los csproj. Por ejemplo, mi csproj del proyecto de tests unitarios quedaría así:

````
<Project Sdk="Microsoft.NET.Sdk">

  ... omited code for brevity ...
  
  <ItemGroup>
    <PackageReference Include="Microsoft.NET.Test.Sdk" />
    <PackageReference Include="xunit" />
    <PackageReference Include="xunit.runner.visualstudio" />
    <PackageReference Include="coverlet.collector" />
  </ItemGroup>
  
  ...
  
</Project>
````

Esto nos permite definir en un único fichero todos los paquetes nuget que estamos consumiendo, con su versión completa. Cuando vaya añadiendo proyectos a mi solución, y estos proyectos tengan que consumir los mismos paquetes nuget que ya consumen otro proyectos, referenciarán todos las misma versión que tiene el fichero Directory.Build.targets.

Centralizando las versiones en un mismo fichero, evitamos que distintos proyectos en la solución consuman el mismo paquete nuget, pero diferentes versiones. Esto provocaría warnings de compilación o errores, y resolverlos suele ser una tarea ardua y engorrosa.


###
