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

# Como aplicar la integración continua en Github actions para publicar un paquete Nuget

Haciendo una github action. 

#### 1. Crear un fichero ci.yaml en .github/workflows

#### 2. Completarlo con algo del estilo a esto:

````
name: Publish on Nuget

on : [push]

jobs:

  build:

    runs-on: ubuntu-latest

    name: Build, pack & publish
    
    steps:

      - uses: actions/checkout@v2

      - name: Info
        run: echo "Executing ${{ github.workflow }} triggered by ${{ github.event_name }} by ${{ github.actor }}"  

      - uses: actions/setup-dotnet@v1
        with:
          dotnet-version: '3.1.x'
      - name: Build & Test
        run: |
          dotnet build
          dotnet test

      - name: publish on version change
        id: publish_nuget
        uses: rohith/publish-nuget@v2
        with:
          # Filepath of the project to be packaged, relative to root of repository
          PROJECT_FILE_PATH: src/Example/Example.csproj
          
          # NuGet package id, used for version detection & defaults to project name
          PACKAGE_NAME: AadvantageLab.Example

          # Regex pattern to extract version info in a capturing group
          VERSION_REGEX: <Version>(.*)<\/Version>

          # API key to authenticate with NuGet server
          NUGET_KEY: ${{secrets.NUGET_API_KEY}}
````

En cada push en cualquier rama:

- Configura un entorno linux con netcore
- Compila la aplicación
- Lanza **todos los test** que haya en el repositorio
- Publica el paquete nuget generado

#### 3. A tener en cuenta:

- Definir el nombre del paquete correctamente (En este caso es AadvantageLab.Example). Si el nombre no es único, y ya existe otro paquete con ese nombre de otra organización, la build puede fallar con un 403.

- Crear una variable en *Settings / Secrets* del repositorio en Github, cuyo nombre será NUGET_API_KEY y su valor la api key de Nuget

- Definir en el csproj lo siguiente:
````
  <PropertyGroup>
    <Version>1.1</Version>
    <PackageId>AadvantageLab.Example</PackageId>
  </PropertyGroup>
````

