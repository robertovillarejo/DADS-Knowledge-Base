### Instalar certificado

1 Descargar el certificado (o lo pueden bajar de aquí [catalogs.repositorionacionalcti.mx.crt](/uploads/4fd9863fe811d84d66e2082181cca581/catalogs.certificado.mx.crt))

```bash
$ echo -n | openssl s_client -connect catalogs.repositorionacionalcti.mx:443 |  sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > /tmp/catalogs.certificado.mx.crt
```

2 (opcional) Probar el certificado

```bash
$ openssl x509 -in /tmp/catalogs.certificado.mx.crt -text
```

3 Instalar el certificado en el *keystore* de Java

```bash
$ cd <PATH_TO_JAVA_HOME>
$ sudo bin/keytool -import -trustcacerts -keystore jre/lib/security/cacerts  -storepass changeit -noprompt -alias "catalogs.certificado.mx.crt" -file /tmp/catalogs.certificado.mx.crt
```

### Update

1. Para actualizar el certificado, primero se debe eliminar el anterior:

```bash
$ cd <PATH_TO_JAVA_HOME>
$ sudo bin/keytool -delete -keystore jre/lib/security/cacerts -storepass changeit -noprompt -alias 'catalogs.certificado.mx.crt'
```