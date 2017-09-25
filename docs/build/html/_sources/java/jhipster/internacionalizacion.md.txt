# Internacionalización

## Introducción

Durante la generación de un nuevo proyecto se te preguntará si deseas o no habilitar el soporte de internacionalización.

Si lo habilitas necesitarás seleccionar el idioma nativo de tu aplicación. Después de eso puedes elegir idiomas adicionales que te gustaría instalar. Si no deseas añadir soporte para ningún idioma adicional al principio siempre puedes añadir idiomas más tarde cuando sea necesario ejecutando el sub-generador de idiomas. 

Si estás seguro de jamás traducir esta aplicación en otro lenguaje no deberías habilitas la internacionalización. 

## Idiomas soportados

Actualmente estos son los idiomas soportados

* Árabe \(Libia\)
* Armenio
* Indonesio
* Catalán
* Chino \(Simplificado\)
* Chino \(Tradicional\)
* Checo
* Danés
* Holandés
* Inglés
* Estonio
* Farsi
* Francés
* Gallego
* Alemás
* Griego
* Hindi
* Húngaro
* Italiano
* Japonés
* Coreano
* Maratí
* Polaco
* Portugués
* Portugués \(Brasil\)
* Rumano
* Ruso
* Eslovaco
* Serbio
* Español
* Sueco
* Turco
* Tamil
* Tailandés
* Ucraniano
* Vietnamita

¿_Falta tu idioma en JHipster? !Ayúdanos a mejorar el proyecto con un Pull Request!_

## ¿Cómo añadir idiomas después de la generación del proyecto?

Para hacer esto puedes ejecutar el sub-generador de idiomas con:

`jhipster languages`

![](http://www.jhipster.tech/images/install_new_languages.png)

Nota que necesitarás regeneras tus entidades si te gustaría tenerlas traducidas en el idioma que acabas de añadir.

## ¿Cómo añadir un nuevo idioma que no es soportado?

Todos los idiomas se guardan en la carpeta `src/main/webapp/i18n` \(del lado del cliente\) y `src/main/resources/i18n` \(del lado del servidor\)

Aquí están los pasos para instalar un nuevo idioma llamado `nuevo_idioma`:

1. Duplica la carpeta `src/main/webapp/i18/en` a `src/main/webapp/i18/nuevo_idioma` \(aquí es donde se almacenan todas las traducciones en el front-end\)
2. Traduce todos los archivos que se encuentran en la carpeta `src/main/webapp/i18/nuevo_idioma`
3. En AngularJS 1 añade el código del idioma `nuevo_idioma` a la constante `LANGUAGES` definida en `src/main/webapp/app/components/language/language.constants.js`

```
.constant('LANGUAGES', [
    'en',
    'fr',
    'nuevo_idioma'
    // jhipster-needle-i18n-language-constant - JHipster will add/remove languages in this array
]
```

En Angular 2+ añade el código del idioma `nuevo_idioma `a la constante `LANGUAGES `definida en `src/main/webapp/app/shared/language/language.constants.ts`

```
export const LANGUAGES: string[] = [
    'en',
    'fr',
    'nuevo_idioma'
    // jhipster-needle-i18n-language-constant - JHipster will add/remove languages in this array
];
```

4. En la carpeta `src/main/resources/i18n`, copia el archivo `messages_en.properties`_ a _`messages_nuevo_idioma.properties` \(aquí es donde se almacenan las traducciones del lado del servidor\).

5. Traduce todas las claves en el archivo `messages_nuevo_idioma.properties`

6. En AngularJS 1 añade el nuevo nombre de idioma en la función de `filter('findLanguageFromKey')` en el archivo `src/main/webapp/app/components/language/language.filter.js`. En Angular 2+ añade el nuevo nombre de idioma en la variable `languages `de `FindLanguageFromKeyPipe` en `src/main/webapp/app/shared/language/language.pipe.ts`

7. En Angular 2+ añade el nuevo idioma agrupándolo en `webpack.common.js`

```
new MergeJsonWebpackPlugin({
    output: {
        groupBy: [
            { pattern: "./src/main/webapp/i18n/en/*.json", fileName: "./i18n/en.json" },
            { pattern: "./src/main/webapp/i18n/nuevo_idioma/*.json", fileName: "./i18n/new_lang.json" }
            // jhipster-needle-i18n-language-webpack - JHipster will add/remove languages in this array
        ]
    }
})
```

El idioma `nuevo_idioma `ahora está disponible en el menú de idiomas y está disponible en la aplicación front-end \(Angular\) y en la aplicación back-end \(Spring\).

### Aportando el idioma al generador de jhipster

Si te gustaría aportar un nuevo idioma al generador sigue los pasos 1, 2,4 y 5 anteriores. Añade una entrada para el nuevo idioma a la constante `LANGUAGES `en `generators/generator-constants.js` y añade el idioma a `test/templates/all-languages/.yo-rc.json` en el proyecto `generator-jhipster`. Envía un _Pull Request _con todos esos cambios. 

## ¿Cómo eliminar un idioma existente?

Aquí los pasos para eliminar un idioma llamado `idioma_viejo`:

1. Elimina la carpeta del idioma de `src/main/webapp/i18/idioma_viejo`
2. Elimina el entrada de constante en `src/main/webapp/app/components/language/language.constants.js` o `src/main/webapp/app/shared/language/language.constants.ts` y `webpack.common.js`
3. Elimina el archivo `src/main/resources/i18n/messages_viejo_idioma.properties`



