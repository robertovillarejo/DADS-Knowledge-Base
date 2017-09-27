# Lineamiento para la creación de Repositorios en Gitlab

La url deberá de cumplir con el siguiente formato:

`gitlab.dads.infotec.mx/{idNombreCliente}/{idNombreProyecto}`

en dónde:

#### {idNombreCliente} :
Es el nombre del cliente para el cual se está realizando el proyecto y deberá de cumplir con la siguiente expresión:

```bash
^[a-z][a-z]{3,7}([-][a-z]{3,7}){0,2}
```

**Nota**: La expresión regular puede ser validada en https://regex101.com/
##### nombres válidos:

`inee`  
`conacyt`  
`profeco`  
`funcion-publica`  
##### nombres no válidos

`3inee_otro`  
`CON`  
`profeco_vuela`  
`funcion_publica`

#### {idNombreProyecto}:
Es el nombre del cliente para un clientey deberá de cumplir con la siguiente expresión:

```bash
^[a-z][a-z]{3,7}([-][a-z]{3,7}){0,2}
```

**Nota**: La expresión regular puede ser validada en https://regex101.com/

##### nombres válidos:

`bien`  
`repositorio`  
`vuela`  

##### nombres no válidos

`CONACYT_bien`  
`REPOSITORIO`  
`1profeco_vuela`
