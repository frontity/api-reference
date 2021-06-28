# @frontity/comscore-analytics

[Comscore](https://www.comscore.com/) Analytics package for Frontity.

## Table of Contents

- [Install](comscore-analytics.md#install)
- [Settings](comscore-analytics.md#settings)
- [Usage](comscore-analytics.md#usage)
  - [`actions.analytics.pageview`](comscore-analytics.md#actions-analytics-pageview)
  - [`actions.analytics.event`](comscore-analytics.md#actions-analytics-event)

## Install

```bash
npm i @frontity/comscore-analytics
```

## Settings

The [namespace](https://docs.frontity.org/learning-frontity/namespaces) for this package is **`comscoreAnalytics`**.

Every Comscore account has a Tracking ID. To connect the package with a specific account \(or accounts\) set the following properties in the `frontity.settings.js` file:

- `state.comscoreAnalytics.trackingId`: to specify just one _tracking ID_
- `state.comscoreAnalytics.trackingIds`: to specify a list of _tracking ID's_

```javascript
export default {
  packages: [
    {
      name: "@frontity/comscore-analytics",
      state: {
        comscoreAnalytics: {
          trackingId: "34567890",
        },
      },
    },
  ],
};
```

```javascript
export default {
  packages: [
    {
      name: "@frontity/google-tag-manager-analytics",
      state: {
        comscoreAnalytics: {
          trackingIds: ["34567890", "56789012"],
        },
      },
    },
  ],
};
```

## Usage

The `@frontity/comscore-analytics` package can co-exist with any of the other `analytics` packages such as [`@frontity/google-analytics`](./google-analytics.md) and [`@frontity/google-tag-manager-analytics`](./gootle-tag-manager-analytics.md). Once these `analytics` packages have been properly installed and configured their actions will be centralized by the `analytics` namespace.

- `actions.analytics.pageview` will take into account settings in `state.analytics.pageviews`
- `actions.analytics.event` will take into account settings in `state.analytics.events`

> Read more about how to use Analytic packages [here](./#how-to-use).

### `actions.analytics.pageview`

If `@frontity/comscore-analytics` is configured [and enabled for _pageviews_](https://api.frontity.org/frontity-packages/features-packages/analytics#actions-analytics-pageview), every time a link changes \(or every time `action.router.set(link)` is launched\) tracking information for that page will be sent to Google Analytics.

### `actions.analytics.event`

This package doesn't actually track events for Comscore so any call of the method `actions.analytics.event()` will have no effect for this service.
