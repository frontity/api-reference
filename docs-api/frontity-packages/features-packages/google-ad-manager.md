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

This package does not have any general settings.

Settings for a specific ad are defined in each fill.

This package needs to be included in your `frontity.settings.js` file as one of the packages that will be part of the Frontity project:

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
          },
        },
      },
    },
  ],
};
```

If you need to add more than one ad you can give each object in the `state.fills.googleAdManager` object it's own key:

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

#### `slot`<img src="https://img.shields.io/badge/REQUIRED-red.svg" >

> Type: `str`

The name of the slot as defined in your theme where you want the ad to go, as a `string`.

#### `library`<img src="https://img.shields.io/badge/REQUIRED-red.svg" >

> Type: `str`

This will be `"googleAdManager.GooglePublisherTag"`.

#### `priority`

> Type: `int`

Assigns a priority in case more than one fill is assigned to that slot.

#### `props`

> Type: `obj`

Props that will be passed to the `<Slot>` component

| Name            | Type   | Required | Description                                                                    |
| --------------- | ------ | -------- | ------------------------------------------------------------------------------ |
| **`id`**        | string | yes      | An identifier that you define                                                  |
| **`unit`**      | string | yes      | The (Google supplied) code for the ad unit to be displayed.                    |
| **`size`**      | array  | yes      | The width and height to display the ad.                                        |
| **`targeting`** | array  | no       | The URL of the archive of this Custom Post Type, where all of them are listed. |
| **`data`**      | array  | no       | One or more keys, each with one or more associated values. \*                  |

_\* [see here](https://developers.google.com/publisher-tag/guides/key-value-targeting) for more info._

## How to use

Recommended usage of this component is as above using the Slot and Fill pattern.

However, the Ad component is exposed in libraries and so you can get the `<GooglePublisherTag />` component from libraries and render it in any place.

```js
const Component = ({ libraries }) => {
  const MyAd = libraries.fills.googleAdManager.GooglePublisherTag;

	Return (
	  <MyAd unit=”/unit/234”,
	        size="[300, 600]",
		/>
	)
}

Export connect(Component);
```

## API Reference

...
