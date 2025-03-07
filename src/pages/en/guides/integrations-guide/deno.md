---
# NOTE: This file is auto-generated from 'scripts/generate-integration-pages.ts'
#       and pulls content directly from the package’s README.
#       DO NOT MAKE EDITS TO THIS FILE DIRECTLY, THEY WILL BE OVERWRITTEN!
#       For corrections, please edit the package README at
#       https://github.com/withastro/astro/tree/main/packages/integrations/deno/
#
# TRANSLATORS: please remove this note and the <DontEditWarning/> component.

layout: ~/layouts/IntegrationLayout.astro
title: '@astrojs/deno'
githubURL: 'https://github.com/withastro/astro/tree/main/packages/integrations/deno/'
hasREADME: true
category: adapter
i18nReady: false
setup: |
  import Video from '~/components/Video.astro';
  import DontEditWarning from '../../../../components/DontEditWarning.astro';
---

<DontEditWarning/>

This adapter allows Astro to deploy your SSR site to Deno targets.

Learn how to deploy your Astro site in our [Deno Deploy deployment guide](/en/guides/deploy/deno/).

## Why Astro Deno

If you're using Astro as a static site builder—its behavior out of the box—you don't need an adapter.

If you wish to [use server-side rendering (SSR)](/en/guides/server-side-rendering/), Astro requires an adapter that matches your deployment runtime.

[Deno](https://deno.land/) is a runtime similar to Node, but with an API that's more similar to the browser's API. This adapter provides access to Deno's API and creates a script to run your project on a Deno server.

## Installation

Add the Deno adapter to enable SSR in your Astro project with the following `astro add` command. This will install the adapter and make the appropriate changes to your `astro.config.mjs` file in one step.

```sh
# Using NPM
npx astro add deno
# Using Yarn
yarn astro add deno
# Using PNPM
pnpm astro add deno
```

If you prefer to install the adapter manually instead, complete the following two steps:

1.  Install the Deno adapter to your project’s dependencies using your preferred package manager. If you’re using npm or aren’t sure, run this in the terminal:

    ```bash
      npm install @astrojs/deno
    ```

2.  Update your `astro.config.mjs` project configuration file with the changes below.

    ```js ins={3,6-7}
    // astro.config.mjs
    import { defineConfig } from 'astro/config';
    import deno from '@astrojs/deno';

    export default defineConfig({
      output: 'server',
      adapter: deno(),
    });
    ```

Next, Update your `preview` script in `package.json` with the change below.

    ```json del={8} ins={9}
    // package.json
    {
      // ...
      "scripts": {
        "dev": "astro dev",
        "start": "astro dev",
        "build": "astro build",
        "preview": "astro preview"
        "preview": "deno run --allow-net --allow-read --allow-env ./dist/server/entry.mjs"
      }
    }
    ```

    You can now use this command to preview your production Astro site locally with Deno.

    ```bash
    npm run preview
    ```

## Usage

After [performing a build](/en/guides/deploy/) there will be a `dist/server/entry.mjs` module. You can start a server by importing this module in your Deno app:

```js
import './dist/entry.mjs';
```

See the `start` option below for how you can have more control over starting the Astro server.

You can also run the script directly using deno:

```sh
deno run --allow-net --allow-read --allow-env ./dist/server/entry.mjs
```

## Configuration

To configure this adapter, pass an object to the `deno()` function call in `astro.config.mjs`.

**`astro.config.mjs`**

```js
import { defineConfig } from 'astro/config';
import deno from '@astrojs/deno';

export default defineConfig({
  output: 'server',
  adapter: deno({
    //options go here
  })
});
```

### start

This adapter automatically starts a server when it is imported. You can turn this off with the `start` option:

```js
import { defineConfig } from 'astro/config';
import deno from '@astrojs/deno';

export default defineConfig({
  output: 'server',
  adapter: deno({
    start: false
  })
});
```

If you disable this, you need to write your own Deno web server. Import and call `handle` from the generated entry script to render requests:

```ts
import { serve } from "https://deno.land/std@0.132.0/http/server.ts";
import { handle } from './dist/entry.mjs';

serve((req: Request) => {
  // Check the request, maybe do static file handling here.

  return handle(req);
});
```

### port and hostname

You can set the port (default: `8085`) and hostname (default: `0.0.0.0`) for the deno server to use. If `start` is false, this has no effect; your own server must configure the port and hostname.

```js
import { defineConfig } from 'astro/config';
import deno from '@astrojs/deno';

export default defineConfig({
  output: 'server',
  adapter: deno({
    port: 8081,
    hostname: 'myhost'
  })
});
```

## Examples

The [Astro Deno](https://github.com/withastro/astro/tree/main/examples/deno) example includes a `preview:deno` command that runs the entry script directly. Run `npm run build` then `npm run preview:deno` to run the production deno server.

## Troubleshooting

For help, check out the `#support-threads` channel on [Discord](https://astro.build/chat). Our friendly Support Squad members are here to help!

You can also check our [Astro Integration Documentation][astro-integration] for more on integrations.

## Contributing

This package is maintained by Astro's Core team. You're welcome to submit an issue or PR!

## Changelog

See [CHANGELOG.md](https://github.com/withastro/astro/tree/main/packages/integrations/deno/CHANGELOG.md) for a history of changes to this integration.

[astro-integration]: /en/guides/integrations-guide/
