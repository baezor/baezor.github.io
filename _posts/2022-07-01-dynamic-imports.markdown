---
layout: post
title: "How to setup dynamic imports in Laravel Mix and WordPress"
date: 2022/7/1 00:00:00 -0500
description: How to setup dynamic imports in Laravel Mix for WordPress performance
categories: webpack
author: You
---

# How to setup dynamic imports in Laravel Mix and WordPress
Let me show you how to set up dynamic imports in Laravel Mix for WordPress performance. 

I'm assuming you already know what dynamic imports in webpack are, and you just need the how-to guide to implement them on your project. Let's dive into it:

## List of Steps

1. Install npm dependencies
2. Create a .babelrc config file
3. Update your webpack.mix.js
4. How to use dynamic imports

### 1. Install dependencies

Install the following dependencies

```bash
npm add @babel/plugin-syntax-dynamic-import --D
```

### 2. Create a .babelrc config file

```bash
{
  "plugins": ["@babel/plugin-syntax-dynamic-import"]
}
```

### 3. Update your webpack.mix.js

```js
// Set the path to where all public assets should be compiled to.
mix.setPublicPath(path.normalize('./wp-content/themes/my-theme/assets/build'))
// Add webpack config:
mix
  .webpackConfig({
    devtool: 'inline-source-map',
    output: {
      chunkFilename: 'chunks/[name].js',
      publicPath: '/wp-content/themes/my-theme/assets/build/'
    }
  })
  // The rest of your js configuration
  .js(`${ASSETS_DIR}/js/main.js`, `main.js`)
```

### 4. How to use dynamic imports

The **[webpack guide](https://webpack.js.org/guides/code-splitting/#dynamic-imports)** has a great documentation to understand how dynamic import works.

I'm using dynamic imports to load only the JavaScript used by the modules on a page. This approach considerably reduces the time of compiling and execution, which means faster load times and user interaction.

Here's how I'm using dynamic imports:

```js
import camelCase from './utils/camelCase'

document.addEventListener('DOMContentLoaded', () => {
  const blocks = document.querySelectorAll('.block')
  blocks.forEach(async (module) => {
    const name = camelCase(module.dataset.block)
    try {
      const { Block } = await import(
        /* webpackChunkName: 'block' */ `./blocks/${name}`
      )
      const block = new Block(name, module)
      block.init()
    } catch (error) {
      console.log(error)
      console.warn(
        `You're missing the js file for the ${name} block. \n Create the missing file inside the blocks folder: my-theme/assets/js/blocks/${name}.js`
      )
    }
  })
})
```

Individual file looks like:

```js
export class Block {
  constructor(name, node) {
    ;(this.name = name), (this.node = node)
  }
  init() {
    console.log(this.name)
    // The code for your block goes here.
  }
}
```

And that's it! I hope you find this quick tutorial useful.

Happy Coding! 🎉💻
