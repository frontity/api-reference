---
description: API reference of Frontity and its packages.
---

# 🍱 Frontity Packages

Frontity projects are built around the idea of packages that encapsulates logic that can be reused across projects. Frontity packages may be considered as the equivalent of WordPress plugins.  They're the ingredients of the final Frontity project.

### Packages & Namespaces

Each type of feature needed in a Frontity project is defined by a specific **Namespace** (`source`, `router`, `comments` ...). Most of Frontity packages are specific implementations of these features (these Namespaces)

Because of this, we may have several packages to choose from, to add a specific feature to our Frontity project. 

For example:

**the `source` namespace**

Represents the logic that requests data from WordPress API

There's an official package (`@frontity/wp-source`) that implements this logic focused on a WordPress REST API

_But there could be another package that requests data from WordPress GraphQL API (this package is still not avalable)_

**the `analytics` namespace**

Represents the logic to include analytics in a Frontity project 

There are actually several official Frontity packages that implement this feature using different services

- [`@frontity/google-analytics`](https://github.com/frontity/frontity/tree/dev/packages/google-analytics) for trackings using [Google Analytics](https://analytics.google.com/)
- [`@frontity/google-tag-manager-analytics`](https://github.com/frontity/frontity/tree/dev/packages/google-tag-manager-analytics) for trackings using [Google Tag Manager](https://tagmanager.google.com/)
- [`@frontity/comscore-analytics`](https://github.com/frontity/frontity/tree/dev/packages/comscore-analytics) for trackings using [Comscore](https://www.comscore.com/) 


## How to use Frontity packages

Frontity packages are available available via [npm](https://www.npmjs.com/search?q=keywords:frontity) and they can be installed as dependencies of your Frontity project (as with any other Node project). 

But installing these packages (and adding them as dependencies) is not enough, they also need to be properly defined and configured (under their proper Namespace) and as part of your Frontity project in the file `frontity.settings.js`


{% hint style="info" %}
We're going to focus on the use of Frontity packages at a Frontity project level, but the same applies if you're creating a custom Frontity package. 
You can see more info about how to create your custom package [here](#)
{% endhint %}

## Official Frontity Packages

Official `frontity` packages are those created and maintained by the ...

