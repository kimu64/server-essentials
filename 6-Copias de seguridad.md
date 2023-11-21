Las copias de seguridad son esenciales para evitar pérdidas importantes de datos, además de reducir el tiempo que nuestros servidores están caídos.

# Buenas copias de seguridad
- Deben ser **redundantes**, utilizando **soportes diferentes** y **ubicaciones diferentes**.
  No guardes todo en el propio servidor, o el USB que llevas en la mochila
- Deben ser hechas **de forma regular**, preferiblemente **programadas**, el día que pase algo solo tendras acceso a lo que tenga la última copia de seguridad.
- Además de esto es importante que nos aseguremos de si las copias están **haciéndose correctamente**, no es gracioso encontrarte que tu backup pesa 2 kilobytes después de que se te dañen los discos.
- Como complemento, podemos hacer una **copia en la nube**, esto no sustituye a un backup bien hecho, pero por supuesto ayuda.

## Deja-Dup
Por defecto en Ubuntu Server, tenemos la herramienta **Deja-Dup**, con esta podemos hacer backups del sistema usando una interfaz gráfica que incluye opciones avanzadas como el cifrado de los respaldos o el almacenamiento en equipos remotos.

En caso de no tener la aplicación, podemos instalar el paquete:
```bash
$ sudo apt install deja-dup
```

Al abrir la aplicación nos encontraremos con 3 opciones:
- **Restaurar**:
  para recuperar datos de una copia hecha con anterioridad, por ejemplo en un equipo diferente.
- **Respaldar ahora**:
  Como indica su nombre, hacer una copia de seguridad del sistema.
- El enlace activar o el botón que tenemos en la parte superior de la ventana para iniciar el programa **deja-dup-monitor** y que éste comience encargarse de la creación sistemática de copias de seguridad.

> `Carpetas que guardar` definiremos qué directorios queremos en la copia de seguridad. De forma predeterminada es la carpeta `Home` del usuario que abre la app.
> `Carpetas que ignorar`, justo para lo contrario.
> `Ubicación de almacenamiento`, donde se guardará la copia de seguridad, por defecto es Google Drive pero puedes usar una carpeta local.
> `Planificación`: Cada cuanto se harán las copias, por desgracia solo tenemos `Día`y `Semana` como opciones.
> `Conservar`: Cuanto tiempo es válida la copia de seguridad, como opciones tenemos: `Al menos 6 meses`, `Al menos 1 año` y `Siempre`.

## Back in Time
Back in time es otra herramienta para hacer copias de seguridad muy sencilla.

Usa el comando `rsync` para ver qué cambios hay desde la última copia de seguridad, si los hay, realiza dos acciones:
1. Crea una copia de todo el contenido de la instantánea anterior sobre la instantánea nueva donde, en realidad, cada archivo es un *hardlink* al archivo original (esto lo consigue, sencillamente, usando el comando cp).
2. Copia cada archivo que haya cambiado sobre su correspondiente enlace duro, creado en el punto anterior y lo configura como de sólo lectura. Esto hace que sólo los archivos modificados ocupen espacio adicional. Para lograrlo, vuelve a utilizar el comando rsync.

Por último, en la carpeta correspondiente a cada instantánea, se crea un sencillo
archivo CSV, llamado fileinfo.bz2, que contiene los permisos, propietario y grupo
originales de cada archivo.