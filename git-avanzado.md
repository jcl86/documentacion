# Gestión avanzada con git

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


### Estrategias de integracion entre ramas

Las siguientes estrategias sirven para, cuando estas trabajando en una rama (Develop, o tu feature-branch), y a medida que se van produciendo commits en master, traerte esos cambios a tu rama

````
git merge master
````
Trae los commits realizados en master, y los pone en tu rama actual, añadiendo un unico commit con esos cambios

````
git rebase master
````

Trae todos los commits realizados en master, y los coloca en orden en antes de los commits de tu rama. Esto en realidad a recreado el índice, por lo que en realidad tu rama ha cambiado completamente. Cuando vuelques los commits de tu rama en el remoto, deberás hacer un push --force, para que se olvide de como está tu rama en el remote, y sobreescriba los cambiso de tu local en el remote. Esto solo es viable si sobre la rama que haces rebase trabaja un solo desarrollador, porque si trabajan varios, al hacer push y reorganizar la rama, harás que los otros desarrolladores tengan conflictos.


