Tasksel es una herramienta de instalación que viene incluida en Debian, y hasta hace relativamente poco también en Ubuntu.
El gestor de paquetes de Ubuntu tiene una funcionalidad similar así que se decidió retirarlo pero sigue siendo potente.

Su funcionamiento es muy sencillo: Si una entrada aparece marcada (`[*]`), es que el
grupo correspondiente se encuentra instalado. Y si está desmarcada ( `[ ]`), el grupo
está desinstalado.
Con las teclas de cursor nos desplazamos hasta la opción que queramos activar o
desactivar y con la barra espaciadora cambiamos su estado. Siguiendo esta
técnica, podemos elegir tantas colecciones como sean precisas.

![Pasted image 20231121235913](img/Pasted%20image%2020231121235913.png)

## Línea de comandos
Para ver el listado con todas las colecciones que tenemos definidas, podemos
escribir en la terminal el siguiente comando:
```bash
$ tasksel -t --list-tasks
```

Observando el primer carácter de cada línea, comprobaremos que se trata de una
letra i o una letra u. Esto te indica si el paquete representado en dicha línea se
encuentra actualmente instalado (installed) o desinstalado (uninstalled)
respectivamente.

Incluso podemos utilizar Tasksel desde la línea de comandos para instalar
cualquiera de sus grupos de paquetes. Por ejemplo, el que nos permite convertir
el equipo en un servidor DNS:
```bash
$ sudo tasksel install dns-server
```

Si lo que quisiéramos fuese desinstalar la colección anterior, escribiríamos esto:
```bash
$ sudo tasksel remove dns-server
```

## Interfaz ???
No entiendo por qué usarías una interfaz gráfica en Ubuntu Server, o cualquier servidor en general, pero si es tu caso por razones de la vida, solo tendrías que ejecutar el programa como `superusuario`:
```bash
$ sudo tasksel
```

Un momento después, aparecerá el menú de la herramienta en pantalla.
Aquí podremos elegir grupos de paquetes para un gran número de funciones...
Incluso si nos centramos únicamente en la interfaz gráfica, además de la
apariencia estándar con GNOME, encontraremos casi todos los escritorios
disponibles.

Algunos de ellos, incluso ofrecen una versión mínima y otra normal.
Una vez seleccionado el elegido, pulsamos la tecla TAB para que se active y
continuamos.