---
description: API reference of Frontity and its packages.
---

# 🍱 Frontity Packages

Frontity projects are built around the idea of packages that encapsulates logic that can be reused across projects. Frontity packages may be considered as the equivalent of WordPress plugins. They're the ingredients of the final Frontity project.

## How to use Frontity packages

Frontity packages are available available via [npm](https://www.npmjs.com/search?q=keywords:frontity) and they can be installed as dependencies of your Frontity project (as with any other Node project).

Frontity needs to know the packages we're going to use in the project and how do we want to use them. We can set this in the file `frontity.settings.js`, where we can define and configure the packages (by using Namespaces) for the Frontity project

{% hint style="info" %}
We're going to focus on the use of Frontity packages at a Frontity project level, but the same applies if you're creating a custom Frontity package.
You can see more info about how to create your custom package [here](#)
{% endhint %}

## Official Frontity Packages

Official Frontity packages are those created and maintained by the [Frontity Team](https://frontity.org/about-us/).

These packages encapsulates the logic to apply the main features needed in a WordPress + React stack project managed by Frontity

A Frontity project is basically the sum of the **Core package** plus a selection of **Features packages**. We can also use **Collections packages** to help us speed up the development of our Frontity project

{% hint style="info" %}
The [full list of available packages](https://www.npmjs.com/search?q=keywords:frontity) include others created by the community. In this site we're going to document only the official `frontity` packages that has a public API
{% endhint %}

### [Core package](core-package/README.md)

Package that is the core of the Frontity framework and that provides main utilities of the framework

- [`frontity`](core-package/frontity.md)

### [Features packages](features-packages/README.md)

#### Source package

This package is in charge of getting data from Wordpress and make it accesible from React components

- [`@frontity/wp-source`](features-packages/wp-source.md)

#### Router package

This package is in charge of managing (React) routes in a Frontity project.

- [`@frontity/tiny-router`](features-packages/tiny-router.md)

#### Render package

This package is in charge of converting HTML to React

- [`@frontity/html2react`](features-packages/html2react.md)

#### SEO package

This package is designed to get automatically all the data that the [REST API Head Tags plugin](https://wordpress.org/plugins/rest-api-head-tags/)n exposes in the REST API

- [`@frontity/head-tags`](features-packages/head-tags.md)

#### Google Ad Manager package

This package enables Frontity to integrate with Google Ad Manager.

- [`@frontity/google-ad-manager`](features-packages/google-ad-manager.md)

#### Yoast package

This package is designed to automatically get and render all the tags that the [Yoast SEO](https://wordpress.org/plugins/wordpress-seo/) plugin for WordPress exposes in the REST API. It works with Yoast SEO version 14.0 or greater.

- [`@frontity/yoast`](features-packages/yoast.md)

#### Analytics packages

A set of official Analytics Frontity packages that you can use to easily add analytics tracking in your project

- [`@frontity/google-analytics`](features-packages/analytics/google-analytics.md)
- [`@frontity/google-tag-manager-analytics`](features-packages/analytics/google-tag-manager-analytics.md)
- [`@frontity/comscore-analytics`](features-packages/analytics/comscore-analytics.md)

#### Comments packages

Comments package that adds integration for WordPress native comments.

- [`@frontity/wp-comments`](features-packages/wp-comments.md)

### Collections packages

These packages are collections or useful components or hooks that you can use in your Frontity project

- [`@frontity/components`](collections-packages/components.md)
- [`@frontity/hooks`](collections-packages/hooks.md)
