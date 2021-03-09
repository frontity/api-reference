# @frontity/twentytwenty-theme

The **Twenty Twenty** default WordPress theme was included in WordPress 5.3 and we ported it over to Frontity so users can use it in a headless setup as well. You can check out its **code** and structure on our [GitHub repository](https://github.com/frontity/frontity/tree/dev/packages/twentytwenty-theme) and find it on [npm](https://www.npmjs.com/package/@frontity/twentytwenty-theme).

<!-- toc -->

- [Features](#features)
- [Theme Anatomy](#theme-anatomy)
- [Demo](#demo)
- [Settings](#settings)
- [API Reference](#api-reference)
  - [Actions](#actions)
    - [actions.theme.openMobileMenu](#actions-theme-openmobilemenu)
    - [actions.theme.closeMobileMenu](#actions-theme-closemobilemenu)
    - [actions.theme.openSearchModal](#actions-theme-opensearchmodal)
    - [actions.theme.closeSearchModal](#actions-theme-closesearchmodal)
  - [Libraries](#libraries)

<!-- tocstop -->

## Features

These are some of the key features included in this theme:

**Accessibility Ready**

The theme is accessible and screen-reader friendly. We added the proper landmarks, roles and labels. We also paid attention to trap focus within modals, ensure focus indicator is visible for all interactive elements.

**Custom Colors**

You can give your site or blog a personal touch by changing the background colors, text colors and primary/accent color in the theme settings. You change the color in one place, all visual elements get updated.

**Search**

The theme comes with a built-in search box to make it easy for your readers to look for specific content. Search box is powered by the robust and performant search engine built into WordPress.

**Featured Images**

Show beautiful featured images for your blog posts. Frontity uses the featured image uploaded to WordPress and renders it on every blog post. You can also opt out of this in the theme settings.

**Content Prefetch**

You can prefetch page for any link to provide an almost instant user experience. All you need do is to change your settings to prefetch pages when the user "hovers" on a link, when the link is visible on screen, or prefetch all links on the current page.

**Pagination**

Frontity's theme has the same pagination as the original WordPress theme. This way you can have access to different pages in the footer, and navigate easily between pages.

## Theme Anatomy

A standard installation of `@frontity/twentytwenty-theme` has the following file structure:

```text
.
├── CHANGELOG.md
├── LICENSE
├── package.json
├── README.md
└── src
   ├── components
   │  ├── archive
   │  │  ├── archive-header.js
   │  │  ├── archive-pagination.js
   │  │  ├── archive.js
   │  │  └── index.js
   │  ├── footer.js
   │  ├── header.js
   │  ├── hooks
   │  │  ├── use-focus-effect.js
   │  │  └── use-trap-focus.js
   │  ├── icons.js
   │  ├── index.js
   │  ├── link.js
   │  ├── loading.js
   │  ├── mobile
   │  │  ├── menu-button.js
   │  │  ├── menu-modal.js
   │  │  └── search-button.js
   │  ├── navigation
   │  │  ├── nav-toggle.js
   │  │  └── navigation.js
   │  ├── page-error.js
   │  ├── page-meta-title.js
   │  ├── post
   │  │  ├── featured-media.js
   │  │  ├── index.js
   │  │  ├── post-categories.js
   │  │  ├── post-item.js
   │  │  ├── post-meta-item.js
   │  │  ├── post-meta.js
   │  │  ├── post-separator.js
   │  │  ├── post-tags.js
   │  │  └── post.js
   │  ├── search
   │  │  ├── search-button.js
   │  │  ├── search-form.js
   │  │  ├── search-modal.js
   │  │  └── search-results.js
   │  └── styles
   │     ├── button.js
   │     ├── font-faces.js
   │     ├── global-styles.js
   │     ├── input.js
   │     ├── screen-reader.js
   │     ├── section-container.js
   │     └── skip-link.js
   ├── fonts
   │  └── inter
   │     ├── Inter-Bold-LATIN.woff2
   │     ├── Inter-Bold-US-ASCII.woff2
   │     ├── Inter-Bold.woff2
   │     ├── Inter-italic-var.woff2
   │     ├── Inter-Medium-LATIN.woff2
   │     ├── Inter-Medium-US-ASCII.woff2
   │     ├── Inter-Medium.woff2
   │     ├── Inter-SemiBold-LATIN.woff2
   │     ├── Inter-SemiBold-US-ASCII.woff2
   │     ├── Inter-SemiBold.woff2
   │     └── Inter-upright-var.woff2
   └── index.js
```

## Demo

![Homepage view in Twenty Twenty Frontity Theme.](../.gitbook/assets/screenshot-homepage-view-twentytwenty-frontity-theme.png)

You can check out all the features in this [**theme demo**](https://twentytwenty.frontity.org/), or even in our [Frontity blog](https://blog.frontity.org/).

## Settings

In this theme, apart from changing the colors, you can also select other options. You can configure it via the `frontity.settings.js` file. The theme options can be specified in the `state.theme` property.

Here you have an example of a possible configuration \(each setting is explained later in detail\):

```javascript
{
  name: "@frontity/twentytwenty-theme",
  state: {
    theme: {
      menu: [
        ["Home", "/"],
        ["Nature", "/category/nature/"],
        ["Travel", "/category/travel/"],
        ["Japan", "/tag/japan/"],
        ["About Us", "/about-us/"]
      ],
      colors: {
        primary: "#E6324B",
        headerBg: "#ffffff",
        footerBg: "#ffffff",
        bodyBg: "#f5efe0"
      },
      showSearchInHeader: true,
      showAllContentOnArchive: false,
      featuredMedia: {
        showOnArchive: true,
        showOnPost: true
      },
      autoPreFetch: "hover",
      fontSets: "us-ascii"
    }
  }
},
```

All the settings that can be set under `state.theme` and their description:

| Key                         | Description                                                                                                                                                                                                                                                         | Default value |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| menu                        | Menu links to display in the header                                                                                                                                                                                                                                 | \[\]          |
| colors.primary              | Used throughout most of the application to style links and other common elements.                                                                                                                                                                                   | `#cd2653`     |
| colors.headerBg             | Color of the header background.                                                                                                                                                                                                                                     | `#ffffff`     |
| colors.footerBg             | Color of the footer background.                                                                                                                                                                                                                                     | `#ffffff`     |
| colors.bodyBg               | Color of the body background.                                                                                                                                                                                                                                       | `#f5efe0`     |
| showSearchInHeader          | Whether to show the search button in page header                                                                                                                                                                                                                    | true          |
| showAllContentOnArchive     | Whether to show all post content or only excerpt in archive view                                                                                                                                                                                                    | false         |
| featuredMedia.showOnArchive | Whether to show featured image on archive view                                                                                                                                                                                                                      | true          |
| featuredMedia.showOnPost    | Whether to show featured media on post view                                                                                                                                                                                                                         | true          |
| autoPreFetch                | Whether to auto-fetch links on a page. <br/>Possible values: <br/>- `no` → don't auto-tech any links <br/>- `all` → auto-fetch all the links of the page <br/>- `in-view` → auto-fetch just the links that are in-view <br/>- `hover` → auto-fetch links when hover | no            |
| fontSets                    | Which font set to use: <br/>- `us-ascii` <br/>- `latin` <br/>- `all`                                                                                                                                                                                                | `all`         |

## API Reference

### Actions

#### actions.theme.openMobileMenu

It changes `state.theme.isMobileMenuOpen` to `true`, so it opens the mobile menu.

#### actions.theme.closeMobileMenu

It changes `state.theme.isMobileMenuOpen` to `false`, so it closes the mobile menu.

#### actions.theme.openSearchModal

It changes `state.theme.isSearchModalOpen` to `true`, so it opens the search bar.

#### actions.theme.closeSearchModal

It changes `state.theme.isSearchModalOpen` to `false`, so it closes the search bar.

### Libraries

This theme doesn't have its own libraries, but it includes the image processor of [@frontity/html2react](frontity-twentytwenty-theme.md), so all the `<img>` tags are converted into the [`<Image />` component](frontity-twentytwenty-theme.md).
