El usuario root es la cuenta de administrador, tiene permisos sobre todo el sistema, por eso es mala práctica hacer todo con ella, puedes borrar datos importantes sin darte cuenta y en general exponer el sistema a riesgos innecesarios, deberíamos hacer uso del comando `sudo` siempre que sea posible.

Esta cuenta es parte de la `familia Unix`, es buena práctica desactivar el acceso a la cuenta y de hecho en Ubuntu viene desactivada por defecto, pero se puede activar explicitamente con el comando `sudo passwd -l root`.

Si necesitamos acceder a la cuenta root, podemos hacerlo con estos comandos:
- sudo -s
- sudo -i
- sudo su
- sudo su -
