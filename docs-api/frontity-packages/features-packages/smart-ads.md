---
description: API reference of `@frontity/smart-adserver` package
---

# @frontity/smart-adserver

Smart Adserver is an ad serving network and they provide an API for displaying the ads which is described [here](https://support.smartadserver.com/s/article/Tagging-guide).

The `@frontity/smart-adserver` package will load a third-party Smart Adserver library which adds certain properties on the global `window` object. Then you as the developer can render the exposed `<SmartAd>` component, which will use those properties to make the "ad call" (which are basically API calls to the Smart Adserver).

In response, the Smart Adserver dynamically loads some code which will modify the DOM to insert the ad in the place that the `<SmartAd>` component was rendered.

The package has 3 main components:

- _The "Root" component_. It includes the `<Head>` that loads the Smart Adserver library. When the user adds the `@frontity/smart-adserver` to their `frontity.settings.js` file, this library will be loaded automatically.
- _The `SmartAd` component_. This component is exposed in `libraries.fills.SmartAdserver.SmartAd`. The users can just use this component directly to display ads by passing it relevant props. The component takes care of calling the Smart Adserver API and injecting the ad into the DOM in the relevant place
- _Ability to specify the ads in `fills` in the `frontity.settings.js` file._ Ads can be placed in specific slots in a theme by using that approach.

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

<!-- tocstop -->

## Installation

The package can be installed like:

```bash
npm i @frontity/smart-adserver
```

## Settings

This package can be included in your `frontity.settings.js` file as one of the packages that will be part of your Frontity project.

The [namespace](https://docs.frontity.org/learning-frontity/namespaces) for this package is **`smartAdserver`**. To use the [Slot and Fill](https://api.frontity.org/frontity-packages/core-package/frontity#slot) pattern with this package we should add a settings object to `state.fills` under the `smartAdserver` namespace.

Each [fill](https://api.frontity.org/frontity-packages/core-package/frontity#fills) in the **`smartAdserver`** namespace is an object which should be assigned to an arbitrarily named key. The structure should be as follows:

```javascript
export default {
  packages: [
    {
      name: "@frontity/smart-adserver",
      state: {
        fills: {
          smartAdserver: {
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

To define a SmartAd using the [Slot & Fills](https://docs.frontity.org/learning-frontity/roots#fills) pattern we use [the standard _FilL_ properties](https://api.frontity.org/frontity-packages/core-package/frontity#fills)

| Name          | Type   | Required | Description                                                                                                                                                      |
| :------------ | :----- | :------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`slot`**    | string | yes      | The name of the slot as defined in your theme where you want the ad to go.                                                                                       |
| **`library`** | string | yes      | The React component used to display the Ad. We can set this value to `"smartAdserver.SmartAd"` \(`SmartAd` component available in the `smartAdserver` package\). |
| `priority`    | int    | no       | Assigns a priority in case more than one fill is assigned to that slot.                                                                                          |
| **`props`**   | obj    | yes      | Props that will be passed to the `<Slot>` component _\(see table below\)_                                                                                        |

#### The `props` property

An object with props that will be passed to the `<Slot>` component.

| Name            | Type                                                                                 | Required | Description                                                                                                                                                                                                                                                       |
| :-------------- | :----------------------------------------------------------------------------------- | :------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`siteId`**    | number                                                                               | yes      | The `id` of the site as defined in [SmartAds](https://support.smartadserver.com/s/article/Ad-API-reference); represents a website or mobile application; [_more info_](https://support.smartadserver.com/s/article/Setting-up-inventory)                          |
| **`pageId`**    | number                                                                               | yes      | The `id` of the page as defined in [SmartAds](https://support.smartadserver.com/s/article/Ad-API-reference); represents a website or mobile application; [_more info_](https://support.smartadserver.com/s/article/Setting-up-inventory)                          |
| **`formatId`**  | number                                                                               | yes      | The `id` of the format as defined in [SmartAds](https://support.smartadserver.com/s/article/Ad-API-reference); represents an ad slot on a page (medium rectangle, skyscraper...); [_more info_](https://support.smartadserver.com/s/article/Setting-up-inventory) |
| **`callType`**  | string                                                                               | yes      | The type of the ad call. Possible values: `iframe` or `std`                                                                                                                                                                                                       |
| **`tagId`**     | string                                                                               | no       | The `id` of the container that will contain the ad. Default Value: `sas_${formatId}"`                                                                                                                                                                             |
| **`width`**     | number                                                                               | no       | The width of the ad. Used with callType `iframe`.                                                                                                                                                                                                                 |
| **`height`**    | number                                                                               | no       | The height of the ad. Used with callType `iframe`.                                                                                                                                                                                                                |
| **`minHeight`** | number                                                                               | no       | Minimum height of the container for the ad. Used with callType `std`.                                                                                                                                                                                             |
| **`css`**       | [style object](https://api.frontity.org/frontity-packages/core-package/frontity#css) | no       | The optional styles that can be passed to the SmartAd component via `css` prop. They will be merged with the default styles of the SmartAd component. [_more info_](https://api.frontity.org/frontity-packages/core-package/frontity#css)                         |
| **`target`**    | string                                                                               | no       | Keyword targeting allows you to display ads only when specific keywords or key/value pairs are passed in the ad request. [_more info_](https://support.smartadserver.com/s/article/Using-keyword-targeting)                                                       |

## Examples

```js
module.exports = {
  packages: [
    ...,
    {
      name: "@frontity/smart-adserver",
      state: {
        // Global settings.
        smartAdserver: {
          networkId: 620,
          subdomain: "www8",
        },
        fills: {
          smartAdserver: {
            // This ad is using the 'std' call: https://support.smartadserver.com/s/article/Tagging-guide
            stdAd: {
              slot: "header",
              library: "smartAdserver.SmartAd",
              props: {
                callType: "std",
                siteId: 103409,
                pageId: 659846,
                formatId: 14968,
                tagId: "below-header-14968", // The id of the container where we render the ad.
                minHeight: 100, // In px, optional.
              },
            },
            // This ad is using the 'iframe' call: https://support.smartadserver.com/s/article/Tagging-guide
            iframeAd: {
              slot: "content",
              library: "smartAdserver.SmartAd",
              props: {
                siteId: 103409,
                pageId: 659846,
                formatId: 14968,
                tagId: "below-content-14968",
                width: 300, // Should be specified if callType === 'iframe'.
                height: 600, // Should also be specified if callType === 'iframe'.
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

The recommended usage of this component is using the [Slot and Fill](https://docs.frontity.org/learning-frontity/roots#fills) pattern. The configuration of the fill\(s\) is done in the `state.fills.smartAdserver` namespace in `frontity.settings.js` as explained above.

With this configuration we can then insert the Slots representing the Ads in any React component.

```jsx
import { Slot, ... } from "frontity";

const MyComponent = () => {

  return (
    <>
      ...
      <Slot name="header" />
      ...
      <Slot name="content" />
      ...
    </>
  );
};

...

export default MyComponent;
```

### Using the Ad component directly

Alternatively, since the Ad component is exposed in `libraries`, you can get the `SmartAd` component from `libraries` and render it wherever you wish.

```jsx
const MyComponent = ({ libraries, ...props }) => {
  const MySmartAd = libraries.fills.smartAdserver.SmartAd;
  const {siteId, pageId, formatId} = props
    return (
      <MySmartAd
        callType="std"
        siteId={siteId}
        pageId={pageId}
        formatId={formatId}
        tagId="test-smartad"
        css={css`
          border: 5px solid dotted;
        `}
      />
    );
};

export connect(MyComponent);
```
