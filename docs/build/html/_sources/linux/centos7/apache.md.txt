# Apache

> **Autor** : INFOTEC - DADS

**Nota**: El documento está basado principalmente en [liquidweb](https://www.liquidweb.com/kb/how-to-install-apache-on-centos-7/) y [digitalOcean](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-using-firewalld-on-centos-7)

## Instalar Apache
Limpiar yum:

```bash
sudo yum clean all
```
Actualizar paquetes

```bash
sudo yum -y update
```

Instalar apache

```bash
sudo yum -y install httpd
```

## Configurar Firewall

Activar el servicio de **firewall-cmd**

```bash
sudo systemctl start firewalld.service
```

Aceptar el tráfico por los puertos 80 (http) y 443(https)

```bash
sudo firewall-cmd --permanent --add-port=80/tcp
sudo firewall-cmd --permanent --add-port=443/tcp
```

Reiniciar el firewall

```bash
sudo firewall-cmd --reload
```

## Configurar Boot

Iniciar APACHE

```bash
sudo systemctl start httpd
```

Configurar Apache para que corra durante boot

```bash
sudo systemctl enable httpd
```

Verificar el estado de apache

```bash
sudo systemctl status httpd
```

Detener apache cuando sea necesario

```bash
sudo systemctl stop httpd
```

## Referencias

[Apache Install](https://www.liquidweb.com/kb/how-to-install-apache-on-centos-7/)
[DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-using-firewalld-on-centos-7)
