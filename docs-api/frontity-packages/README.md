---
description: API reference of Frontity and its packages.
---

# 🍱 Frontity Packages

Frontity projects are built around the idea of packages that encapsulates logic that can be reused across projects. Frontity packages may be considered as the equivalent of WordPress plugins.  They're the ingredients of the final Frontity project.

## How to use Frontity packages

Frontity packages are available available via [npm](https://www.npmjs.com/search?q=keywords:frontity) and they can be installed as dependencies of your Frontity project (as with any other Node project). 

But installing these packages (and adding them as dependencies) is not enough, they also need to be properly defined and configured (under their proper Namespace) and as part of your Frontity project in the file `frontity.settings.js`


{% hint style="info" %}
We're going to focus on the use of Frontity packages at a Frontity project level, but the same applies if you're creating a custom Frontity package. 
You can see more info about how to create your custom package [here](#)
{% endhint %}

## Official Frontity Packages

Official Frontity packages are those created and maintained by the [Frontity Team](https://frontity.org/about-us/). 

These packages encapsulates the logic to apply the main features needed in a WordPress + React stack project (a Frontity project)

{% hint style="info" %}
The [full list of available packages](https://www.npmjs.com/search?q=keywords:frontity) include others created by the community. In this site we're going to document only the official `frontity` packages that has a public API
{% endhint %}

### Core package

Package that is the core of the Frontity framework and that provides main utilities of the framework

* [`frontity`](#)

### Features packages

#### Analytics packages

A set of official Analytics Frontity packages that you can use to easily add analytics tracking in your project

- [`@frontity/google-analytics`](#)
- [`@frontity/google-tag-manager-analytics`](#) 
- [`@frontity/comscore-analytics`](#)

#### SEO package

This package is designed to get automatically all the data that the REST API Head Tags plugin exposes in the REST API

- [`@frontity/head-tags`](#)

#### Render package 

This package is in charge of converting HTML to React

- [`@frontity/html2react`](#)

#### Router package 

This package is in charge of managing (React) routes in a Frontity project.

- [`@frontity/tiny-router`](#)

#### Source package 

This package is in charge of getting data from Wordpress and make it accesible from React components

- [`@frontity/wp-source`](#)

