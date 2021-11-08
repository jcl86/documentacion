
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

---

**Habilitar Source Link**

Activando source link en visual studio, podemos depurar dentro del código de las dependencias nuget que utilicemos.
Para que funcione, el paquete nugget en cuestión tiene que tenerlo hablitado, y hay que configurar las siguientes opciones en visual studio

- [ ] Herramientas -> Opciones -> Depuración -> General -> Habilitar solo mi código
- [x] Herramientas -> Opciones -> Depuración -> General -> Habilitar compatibilidad con vínculos de origen




[Fuente](https://www.fixedbuffer.com/sourcelink-habilitando-la-depuracion-de-codigo-bajo-demanda/)

---

