---
description: API reference of `@frontity/google-ad-manager` package
---

# @frontity/google-ad-manager

This package enables Frontity to integrate with Google Ad Manager. It allows you to add ads as **fills** in `frontity.settings.js` so that they will appear in a specific **slot** defined in your theme. (see [Slot and Fill](../core-package/frontity.md#slot))

## Table of Contents

<!-- toc -->

- [Installation](#installation)
- [Settings](#settings)
- [How to use](#how-to-use)
- [API Reference](#api-reference)

<!-- tocstop -->

## Installation

Add the `google-ad-manager` package to your project:

```bash
npm i @frontity/google-ad-manager
```

## Settings

The [namespace](https://docs.frontity.org/learning-frontity/namespaces) for this package is **`googleAdManager`**.

```js
export default {
  packages: [
    {
      name: "@frontity/google-ad-manager",
      state: {
        fills: {
          googleAdManager: {
            belowHeaderAd: {
              slot: "Below Header",
              library: "googleAdManager.GooglePublisherTag",
              priority: 5,
              props: {
                id: "div-gpt-below-header",
                unit: "/6499/example/banner",
                size: [320, 100],
              },
            },
            belowContentAd: {
              slot: "Below Content",
              library: "googleAdManager.GooglePublisherTag",
              priority: 5,
              props: {
                id: "Below Content",
                unit: "/6499/example/banner",
                size: [300, 600],
                targeting: {
                  interests: ["sports", "music", "movies"],
                },
              },
            },
          },
        },
      },
    },
  ],
};
```

## How to use

This package will...

## API Reference

...
