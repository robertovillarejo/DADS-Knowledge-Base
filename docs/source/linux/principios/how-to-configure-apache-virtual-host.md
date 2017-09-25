**Nota**: el presente documento está basado en [Ditial Ocean](https://www.digitalocean.com/community/tutorials/how-to-set-up-apache-virtual-hosts-on-centos-7)



### Pre-Requisitos

Se tiene instalada una versión de [APACHE SERVER](../centos7/apache.md)

### Crear la estructura de Directorios

```bash
sudo mkdir -p /var/www/example.com/public_html 
sudo mkdir -p /var/www/example2.com/public_html
```
### Configurar los permisos

```bash
sudo chown -R $USER:$USER /var/www/example.com/public_html 
sudo chown -R $USER:$USER /var/www/example2.com/public_html
```

Configurar los permisos para el directorio `/var/www`

```bash
sudo chmod -R 755 /var/www
```

### Crear las páginas Demo para cada Virtual Host

```bash
vi /var/www/example.com/public_html/index.html
```

copiar el siguiente texto


```html
<html> 
    <head> 
        <title>Welcome to Example.com!</title> 
    </head> 
    <body> 
        <h1>Success! The example.com virtual host is working!</h1> 
    </body> 
</html>
```

Copiar el archivo anterior y editar la pantalla de inicio para el segundo host

```bash
cp /var/www/example.com/public_html/index.html /var/www/example2.com/public_html/index.html
```

### Crear los archivos de configuración para cada Virtual Host

```bash
sudo mkdir /etc/httpd/sites-available 
sudo mkdir /etc/httpd/sites-enabled
```

Modificar el archivo `/etc/httpd/conf/httpd.conf` y agregar la siguiente línea al final del archivo:

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

```bash
IncludeOptional sites-enabled/*.conf
```

### Crear los archivos de cada Virtual Host

```bash
sudo vi /etc/httpd/sites-available/example.com.conf
```
Agregar lo siguiente:

```bash
<VirtualHost *:80> 
    ServerName www.example.com 
    ServerAlias example.com 
    DocumentRoot /var/www/example.com/public_html 
    ErrorLog /var/www/example.com/error.log 
    CustomLog /var/www/example.com/requests.log combined 
</VirtualHost>
```

Copiar el archivo anterior y modificarlo para el segundo host

```bash
sudo cp /etc/httpd/sites-available/example.com.conf /etc/httpd/sites-available/example2.com.conf
```

Modificar 

```bash
sudo nano /etc/httpd/sites-available/example2.com.conf
```

Contenido


```bash
<VirtualHost *:80> 
    ServerName www.example2.com 
    DocumentRoot /var/www/example2.com/public_html 
    ServerAlias example2.com 
    ErrorLog /var/www/example2.com/error.log 
    CustomLog /var/www/example2.com/requests.log combined </VirtualHost>
```
## Habilitar los Archivos

```bash
sudo ln -s /etc/httpd/sites-available/example.com.conf /etc/httpd/sites-enabled/example.com.conf 
sudo ln -s /etc/httpd/sites-available/example2.com.conf /etc/httpd/sites-enabled/example2.com.conf
```

Reiniciar el servidor Apache

```bash
sudo apachectl restart
```

### Configurar los hostname locales

Con la finalidad de probar que los servidores virtuales están configurados de manera correcta, desde la máquina cliente se pueden configurar los dominios.

```bash
sudo vi /etc/hosts
```

Agregar algo semejante a lo siguiente:

```bash
127.0.0.1         localhost 
127.0.1.1         guest-desktop 
server_ip_address example.com 
server_ip_address example2.com
```

## Probar la configuración

Desde el servidor cliente ingresar la siguiente url:

```bash
http://example.com
o
http://example2.com
```

Referencias:

[Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-set-up-apache-virtual-hosts-on-centos-7)
