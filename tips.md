
# Bases de datos

#### 1. Hacer un backup de la base de datos de producción

````
BACKUP DATABASE dbname TO DISK = 'E:\somefolder\file.bak'
  WITH INIT, COPY_ONLY;
````

#### 2. Restaurar la copia de la base de datos en localhost
````
RESTORE DATABASE dbname FROM DISK = 'C:\temp\file.bak'
  WITH REPLACE, RECOVERY,
  MOVE 'dbname_data' TO 'C:\...\dbname.mdf',
  MOVE 'dbname_log' TO 'C:\...\dbname.ldf';
````

[Fuente](https://dba.stackexchange.com/questions/76210/how-to-copy-database-from-server-to-local-machine-in-sql-server-management-studi)

# Visual studio

**Activar / desactivar los colores mejorados de visual studio 2019:**

- [x] Herramientas -> Opciones -> Editor de texto -> C# -> Avanazadas -> Use los colores mejorados para C# y Basic


**Habilitar Source Link**

Activando source link en visual studio, podemos depurar dentro del código de las dependencias nuget que utilicemos.
Para que funcione, el paquete nugget en cuestión tiene que tenerlo hablitado, y hay que configurar las siguientes opciones en visual studio

- [ ] Herramientas -> Opciones -> Depuración -> General -> Habilitar solo mi código
- [x] Herramientas -> Opciones -> Depuración -> General -> Habilitar compatibilidad con vínculos de origen




[Fuente](https://www.fixedbuffer.com/sourcelink-habilitando-la-depuracion-de-codigo-bajo-demanda/)


# Windows terminal

Para configurarla, este post detalla como hacerlo:

https://www.developerro.com/2020/11/18/custom-windows-terminal/

Si tienes instalado git for windows, puedes editar el fichero .bashrc que hay en la raiz de tu propio usuario. Yo lo tengo así:

````
cd C:/Users/jcl/Documents/Source/Repos;
alias gs="git status";
alias gps="git push";  
alias gc="git commit";
alias gl="git log --graph --abbrev-commit --oneline";
````

La primera linea, para que vaya al directorio de inicio que quiero. Las otras me añaden alias útiles

# Como hacer funcionar un proyecto de .NET

A veces, las cosas no salen bien a la primera. Ya sea porque te clonas un repo algo antiguo, o porque hay algún problema con el gitignore y tienes paquetes obsoletos, puede que el repositorio que te has clonado no funcione correctamente. Si es el caso, puedes intentar lo siguiente:

1. dotnet restore

O también desde Visual Studio -> Botón derecho sobre la solución -> Restaurar paquetes nuget

2. Reinstalar los paquetes nugget (mediante la consola del administrador de paquetes)

````
update-package -reinstall 
````
*Si te diera error algún paquete, Botón derecho sobre la solución -> Restaurar paquetes de nuget*

Si sigue fallando, vuelve a empezar. Haz un git reset HEAD~0 --hard

3. dotnet run

