# Como hacer funcionar un proyecto de .NET

1. Clonarse el repositorio (o hacer un fork y clonarlo, si no es tuyo)

2. dotnet restore 

Desde Visual Studio -> Botón derecho sobre la solución -> Restaurar paquetes nuget

3. Reinstalar los paquetes nugget (mediante la consola del administrador de paquetes)

````
update-package -reinstall 
````
*Si te diera error algún paquete, Botón derecho sobre la solución -> Restaurar paquetes de nuget*

Si sigue fallando, vuelve a empezar. Haz un git reset HEAD~0 --hard

4. Compilar

````
dotnet run
````
