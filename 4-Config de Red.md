Antes de 2017, en Ubuntu la gestión de la configuración de red había que realizarla desde el archivo de configuración en `/etc/network/interfaces`, cosa que es heredada de Debian.

Desde la versión 17.10 podemos hacer uso de una herramienta llamada NetPlan, la cual busca facilitarnos la vida.

## Comandos
Para ver la configuración actual, tenemos que ver el contenido del fichero ubicado en `/etc/netplan/00-installer-config.yaml`
```bash
$ cat /etc/netplan/00-installer-config.yaml
```

Esta configuración se verá algo así:
```yaml
network:
	ethernets:
		enp0s3:
		dhcp4: true
version: 2
```

Una configuración de red podría ser esta:
```yaml
network:
	ethernets:
		enp0s3:
			dhcp4: no
			addresses: [192.168.1.10/24]
			gateway4: 192.168.1.1
			nameservers:
				addresses: [8.8.8.8]
version: 2
```

Fíjate en los campos con `[`corchetes`]`, estos pueden tener más de un valor.
Imagina que necesitamos más de un servidor DNS, podríamos modificar la configuración así:
```yaml
nameservers:
	addresses: [8.8.8.8, 8.1.1.8, 1.1.1.1]
```

Si queremos aplicar estos cambios, ejecutaremos el comando para ello:
```bash
$ sudo netplan apply
```

Y por último, para ver que se ha aplicado la configuración, usaremos el clásico comando:
```bash
$ ip addr
```