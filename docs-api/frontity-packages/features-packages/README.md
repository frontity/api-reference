# Features packages

Features packages are those packages that add a specific feature to a Frontity project. They need to be defined and configured in the `frontity.settings.js` file so Frontity can properly use them in the project.

Most of the logic needed for specific features in a Frontity project are delegated in packages. Some of the packages contain essential features like [_routing_](./#router-package) and [_source_](./#source-package), so they will be used by most of the projects. Other optional features \(like [_analytics_](./#analytics-packages)\) can be easily implemented through other packages.

## Source package

This package is in charge of getting data from WordPress and make it accesible from React components.

- [`@frontity/wp-source`](wp-source.md)

## Router package

This package is in charge of managing \(React\) routes in a Frontity project.

- [`@frontity/tiny-router`](tiny-router.md)

## Render package

This package is in charge of converting HTML to React.

- [`@frontity/html2react`](html2react.md)

## SEO packages

These packages are designed to get automatically all the data from WordPress SEO plugins and render it \(along with the content\) in the final HTML.

- [`@frontity/head-tags`](head-tags.md)
- [`@frontity/yoast`](yoast.md)

## Ad packages

These packages allow you to insert ads in your Frontity projects from services such as Google Ad Manager.

- [`@frontity/google-ad-manager`](google-ad-manager.md)
- [`@frontity/smart-adserver`](smart-ads.md)

## Analytics packages

A set of official Analytics Frontity packages that you can use to easily add analytics services to your project.

- [`@frontity/google-analytics`](analytics/google-analytics.md)
- [`@frontity/google-tag-manager-analytics`](analytics/google-tag-manager-analytics.md)
- [`@frontity/comscore-analytics`](analytics/comscore-analytics.md)

## Comments packages

This package adds support for WordPress' native comments.

- [`@frontity/wp-comments`](wp-comments.md)
