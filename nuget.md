# Como crear un paquete nuget manualmente

[Fuente](https://www.variablenotfound.com/2019/03/como-crear-un-paquete-nuget-y.html)

### 1. Tener una cuenta en nuget.org.

Una vez en la web, obtener tu api key en tu perfil (Caduca cada 365 días como mucho)

### 2. Configurar el paquete nuget

- Botón derecho sobre el proyecto -> Propiedades -> Pestaña Paquete
- Darle un nombre único

Opcionalmente, si se quiere, se puede dar soporte para sourcelink, añadiendo en el csproj del proyecto 
````
<PublishRepositoryUrl>true</PublishRepositoryUrl>
<RootNamespace>{NombrePaquete}</RootNamespace>
<AssemblyName>{NombrePaquete}</AssemblyName>

<ItemGroup>
  <PackageReference Include="Microsoft.SourceLink.Bitbucket.Git" Version="1.0.0" PrivateAssets="All"/>
</ItemGroup>
````

### 3. Tener configurado el cliente nuget

- Bajarse la última versión del cliente en nuget.org/downloads (4.1.0+ es la versión necesaria para publicar paquetes en nuget.org)
- Guardar la descarga en un sitio localizado. **El archivo no es un instalador.**
- Agregar la carpeta donde colocó nuget.exe a la variable de entorno de RUTA DE ACCESO para usar la herramienta CLI desde cualquier lugar.

*Con copiar el fichero que te descargas (nuget.exe) dentro de la carpeta Cmder/bin valdría*

### 4. Publicar el paquete

- Cambiar a Release
- Compilar la solución
- Menu Compilar -> Paquete nombre del paquete
- Abrir cmder en la carpeta donde se ha compilado el fichero nupkg
- Ejecutar:
````
nuget push nombreFichero.1.0.0.nupkg -ApiKey valorApikey -Source https://api.nuget.org/v3/index.json
````
