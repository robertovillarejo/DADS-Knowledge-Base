> **Autor** : INFOTEC - DADS

# tar con gzip

**Comprimir**
```shell
tar czvf compressed.tar.gz /the/directory
```
**ver contenido**

```shell
tar tzvf compressed.tar.gz
```

**Extraer**

```shell
tar xzvf compressed.tar.gz
```

# tar with bzip2

**Comprimir**

```shell
tar cjvf bzipcompressed.tar.bz2 directory2
```
**Ver contenido**

```shell
tar tjvf bzipcompressed.tar.bz2
```

**Extraer**

```shell
tar xjvf bzipcompressed.tar.bz2
```

# Utilizar tar con xz

**Comprimir**

```shell
tar cJvf xzcompressed.tar.xz directory3
```

**Ver contenido**`

``shell
tar tJvf xzcompressed.tar.xz
```

**Extraer**

```shell
tar xJvf xzcompressed.tar.xz
```
# Utilizar ZIP

**Comprimir**

```shell
zip -r archivename.zip file1 file2 folder1
```
**Ver contenido**

```shell
less archivename
```
**Extraer**

```shell
unzip -l abc.zip
```

# Realizar respaldo del sistema LINUX - COMPLETO

Colocarse en **/** como usuario **root**

**Compactar**

tar cvpzf backup.tgz --exclude=/proc --exclude=/lost+found --exclude=/backup.tgz --exclude=/mnt --exclude=/sys /

**Extraer**

tar xvpfz backup.tgz -C /


**Compactar**
tar cvpjf backup.tar.bz2 --exclude=/proc --exclude=/lost+found --exclude=/backup.tar.bz2 --exclude=/mnt --exclude=/sys /

**Extraer**
tar xvpfj backup.tar.bz2 -C /


Después, crear nuevamente los directorios que se excluyeron:

```bash
mkdir proc
mkdir lost+found
mkdir mnt
mkdir sys
```

#### Referencias:

[tar](https://www.digitalocean.com/community/tutorials/an-introduction-to-file-compression-tools-on-linux-servers)
[zip](http://www.linuxnix.com/linuxunix-zip-and-unzip-command-examples/)