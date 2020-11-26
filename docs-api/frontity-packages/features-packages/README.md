# Features packages

Features packages are those packages that add a specific feature to a Frontity project. They need to be defined and configured in the `frontity.settings.js` so Frontity can properly use them in the project

Most of the logic needed for specific features in a Frontity project are delegated in packages. Some of the packages contains essential features like [_routing_](#router-package) and [_source_](#source-package), so they will be used by most of the projects. Other optional features (like [_analytics_](#analytics-packages)) can be easily implemented through other packages

#### Source package 

This package is in charge of getting data from Wordpress and make it accesible from React components

- [`@frontity/wp-source`](wp-source.md)

#### Router package 

This package is in charge of managing (React) routes in a Frontity project.

- [`@frontity/tiny-router`](tiny-router.md)

#### Render package 

This package is in charge of converting HTML to React

- [`@frontity/html2react`](html2react.md)

#### SEO package

This package is designed to get automatically all the data that the REST API Head Tags plugin exposes in the REST API

- [`@frontity/head-tags`](head-tags.md)

#### Analytics packages

A set of official Analytics Frontity packages that you can use to easily add analytics tracking in your project

- [`@frontity/google-analytics`](analytics/google-analytics.md)
- [`@frontity/google-tag-manager-analytics`](analytics/google-tag-manager-analytics.md) 
- [`@frontity/comscore-analytics`](analytics/comscore-analytics.md)