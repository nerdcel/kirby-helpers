# Vite

Allows you to load assets generated by [Vite](https://vitejs.dev).

- In development mode, assets are loaded by Vite's development server, allowing you to benefit from all of its features, such as HMR.
- In production mode, URLs to the built assets are provided and served by PHP.

> ℹ️ Developed and tested with [Vite v4.0.2](https://github.com/vitejs/vite/tree/v4.0.2).

## How It Works

The plugin depends on Vite's [manifest functionality](https://vitejs.dev/config/build-options.html#build-manifest), which generates a `manifest.json` file that contains a mapping of non-hashed asset filenames to their hashed versions, used by this plugin to render the correct asset links. If this file doesn't exist, it's assumed that Vite is running in development mode and the plugin attempts to request files from Vite's development server.

## How to Use

### Enable Manifest

First and foremost, enable the [`build.manifest`](https://vitejs.dev/config/build-options.html#build-manifest) option in your `vite.config.ts`:

```ts
// vite.config.ts
import { defineConfig } from "vite";

export default defineConfig({
  build: {
    manifest: true,
  },
});
```

> ℹ️ You may want to take a deep dive into [Vite's backend integration guide](https://vitejs.dev/guide/backend-integration.html) to get an idea how Vite handles assets in traditional backends.

### Output JavaScript

You need to pass your entry script to the `js()` method:

```php
<?= vite()->js('src/index.js') ?>
```

### Output CSS

You have to do the same as for JavaScript, but use the `css()` method:

```php
<?= vite()->css('src/index.js') ?>
```

> ℹ️ When using Vite, CSS files are imported in the main JavaScript file. In order to find the CSS from the generated manifest, the plugin needs the file that _imports_ your CSS, not the CSS file itself.

> ℹ️ In development mode, the `css()` method does nothing because Vite automatically loads your CSS when your JavaScript loads, as well as the required `@vite/client` script. In other words, `vite()->js()` loads everything.

### Remove Generated Assets

As explained in the [How It Works](#how-it-works) section, to ensure proper function, you should remove the build folder every time you start your development server. This can easily be done with `rm -rf` in your npm dev script:

```json
{
  "scripts": {
    "dev": "rm -rf dist && vite"
  }
}
```

## Options

In your `config.php`, make sure you configure the plugin to match your `vite.config.ts`:

```php
// config.php
return [
    'johannschopplich.helpers.vite' => [
        'server' => [
            'port' => 5173,
            'https' => false,
        ],
        'build' => [
            'outDir' => 'dist'
        ]
    ]
];
```

> ℹ️ The values above are the default ones, so if they match your setup, you don't need to configure anything. 🤙

## Credits

Forked from [OblikStudio's `kirby-vite` plugin](https://github.com/OblikStudio/kirby-vite).

## License

[MIT](../LICENSE) License © 2021-2022 [Oblik Studio](https://github.com/OblikStudio)

[MIT](../LICENSE) License © 2022 [Johann Schopplich](https://github.com/johannschopplich)
