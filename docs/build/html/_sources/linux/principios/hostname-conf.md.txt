> **Autor** : INFOTEC - DADS

# Configuración del Hostname

### Arch / CentOS 7 / Debian 8 / Fedora version 18 and above / Ubuntu 15.04 and above

```bash
hostnamectl set-hostname hostname-arq
```

### Debian 7 / Slackware / Ubuntu 14.04

```bash
echo "example_hostname" > /etc/hostname
hostname -F /etc/hostname
```

Verificar que el archivo `/etc/default/dhcpcd` exista

```bash
cat /etc/default/dhcpcd | grep SET_HOSTNAME
```

Si el valor regresado es `SET_HOSTNAME='YES'`, entonces editar el archivo `/etc/default/dhcpcd` y descomentar la línea `SET_HOSTNAME`:

Archivo `Archivo ``/etc/default/dhcpcd`
```bash
#SET_HOSTNAME='yes'
```

### CentOS 6 / Fedora version 17 and below

```bash
echo "HOSTNAME=hostname" >> /etc/sysconfig/network
hostname "hostname"
```

## Update /etc/hosts


