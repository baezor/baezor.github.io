---
layout: post
title: How to Set Up Dynamic Imports in Laravel Mix and TypeScript
date: 2024-07-03 00:00:00 -0600
description: This tutorial guides you on how to set up dynamic imports in Laravel Mix and handle them with TypeScript
categories: webpack laravel-mix typescript
author: Angel Baez
---

## Introduction

This tutorial will guide you through setting up dynamic imports in Laravel Mix. It assumes you are already familiar with dynamic imports in webpack and need a how-to guide to implement them in your project.

## Why Use Webpack's Dynamic Imports?

As a web developer, creating stunning yet performant websites is one of my key responsibilities. Dynamic imports have been a great tool to help me deliver excellent results to my clients.

## Steps

1. Install the `@babel/plugin-syntax-dynamic-import` package.
2. Create a `.babelrc` config file.
3. Update your `webpack.mix.js` file.

### 1. Install the `@babel/plugin-syntax-dynamic-import` package

```bash
npm add @babel/plugin-syntax-dynamic-import --D
```

### 2. Create a `.babelrc` config file in the root of your project

```json
{
  "plugins": ["@babel/plugin-syntax-dynamic-import"]
}
```

### 3. Update your `webpack.mix.js` file

You need to add two elements:

1. Set the public path.
2. Add a webpack custom configuration.

```js
// 1. Set the path to where all public assets should be compiled.
mix.setPublicPath(path.normalize("./wp-content/themes/my-theme/assets/build"));
// 2. Add webpack config:
mix
  .webpackConfig({
    devtool: "inline-source-map",
    output: {
      chunkFilename: "chunks/[name].js",
      publicPath: "/public/build/",
    },
  })
  // The rest of your webpack.mix.js configuration...
  .js(`assets/js/app.js`, `app.js`);
```

## Managing Laravel Mix Dynamic Imports in Projects

For most projects at [HUSL Digital](https://husldigital.com/), we follow a module-based architecture. This means every page is split into modules, and each module has its own JavaScript loaded only when the module is used on the page. This approach significantly reduces the load and execution time of JavaScript.

First, our main TypeScript file invokes the `Blocks` class.

```ts
import { DOMReady } from "./utils/domReady";
import { Blocks } from "./blocks";

DOMReady(() => {
  new Blocks(".husl-block");
});
```

The `Blocks` class handles the dynamic imports:

```ts
import camelCase from "../utils/camelCase";
/**
 * Blocks class
 * This class is responsible for importing the block files dynamically
 */
export class Blocks {
  private selector: string = ".husl-block";
  private nodeList: NodeListOf<HTMLElement>;
  private listOfBlockNames: Map<string, HTMLElement[]> = new Map();

  constructor(selector: string) {
    this.selector = selector;
    this.nodeList = document.querySelectorAll(selector);
    this.init();
  }

  /**
   * Get the block names and the HTML elements
   *
   * @returns {Map<string, HTMLElement[]>}
   */
  private getNamesAndHTMLElements(): Map<string, HTMLElement[]> {
    const elements = new Map();
    Array.from(this.nodeList).forEach((node) => {
      if (!node.getAttribute("data-block")) {
        return;
      }
      const blockName = camelCase(node.getAttribute("data-block") as string);
      const jsFileEnabled = node.getAttribute("data-has-js-file") as
        | true
        | null;
      if (!blockName) {
        console.info(
          "data-primitive attribute is missing or empty for this primitive: ",
          node
        );
        return;
      }
      if (jsFileEnabled === null) {
        return;
      }
      if (!elements.has(blockName)) {
        elements.set(blockName, []);
      }
      elements.get(blockName)?.push(node);
    });
    return elements;
  }

  /**
   * Import the block files dynamically using webpack
   *
   * @returns {void}
   */
  private async importFiles(): Promise<void> {
    for (const [name, nodeListOfHTMLElements] of this.listOfBlockNames) {
      try {
        const Block = await import(
          /* webpackChunkName: "[request]" */
          `./${name}`
        );
        /*
        Block is another class that I use to handle the logic of the blocks, but it is not necessary. You can reduce the abstraction by directly loading the file.
        */
        new Block.default(name, nodeListOfHTMLElements);
      } catch (error) {
        console.error(error);
        /*
        Showing helpful error always helps your teammates and your future self.
        */
        console.info(
          `Add the JS/TS file for the ${name} block. \nCreate the missing file inside the blocks folder: /assets/js/blocks/${name}.js`
        );
      }
    }
  }

  /**
   * The Block class initializer
   *
   * @returns {void}
   */
  init() {
    console.group("🚀 Loading Nylon Blocks");

    if (this.nodeList && this.nodeList.length === 0) {
      console.info(
        "No blocks were found with the CSS class name: ",
        this.selector
      );
      return;
    }
    const len = this.nodeList.length;
    console.info(`${len} ${len > 1 ? "Blocks" : "Block"} found.`);

    this.listOfBlockNames = this.getNamesAndHTMLElements();
    console.info("Blocks list with JS file: ", this.listOfBlockNames);

    this.importFiles();
    console.groupEnd();
  }
}
```

Then, I created a `Block` class to manage individual modules:

```ts
export class Block {
  [x: string]: any;
  name: string;
  nodeList: NodeListOf<HTMLElement>;
  items: Array<any> = [];
  constructor(name: string, nodeList: NodeListOf<HTMLElement>) {
    this.name = name;
    this.nodeList = nodeList;
    if (this.nodeList.length > 0) {
      this.nodeList.forEach((node: HTMLElement) => {
        this.items.push(node);
        this.init(node);
      });
    }
  }
}
```

As I mentioned earlier, the `Block` class might not be necessary, and you can reduce complexity by only loading the file of your JS block. However, I wanted to show you my initial approach.

Finally, an individual JS for a module inherits from the `Block` class. Here’s what it looks like:

```ts
import { Block } from "./block";

/**
 * ExampleBlock
 *
 * Use this block as a template to create your blocks.
 *
 * @param {string} name - The name of the block.
 * @param {NodeList} nodeListOfHTMLElements - The NodeList of HTML elements that match the block's selector.
 */
class ExampleBlock extends Block {
  constructor(name, nodeListOfHTMLElements) {
    super(name, nodeListOfHTMLElements);
  }
  init(htmlElement) {
    console.log("ExampleBlock block: ", htmlElement);
    // your code per module
  }
}
export default ExampleBlock;
```

The `init` function runs for each DOM instance of the module.

## That's All, Folks!

I hope you find this quick tutorial useful. Happy coding! 🎉💻
