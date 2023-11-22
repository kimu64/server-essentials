- Redundant Array of Independent Disks | Matrix de Discos Independientes Redundantes.
- No todos los RAID son redundantes por contradictorio que sea.

> Su finalidad es proteger la información en caso de fallo en un disco o en el caso de los que no proporcionan redundancia, mejorar la velocidad de este.
> En definitiva, crear un único volumen con varios discos duros funcionando en conjunto, tenemos dos tipos de configuraciones:

- **Disk Mirroring**: Un tipo de configuración RAID que _busca redundancia_ de datos ante un posible posible fallo en uno de estos.
- **Disk Stripping**: Esta configuración no busca la redundancia, sino conseguir *mayores velocidades* de transferencia de datos.

# Paridad
Antes de entrar en el mundo de los RAID, vamos a ver como funciona la paridad en estos, así te harás una idea de cómo se tiene constancia de lo que hay en varios bloques usando uno.

Para la paridad tenemos que irnos al nivel de los bytes, digamos que tenemos tres bytes.
Queremos que si un byte se pierde, podamos saber qué había en él, para esto vamos a usar otro byte y veremos gráficamente el proceso para que no te me duermas:

Estos serán nuestro tres bytes. ¿Cómo sabremos qué había en uno si lo perdemos?
### Paridad Impar
![](img/Pasted%20image%2020231122013945.png)

Primero vamos a contar el número de ceros totales en cada columna:
![](img/Pasted%20image%2020231122020241.png)

Si el número de ceros es par, lo sustituiremos por un `0`, si es impar podremos un `1`:
![](img/Pasted%20image%2020231122014337.png)

## Paridad Par
De igual forma, en la paridad par, si el número total de `0` es par, los sustituiremos por un `1`, si son impares, por un `0`

En este caso, la respuesta sería `10010011`, lo contrarío, obviamente.

## Resumen
|  | PAR | IMPAR |
|---|---|---|
| BYTE 1 | 1001 | 1001 |
| BYTE 2 | 1101 | 1101 |
| BYTE 3 | 1000 | 1000 |
| PARIDAD | 1100 | 0011 |

PAR: Si hay un número **par** de `0` -> 1
IMPAR : Si hay un número **impar** de `0` -> `

# Cómo funciona
El sistema RAID permite operaciones de **I/O** en los discos de forma balanceada, por lo que los datos pueden escribirse en varios discos al mismo tiempo, o de forma secuencial para repartir el trabajo.
En el sistema operativo, el sistema RAID se presenta como una sola unidad lógica, ya que son un solo volumen.

Para que funcione el sistema RAID, es necesaria la presencia de una controladora RAID, estas pueden ser por **software** o **hardware**, las de software suelen venir integradas en las placas bases de ordenadores modernos, algunas placas ya incluyen controladoras por hardware.
Las controladoras por hardware son más caras pero más rápidas, por esto solo se suelen encontrar en entornos empresariales.
# Posibilidades de configuración
Existen muchos tipos de configuraciones, cada una ofreciendo una solución, pero nos centraremos en las más populares:

## RAID 0
En el RAID 0 no hay redundancia de datos, los bits están "esparcidos" por el disco, si uno se rompe, adiós a tus datos.
Los bits se escriben de forma secuencial mejorando las velocidades de lectura y escritura hasta el doble, aunque se pierde entre el 10-15% de rendimiento.
Para esta configuración se necesitan al menos 2 discos, el máximo dependerá de la controladora.
![Pasted image 20231121163110](img/Pasted%20image%2020231121163110.png)

## RAID I
En el RAID 1 se usa el **mirroring**, un disco es siempre igual a otro por lo que si uno cae, siempre hay una réplica exacta (si es que no se estropean simultaneamente).
También se requieren como mínimo dos unidades de almacenamiento y el máximo lo delimita la controladora.
Para poder tener una copia exacta, hay que escribir los datos en ambos discos, reduciendo la velocidad hasta en un 25%, pero la lectura está disponible en dos discos, permitiendo esta desde dos fuentes, aumentando la velocidad de lectura también en un 25%.
![Pasted image 20231121163410](img/Pasted%20image%2020231121163410.png)

## RAID III
En los RAID 3, los datos se distribuyen a nivel de byte en vez de bloque entre los discos.
Un disco duro hace de **paridad**, y los demás funcionan como un RAID 0, por lo que se necesitan 3 discos como mínimo.
La velocidad es bastante buena por esto último, pero los discos son usados simultaneamente en todo momento, por la velocidad en muchas peticiones pequeñas será mala.

Se usa un sistema de **paridad**, si un disco falla, la información en este se podrá recuperar usando los datos guardados en el disco dedicado a esto.
![Pasted image 20231121163829](img/Pasted%20image%2020231121163829.png)
## RAID V
El RAID 5 utiliza *paridad distribuida*, donde las unidades de almacenamiento se dividen en bloques y uno de estos se dedica a la redundancia de datos.
El inconveniente es que solo un disco puede fallar antes de que se pierdan los datos definitivamente.
![Pasted image 20231121172929](img/Pasted%20image%2020231121172929.png)
## RAID 01
Un RAID 1 con dos conjuntos de RAIDs 0.
Tiene una escalabilidad y una tolerancia a fallos bastante limitadas, la primera debido a que el número de discos deben ser los mismos en ambos RAIDs 0.

## RAID 10
Al revés que el anterior, un RAID 0 com dos conjuntos de RAIDS 1.
La información se guarda de forma secuencial en los dos RAIDS 1, y estos guardan la información en otro disco espejo.
Esto permite tener una velocidad y redundancia de datos alta.
Cabe recalcar que se necesitan 4 discos para esta configuración.

![Pasted image 20231121173640](img/Pasted%20image%2020231121173640.png)
## Otras configuraciones anidadas
Otras configuraciones que aprovechan la anidación de RAIDs son:
- RAID 30
- RAID 50
- RAID 60
- RAID 100

El RAID 100 se usa en sistemas de datos muy grandes.
## JBOD?
Just a Bunch of Disks.
La principal desventaja del RAID 0 es que si un disco se daña, toda la unidad lógica se pierde, ya que los datos no son independientes del disco, esto es lo que busca arreglar JBOD.
> Además de esto, las unidades pueden ser de distintas capacidades.


![Pasted image 20231121174215](img/Pasted%20image%2020231121174215.png)
