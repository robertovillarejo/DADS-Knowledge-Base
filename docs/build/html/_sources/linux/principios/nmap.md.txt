# Nmap: referencia rápida

**Nota** Este documento está basado en [HackerTarget](https://hackertarget.com/nmap-cheatsheet-a-quick-reference-guide/)

## Escanear por IP

Escanear una **IP**

```bash
nmap 192.168.1.1
```

Escanear un **host**

```bash
nmap www.testhostname.com
```

Escanear un rango de **IP's**

```bash
nmap 192.168.1.1-20
```

Escanear una **Subnet**

```bash
nmap 192.168.1.0/24
```

Escanear desde un archivo

```bash
nmap -iL list-of-ips.txt
```


## Escanear por IP y Puerto

Escanear un sólo puerto

```bash
nmap -p 22 192.168.1.1
```

Escanear por un rango de puertos

```bash
nmap -p 1-100 192.168.1.1
```

Escanear los puertos **más comúnes**

```bash
nmap -F 192.168.1.1
```

Escanear todos los **65535**

```bash
nmap -p- 192.168.1.1
```

## Scanear por protocolos

Utilizando la conexión TCP

```bash
nmap -sT 192.168.1.1
```

Utilizando la conexión TCP SYN (por Default)

```bash
nmap -sS 192.168.1.1
```

Escaneando los puertos **UDP**

```bash
nmap -sU -p 123,161,162 192.168.1.1
```

Escaneando con puertos seleccionados

```bash
nmap -Pn -F 192.168.1.1
```
