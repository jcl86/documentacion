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


