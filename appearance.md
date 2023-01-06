# Appearance

Forumium uses **Vite** as a tool to build the application assets, and **Tailwind CSS** as the main style Framework.

## Vite

You can find the application Vite configurations in the file `vite.config.js`:

```javascript
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';

export default defineConfig({
    plugins: [
        laravel({
            input: [
                'resources/js/app.js',
                'resources/css/filament.scss'
            ],
            refresh: true,
        }),
    ],
});
```

You can see there are two files included in the build:

- `resources/js/app.js`: it's the main application JavaScript file, this file include also the main styles file `resources/css/app.scss`, this file is included only for the **Front** section of the platform (not the **Administration** section)
- `resources/css/filament.scss`: it's the file used to customize the **Filament** administration section, as mentionned it is used only in the **Administration** section (not the **Front** section)

## Tailwind CSS

The Tailwind CSS is the main style Framework used by Forumium platform.

You can find the Tailwind CSS configurations in the file `tailwind.config.js`:

```javascript
/** @type {import('tailwindcss').Config} */
const colors = require('tailwindcss/colors')

module.exports = {
    mode: 'jit',
    content: [
        './resources/**/*.blade.php',
        './resources/**/*.js',
        './node_modules/flowbite/**/*.js',
        './vendor/filament/**/*.blade.php',
        './app/Http/Livewire/**/*.php',
        './app/Filament/Resources/**/*.php'
    ],
    theme: {
        extend: {
            colors: {
                danger: colors.rose,
                primary: colors.blue,
                success: colors.green,
                warning: colors.yellow,
            },
        },
    },
    plugins: [
        require('flowbite/plugin'),
        require('@tailwindcss/forms'),
        require('@tailwindcss/typography'),
        require('tailwindcss-textshadow')
    ],
}
```

As mentionned in the `plugins` section of this file, Forumium is using:

- `flowbite/plugin`: is an open-source library of UI components, sections, and pages build with utility classes from Tailwind CSS and designed in Figma
  - Check the plugin website: [https://flowbite.com](https://flowbite.com/)
- `@tailwindcss/forms` and `@tailwindcss/typography`: two official plugins of Tailwind CSS
  - Check `@tailwindcss/forms` docs: [https://tailwindcss.com/docs/plugins#forms](https://tailwindcss.com/docs/plugins#forms)
  - Check `@tailwindcss/typography` docs: [https://tailwindcss.com/docs/plugins#typography](https://tailwindcss.com/docs/plugins#typography)
- `tailwindcss-textshadow`: a plugin used to inject the `text-shadow` classes into HTML attributes
  - Check the plugin docs: [https://www.npmjs.com/package/tailwindcss-textshadow](https://www.npmjs.com/package/tailwindcss-textshadow)

## Customization

If you want to customize the appearance of the Forumium platform, the file `resources/js/app.js` is where you need to add your codes!