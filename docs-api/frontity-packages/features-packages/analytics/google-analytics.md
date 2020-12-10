# `@frontity/google-analytics`

Analytics package to use [Google Analytics](https://analytics.google.com/) with Frontity

## Table of Contents

<!-- toc -->

- [Install](#install)
- [Settings](#settings)
- [Usage](#usage)
    + [`actions.analytics.pageview`](#actions-analytics-pageview)
    + [`actions.analytics.event`](#actions-analytics-event)

<!-- tocstop -->

## Install

```sh
npm i @frontity/google-analytics
```

## Settings

The [namespace](https://docs.frontity.org/learning-frontity/namespaces) for this package is **`googleAnalytics`** 

Every Google Analytics account has a [Tracking ID](https://support.google.com/analytics/answer/7372977?hl=en).   
To connect the package with a specific account (or accounts) we can set the following properties in the `frontity.settings.js`:
- `state.googleAnalytics.trackingId`: to specify just one _tracking ID_
- `state.googleAnalytics.trackingIds`: to specify a list of tracking ID's


```js
export default {
  packages: [
    {
      name: "@frontity/google-analytics",
      state: {
        googleAnalytics: {
          trackingId: "UA-12345678-9",
        },
      },
    },
  ],
};
```

```js

export default {
  packages: [
    {
      name: "@frontity/google-analytics",
      state: {
        googleAnalytics: {
          trackingIds: ["UA-34567890-12", "UA-34567890-13"]
        },
      },
    },
  ],
};
```

## Usage

This `@frontity/google-analytics` package can co-exist with some other `analytics` packages. Once we have properly installed and configured these `analytics` packages, their actions will be centralized by the `analytics` namespace 

- `actions.analytics.pageview()` will take into account settings in `state.analytics.pageviews`
- `actions.analytics.event()` will take into account settings in `state.analytics.events`

> Read more [here](README.md#how-to-use) about how to use Analytic packages 

#### `actions.analytics.pageview`

If `@frontity/google-analytics` is configured and enabled for _pageviews_ in `state.analytics.pageviews`, every time a link changes (or every time `action.router.set(link)` is launched) a tracking for that page will be sent to Google Analytics by using internally `actions.analytics.pageview()`

#### `actions.analytics.event`

If `@frontity/google-analytics` is configured and enabled for _events_ in `state.analytics.events`, every time you call the method `actions.analytics.event()` from any of your React components, the proper tracking info will be sent to Google Analytics.

The `actions.analytics.event()` must receive an event object with the following properties.


| Name          | Type   | Required | Description                                                                                                                                                                                       |
| :------------ | :----- | :------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **`name`**    | string | yes     | The value of this property is mapped to the [`eventAction`](https://developers.google.com/analytics/devguides/collection/analyticsjs/field-reference#eventAction) field of `analytics.js` events. |
| **`payload`** | object | yes     | Event payload.                                                                                                                                                                                    |

The `payload` object has to have the following format:

| Name           | Type   | Required | Description                                                                                                                                                                                           |
| :------------- | :----- |  :------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`category`** | string | yes     | The value of this property is mapped to the [`eventCategory`](https://developers.google.com/analytics/devguides/collection/analyticsjs/field-reference#eventCategory) field of `analytics.js` events. |
| `label`    | string | no    | The value of this property is mapped to the [`eventLabel`](https://developers.google.com/analytics/devguides/collection/analyticsjs/field-reference#eventLabel) field of `analytics.js` events.       |
| `value`    | number | no    | The value of this property is mapped to the [`eventValue`](https://developers.google.com/analytics/devguides/collection/analyticsjs/field-reference#eventValue) field of `analytics.js` events.       |
| `[key]`    | any    | no    | Any other property specified in [`analytics.js` field reference](https://developers.google.com/analytics/devguides/collection/analyticsjs/field-reference).

These values will be transfomed (by this package) into the proper format before sending the data to Google Analytics

