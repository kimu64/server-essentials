# Particiones
Un disco puede ser dividido en particiones, puedes pensar en ellas como bloques en los que dividimos el disco.
Generalmente, no usamos un disco como una sola unidad, sino varias que utilizamos para la UEFI, el sistema, nuestros archivos, juegos, etc.

Disco en linux particionado:
```bash
sda      8:0    0 232.9G  0 disk
├─sda1   8:1    0   512M  0 part /boot
└─sda2   8:2    0 232.4G  0 part /
```

Los discos ópticos utilizan un formato de partición especial denominado UDF, por
sus siglas del inglés Universal Disc Format.

Por su parte, los discos duros y los sistemas de memoria flash disponen de tres
clases distintas de particiones:
- Particiones primarias
- Particiones extendidas
- Particiones lógicas

## Particiones lógicas
En un disco puede haber hasta cuatro, aunque para que el sistema pueda leer del disco se necesita una cómo mínimo, piensa que el disco es como un cubo que puedes llenar de arena, el cubo le puedes separar en hasta 4 bloques, pero no puedes tener menos de uno, entonces no podrías llenarlo de nada.
![particiones](img/Pasted%20image%2020231121185403.png)

## Lógicas
Siempre se encuentran dentro de una partición extenida, puede haber hasta 23 de ellas dentro de esta.

## Extendidas
Dentro de un disco solo puede existiir una y ocupará el espacio de una partición primaria, osea si tienes una partición extendida, solo puedes tener 3 primarias.
Dentro de esta partición solo puedes tener otras particiones lógicas, no guarda información como tal.
> Las particiones extendidas se crearon por la limitación que creó solo poder tener 4 particiones primarias.

# Nomenclaturas
Los discos HDD siempre se nombran con `hd`, mientras que las unidades SDD o SCSI usan el prefijo `sd`.

Para que te hagas una idea:
```bash
sda      8:0    0 232.9G  0 disk # Esto es un SSD
hda      8:0    0 984.6G  0 disk # Esto es un disco duro
```

Luego, se nombran en orden alfabético, por lo que la primera unidad SSD será `sda`, la segunda `sdb`, `sdc` y así sucesivamente.

Por último, las particiones.
Estas tienen el nombre del disco seguido del número de la partición, es más, el primer bloque de código de este texto lo muestra:
```bash
sda      8:0    0 232.9G  0 disk
├─sda1   8:1    0   512M  0 part /boot
└─sda2   8:2    0 232.4G  0 part /
```

Aquí se muestra un disco llamado `sda`:
- Sabemos que es un SSD ya que empieza por `sd`
- Es el primer disco SSD, empieza por `a`
- Tiene dos particiones: `sda1` y `sda2`

Tenemos que tener algo en cuenta, el número máximo de particiones primarias son `4` como hemos visto, entonces la primera partición extendida siempre será `<disco>5`, veamos algún ejemplo:

**Un disco con 4 particiones primarias**
| sda1 | sda2 | sda3 | sda4 |

**Un disco con 3 particiones primarias, una extendida y 2 lógicas**
| sda1 | sda2 | [| sda5 | sda6 |]

**Un disco con 1 partición primaria y una extendida con 2 lógicas**
| sda1 | [| sda5 | sda6 |]

# Comandos
Para ver los discos en el sistema y sus particiones, podemos usar este comando:
```bash
$ lsblk -fm
```
donde `-fm` nos dará más información acerca de los discos.

Para modificar el disco, por ejemplo añadiendo una partición, podemos usar la utilidad `fdisk`, su sintaxis es la siguiente:
```bash
$ fdisk /dev/<disco>
```

Esto abrirá la utilidad, para interactuar con ella podemos usar las siguientes letras:
`p`: Listar las particiones.
`n`: Crear una nueva partición.
`d`: Borrar una partición.
`w`: Guardar los cambios.
`q`: Salir de la utilidad.

Para crear una partición seguiremos estos pasos:
- Indicamos que queremos crear una nueva partición con la letra `n`.
- Se nos preguntará qué tipo de partición queremos, pulsaremos `p` para indicar que es **primaria**.
- Pulsaremos `1` para indicar que será la primera partición del disco.
- Ahora se nos preguntará por el inicio y final del sector, dejaremos esto por defecto pulsando `intro` dos veces.
- Pulsamos `w` para guardar los cambios, no sin antes ver si los cambios son correctos con la tecla `p`.

Por defecto la partición será de tipo GNU/Linux, podemos formatear las particiones con la utilidad `mkfs` (make file system):
```bash
$ sudo mkfs.ext4 /dev/sdb1
```
este comando formateará en el formato `ext4` la partición `1` del disco `sdb`

Para usar este disco hay que `montarlo`, la forma más fácil de hacer esto es con el comando `mount`:
```bash
$ sudo mount /dev/sdb1 /mnt
```
Esto monta la partición `1` del disco `sdb` en la carpeta `/mnt`, ubicada en el root y comúnmente usada para montar discos, puedes usar cualquier carpeta para esto mientras tengas permisos.
