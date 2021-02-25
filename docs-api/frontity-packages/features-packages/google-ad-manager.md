---
description: API reference of `@frontity/google-ad-manager` package
---

# @frontity/google-ad-manager

This package enables Frontity to integrate with Google Ad Manager. It allows you to add ads as **fills** in `frontity.settings.js` so that they will appear in a specific **slot** defined in your theme. (see [Slot and Fill](../core-package/frontity.md#slot))

## Table of Contents

<!-- toc -->

- [Installation](#installation)
- [Settings](#settings)
  - [Object properties](#object-properties)
    - [The `props` property](#the-props-property)
- [Examples](#examples)
- [Usage](#usage)
  - [Using the `Slot & Fill` pattern](#using-the-slot-fill-pattern)
  - [Using the Ad component directly](#using-the-ad-component-directly)
- [Video](#video)

<!-- tocstop -->

## Installation

Add the `google-ad-manager` package to your project:

```bash
npm i @frontity/google-ad-manager
```

## Settings

This package can be included in your `frontity.settings.js` file as one of the packages that will be part of your Frontity project.

The [namespace](https://docs.frontity.org/learning-frontity/namespaces) for this package is **`googleAdManager`**. The object should be added to `state.fills`.

Each fill in the **`googleAdManager`** namespace is an object which should be assigned to an arbitrarily named key. The structure should be as follows:

```js
export default {
  packages: [
    {
      name: "@frontity/google-ad-manager",
      state: {
        fills: {
          googleAdManager: {
            arbitrary_fill_name: {
              // Object properties
            },
          },
        },
      },
    },
  ],
};
```

### Object properties

| Name          | Type   | Required | Description                                                                                                                                                                              |
| ------------- | ------ | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`slot`**    | string | yes      | The name of the slot as defined in your theme where you want the ad to go.                                                                                                               |
| **`library`** | string | yes      | The React component used to display the Ad. We can set this value to `"googleAdManager.GooglePublisherTag"` (`GooglePublisherTag` component available in the `googleAdManager` package). |
| `priority`    | int    | no       | Assigns a priority in case more than one fill is assigned to that slot.                                                                                                                  |
| **`props`**   | obj    | yes      | Props that will be passed to the `<Slot>` component _(see table below)_                                                                                                                  |

#### The `props` property

An object with props that will be passed to the `<Slot>` component.

| Name        | Type   | Required | Description                                                                                                                                                                                              |
| ----------- | ------ | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`id`**    | string | yes      | An `id` for Ad block                                                                                                                                                                                     |
| **`unit`**  | string | yes      | The (Google supplied) adUnitPath code for the ad unit to be displayed. _[more info](https://developers.google.com/publisher-tag/reference#googletag.slot-googletag.defineslotadunitpath,-size,-opt_div)_ |
| **`size`**  | array  | yes      | The width and height to display the ad.                                                                                                                                                                  |
| `targeting` | array  | no       | One or more keys, each with one or more associated values. _[more info](https://developers.google.com/publisher-tag/guides/key-value-targeting)_.                                                        |
| `data`      | array  | no       | Other data that you want to pass to the Slot.                                                                                                                                                            |

## Examples

Example with one ad:

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
                id: "div-gpt-below-content",
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

## Usage

### Using the `Slot & Fill` pattern

The recommended usage of this component is using the Slot and Fill pattern. The configuration of the fill(s) is done in the `state.fills.googleAdManager` namespace in `frontity.settings.js` as explained above.

With this configuration we can then insert the Slots representing the Ads in any React component.

```jsx
import { Slot, ... } from "frontity";

const MyComponent = () => {

  return (
    <>
      ...
      <Slot name="Below Header" />
      ...
      <Slot name="Below Content" />
      ...
    </>
  );
};

...

export default MyComponent;

```

### Using the Ad component directly

Alternatively, since the Ad component is exposed in `libraries`, you can get the `GooglePublisherTag` component from `libraries` and render it wherever you wish.

```jsx
const MyComponent = ({ libraries }) => {
  const MyAd = libraries.fills.googleAdManager.GooglePublisherTag;

	Return (
	  <MyAd
        unit=”/unit/234”
	      size="[300, 600]"
		/>
	)
}

export connect(MyComponent);
```

## Video

This short video demonstrates the usage of the `@frontity/google-ad-manager` package.

{% embed url="https://www.youtube.com/watch?v=Esm8cs0jMoY" caption="" %}
