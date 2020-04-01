# Como hacer funcionar un proyecto de .NET

1. Clonarse el repositorio (o hacer un fork y clonarlo, si no es tuyo)

2. Reinstalar los paquetes nugget (mediante la consola del administrador de paquetes)

````
update-package -reinstall 
````
*Si te diera error algún paquete, Botón derecho sobre la solución -> Restaurar paquetes de nuget*

3. Compilar

````
dotnet run
````
