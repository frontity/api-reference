---
description: API reference of `@frontity/yoast` package
---

# @frontity/yoast

This package is designed to automatically get and render all the tags that the [Yoast SEO](https://wordpress.org/plugins/wordpress-seo/) plugin for WordPress exposes in the REST API. It works with Yoast SEO version 14.0 or greater.

For earlier versions of the Yoast SEO plugin (i.e. < 14.0) use the [`@frontity/head-tags`](https://www.npmjs.com/package/@frontity/head-tags) package instead.

## Table of Contents

<!-- toc -->

- [Installation](#installation)
- [Settings](#settings)
- [Usage](#usage)
- [Video](#video)

<!-- tocstop -->

## Installation

Add the `@frontity/yoast` package to your project:

```bash
npm i @frontity/yoast
```

This package requires the [Yoast SEO](https://wordpress.org/plugins/wordpress-seo/) plugin (v14.0 or above) to be installed on the WordPress site in order to function.

## Settings

## Usage

Once installed it should be included in your `frontity.settings.js`

```js
export default {
  packages: ["@frontity/yoast"],
};
```

## Video

This short video demonstrates the usage of the `@frontity/yoast` package.

{% embed url="https://www.youtube.com/watch?v=nqXl-2zC9Eo" caption="" %}
