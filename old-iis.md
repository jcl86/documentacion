# Tratar con IIS 

**Como alojar webs nuevas en servidores o maquinas virtuales con windows server y IIS**

- Normalmente será necesario instalar el runtime (no el sdk, el runtime que es más ligero), con el bundle para IIS. Se descarga desde la web
- Aspnetcore, a diferencia de Full Framework, no permite utilizar el mismo appPool para varias aplicaciones. En este caso, habrá que crear un nuevo AppPool y asignarselo a la nueva aplicación, para que corra con su appPool dedicado.
- Al acceder a la base de datos sql server en el mismo servidor, hay dos formas
  - Método clásico, con un usuario y contraseña de sql server en la cadena de conexión
  - Sin usuario y contraseña, pero para habrá que:
    - Dar de alta el AppPool de la aplicación como usuario en *Databases -> Security -> Logins -> New Login* [Source](https://stackoverflow.com/questions/1933134/add-iis-7-apppool-identities-as-sql-server-logons)
    - Una vez creado el login, darle *Botón derecho -> Propiedades -> User Mappings* y asignarle los permisos en la base de datos correspondiente
