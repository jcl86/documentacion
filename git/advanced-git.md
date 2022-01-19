# GESTIÓN AVANZADA CON GIT

### Gestionar claves https y ssh con github

https://www.lemoncode.tv/curso/github-windows/leccion/personal-access-token-windows

### Alias

Distingimos dos tipos de alias:

##### Alias de git

Los alias de git se guardan en C:\Users\usuario\.gitconfig, en el apartado [alias].
Para ejecutarlos tendrás que teclear git seguido del alias

##### Alias de terminal

Los alias de terminal se guardan en la propia configuración de la terminal. En el caso de cmder en: *Cmder\config\user_aliases.cmd*

````
alias                                         -> Muestra lista de alias y lista de comandos relacionados
alias {nombre alias}={comando largo}          -> Crea un alias (¡No poner comillas!)
unalias {nombre de alias}                     -> Elimina un alias
````

**EJEMPLO: Paso a paso para crear un alias**

Voy a combinar los dos tipos de alias.

1. Primero creo el alias de git.

````
cd C:\Users\Jorge
code .gitconfig
````

Añado en la sección de alias *alias s = status*

2. Luego creo el alias de terminal

````
alias gs=git s
````

**Resultado final**

Hemos conseguido que al escribir *gs* se ejecute el comando *git status*

---

### .gitignore global

Se pueden mover los archivos a ignorar globales, que no son parte del proyecto sino que dependen de tu entorno de desarrollo (.vs etc) a un fichero de ignore global

El archivo de ignore global se suele llamar .gitignore_global y se situa en la raiz del usuario.

````
touch ~/.gitignore_global
git config --global core.excludesfile ~/.gitignore_global
````


### Tips para escribir commits

Los commits se componen de título y cuerpo. El título en la primera linea, luego una linea en blanco, y luego ua o varias lineas de cuerpo.

- Separa el título del contenido con un intro

- Título del commit no más largo de 50 caracteres

- Cuerpo del commit con no más de 72 caracteres por linea

- Primera linea en mayúscula

- No acabar con punto

- Utilizar el imperativo al escribir

- Utilizar el cuerpo para definir el "qué" y el "por qué"


# ESTRATEGIAS DE INTEGRACIÓN ENTRE RAMAS

Las siguientes estrategias sirven para, cuando estas trabajando en una rama (Develop, o tu feature-branch), y a medida que se van produciendo commits en master, traerte esos cambios a tu rama.

### Merge y rebase

````
git merge master
````
Merge trae los commits realizados en master, y los pone en tu rama actual, añadiendo un unico commit con esos cambios

````
git rebase master
````

Rebase trae todos los commits realizados en master, y los coloca en orden en antes de los commits de tu rama. Esto en realidad ha recreado el índice, por lo que en realidad tu rama ha cambiado completamente. Cuando vuelques los commits de tu rama en el remoto, deberás hacer un push --force, para que se olvide de como está tu rama en el remote, y sobreescriba los cambios de tu local en el remote. Esto solo es viable si sobre la rama que haces rebase trabaja un solo desarrollador, porque si trabajan varios, al hacer push y reorganizar la rama, harás que los otros desarrolladores tengan conflictos.

````
git rebase -i HEAD~3
r i-commit-1
s i-commit-2
s i-commit-3
[esc]:wq
````

Squash agrupa varios commits en uno. En este caso, coge el commit en el que está el head y los dos anteriores, y los agrupa en un solo commit cuyo nombre habrá que introducir


# VOLVIENDO ATRÁS

#### Revert 

Revertir commits, respetando el historico. El revert crea un nuevo commit que deshace los cambios.

````
git revert @  -> revierte el último commit
git revert HEAD~n  -> revierte el commit n, donde n es el número de commits que te vas hacia atrás
git revert @  -> revierte el último commit

````

#### Reset

Deshacer commits, es decir, borrarlos del log como si no se hubieran hecho

````
git reset HEAD~1 --soft
git reset HEAD~1 --mixed
git reset HEAD~1 --hard
````
HEAD~1 es el útimo commit.

Soft -> Deja los cambios deshechos del commit subidos en el staging area

Mixed -> Deja los cambios deshechos del commit en el working tree (sin subir al staging area)

Hard -> Descarta los cambios deshechos del commit del working tree


#### Amend
````
git commit --amend -m "nuevo titulo" -> pudiendo editar el título del commit    
git commit --amend --no-edit  -> Sin editar el título del commit
````

Hace commit de los cambios subidos y los junta al commit anterior

### Cherry pick

Cherry pick se usa para volcar unos cambios concretos de una rama a otra. Por ejemplo, has hecho commit que arregla un bug en producción, y solo quieres volcar ese commit.
Desde main, cojo una o más cerezas de otra rama (cereza = commit) y me la como (la integro en main)

````
git checkout main -> Me cambio a la rama main
git cherry-pick identificadorCommit
git cherry-pick commitDesde~..commitHasta
````
