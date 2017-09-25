# Extender LVM
1. Revisar si se cuenta con el espacio necesario en el VolumeGroup con el comando `vgs` :

```bash
[root@dadsgr admlinux]# vgs
VG              #PV #LV #SN Attr        VSize   VFree
dadsgr          1      9      0  wz--n- 495.50g 447.50g
```

2. Hay 447.5GB libres para incrementar el filesystem usr
 /dev/mapper/dadsgr-usr     3.0G  2.7G  146M  95% /usr
ejecutamos el siguiente comando:

```bash
lvextend -L+5G /dev/dadsgr/usr
```
**Nota*** donde +5G son los gigas a incrementar

3. Una vez incrementado el **lvm** hay que efectuar los cambios en el filesystem con el siguiente comando:

```bash
resize2fs /dev/dadsgr/usr
```

4. Validamos los cambios con el siguiente comando

```bash
df –h
```

