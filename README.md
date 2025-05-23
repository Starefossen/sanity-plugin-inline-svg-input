# Sanity Plugin Inline SVG Input [![npm version](https://badge.fury.io/js/@starefossen%2Fsanity-plugin-inline-svg-input.svg)](https://badge.fury.io/js/@starefossen%2Fsanity-plugin-inline-svg-input) [![Sanity Exchange](https://img.shields.io/badge/sanity-exchange-green)](https://www.sanity.io/plugins/sanity-plugin-inline-svg-input)

**Sanity Studio v3** plugin to upload and preview inline SVGs.

- [Simple and easy to use](#usage)
- Safe SVG sanitization with [dompurify](https://github.com/cure53/DOMPurify)
- [SVG preview in arrays](#within-arrays)
- [Customizable preview component](#custom-preview-component)

![preview](https://raw.githubusercontent.com/starefossen/sanity-plugin-inline-svg-input/main/docs/preview.gif)

## Installation

```sh
npm install @starefossen/sanity-plugin-inline-svg-input
```

```sh
yarn add @starefossen/sanity-plugin-inline-svg-input
```

```sh
pnpm add @starefossen/sanity-plugin-inline-svg-input
```

## Usage

1. Add it as a plugin in `sanity.config.ts` (or .js):

```ts
import { defineConfig } from 'sanity'
import { inlineSvgInput } from '@starefossen/sanity-plugin-inline-svg-input'

export default defineConfig({
  // ...
  plugins: [inlineSvgInput()],
})
```

2. Use the `inlineSvg` type in your schema types:

```ts
// ...
{
  fields: [
    // ...
    {
      name: 'svgIcon',
      title: 'SVG Icon',
      type: 'inlineSvg',
    },
  ]
}
```

That's it! Now you can use this field to upload SVG images and see the preview in your Sanity Studio.

## Preview

The sections below describe how to preview your SVG within arrays, objects or your own custom preview component.

### Within arrays

Sanity offers a convenient way to preview arrays out of the box, but it only allows the use of the `image` type in `media`.
To provide same functionality for SVGs, we provide a ready-to-use preview component that mimics the default array preview.

![preview](https://raw.githubusercontent.com/starefossen/sanity-plugin-inline-svg-input/main/docs/preview-list.jpg)

To preview your SVG in arrays, use the `InlineSvgPreviewItem` component.\
It accepts the following props:

- `icon` [string] - inline SVG
- `title` [string] - the title of the item
- `subtitle` [string] - the subtitle of the item

`iconsList` in the example below is an array of objects with `icon`, `title` and `subtitle` fields.
To use them in preview the same way as the default array preview,
only what you need is to replace default preview component with `InlineSvgPreviewItem`.

```tsx
import { defineType } from 'sanity'
import { InlineSvgPreviewItem } from '@starefossen/sanity-plugin-inline-svg-input'

const IconsListItem = defineType({
  type: 'object',
  name: 'iconsListItem',
  fields: [
    {
      name: 'icon',
      type: 'inlineSvg',
    },
    {
      name: 'title',
      type: 'string',
    },
    {
      name: 'subtitle',
      type: 'string',
    },
  ],
  preview: {
    select: {
      icon: 'icon',
      title: 'title',
      subtitle: 'subtitle',
    },
  },
  components: {
    preview: ({ icon, title, subtitle }) => {
      return <InlineSvgPreviewItem icon={icon} title={title} subtitle={subtitle} />
    },
  },
})

const IconsList = defineType({
  name: 'iconsList',
  type: 'array',
  of: [{ type: IconsListItem.name }],
})
```

### Custom preview component

If you need more customization or want to build your own preview component using SVG icons,
you can use the `InlineSvgPreviewComponent`.

`InlineSvgPreviewComponent` is the smallest building block of the plugin the only purpose of which is to sanitize and render SVG.\
It accepts the following props:

- `value` [string] - inline SVG

```tsx
import { InlineSvgPreviewComponent } from '@starefossen/sanity-plugin-inline-svg-input'

export const PreviewComponent = ({ value }) => {
  return (
    // ...
    <InlineSvgPreviewComponent value={value} />
    // ...
  )
}
```

To customize the preview component, you can either:

- Pass `className` or `style` props
- Extend default styles with `styled-components`

```tsx
import { InlineSvgPreviewComponent } from '@starefossen/sanity-plugin-inline-svg-input'
import styled from 'styled-components'

const StyledInlineSvg = styled(InlineSvgPreviewComponent)`
  // ...
`
```

## Development, testing and publishing

This plugin uses [@sanity/plugin-kit](https://github.com/sanity-io/plugin-kit)
with default configuration for build & watch scripts.

`npm run build` - creates a ready to publish plugin in `/dist` folder.

`npm run link-watch` - creates a symlink from the plugin repo to the local sanity repo and watches for changes.
After running this command, you will see the instructions on how to add this plugin to your Sanity Studio in the terminal.

`npm publish` - there is pre-publish script that prepares the plugin for publishing, you don't need to build it manually.
Please run `npm publish --dry-run` to make sure that everything is ok before publishing. When publishing, make sure to set `access=public`.

See [Testing a plugin in Sanity Studio](https://github.com/sanity-io/plugin-kit#testing-a-plugin-in-sanity-studio)
for additional information.

### Setting up NPM_TOKEN secret

To publish the package to the npm registry using GitHub Actions, you need to set up the `NPM_TOKEN` secret in your GitHub repository settings. Follow these steps:

1. Go to your repository on GitHub.
2. Click on the `Settings` tab.
3. In the left sidebar, click on `Secrets` and then `Actions`.
4. Click the `New repository secret` button.
5. In the `Name` field, enter `NPM_TOKEN`.
6. In the `Value` field, enter your npm token. You can generate a new token from your npm account settings.
7. Click the `Add secret` button.

Once the `NPM_TOKEN` secret is set up, the GitHub Actions workflow will be able to use it to authenticate and publish the package to the npm registry.

## Credits

This project was created at **FocusReactive** - the [Sanity Partner Agency](https://www.sanity.io/agency-partners/focusreactive). We specialize in helping clients beat the competition and accelerate business growth. With a deep expertise in headless CMS, NextJS, and eCommerce, we deliver cutting-edge solutions that prioritize your business goals.

### Our Expertise

- **Content-Centric Websites**: We have a deep experience building extendable, SEO optimized content and marketing websites with advanced CMS integrations and analytics.
- **Headless eCommerce**: Our next-generation, content-rich, and performant online eCommerce websites come with end-to-end integrations to power your digital business.
- **Headless CMS Consulting**: We offer multi-channel CMS development, modeling, customization, and support to help you manage your content seamlessly across various platforms.
- **Web Performance**: Our experts can audit, transform the architecture, and optimize your website to meet the 100 SCORE Core Web Vitals for exceptional web performance.

If you're looking for expertise in headless CMS, NextJS, or eCommerce, get in touch with **FocusReactive** today. Visit our website at [focusreactive.com](https://focusreactive.com/) to learn more about how we can help you accelerate your business growth.

[![FocusReactive](https://raw.githubusercontent.com/starefossen/sanity-plugin-inline-svg-input/main/docs/FR-logo-long.png)](https://focusreactive.com/sanity-expert-agency/)
---

_This project is licensed under the MIT License. © 2023 FocusReactive._
