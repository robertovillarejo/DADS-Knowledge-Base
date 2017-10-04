# Configuración de SSH

### Precondiciones
1. Se tiene instalada una versión de SSH
2. Se cuenta con un usuario creado en el sistema operativo


## Generar llave Privada y Pública

Primero, se deberá de generar la llave **privada** y **pública** de la máquina que se quiera conectar a un servidor remoto. Para esto, se utilizará el siguiente comando (se aceptará la llave por defecto):

```bash
# Generación Rápida
ssh-keygen

# Generación con mayor seguridad
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
si se aceptó el archivo por defecto, entonces se tendrá la llave privada ```id_rsa``` y la llave pública  ```id_rsa.pub```

## Copiar la llave Pública

### Désde la Máquina Local

```bash
cat ~/.ssh/id_rsa.pub | xclip -selection c
```
### En la máquina remota
en el home
```bash
mkdir .ssh
chmod 700 .ssh
```

Abrir y copiar la llave pública en:
```
vi .ssh/authorized_keys
```

Restringir el acceso a las llave autorizadas:

```bash
chmod 600 .ssh/authorized_keys

#Reiniciar el servicio sshd
systemctl reload sshd
```

### Referencias
[Digital Ocean](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-centos-7)
[Github](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)
