
# Comandos básicos de git

Situarse en la carpeta correspondiente e iniciar el repositorio
````
git init 
````

Crear un fichero readme.md, editarlo y guardarlo y visualizarlo
````
touch readme.md   -> Crea un fichero
vim readme.md     -> abre vim para editarlo
[esc] :ws         -> Guarda los cambios y sales
cat readme.md     -> Visualizar el fichero
````

Ejemplos y ayuda sobre un comando en concreto (Too long didn't read)
````
tldr comando
````

Estado del repositorio
````
git status      -> Muestra el estado del repositorio
git status -s   -> Muestra el estado del repositorio de forma resumida
git log         -> Muestra los commits del repositorio 
git log         -> Muestra los commits del repositorio con el gráfico
git log --graph --abbrev-commit --oneline  -> Muestra los commits del repositorio con el gráfico pero abreviado
````

### Staging y commits

El working tree se corresponde con los cambios que se hacen en el directorio local, antes de ser añadidos al área de staging.
El area de staging es la zona donde se añaden los cambios en los ficheros que están listos para ser commiteados definitivamente.

````
git add readme.md  -> Añade el fichero readme.md al area de staging
git add .          -> Añade todos los cambios al area de staging
git add -A         -> Añade todos los cambios al área de staging
````

El comando commit guarda el estado del repositorio, añadiendo todos los cambios añadidos a la zona de staging
````
git commit -m "mensaje"
git commit -am "mensaje"  -> Si incluimos la a añade al area de staging todos los ficheros modificados (No así los creados)
````


### Stash

El area de stash es como un agujero en el suelo donde guardas los cambios que tienes en el area de staging, para recuperarlos más tarde. en vez de hacer commit, haces stash

````
git stash        -> Añade los ficheros del area de staging a la zona de stash
git stash pop    -> Recupera los ficheros de stash
git stash pop n  -> Donde n es el índice numerico. Si has hecho varios stash, el último será 0, el anterior 1, etc
git stash list   -> Muestra la lista de stash
````

### Tag

Se pueden tagear los commits para identificarlos

````
git tag v1.0.0   -> Aplicar el tag v1.0.0 al commit actual   
````

### Revert

Deshacer un commit. Crea otro commit con el proceso inverso

````
git revert identificadorCommit   -> El identificador del commit es el hash del commit sobre el que se quiere aplicar el revert
````


### Ramas

````
git branch            -> Lita todas las ramas del repositorio local
git branch -a         -> Lista todas las ramas del repositorio local y remoto
git branch rama1      -> Crea una rama con nombre rama1
git checkout rama1    -> Te posiciona en la rama rama1
git checkout -b rama1 -> Crea la rama rama1 y te posiciona en ella
git branch -d rama1   -> Borra la rama rama1
````

### Push

````
git push                -> Vuelca todos los cambios de local sobre el remoto
git push origin master  -> Vuelca los cambios sobre el remoto (llamado origin) de la rama master
````

### Pull request

En vez de añadir los cambios al repositorio remoto, si se necesita validarlo, se puede hacer una pull request,
Se crea la pull-request y con ella una nueva rama.
Una vez que se ve que los cambios son correctos, se valida la pull request, se mergean esos cambios, y se borra la rama (Esto se puede hacer desde github)  

### Fetch, merge y pull

Estos comandos sirven para traerse cambios del repositorio remoto.
Fetch trae los commits y las ramas, digamos la estructura
Merge trae los cambiso en los ficheros
Pull es un comando que combina ambos

````
git fetch origin    -> Trae la información de origin (el repositorio remoto)
git pull            -> Trae los cambios del remoto
git fetch -ap       -> Trae los cambios en el índice -a (todos los remotos) y -p de prume (Borra las ramas que ya no existan) 
````

