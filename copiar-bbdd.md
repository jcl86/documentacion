
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

(Fuente)[https://dba.stackexchange.com/questions/76210/how-to-copy-database-from-server-to-local-machine-in-sql-server-management-studi]
