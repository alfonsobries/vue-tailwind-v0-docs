# Vue-Tailwind 

**VueTailwind** is a set of Vue components created to be customized to adapt to the unique design of your application.

It uses [TailwindCss](https://tailwindcss.com) classes by default, and all classes are configurable, that give you total control of how the components will look like.

No more Bootstrap like sites, you just need to configure your theme classes once and all set.

### Roadmap to version 1

Even though some components are missing, I decided that I want to create a new version of this package based on my experience in the projects I used it.

For this new version, I will not focus (yet) in more complex components, but in creating a more robust and flexible version of the components we have:

These will go to be the main features:

- Rebuilt in typescript
- Avoid (as possible) to use third party library and focus on optimizing the size in the bundle
- A better way to import just single components
- A better way to define the classes based on dynamic states and a single prop to define the "blueprint" thinking in something like this (WIP):

```
<t-input :classes-blueprint="{
  'success':  '...',
  'success-smal':  '... text-sm'
  'custom-state':  '... border-2'
}" state="custom-state" />
```

- Also considering create separate packages for the more complex components that for this version could be based in third party libraries (date picker, rich select, etc.)

## Help me to grow this library

Is this project helpful for you? Consider sponsoring me. I have the goal of me able to spend full-time in this project; I'm sure I can make the best library of customizable components with a little more time:

[https://github.com/sponsors/alfonsobries)

Of course, any other kind help is welcome, even if you notice some grammar mistakes (English is not my primary language) [CONTRIBUTING](https://github.com/alfonsobries/vue-tailwind/blob/master/CONTRIBUTING.md) for details.

## Install and use
### 1. Install the dependencies 

```console
npm install vue-tailwind --save
``` 

Or: 
```console
yarn add vue-tailwind
``` 

::: tip 
If you using the default theme you need to [install TailwindCSS](https://tailwindcss.com/docs/installation) first
:::


### 2. Configure your project to use `vue-tailwind` 

#### 2.1 Do nothing if you want to use our default theme:

```js
import Vue from 'vue'
import VueTailwind from 'vue-tailwind'

Vue.use(VueTailwind)
```

### 2.2 Or better yet, create your own theme:

Let's say, for example, that for the specific needs of your project the text inputs should have a `blue two width border` instead of the default border, the button should have `more rounded borders`, and the primary button should be `purple`.

::: tip 

Notice that you don't need to set all the classes settings, just the ones you want to override from `src/themes/default.js`

:::

```js
// `./myOwnTheme.js`
const TInput = {
  // Notice that this will override the full `baseClass` setting so probably you want to keep some
  // of the clases and just replace the ones you want to override.
  // baseClass: 'border block w-full rounded',
  baseClass: 'border-2 border-blue-500 block w-full rounded',
}

const TButton = {
  // baseClass: 'border block rounded inline-flex items-center justify-center',
  baseClass: 'rounded-lg border block inline-flex items-center justify-center',
  // primaryClass: 'text-white bg-blue-500 border-blue-500 hover:bg-blue-600 hover:border-blue-600',
  primaryClass: 'text-white bg-purple-500 border-purple-500 hover:bg-purple-600 hover:border-purple-600',
}

const MyOwnTheme = {
  TInput,
  TButton,
}

export default MyOwnTheme
```

Finally, add your custom theme when you install VueTailwind:

```js {3,6}
import Vue from 'vue'
import VueTailwind from 'vue-tailwind'
import MyOwnTheme from './myOwnTheme.js'

Vue.use(VueTailwind, {
  theme: MyOwnTheme
})
```

Another option is to set the settings directly, check at this example:

```js {4,5,6,11}
import Vue from 'vue'
import VueTailwind from 'vue-tailwind'

const TInput = {
  baseClass: 'border-2 border-blue-500 block w-full rounded',
}
// Or create a separate file like `src/themes/default/TInput.js` and import it
// import TInput from './myOwnTInput'
Vue.use(VueTailwind, {
  theme: {
    TInput
  }
})
// Or why not define the settings inline:
Vue.use(VueTailwind, {
  theme: {
    TInput: {
      baseClass: 'border-2 border-blue-500 block w-full rounded',
    }
  }
})
```

### 3. (Optional) configure `purgecss`

Using `purgecss` postcss plugin? Add your theme file to the postcss config (or if you using the default theme add the theme path):

```js
// postcss.config.js (from https://tailwindcss.com/docs/controlling-file-size#setting-up-purgecss)
const purgecss = require('@fullhuman/postcss-purgecss')({
  content: [
    // ...Default config
    // Your custom theme file:
    './myOwnTheme.js',
    // Or the default 
    // './node_modules/vue-tailwind/node_modules/vue-tailwind/src/themes/default.js'
  ],

  // Include any special characters you're using in this regular expression
  defaultExtractor: content => content.match(/[A-Za-z0-9-_:/]+/g) || []
})

module.exports = {
  plugins: [
    require('tailwindcss'),
    require('autoprefixer'),
    ...process.env.NODE_ENV === 'production'
      ? [purgecss]
      : []
  ]
}
```

## Install only the components you need

If you want to reduce the bundle size by importing only the components you need you can do it by importing the component directly and registering it like this:

```
import TInput from 'vue-tailwind/src/elements/TInput.vue'
import TAlert from 'vue-tailwind/src/components/TAlert.vue'

  Vue.use(TInput, {
  successStatusClass: 'border-green-600 bg-green-300 text-white',
})

Vue.use(TAlert, {
  baseClass: 'border-2 p-3 relative flex',
})
```

_* Notice that you can pass the classes you want to override as you do when importing the full library._

_** Also notice that the form inputs are in the `src/elements/` path and the components in `src/components/` path._

You can also import the component from another custom component but in that case, you currently can't override the default theme. However, you can set the classes by using the props, look at this example:

```
<template>
<div>
  <t-input :success-status-class="border-green-600 bg-green-300 text-white" >
</div>
</template>

<script>
import TInput from 'vue-tailwind/src/elements/TInput.vue'
export default {
  components: {
    TInput
  }
}
</script>
```

## What's next?

The idea is to create a big set of common components using the same philosophy: Configurable elements that could be adapted to your project style:

For now, these are the priorities, of course are subject to change.

**Basic inputs**
- [x] [Text input](/elements/input.html)
- [x] [Textarea](/elements/textarea.html)
- [x] [Select](/elements/select.html)
- [x] [Radio](/elements/radio.html)
- [x] [Radio Group](/elements/radio-group.html)
- [x] [Button](/elements/button.html)
- [x] [Checkbox](/elements/checkbox.html)
- [x] [Checkbox Group](/elements/checkbox-group.html)
- [ ] File input

**Rich inputs**
- [ ] __IN PROGRESS__ Rich Select (tagging, autocomplete, remote data sets, etc.)  
- [ ] Date/Time Picker
- [ ] Rich file input (drop, multiupload, progress bar, etc)

**Components**
- [x] [Alert](/components/alert.html)
- [x] [Card](/components/card.html)
- [x] [Dropdown](/components/dropdown.html)
- [x] [InputGroup](/components/input-group.html)
- [x] __NEW__ [Table](/components/table.html)
- [x] __NEW__ [Pagination](/components/pagination.html)
- [x] __NEW__ [Modal](/components/modal.html)
- [ ] __IN PROGRESS__ Pagination Nav
- [ ] __IN PROGRESS__ Dialogs
- [ ] Tooltip
- [ ] Progress bar

**More features**
- [ ] Table filter, pagination & sorting
- *Send your feature requests*

**More**

- Once we have a reasonable amount of components I'm also planning:
  - Create some more default themes
  - Improve the documentation with better playgrounds
  - Create a theme editor. (And maybe a "submit your theme" page)
  - Add more features to the components

### Changelog

Please see [CHANGELOG](https://github.com/alfonsobries/vue-tailwind/blob/master/CHANGELOG.md) for more information what has changed recently.

### Security

If you discover any security related issues, please email alfonso@vexilo.com instead of using the issue tracker.

## Credits

- [Alfonso Bribiesca](https://github.com/alfonsobries)
- [All Contributors](https://github.com/alfonsobries/vue-tailwind/graphs/contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE) for more information.

_Made with love by [@alfonsobries](https://twitter.com/alfonsobries)_


