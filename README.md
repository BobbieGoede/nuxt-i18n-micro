<div class="links">
    <a href="https://www.npmjs.com/package/nuxt-i18n-micro" target="_blank">
        <img src="https://img.shields.io/npm/v/nuxt-i18n-micro/latest?style=for-the-badge" alt="npm version">
    </a>
    <a href="https://www.npmjs.com/package/nuxt-i18n-micro" target="_blank">
        <img src="https://img.shields.io/npm/dw/nuxt-i18n-micro?style=for-the-badge" alt="npm downloads">
    </a>
    <a href="https://www.npmjs.com/package/nuxt-i18n-micro" target="_blank">
        <img src="https://img.shields.io/npm/l/nuxt-i18n-micro?style=for-the-badge" alt="License">
    </a>
    <a href="https://nuxt.com" target="_blank">
        <img src="https://img.shields.io/badge/Nuxt-020420?logo=nuxt.js&style=for-the-badge" alt="Nuxt">
    </a>
</div>

<p align="center">
<img src="https://github.com/s00d/nuxt-i18n-micro/blob/main/logo.png?raw=true" alt="logo">
</p>

# Nuxt I18n Micro

`Nuxt I18n Micro` is a fast, simple, and lightweight internationalization (i18n) module for Nuxt. Despite its compact size, it's designed with large projects in mind, offering significant performance improvements over traditional i18n solutions like `nuxt-i18n`. The module was built from the ground up to be highly efficient, focusing on minimizing build times, reducing server load, and shrinking bundle sizes.

## Why Nuxt I18n Micro?

The `Nuxt I18n Micro` module was created to address critical performance issues found in the original `nuxt-i18n` module, particularly in high-traffic environments and projects with large translation files. Key issues with `nuxt-i18n` include:

- **High Memory Consumption**: Consumes significant memory during both build and runtime, leading to performance bottlenecks.
- **Slow Performance**: Especially with large translation files, it causes noticeable slowdowns in build times and server response.
- **Large Bundle Size**: Generates a large bundle, negatively impacting application performance.
- **Memory Leaks and Bugs**: Known for memory leaks and unpredictable behavior under heavy load.

### Performance Comparison

To showcase the efficiency of `Nuxt I18n Micro`, we conducted tests under identical conditions. Both modules were tested with a 10MB translation file on the same hardware.

#### Build Time and Resource Consumption

**Nuxt I18n**:
- **Total size**: 54.7 MB (3.31 MB gzip)
- **Max CPU Usage**: 391.4%
- **Max Memory Usage**: 8305 MB
- **Elapsed Time**: 0h 1m 31s

**Nuxt I18n Micro**:
- **Total size**: 1.93 MB (473 kB gzip) — **96% smaller**
- **Max CPU Usage**: 220.1% — **44% lower**
- **Max Memory Usage**: 655 MB — **92% less memory**
- **Elapsed Time**: 0h 0m 5s — **94% faster**

#### Server Performance (10k Requests)

**Nuxt I18n**:
- **Requests per second**: 49.05 [#/sec] (mean)
- **Time per request**: 611.599 ms (mean)
- **Max Memory Usage**: 703.73 MB

**Nuxt I18n Micro**:
- **Requests per second**: 61.18 [#/sec] (mean) — **25% more requests per second**
- **Time per request**: 490.379 ms (mean) — **20% faster**
- **Max Memory Usage**: 323.00 MB — **54% less memory usage**

These results clearly demonstrate that `Nuxt I18n Micro` significantly outperforms the original module in every critical area.

## Key Features

- 🌐 **Compact Yet Powerful**: Despite its small size, `Nuxt I18n Micro` is designed for large-scale projects, focusing on performance and efficiency.
- ⚡ **Optimized Build and Runtime**: Reduces build times, memory usage, and server load, making it ideal for high-traffic applications.
- 🛠 **Minimalist Design**: The module is structured around just 5 components (1 module and 4 plugins), making it easy to understand, extend, and maintain.
- 📏 **Efficient Routing**: Generates only 2 routes regardless of the number of locales, thanks to dynamic regex-based routing, unlike other i18n modules that generate separate routes for each locale.
- 🗂 **Streamlined Translation Loading**: Only JSON files are supported, with translations split between a global file for common texts (e.g., menus) and page-specific files, which are auto-generated in the `dev` mode if not present.

## Quick Setup

Install the module in your Nuxt application with:

```bash
npm install nuxt-i18n-micro
```

Then, add it to your `nuxt.config.ts`:

```typescript
export default defineNuxtConfig({
  modules: [
    'nuxt-i18n-micro',
  ],
  i18n: {
    locales: [
      { code: 'en', iso: 'en-US', dir: 'ltr' },
      { code: 'fr', iso: 'fr-FR', dir: 'ltr' },
      { code: 'ar', iso: 'ar-SA', dir: 'rtl' },
    ],
    defaultLocale: 'en',
    translationDir: 'locales',
    meta: true,
  },
})
```

That's it! You're now ready to use Nuxt I18n Micro in your Nuxt app.

## Folder Structure

Translations are organized into global and page-specific files:

```
/locales
  /pages
    /index
      en.json
      fr.json
      ar.json
    /about
      en.json
      fr.json
      ar.json
  en.json
  fr.json
  ar.json
```

- **Global File**: Located at `locales/{locale}.json` (e.g., `locales/en.json`). Used for common texts shared across multiple pages, such as menus.
- **Page-Specific Files**: Located at `locales/pages/{routeName}/{locale}.json` (e.g., `locales/pages/index/en.json`). These are used for translations specific to individual pages.

## Plugin Methods

### `$getLocale()`
Returns the current locale code.

```typescript
const locale = $getLocale()
```

### `$getLocales()`
Returns an array of all available locales configured in the module.

```typescript
const locales = $getLocales()
```

### `$t(key: string, params?: Record<string, any>, defaultValue?: string)`
Fetches a translation for the given key. Optionally interpolates parameters into the translation.

```typescript
const welcomeMessage = $t('welcome', { username: 'Alice', unreadCount: 5 })
```

### `$tc(key: string, count: number, defaultValue?: string)`
Fetches a pluralized translation for the given key based on the count.

```typescript
const appleCountMessage = $tc('apples', 10)
```

### `$switchLocale(locale: string)`
Switches to the given locale and redirects the user to the appropriate localized route.

```typescript
$switchLocale('fr')
```

### `$localeRoute(to: RouteLocationRaw, locale?: string): RouteLocationRaw`
Generates a localized route object based on the target route.

```typescript
const localizedRoute = $localeRoute({ name: 'index' })
```

### `$mergeTranslations(newTranslations: Translations)`
Merges new translations into the existing translation cache for the current route and locale.

```typescript
$mergeTranslations({
  welcome: 'Bienvenue, {username}!'
})
```

## useNuxtApp

```ts
import { useNuxtApp } from '#imports'

const { $getLocale, $switchLocale, $getLocales, $localeRoute, $t } = useNuxtApp()
```

## Composable

```ts
import { useI18n } from '#imports'

const { $getLocale, $switchLocale, $getLocales, $localeRoute, $t } = useI18n()
// or
const i18n = useI18n()

```

## Example

```vue
<template>
  <div>
    <p>{{ $t('key2.key2.key2.key2.key2') }}</p>
    <p>Current Locale: {{ $getLocale() }}</p>

    <div>
      {{ $t('welcome', { username: 'Alice', unreadCount: 5 }) }}
    </div>
    <div>
      {{ $tc('apples', 10) }}
    </div>

    <!-- Ссылки для переключения локалей -->
    <div>
      <button
        v-for="locale in $getLocales()"
        :key="locale"
        :disabled="locale === $getLocale()"
        @click="$switchLocale(locale.code)"
      >
        Switch to {{ locale.code }}
      </button>
    </div>

    <div>
      <NuxtLink :to="$localeRoute({ name: 'index' })">
        Go to Index
      </NuxtLink>
    </div>
  </div>
</template>

<script setup>
  import { useI18n } from '#imports'

  const { $getLocale, $switchLocale, $getLocales, $localeRoute, $t, $tc } = useI18n()
</script>

```

## Module Options

The module accepts the following options in the Nuxt configuration:

- **locales**: An array of locale objects. Each locale should have a `code`, and optionally, `iso` and `dir` (for RTL/LTR).
  - Example:
    ```typescript
    locales: [
      { code: 'en', iso: 'en-US', dir: 'ltr' },
      { code: 'fr', iso: 'fr-FR', dir: 'ltr' },
      { code: 'ar', iso: 'ar-SA', dir: 'rtl' }
    ]
    ```
- **meta**: A boolean indicating whether to automatically generate SEO-related meta tags (like `alternate` links).
- **defaultLocale**: The default locale code (e.g., `'en'`).
- **translationDir**: The directory where translation files are stored (e.g., `'locales'`).
- **autoDetectLanguage**: If `true`, automatically detects the user's preferred language and redirects accordingly.
- **plural**: A custom function for handling pluralization.
- **includeDefaultLocaleRoute**: A boolean. If enabled, all routes without a locale prefix will redirect to the default locale route.
- **cache**: (In development) A boolean option designed to optimize performance when working with large JSON translation files. When enabled, it caches translations specific to the current page, reducing search times and minimizing client-side load. This cached data is then sent to the client, resulting in faster page loads and improved user experience.

## Locale Loading in Nuxt I18n Micro

The `Nuxt I18n Micro` module provides an efficient way to manage internationalization (i18n) in your Nuxt application, with a focus on performance and simplicity. One of the key features of this module is how it handles the loading of locale-specific translations. Below is an explanation of how this process works.

### How Locale Loading Works

In `Nuxt I18n Micro`, translations are loaded dynamically based on the user's selected locale. The module uses a specific route pattern (`/_locales/:page/:locale/data.json`) to fetch the translation files for each page and locale. This approach is designed to minimize the initial load time and only load the necessary translations for the current page and locale.

#### Route Structure

The translations are organized into JSON files located in the `locales` directory. These files are split into:

- **Global Files**: Located at `locales/{locale}.json` (e.g., `locales/en.json`). These files contain translations shared across the entire application, such as menu items or common UI elements.
- **Page-Specific Files**: Located at `locales/pages/{routeName}/{locale}.json` (e.g., `locales/pages/index/en.json`). These files contain translations specific to individual pages.

#### Dynamic Locale Routes

To accommodate the dynamic nature of translation loading, `Nuxt I18n Micro` extends the default routing configuration by adding locale-specific routes. This means that for each page, a localized version of the route is generated. For example, if your application has an `index` page and supports English (`en`) and French (`fr`), the following routes would be generated:

- `/en/index`
- `/fr/index`

This structure allows the module to load the appropriate translations based on the user's selected locale.

### Loading Translations via _locales Route

Given the way Vite and Nuxt handle modules, the `Nuxt I18n Micro` module uses a special route (`/_locales/:page/:locale/data.json`) to fetch translations. This is necessary because, during the build process, Vite may not bundle all translation files directly into the application. Instead, the module dynamically loads these files from the server as needed.

When a user navigates to a page, the module will:

1. Check if the necessary translation file for the current page and locale is already loaded in the cache.
2. If not, it will send a request to the `_locales` route to fetch the required translation file.
3. Once the translation file is loaded, it is cached for future use, ensuring that the translations are readily available when the user navigates to another part of the application.

### Caching and Pre-rendering

To further optimize performance, `Nuxt I18n Micro` supports caching and pre-rendering of translation files:

- **Caching**: Once a translation file is loaded, it is stored in the cache, reducing the need for subsequent requests.
- **Pre-rendering**: During the build process, the module can pre-render translation files for all configured locales and routes, which are then served directly from the server without the need for runtime requests.


## Conclusion

`Nuxt I18n Micro` offers a highly efficient, minimalist approach to internationalization in Nuxt applications. By focusing on performance and simplicity, it provides a powerful alternative to heavier, more complex i18n solutions. Whether you're building a small website or a large-scale application, `Nuxt I18n Micro` helps you manage multilingual content with ease.

For more details and updates, visit the [Nuxt I18n Micro GitHub repository](https://github.com/s00d/nuxt-i18n-micro).
