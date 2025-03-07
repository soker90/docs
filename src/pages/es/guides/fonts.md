---
title: Usando fuentes personalizadas
description: ¿Buscas agregar tipografías personalizadas a un proyecto de Astro? Usa Google Fonts con Fontsource o añade una fuente a elección.
i18nReady: true
layout: ~/layouts/MainLayout.astro
setup: |
    import PackageManagerTabs from '~/components/tabs/PackageManagerTabs.astro';
---

Astro soporta todas las estrategias comunes para agregar tipografías personalizadas al diseño de tu sitio. Esta guía te mostrará dos opciones diferentes para incluir fuentes web en tu proyecto.

## Usando un archivo de fuente local

Si deseas añadir archivos de fuentes a tu proyecto, te recomendamos posicionarlas en tu [carpeta `public/`](/es/core-concepts/project-structure/#public). Luego en tu CSS puedes registrar las fuentes mediante la [declaración `@font-face`](https://developer.mozilla.org/es/docs/Web/CSS/@font-face) y usar la propiedad `font-family` para estilar tu sitio.

### Ejemplo

Imaginemos que tienes un archivo de fuente `DistantGalaxy.woff`.

1. Añade tu archivo de fuente a la carpeta `public/fonts/`.

2. Añade una declaración `@font-face` en tu CSS. Esto puede estar en un archivo global `.css` importado o en un bloque `<style>` en la plantilla de página o componente donde quieras usar esta fuente.

    ```css
    /* Registramos nuestra font family personalizada y le indicamos al navegador dónde encontrarla. */
    @font-face {
      font-family: 'DistantGalaxy';
      src: url('/fonts/DistantGalaxy.woff') format('woff');
      font-weight: normal;
      font-style: normal;
      font-display: swap;
    }
    ```

    :::note
    No debemos incluir `public` en la URL de la fuente porque todos los archivos en `public` son añadidos a la carpeta raíz de tu sitio.
    :::

3. Usa la `font-family` de la declaración `@font-face` para estilar elementos en tu componente o plantilla de página. En este ejemplo, el título `<h1>` tendrá aplicada la fuente personalizada, mientras que el párrafo `<p>` no.

    ```astro {10-12}
    ---
    // src/pages/example.astro
    ---

    <h1>En una galaxia muy, muy lejana...</h1>

    <p>¡Las fuentes personalizadas hace que mis títulos se vean mucho mejor!</p>

    <style>
    h1 {
      font-family: 'DistantGalaxy', sans-serif;
    }
    </style>
    ```

## Usando Fontsource

El proyecto [Fontsource](https://fontsource.org/) simplifica el uso de Google Fonts y otras fuentes de código abierto. Provee módulos npm que puedes instalar para las fuentes que desees utilizar.

1. Encuentra la fuente que desees usar en el [catálogo de Fontsource](https://fontsource.org/fonts). Para este ejemplo, usaremos [Twinkle Star](https://fontsource.org/fonts/twinkle-star).

2. Instala el paquete de la fuente escogida.

    <PackageManagerTabs>
      <Fragment slot="npm">
      ```shell
      npm i @fontsource/twinkle-star
      ```
      </Fragment>
      <Fragment slot="pnpm">
      ```shell
      pnpm i @fontsource/twinkle-star
      ```
      </Fragment>
      <Fragment slot="yarn">
      ```shell
      yarn add @fontsource/twinkle-star
      ```
      </Fragment>
    </PackageManagerTabs>

    :::tip
    Puedes encontrar el nombre correcto del paquete en la sección “Quick Installation” en la página de cada fuente en el sitio de Fontsource. Comienza con `@fontsource/` seguido del nombre de la fuente.
    :::

3. Importa el paquete de la fuente en la plantilla de página o componente donde desees utilizar la fuente. Comúnmente, querrás hacer esto en un componente de plantilla de página común para asegurarte que la fuente esté disponible en todo tu sitio.

    La importación añadirá las reglas de `@font-face` necesarias para configurar la fuente.

    ```astro
    ---
    // src/layouts/BaseLayout.astro
    import '@fontsource/twinkle-star';
    ---
    ```

4. Usa `font-family` tal como se muestra en la página de la fuente en Fontsource. Esto funcionará en cualquier lugar donde puedas escribir CSS en tu proyecto de Astro.

    ```css
    h1 {
      font-family: "Twinkle Star", cursive;
    }
    ```

## Más recursos

### Añade fuentes usando Tailwind

Si estás usando la [integración de Tailwind](/es/guides/integrations-guide/tailwind/), puedes añadir una declaración de `@font-face` para usar una fuente local statement o bien puedes usar la estrategia `import` de Fontsource para registrar tu fuente. Luego, sigue la [documentación de Tailwind sobre cómo añadir familias de fuentes personalizadas](https://tailwindcss.com/docs/font-family#using-custom-values).

### Aprende cómo funcionan las fuentes web

La [guía de fuentes web de MDN](https://developer.mozilla.org/es/docs/Learn/CSS/Styling_text/Web_fonts) tiene una introducción al tema.

### Genera CSS para tu fuente

El [Generador de Webfont de Font Squirrel](https://www.fontsquirrel.com/tools/webfont-generator) puede ayudar a preparar los archivos de fuentes por ti.
