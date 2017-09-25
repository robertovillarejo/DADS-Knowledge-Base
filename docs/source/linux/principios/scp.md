> **Autor** : INFOTEC - DADS

# SCP

## Para Archivos

**Archivo** foobar.txt - **local** host **->** **remote** host

```shell
$ scp foobar.txt username@example.com:/some/remote/directory
```

**Archivo** foobar.txt - **local** host **->** **remote** host
**remote** host a **local** host

```shell
$ scp username@example.com:foobar.txt /some/local/directory
```

## Para Directorios

**Directorio** "foo" **local** **->** **remote** "bar"

```shell
$ scp -r foo your_username@remotehost.edu:/some/remote/directory/bar
```
**Directorio** "foo" **local** **->** **remote** "bar"

## Otros usos

[SCP](http://www.hypexr.org/linux_scp_help.php)

