# `@frontity/comscore-analytics`

[Comscore](https://www.comscore.com/) Analytics package for Frontity

## Install

```sh
npm i @frontity/comscore-analytics
```

## Settings

The [namespace](https://docs.frontity.org/learning-frontity/namespaces) for this package is **`comscoreAnalytics`** 

Every Comscore account has a [Tracking ID](#).   
To connect the package with a specific account (or accounts) we can set the following properties in the `frontity.settings.js`:
- `state.comscoreAnalytics.trackingId`: to specify just one _tracking ID_
- `state.comscoreAnalytics.trackingIds`: to specify a list of _tracking ID's_


```js
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

```js

export default {
  packages: [
    {
      name: "@frontity/google-tag-manager-analytics",
      state: {
        comscoreAnalytics: {
          trackingIds:  ["34567890", "56789012"]
        },
      },
    },
  ],
};
```


## Usage

This `@frontity/comscore-analytics` package can co-exist with some other `analytics` packages. Once we have properly installed and configured these `analytics` packages, their actions will be centralized by the `analytics` namespace 

- `actions.analytics.pageview` will take into account settings in `state.analytics.pageviews`
- `actions.analytics.event` will take into account settings in `state.analytics.events`

> Read More info about how to use Analytic packages in the [docs](https://docs.frontity.org/api-reference-1/frontity-analytics)

#### `actions.analytics.pageview`

If `@frontity/comscore-analytics` is configured [and enabled for _pageviews_](), every time a link changes (or every time `action.router.set(link)` is launched) a tracking for that page will be sent to Google Analytics

#### `actions.analytics.event`

This package doesn't actually track events for Comscore so any call of the method `actions.analytics.event()` will have no effect for this service  

