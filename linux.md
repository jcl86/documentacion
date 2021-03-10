# Preparación

- Instalar una distribución como ubuntu
- Instalar xclip
- Instalar git
- En etc/passwd o etc/shadow se guardan las contraseñas hasheadas


| Comando | Explicación |
| -- | -- |
| whoami/who am i | Identificador de usuario |
| id | Identificador de usuario y grupos |
| pwd | Lugar en el sistema de archivos |
| hostname | Nombre de la máquina |
| who/finger | Quién está registrado |
| w | Quién está registrado y qué hace |
| date | Fecha y hora |
| cal | Calendario del mes |
| man | manual |


# Ficheros

| Comando | Explicación |
| -- | -- |
| cat | Mostrar el contenido de un fichero |
| more / less | Mostrar el contenido de un fichero página a página |
| cp | Copia un fichero |
| mv | Mueve y renombra |
| rm | Elimina un fichero |
| file | Muestra el tipo de un fichero |
| cd | Cambia de directorio |
| cd | Cambia al directorio home del usuario |
| cd ~ | Cambia al directorio home del usuario |
| cd ~username | Cambia al directorio home del usuario username |
| cd /home/username | Cambia al directorio home del usuario fulano |
| cd .. | Cambia al directorio superior |
| cd mibin | Cambia al subdirectorio 'mibin' del directorio actual |
| mkdir | Crea un directorio |
| rmdir | Eliomina un directorio |
| df | Muestra el espacio disponible |
| du | Espacio ocupado por un subárbol del sistema de archivos |


# Vi

Para editar un fichero:
````
vi file.txt
vi -r file.txt (Para recuperar el fichero tras una caída)
````

En vi, solo se utiliza el teclado. Tiene dos modos:

- Modo comando. El texto se interpreta como un comando
- Modo inserción. El texto se inserta en el documento.

Cuando vi arranca, lo hace en modo comando.
- Para pasar a modo inserción: i
- Para salir a modo comando: ESC

| Comando | Explicación |
| -- | -- |
| i | Pasa a modo inserción antes del cursor |
| a |  Pasa a modo inserción después del cursor |
| o |  Pasa a modo inserción al comienxo de una nueva linea |
| ZZ | Guardar y salir |
| :q | Salir sin guardar (aqune pregunta) |
| CTRL+z | Suspender edición (Se recupera con fg) |
| dd | Borra la linea donde está el cursor |
| u | Deshacer último cambio |
| /texto | Busca texto hacia delante |
| :r fichero | inserta el contenido de fichero |


Fuente [Fuente](https://miriadax.net/web/introduccion-a-linux-como-entorno-de-desarrollo-de-sistemas-software-2-edicion-consulta/)

