---
description: API reference of `@frontity/html2react` package
---

# @frontity/html2react

This package is in charge of converting HTML to React. It works with [_processors_](#processors) that match HTML portions and replaces them with React components.

## Table of Contents

<!-- toc -->

- [Installation](#installation)
- [Settings](#settings)
- [How to use](#how-to-use)
  - [Rendering the parsed content](#rendering-the-parsed-content)
- [Processors](#processors)
  - [Loading processors](#loading-processors)
  - [Creating your own processors](#creating-your-own-processors)
    - [Example](#example)
  - [Nodes](#nodes)
- [Default Processors](#default-processors)
  - [Script](#script)
    - [Usage](#usage)
  - [Iframe](#iframe)
    - [Usage](#usage)
- [API Reference](#api-reference)
  - [Libraries](#libraries)
    - [`libraries.html2react.processors`](#libraries-html-2-react-processors)
    - [`libraries.html2react.Component`](#libraries-html-2-react-component)

<!-- tocstop -->

## Installation

Add the `html2react` package to your project:

```text
npm i @frontity/html2react
```

## Settings

This package needs to be included in your `frontity.settings.js` file as one of the packages that will be part of the Frontity project:

{% code title="frontity.settings.js" %}

```javascript
module.exports = {
  packages: ["@frontity/html2react"],
};
```

{% endcode %}

If you use an already created theme this package will already be configured so you don't need to do anything else.

If you're creating a custom theme you'll have to [define the processors you want to use in the configuration of the package](#loading-processors)

## How to use

### Rendering the parsed content

This is how you need to include the Component that will render the parsed content. The only prop it takes is `html`, and you'll usually pass `post.content.rendered` to it:

```jsx
import React from "react";

const Post = ({ state, libraries }) => {
  const data = state.source.get(state.router.link);
  const post = state.source[data.type][data.id];

  // Component exposed by html2react.
  const Html2React = libraries.html2react.Component;

  return (
    <div>
      <Title />
      <AuthorAndDate />
      <FeaturedImage />
      {/* Use Html2React to render the post HTML content */}
      <Html2React html={post.content.rendered} />
    </div>
  );
};
```

## Processors

Processors are the blocks of logic used by `html2react` to detect specific portions of HTML and return custom HTML or React components

The `processors` field is an _array_ where you can push all the processors you want to use with `html2react`. You can check the default processors [here](#default-processors).

### Loading processors

You can add your processors directly in [`libraries.html2react.processors`](#libraries-html-2-react-processors). Here you can see as an example how this is done in `mars-theme`:

```jsx
import image from "@frontity/html2react/processors/image";
import customProcessor from "./processors/custom";

const myPackage = {
  roots: { ... },
  state: { ... },
  actions: { ... },
  libraries: {
    html2react: {
      processors: [image, customProcessor]
    }
  }
};

export default myPackage;
```

### Creating your own processors

A processor is an object with four properties: `name` , `priority` , `test`,and `processor`.

| Name            | Type     | Required | Description                                                                                                                                                                                                                                                                                   |
| --------------- | -------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`name`**      | string   | yes      | the name of your processor                                                                                                                                                                                                                                                                    |
| **`priority`**  | number   | yes      | A number that lets the package know in which order processors should be evaluated. The processors are evaluated in numeric order. For example, a processor with `priority` of `10` will be applied **before** a processor with a `priority` of `20`                                           |
| **`test`**      | function | yes      | A function that evaluate each [node](frontity-html2react.md#nodes), and if it returns `true`, this node will be passed down to the `processor` function                                                                                                                                       |
| **`processor`** | function | yes      | A function to apply some logic to the [node](frontity-html2react.md#nodes) that we want to modify. It could be substituting HTML tags for React component with some logic, as adding `lazy-loading` to images, or just modifying some attributes, like adding `target="_blank"` to the links. |

Both the `test` and the `processor` functions receive the same arguments `({ node, root, state, libraries })`

| Name        | Type   | Description                                                                                                                                              |
| ----------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `node`      | object | The HTML node tag the processor is evaluating                                                                                                            |
| `root`      | object | The top node of the node tree                                                                                                                            |
| `state`     | object | Access to Frontity's `state` . This could be useful to use some parts of the `state` inside your processor. For example, using your `state.theme.colors` |
| `libraries` | object | Access to Frontity's `libraries`. As it happens with the `state`, sometimes could be useful to access your `libraries` as well                           |

The **`test`** function _returns_ a boolean to indicate `processor` function should be executed (the node matches the pattern)

The **`processor`** function _returns_ a `node` object

#### Example

This is how the `image` processor is implemented in `html2react`:

```typescript
import Image from "@frontity/components/image";

const image = {
  // We can add a name to identify it later.
  name: "image",

  // We can add a priority so it executes before or after other processors.
  priority: 10,

  // Only process the node it if it's an image.
  test: ({ node }) => node.component === "img",

  processor: ({ node }) => {
    // If the image is inside a <noscript> tag, we don't want to process it.
    if (node.parent.component === "noscript") return null;

    // Many WP lazy load plugins move the real "src" to "data-src", so we move it back.
    if (node.props["data-src"]) node.props.src = node.props["data-src"];
    if (node.props["data-srcset"])
      node.props.srcSet = node.props["data-srcset"];

    // We tell Html2React that it should use the <Image /> component
    // from @frontity/components, which includes lazy loading support.
    node.component = Image;

    return node;
  },
};

export default image;
```

You don't need to return a React component, you can also modify the attributes \(props\) of the node. For example, this processor adds `target="_blank"` to the `<a>` tags with href starting with `http`:

```typescript
const extAnchors = {
  name: "external anchors",
  priority: 10,
  // Only process the node it if it's an anchor and href starts with http.
  test: ({ node }) =>
    node.component === "a" && node.props.href.startsWith("http"),
  // Add the target attribute.
  processor: ({ node }) => {
    node.props.target = "_blank";
    return node;
  },
};
```

### Nodes

The object `node` received by both `test` and `processor`can be an `Element`, a `Text` or a `Comment`. You can distinguish between them using `node.type`.

- An `Element` is an HTML tag or a React component.
- A `Text` is a text content. For example, the text inside a `<p>` tag.
- A `Comment` is just an HTML comment. Like this `<!-- comment -->`.

The common properties are:

| Name     | Type    | Description                                                                                                                                                           |
| -------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ---------- |
| `type`   | string  | The Node type. </br> Possible values: `"element"                                                                                                                      | "text" | "comment"` |
| `parent` | Element | The parent of this node, which is always an `element` \(`text` or `comment` can't have children\)                                                                     |
| `ignore` | boolean | If you set `ignore` to `true` for a node, it won't pass any `test`. This is useful in some situations when you don't want additional processors applied to this node. |

Besides common properties, `Element` nodes are also defined by the following properties:

| Name        | Type                                 | Description                                                                                                                                                                                                  |
| ----------- | ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `component` | string or function (React component) | If it's a string, it's an HTML tag and if it's a function is a React component. You can change it at will and it is what you would usually do when you want to convert HTML tags to React components         |
| `props`     | object                               | An object containing all the HTML attributes of that node or props of that React component. You can also change them at will. All the attributes are converted to the React equivalents, even for HTML tags. |
| `children`  | array (of nodes)                     | An array containing other nodes, children to this one. If you want to get rid of the children, just overwrite it with `null` or an empty array                                                               |

Examples of `props` values (and their equivalent React props):

- `class` -&gt; `className`
- `style` -&gt; `css`
- `srcset` -&gt; `srcSet`
- `onclick` -&gt; `onClick`
- ..

Besides common properties, `Text` and `Comment` nodes will also have the following property:

| Name      | Type   | Description         |
| --------- | ------ | ------------------- |
| `content` | string | Content of the Node |

## Default Processors

### Script

React doesn’t execute the code inside a `<script>` tags. For that reason, html2react doesn’t execute the script tags included in the contents.

The script processor, with a priority of `20`, processes `<script>` tags found in the HTML for execution. `<script>` type must either be `application/javascript`, `text/javascript` or `application/ecmascript` to pass the test of the processor.

#### Usage

The script processor is included by default in html2react. Therefore, no extra procedure is required to use the processor.

### Iframe

Iframes can impact the loading time and performance of a site. The iframe processor adds lazy-loading to the `<iframe>` tags found in the HTML.

#### Usage

Add `iframe` to the `processors` array in your package `index.js` file.

```javascript
import iframe from "@frontity/html2react/processors/iframe";

const themeName = {
    name: "theme-name",
    ...
    libraries: {
        html2react: {
            processors: [iframe]
        }
    }
}
```

## API Reference

### Libraries

#### `libraries.html2react.processors`

An array of the processors that will be used by `html2react`.

You can add, remove or mutate any processor from the array:

```jsx
// Add a processor.
libraries.html2react.processors.push(image);

// Remove a processor.
const index = libraries.html2react.processors.findIndex(
  (pr) => pr.name === "image"
);
libraries.html2react.processors.splice(index, 1);

// Change a processor priority.
const processor = libraries.html2react.processors.find(
  (pr) => pr.name === "image"
);
processor.priority = 20;
```

#### `libraries.html2react.Component`

The React component used to render the parsed HTML.

##### Props

| Name       | Type   | Required | Description                        |
| ---------- | ------ | -------- | ---------------------------------- |
| **`html`** | string | yes      | The HTML that needs to be rendered |

```jsx
import React from "react";

const Post = ({ libraries }) => {
  // Get the component exposed by html2react.
  const Html2React = libraries.html2react.Component;

  return (
    <>
      {/* Use it to render the HTML. */}
      <Html2React html={html} />
    </>
  );
};
```
