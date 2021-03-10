# @frontity/twentytwenty-theme Reference

<!-- toc -->

- [Settings](#settings)
- [API Reference](#api-reference)
  - [Actions](#actions)
    - [actions.theme.openMobileMenu](#actions-theme-openmobilemenu)
    - [actions.theme.closeMobileMenu](#actions-theme-closemobilemenu)
    - [actions.theme.openSearchModal](#actions-theme-opensearchmodal)
    - [actions.theme.closeSearchModal](#actions-theme-closesearchmodal)
  - [Libraries](#libraries)

<!-- tocstop -->

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
