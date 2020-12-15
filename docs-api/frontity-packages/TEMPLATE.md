---
description: API reference of `@frontity/wp-source` package
---

<!-- this is a template with the structure to document a Frontity package -->

# `package-name`

This package is in charge of ...

<!-- optional "Table of contents" if the page is too long -->

## Table of contents

- [Installation](#)
- [Settings](#)
  - [`state.packageNamespace.setting1`](#)
  - [`state.packageNamespace.setting2`](#)
- [How to use](#)
- [API Reference](#)
  - [Actions](#)
    - [`actions.packageNamespace.action1()`](#)
  - [State](#)
    - [`state.packageNamespace.method1()`](#)
    - [`state.packageNamespace[propA][propB]`](#)
  - [Libraries](#)
    - [`libraries.packageNamespace.method1()`](#)

## Installation

Add the `package-name` package to your project:

```text
npm i @frontity/package-name
```

And include it in your `frontity.settings.js` file:

```javascript
module.exports = {
  packages: [
    "@frontity/mars-theme",
    "@frontity/tiny-router",
    {
      name: "@frontity/package-name",
      state: {
        packageNamespace: {
          setting1: "XXXXX",
        },
      },
    },
  ],
};
```

## Settings

These are the settings you can change in your `frontity.settings.js` file:

#### [`state.packageNamespace.setting1`](#)

Description of the setting with examples of possible values and images if needed

It has three arguments:

| Name           | Type   | Default | Required | Description                                                                    | Example           |
| -------------- | ------ | ------- | -------- | ------------------------------------------------------------------------------ | ----------------- |
| **`type`**     | string | -       | true     | The slug you configured for your Custom Post Type                              | `movies`          |
| **`endpoint`** | string | -       | true     | REST API endpoint from where this post type can be fetched.                    | `movies`          |
| **`archive`**  | string | -       | false    | the URL of the archive of this Custom Post Type, where all of them are listed. | `/movies_archive` |

## How to use

Let’s start by explaining...

{% hint style="info" %}
If you want to know more about how to use the `wp-source` package, here you have some videos where Frontity DevRel team talks about it:

- 📺 [Frontity Talks 2020-01 - wp-source & CSS In JS \[1:36\]](https://www.youtube.com/watch?v=e-_66W8pfdY&t=96s)
- 📺 [Frontity Talks 2020-02 - Pagination example & wp-source \(state & fetch\) \[17:53\]](https://www.youtube.com/watch?v=eW5xZlpcqQk&t=1073s)
  {% endhint %}

## API Reference

The [`package-name` package](#) implements the [interface defined in the `source` package](https://github.com/frontity/frontity/blob/dev/packages/source/types.ts) and [adds some extra API](https://github.com/frontity/frontity/blob/dev/packages/wp-source/types.ts)

### Actions

Actions don't return data. Data is always accessed via the state. That's because Frontity is following the [Flux pattern](https://facebook.github.io/flux/) \(like Redux\).

{% hint style="info" %}
Read more about actions [here](../learning-frontity/actions.md)
{% endhint %}

#### `actions.packageNamespace.action1()`

This action ...

...

{% hint style="info" %}
Still have questions? Ask [the community](https://community.frontity.org/)! We are here to help 😊
{% endhint %}
