# Desplegar una aplicación blazor manualmente

Para desplegar una aplicación blazor a un servidor on premise, lo podemos automatizar con un script tipo:

```` bash
#!/bin/bash
#Add the dotnet path to the path
export PATH="$HOME/.dotnet":$PATH

if [ -d "./artifacts" ]
then
    rm -Rf "./artifacts"; 
fi

projectFolder = "my-blazor-app"
projectPath = "./src/MyApp/MyApp.csproj"
outputPath="X:\inetpub\wwwroot\my-blazor-app"
commitHash=$(git rev-parse --short HEAD)
suffix="-ci-local"
buildSuffix="$suffix-$commitHash"

cd $projectFolder

dotnet restore

echo "Building: Version suffix is $buildSuffix"
dotnet build -c Release --version-suffix "$buildSuffix" -v q "$projectPath"

echo "Deleting all files in $outputPath"
if [ -d outputPath ]
then
    rm -Rf outputPath; 
fi

echo "Publishing"
dotnet publish -c Release -o "$outputPath" $projectPath

echo "Replacing base href in index"
sed -i -e 's/base href="\/"/base href="\/my-app\/"/g' "$outputPath/wwwroot/index.html"

````

Para que funcione, sustituir las variables
- projectFolder -> La ruta desde el directorio de inicio de tu terminal hasta la carpeta raiz de tu repo.
- projectPath -> La ruta del proyecto de blazor que quieres desplegar, suponiendo que estamos en la raiz de repo.
- outputPath -> Url del servidor on-premise donde se copiará. Debes tener acceso, en este caso X: es una conexión a una unidad de red

En el último paso, sustituye el base href del index.html.

En desarrollo, lo tendremos así:

```` html
<base href="/" />
````

Y en producción queremos que todas las rutas queden prefijadas con el nombre de la aplicación. 
```` html
<base href="/my-app/" />
````

Si no alojas varias aplicaciones en el mismo servidor, y accedes directamente desde un dominio a tu aplicación, puede que este paso no te haga falta.
