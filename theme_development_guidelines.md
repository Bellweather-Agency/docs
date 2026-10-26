# Theme Development Guidelines

## Timber and Twig

We use [Timber](https://timber.github.io/docs/) to separate WordPress logic from presentation.

- PHP prepares WordPress data, queries, and ACF fields and adds them to the Timber context.
- Twig templates render that context into HTML.
- Where possible UI should use reusable Twig components or includes.
- Keep complex logic and data transformation in PHP rather than Twig.
- Run `composer install` from the directory containing `composer.json` to install Timber and other required PHP packages.


## Connecting Vue Components to Twig

We sometimes use Vue for interfaces that require more client-side state or interaction, such as navigation.

1. Create the Vue component in `assets/_src/js/vue-components/`.
2. Register it in `vue-components/index.js`.
3. Add a `data-vue` wrapper to the Twig template.
4. Render the registered component inside that wrapper.
5. Pass Timber or ACF data into the component through escaped props.

```twig
<header data-vue>
  <nav-bar
    items-str="{{ menu_items|json_encode|e('html_attr') }}"
  ></nav-bar>
</header>
```

Use Vue only when a component is especially complex or benefits from persistent client-side state. Simpler interactions should generally use HTML, CSS, or vanilla JavaScript.

## Styling

We generally use Sass for theme styling and compile it through the project’s build process--be sure to `npm run build` after any styling changes. 

Use BEM syntax wherever possible:

- **Block:** `.content-card`
- **Element:** `.content-card__image`
- **Modifier:** `.content-card--featured`

### General Guidelines

- Write default desktop styles first, followed by responsive media-query sections in descending breakpoint order. If an existing project uses a different convention, follow it rather than mixing approaches.
- Use `rem` units over `px` for typography, spacing, and component dimensions. Reserve pixels for details that require precise values regardless of viewport.
- Create or use existing Sass variables for colors, spacing, typography, and breakpoints instead of hardcoded values.
- Do not use `!important`. Resolve conflicts through appropriate selector specificity, source order, or component structure.
- Avoid styling with IDs.

There are exceptions to these guidelines and cases where using BEM and avoid `!important` only adds complexity--for example sometimes when we're adding rules to override existing third-party styles from a plugin or embed.
