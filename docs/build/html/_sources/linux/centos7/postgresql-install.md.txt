> **Autor** : INFOTEC - DADS

# Instalación de PostgreSQL en CentOS 7

#### Instalación de paquetes

```bash
sudo yum -y install postgresql postgresql-server postgresql-contrib
```

#### Inicialización de la base de datos PostgreSQL

La primera vez que se va a iniciar PostgreSQL, es necesario iniciar la base de datos

```bash
sudo postgresql-setup initdb
```

#### Habilitación del acceso por TCP.

Modificando configuración de acceso para `_localhost_`.

##### Se modifica el archivo `/var/lib/pgsql/data/pg_hba.conf`

Se cambian las líneas:

```ini
host all all 127.0.0.1/32 ident
host all all ::1/128      ident
```

por:

```ini
host all all 127.0.0.1/32 md5
host all all ::1/128      md5
```

> Si se requiere acceder a PostgreSQL desde otra IP, se deberá agregar una línea extra por IP o Rango de IPs.

##### Se modifica el archivo `/var/lib/pgsql/data/postgresql.conf`

Se descomentan las líneas:

```ini
listen_addresses = 'localhost'
port = 5432
```

> Si se requiere acceder a PostgreSQL desde otra IP, se deberá cambiar `'localhost'`
> por `'*'`.

#### Iniciando el servicio de postgresql

```bash
sudo systemctl start postgresql
```

#### Habilitando el servicio de postgresql

```bash
sudo systemctl enable postgresql
```

#### Configurando el firewall para postgresql

Activar el servicio de **firewall-cmd**

```bash
sudo systemctl start firewalld.service
```

```bash
sudo firewall-cmd --permanent --zone=public --add-service=postgresql
```

#### Creando usuario para nuestra aplicación

```bash
sudo su - postgres -c "createuser -DISRP appuser"
```

> Se dejó como password: `appuser`

#### Creación de la base de datos `appdb` para nuestro proyecto, sociada al usuario `appuser`

```bash
sudo su - postgres -c "createdb -O appuser appdb"
```

#### Probar usuario y contraseña \(opcional\)

```bash
psql -h localhost -U appuser -W
```
