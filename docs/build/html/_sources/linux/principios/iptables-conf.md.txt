> **Autor** : INFOTEC - DADS

**Nota** El presente documento está basado en [Linux Tips](http://linuxtips-nitin.blogspot.mx/2009/03/linux-iptables-configuration-step-by.html) y [How to Geek](http://www.howtogeek.com/177621/the-beginners-guide-to-iptables-the-linux-firewall/)

# Configuración de IPTABLES

Configuración deseada:

```bash
$ iptables -L -v

Chain INPUT (policy DROP 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination
    0     0 ACCEPT     all  --  lo     any     anywhere             anywhere
    0     0 ACCEPT     all  --  any    any     anywhere             anywhere            state RELATED,ESTABLISHED
Chain FORWARD (policy DROP 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination
Chain OUTPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination
```

## Borrar Reglas

Establecer la política por defecto en la cadena **INPUT** a **ACCEPT**:

```bash
iptables -P INPUT ACCEPT
```

Borrar todas la reglas actuales:

```bash
iptables -F
```

Se habilitan las conexiones a través del puerto 22 \(**SSH**\)

```bash
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

Se habilita la política por defecto para la cadena **INPUT** a **DROP**\(No se confía en el tráfico externo\)

```bash
iptables -P INPUT DROP
```

Debido a que nuestro servidor no estará configurado como router, se tiene que bloquear el tráfico en la cadena **FOWARD**

```bash
iptables -P FORWARD DROP
```

Se habilita la política por defecto para la cadena **OUTPUT** a **ACCEPT** \(se confía en el tráfico interno\)

```bash
iptables -P OUTPUT ACCEPT
```

## Agregar Nuevas Reglas

Aceptar el tráfico por la interfaz lo \(127.0.0.1\)

```bash
iptables -A INPUT -i lo -j ACCEPT
```

Permitir el tráfico de una **IP** específica

```bash
iptables -A INPUT -s 10.10.10.10 -j ACCEPT

```

No aceptar tráfico de una **IP** específica

```bash
iptables -A INPUT -s 10.10.10.10 -j DROP
```

No acepta tráfico en un rango de IPs
```bash
iptables -A INPUT -s 10.10.10.0/255.255.255.0 -j DROP
```

Conexión a un puerto específico

```bash
iptables -A INPUT -p tcp --dport ssh -s 10.10.10.10 -j DROP
```

Cargar el módulo **state** para determinar si es **NEW**, **ESTABLISHED** o **RELATED**. **NEW** se refiere a paquetes de entrada que provienen de una nueva conexión que no ha sido inicializada por el _host_. **ESTABLISHED** y **RELATED** se refieren a paquetes de entrada que son parte de una conexión ya establecida \(**ESTABLISHED**\) o relacionada \(**RELATED**\).

```bash
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```

## REFERENCIAS:

[Linux Tips](http://linuxtips-nitin.blogspot.mx/2009/03/linux-iptables-configuration-step-by.html)

[How to Geek](http://www.howtogeek.com/177621/the-beginners-guide-to-iptables-the-linux-firewall/)
