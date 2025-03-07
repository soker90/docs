---
layout: ~/layouts/MainLayout.astro
title: Páginas
description: Introducción a páginas de Astro
i18nReady: true
---

Las **páginas** son [componentes de Astro](/es/core-concepts/astro-components/) que se encuentran en la subcarpeta `src/pages/`. Ellas son las responsables de manejar el enrutamiento, la carga de datos y el diseño general de cada página HTML de tu proyecto.

## Tipos de página compatibles

Astro es compatible con los siguientes tipos de archivos en el directorio `src/pages/`:

- [`.astro`](#páginas-de-astro)
- [`.md`](#páginas-markdownmdx)
- `.mdx` (con la [integración de MDX](/es/guides/integrations-guide/mdx/#installation)) instalada
- [`.js`/`.ts`](#ruta-de-archivos)
- [`.html`](#páginas-html)


## Enrutamiento basado en archivos

Astro aprovecha una estrategia de enrutamiento llamada **enrutamiento basado en archivos**. Cada archivo `.astro` en la carpeta `src/pages` se convierte en una página o un endpoint en tu proyecto deacuerdo a su ruta.

Escriba elementos HTML [`<a>`](https://developer.mozilla.org/es/docs/Web/HTML/Element/a) estándar en el maquetado del componente para vincular entre páginas.

📚 Lea más sobre [enrutamiento en Astro](/es/core-concepts/routing/)

## Páginas de Astro

Las páginas de Astro utilizan la extensión `.astro` y tienen las mismas funcionalidades que los [componentes de Astro](/es/core-concepts/astro-components/).

```astro title="src/pages/index.astro"
---
---
<html lang="es">
  <head>
    <title>Mi página de inicio</title>
  </head>
  <body>
    <h1>Bienvenido a mi sitio web</h1>
  </body>
</html>
```

Para evitar repetir los mismos elementos HTML en cada página, puedes mover los elementos comunes `<head>` y `<body>` a sus propios [componentes de plantilla](/es/core-concepts/layouts/). Puedes usar tantos o tan pocos componentes de plantilla como desees.

```astro title="src/pages/index.astro" {2} /</?MySiteLayout>/
---
import MySiteLayout from '../layouts/MySiteLayout.astro';
---
<MySiteLayout>
  <p>El contenido de mi página envuelto en un componente de plantilla</p>
</MySiteLayout>
```

📚 Lea más sobre [componentes de plantilla](/es/core-concepts/layouts/) en Astro.

## Páginas Markdown/MDX

Astro trata archivos Markdown (`.md`) dentro de `src/pages/` como páginas en tu proyecto. Si tienes la [integración de MDX instalada](/es/guides/integrations-guide/mdx/#installation), también procesa los archivos MDX (`.mdx`) de la misma manera. Estos se usan comúnmente para páginas con mucho texto, como artículos de blog y documentación.

Las plantillas de página son especialmente útiles para [archivos Markdown](#páginas-markdownmdx). Los archivos Markdown pueden usar la propiedad de frontmatter `layout` para especificar un [componente de plantilla](/es/core-concepts/layouts/) que envolverá el contenido de Markdown en un  documento de página `<html>...</html>`.

```md {3}
---
# Example: src/pages/page.md
layout: '../layouts/MySiteLayout.astro'
title: 'Mis páginas Markdown'
---
# Título

Esta es mi página, escrita en **Markdown.**
```

📚 Lea más sobre [Markdown](/es/guides/markdown-content/) en Astro.

## Ruta de archivos

**Las rutas de archivo** son archivos script que terminan con la extensión `.js` o `.ts` y se encuentran dentro de la carpeta `src/pages/`. Estos pueden crear páginas que no sean HTML, como .json o .xml, o incluso activos como imágenes.

La extensión `.js` o `.ts` se eliminará durante el proceso de compilación. Por ejemplo, `src/pages/data.json.ts` se creará para que coincida con la ruta `/data.json`.

```js title="src/pages/builtwith.json.ts"
// Resultado: /builtwith.json

// Las rutas de archivo exportan una función get(), que se llama al generar el archivo.
// Devuelve un objeto con `body` para guardar el contenido del archivo en la compilación final.
export async function get() {
  return {
    body: JSON.stringify({
      name: 'Astro',
      url: 'https://astro.build/',
    }),
  };
}
```

Como las rutas API, las rutas de archivos reciben un objeto `APIContext` que contiene [params](/es/reference/api-reference/#params) y [request](https://developer.mozilla.org/en-US/docs/Web/API/Request):

```ts title="src/pages/request-path.json.ts"
import type { APIContext } from 'astro';

export async function get({ params, request }: APIContext) {
  return {
    body: JSON.stringify({
      path: new URL(request.url).pathname
    })
  };
}
```

También puedes escribir rutas API usando el tipo `APIRoute`. Esto te dará mejores mensajes de error cuando la ruta API devuelva el tipo incorrecto:

```ts title="src/pages/request-path.json.ts"
import type { APIRoute } from 'astro';

export const get: APIRoute = ({ params, request }) => {
  return {
    body: JSON.stringify({
      path: new URL(request.url).pathname
    })
  };
};
```

Opcionalmente, puedes devolver una opción de codificación `encoding` en compilaciones estáticas. Este puede ser cualquier [`BufferEncoding`](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/bdd02508ddb5eebcf701fdb8ffd6e84eabf47885/types/node/buffer.d.ts#L169) válido aceptado por node `fs.writeFile`. Por ejemplo, para producir una imagen png binaria usando SSG:

```ts title="src/pages/image.png.ts" {7}
import type { APIRoute } from 'astro';

export const get: APIRoute = ({ params, request }) => {
  const buffer = ...;
  return {
    body: buffer.toString('binary'),
    encoding: 'binary',
  };
};

```

## Páginas HTML

Los archivos `.html` pueden ser colocados en `src/pages/` y usados directamente como páginas en tu proyecto. Ten en cuenta que algunas funcionalidades clave de Astro no son compatibles con [Componentes HTML](/es/core-concepts/astro-components/#componentes-html).

## Página de error 404 personalizada

Para crear una página de error 404 personalizada, puedes crear un archivo `404.astro` o `404.md` en `/src/pages`.

Esto generará una página `404.html` que la mayoría de los [servicios de despliegue](/es/guides/deploy/) encontrarán y usarán.
