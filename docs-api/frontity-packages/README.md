---
description: API reference of Frontity and its packages.
---

# 🍱 Packages

Frontity projects are built around the idea of packages that encapsulate logic that can be reused across projects. Frontity packages may be considered as the equivalent of WordPress plugins. They're the ingredients of the final Frontity project.

## How to use Frontity packages

Frontity packages are available via [npm](https://www.npmjs.com/search?q=keywords:frontity) and they can be installed as dependencies of your Frontity project \(as with any other Node project\).

Frontity needs to know the packages we're going to use in the project and how we want to use them. We can set this in the `frontity.settings.js` file, where we can define and configure the packages \(by using Namespaces\) for the Frontity project.

{% hint style="info" %}
We're going to focus on the use of Frontity packages at a Frontity project level, but the same applies if you're creating a custom Frontity package. You can see more info about how to create your custom package [here](../frontity-cli/create-commands/create-package.md).
{% endhint %}

## Official Frontity Packages

The official Frontity packages are those created and maintained by the [Frontity Team](https://frontity.org/about-us/).

These packages encapsulate the logic to apply the main features needed in a WordPress + React stack project managed by Frontity.

A Frontity project is basically the sum of the **Core package** plus a selection of **Features packages**. You can also use **Collections packages** to help speed up the development of your Frontity project.

{% hint style="info" %}
The [full list of available packages](https://www.npmjs.com/search?q=keywords:frontity) includes others created by the community. However, in this site we're going to document only the official `frontity` packages that have a public API.
{% endhint %}

### 💙 [Core package](core-package/)

This package is the core of the Frontity framework and provides the main utilities of the framework.

- [`frontity`](core-package/frontity.md)

### 🎨 [Themes packages](themes-packages/)

These packages contain the React project used to render the final output (both in client and server side).

- [`@frontity/mars-theme`](themes-packages/frontity-mars-theme.md)
- [`@frontity/twentytwenty-theme`](themes-packages/frontity-twentytwenty-theme.md)

### ⚡️ [Features packages](features-packages/)

#### Source package

This package is in charge of getting data from WordPress and make it accesible from React components.

- [`@frontity/wp-source`](features-packages/wp-source.md)

#### Router package

This package is in charge of managing \(React\) routes in a Frontity project.

- [`@frontity/tiny-router`](features-packages/tiny-router.md)

#### Render package

This package is in charge of converting HTML to React.

- [`@frontity/html2react`](features-packages/html2react.md)

#### SEO packages

These packages are designed to get automatically all the data from WordPress SEO plugins and render it \(along with the content\) in the final HTML.

- [`@frontity/head-tags`](features-packages/head-tags.md)
- [`@frontity/yoast`](features-packages/yoast.md)

#### Ads packages

These packages allow you to insert ads in your Frontity projects from services such as Google Ad Manager.

- [`@frontity/google-ad-manager`](features-packages/google-ad-manager.md)
- [`@frontity/smart-adserver`](features-packages/smart-ads.md)

#### Analytics packages

A set of official Analytics Frontity packages that you can use to easily add analytics services to your project.

- [`@frontity/google-analytics`](features-packages/analytics/google-analytics.md)
- [`@frontity/google-tag-manager-analytics`](features-packages/analytics/google-tag-manager-analytics.md)
- [`@frontity/comscore-analytics`](features-packages/analytics/comscore-analytics.md)

#### Comments packages

This package adds support for WordPress' native comments.

- [`@frontity/wp-comments`](features-packages/wp-comments.md)

### 📚 Collections packages

These packages are collections of useful components or hooks that you can use in your Frontity project.

- [`@frontity/components`](collections-packages/components.md)
- [`@frontity/hooks`](collections-packages/hooks/)
