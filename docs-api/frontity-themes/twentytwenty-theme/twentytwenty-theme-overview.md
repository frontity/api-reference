# @frontity/twentytwenty-theme Overview

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

To customise the theme you would edit the files within the `src` folder.

Inside the `src` folder are:

- the root level `index.js` file - this contains the settings for configuring the theme (see the Settings section below)
- the `fonts` folder - this contains the `.woff2` font files used in the theme
- the `components` folder - this contains the React components that make up the functionality of the theme
