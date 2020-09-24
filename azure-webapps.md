# Azure webapps

Un **App service plan** puede contener hasta 10 webapps. (Recomiendan máximo 8). En el definimos la potencia de procesador, memoria, etc

Dentro del service plan existen los **workers**. Cada worker es una especie de máquina virtual que ejecuta las instancias de las web apps. Por ejemplo, si tenemos en un service plan dos webapps, y de cada web app tenemos 2 instancias:

- El worker 1 correrá la instancia 1 de la webapp1 y la instancia 1 de la webapp2
- El worker 2 correrá la instancia 2 de la webapp1 y la instancia 2 de la webapp2

Adicionalmente, dentro de cada instancia están los procesos que hacen funcionar la web app. Por defecto, dentro de cada instancia de la webapp habra dos procesos:

- Proceso de IIS
- Proceso que es la propia webapp

Si hicieramos webjobs, irían generando nuevos procesos.

# Monitoring

Métricas de la webapp:

- Errores 500 -> Son los errores no tratados. Los errores tratados deberían ser nien gestionados devolviendo 400s

- Average response time -> El tiempo medio de respuesta de las peticiones

Métricas del service plan:

##### CPU Percentage

##### Tcp established 
Numero de conexiones tcp establecidas. Las tcps son las conexiones que hacen los clientes y por las que la applicación da el servicio. 
Cada instancia tiene un número límite de conexiones tcp que puede soportar. Si las superas, la instancia se viene abajo

- B1 / S1 / P1 => 1920 conexiones tcp
- B2 / S2 / P2 => 3968 conexiones tcp
- B3 / S3 / P3 => 8064 conexiones tcp
- I1 / I2 / I3 => 16000 conexiones tcp

Los propios app services ya gastan algunas conexiones de por sí, para application insights o para los logs, si tenemos activadas esas opciones.

##### Http Queue Lenth -> Cuantas peticiones están en cola


# Escalado

#### Scale up  o escalado vertical

Subir el app service plan para aumentar el procesador, memoria, etc (a costa de pagar más, claro). Este escalado se hace manualmente.

#### Scale out o escalado horizontal

Aumentar el número de instancias. Se puede hacer manualmente (ir al app service plan y decir el número de instancias que quieres), o de manera automática

Por ejemplo, se podría establecer la regla de que cuando el CPU percentage llega al 70%, escale en x instancias, y cada vez que baja de 30% desescale. 
Aquí sería muy importante que no esté constantemente escalando y desescalando, porque cada vez que realiza este proceso no es instantaneo, y puede hacer que la aplicación funcione mal.
Para que esto no ocurre, dejar diferencia entre el % de escalado y desescalado.


# Opciones al crear un App Service

Cuando creas una webapp, hay dos opciones importantes:

##### Always on -> On

Para que el App Service esté siempre levantado. No te vas a ahorrar nada, porque el App Service lo vas a pagar igual, esté funcionando o no (Esto no es serverless, que se paga por minuto de Cpu utilizado)

##### ARR affinity -> Off

Para que las peticiones no vayan por afinidad a una instancia concreta. Así dejamos que Load Balancer derive el tráfico de manera equilibrada, porque si lo dejamos por afinidad, una instancia probablemente recogerá más tráfico.

# Deployment Slots

Tienes un slot de producción y uno de test. Haces el deploy en el slot de test, y luego haces swap en azure, para intercambiar los slots.

*A/B testing*, *Blue green deployment*

# Warm up

"Calentar" la instancia al hacer el swap, para que no haya un tiempo de espera en lo que tarda en arrancarse la aplicación.

# Local cache

Para guardar en el propio almacenamiento del App Service, se puede utilizar la opción de local cahé

- Pros: No dependes del almacenamiento externo donde se guardan por defecto los datos

- Contras: Cada vez que despliegas debes reiniciar el App Service

Para activarlo, ir Settings -> Configuration y Añadir una application setting:

- Name: WEBSITE_LOCAL_CACHE_OPTION

- Value: Always

# Health Check

En el menú de App Service, opción Health check (en preview), Enable, y en path le pones el endpoint que dice si la aplicación está funcionando ok ("/alive", por ejemploj)




