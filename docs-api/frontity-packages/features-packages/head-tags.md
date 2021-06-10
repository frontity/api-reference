---
description: API reference of `@frontity/head-tags` package
---

# @frontity/head-tags

This package is designed to get automatically all the metadata that the [REST API Head Tags plugin](https://wordpress.org/plugins/rest-api-head-tags/) exposes in the REST API \(SEO metadata from plugins like Yoast SEO or All in One SEO\), and **add them as meta tags in the `<head>` section of the rendered page**.

{% hint style="warning" %}
This package won't work without [REST API Head Tags plugin](https://wordpress.org/plugins/rest-api-head-tags/) installed and activated in your WordPress backend, so make sure you have it before using this package.
{% endhint %}

{% hint style="info" %}
If you are using Yoast SEO &gt;v14.0 we recommend that you instead use the [@frontity/yoast](yoast.md) package which does not require an additional plugin to be installed in WordPress.
{% endhint %}

## Table of Contents

- [Installation](head-tags.md#installation)
- [Settings](head-tags.md#settings)
- [How to use](head-tags.md#how-to-use)
- [API Reference](head-tags.md#api-reference)
  - [State](head-tags.md#state)
    - [`headTags.get`](head-tags.md#headtags-get)

## Installation

Add the `head-tags` package to your project:

```text
npm i @frontity/head-tags
```

Do this in your root and include it in your `frontity.settings.js` file:

```javascript
...
packages: [
    "@frontity/mars-theme",
    "@frontity/tiny-router",
    ...
    "@frontity/head-tags"
]
...
```

{% hint style="warning" %}
If you have an existing project make sure your [@frontity/wp-source](wp-source.md) package is at least on the 1.5.0 version. If not, update it using this command: `> npm install @frontity/wp-source@latest`.
{% endhint %}

## Settings

As it works automatically, It doesn't have settings itself, but it requires two Frontity parameters to work:

- `state.frontity.url` : The URL of your site. Usually defined in the `frontity.settings.js` file.
- `state.source.url` or `state.source.api`: The API where your project is pointing. Defined at [@frontity/wp-source](wp-source.md#settings) if you haven't changed your Source.

It needs `@frontity/wp-source` installed and updated to at least the `1.5.0` version.

## How to use

This package will automatically add all the meta tags defined in WordPress for the page \(through plugins like Yoast SEO or All in One SEO\) in the `<head>` section of the rendered page. So there are no additional steps to do. Just install the package and everything will work out of the box.

Remember that you'll need the [REST API Head Tags plugin](https://wordpress.org/plugins/rest-api-head-tags/) installed in your WordPress. With that, this package will take care of the rest.

If you want to access the metadata available for a specific link you can use the [`headTags.get`](head-tags.md#headtags-get) method.

## API Reference

### State

#### `headTags.get`

It is a function that accepts a `link`as parameter and it returns an array with the `head_tags` field of that link.

```javascript
state.headTags.get("/blog/hello-world/");
```

will return something like

```javascript
[
  {
    tag: "title",
    content: "Hello world! - My Site",
  },
  {
    tag: "meta",
    attributes: {
      name: "robots",
      content: "max-snippet:-1, max-image-preview:large, max-video-preview:-1",
    },
  },
  {
    tag: "link",
    attributes: {
      rel: "canonical",
      href: "http://mysite.com/hello-world/",
    },
  },
];
```

