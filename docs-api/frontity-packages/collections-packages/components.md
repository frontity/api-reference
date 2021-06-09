---
description: API reference of `@frontity/components` package
---

# @frontity/components

This package is a collection of React components that have proven to be pretty useful for a Frontity project.

## Table of Contents

- [How to use](components.md#how-to-use)
- [Components](components.md#components)
  - [Link](components.md#link)
    - [Props](components.md#props)
    - [Usage](components.md#usage)
    - [Auto Prefetch](components.md#auto-prefetch)
    - [Custom `Link` component](components.md#custom-link-component)
    - [The `link` processor](components.md#the-link-processor)
  - [Image](components.md#image)
  - [Script](components.md#script)
    - [Props](components.md#props)
    - [Usage](components.md#usage)
  - [Iframe](components.md#iframe)
    - [Props](components.md#props)
    - [Usage](components.md#usage)
  - [Switch](components.md#switch)

## How to use

In order to use it, you just have to import the component you want to use in your theme from `@frontity/components/` and place it wherever needed. For example, if we want to use the `<Image />`component:

```javascript
import Image from "@frontity/components/image";
```

## Components

### Link

`<Link />` is a React component that you can use in your Frontity project to define links that works with the internal routing system. Under the hood, this component uses the `actions.router.set(link)` method from `@frontity/tiny-router` and creates an `<a/>` tag.

{% hint style="info" %}
This component requires having `state.source.url` properly configured. Have a look at the guide [Setting the URL of the WordPress data source](https://docs.frontity.org/guides/setting-url-wordpress-source-data) to learn more about this
{% endhint %}

#### Props

| Name           | Type     | Required | Default     | Description                                                                                                                                                                                                                                                                                                            |
| :------------- | :------- | :------- | :---------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `link`         | string   | yes      | ---         | The URL to link to.                                                                                                                                                                                                                                                                                                    |
| `target`       | string   | no       | `_self`     | The [target](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a#target) of the anchor. Possible values: `_self` or `_blank`                                                                                                                                                                                   |
| `onClick`      | function | no       | `undefined` | The `onClick` handler. Can be used to pass an optional callback that will be invoked on click.                                                                                                                                                                                                                         |
| `scroll`       | boolean  | no       | `true`      | Whether the browser should scroll up to the top upon navigating to a new page.                                                                                                                                                                                                                                         |
| `prefetch`     | boolean  | no       | `true`      | Whether Frontity should automatically prefetch this link or not. The prefetching mode is controlled through [`state.theme.autoPrefetch`](https://github.com/frontity/api-reference/tree/5cb70b185de018562902d073a62bb934053a5445/docs-api/frontity-packages/collections-packages/frontity-components.md#auto-prefetch) |
| `aria-current` | string   | no       | `undefined` | [Indicates the element that represents the current item within a container or set of related elements](https://www.w3.org/TR/wai-aria-1.1/#aria-current)                                                                                                                                                               |

All _"unknown"_ props passed to the Link are passed down to an anchor `</a>` tag.

#### Usage

```jsx
import Link from "@frontity/components/link";

const MyComponent = () => (
  <Link link={linkUrl} onClick={(e) => console.log(e)}>
    This is a link
  </Link>
);
```

#### Auto Prefetch

This component can help implementing some auto prefetching strategies. The configuration for this is stored in the `state` so final users can modify it in their sites using their `frontity.settings.js` file.

Imagine that `my-awesome-theme` uses this component. Then, people can set the auto prefetch setting like this:

```javascript
const settings = {
  // Other settings...
  packages: [
    {
      name: "my-awesome-theme",
      state: {
        theme: {
          autoPrefetch: "hover",
        },
      },
    },
    // Other packages...
  ],
};
```

The possible values for `state.theme.autoPrefetch` are:

| Value     | Description                                       |
| :-------- | :------------------------------------------------ |
| `no`      | No auto prefetch.                                 |
| `hover`   | Prefetches links on hover.                        |
| `in-view` | Prefetch links currently visible in the viewport. |
| `all`     | Prefetches all internal links on the page.        |

#### Custom `Link` component

Using this `<Link />` component is optional. You can create your own `<Link />` component with your own logic.

_Example of a custom `<Link />` component implementation_

```jsx
import React from "react";
import { connect } from "frontity";

const Link = ({
  state,
  actions,
  link,
  className,
  children,
  "aria-current": ariaCurrent,
}) => {
  const onClick = (event) => {
    // Do nothing if it's an external link
    if (link.startsWith("http")) return;

    event.preventDefault();
    // Set the router to the new url.
    actions.router.set(link);

    // Scroll the page to the top
    window.scrollTo(0, 0);
  };

  return (
    <a
      href={link}
      onClick={onClick}
      className={className}
      aria-current={ariaCurrent}
    >
      {children}
    </a>
  );
};

export default connect(Link);
```

#### The `link` processor

Frontity provides a `link` processor. The `link` processor works with the `<html2react>` component and can automatically detect `<a>` tags in the page/post content and intelligently convert them into `<Link>` components.

If the `href` attribute of the `<a>` tag is either:

- a relative link, or
- an absolute link on the same domain as the WordPress data source

then the processor will convert the the `<a>` tag into a `<Link>` component.

The `<Link>` component created by the processor will be modelled on the `<a>` tag and will have properties consistent with its attributes - e.g. the `link` property of the `<Link>` component will be the same as the `href` attribute of `<a>` tag being replaced. The processor will also convert absolute links on the same domain to be relative links.

If the `href` attribute of the `<a>` tag is an absolute link on a different domain from the WordPress data source, i.e. it is a link to an external site, then that tag will remain as is and will not be replaced or converted.

In order for this to work the `link` processor must be imported into the theme and included in the list of `html2react` processors. This would normally be done in the root level `index.js` of your theme. See [here](https://github.com/frontity/api-reference/tree/5cb70b185de018562902d073a62bb934053a5445/docs-api/frontity-packages/features-packages/html2reeact.md) and [here](https://docs.frontity.org/learning-frontity/libraries#array-of-processors-from-html-2-react) for more info.

```javascript
import link from "@frontity/html2react/processors/link";
```

```javascript
libraries: {
  html2react: {
    processors: [link],
  },
```

{% hint style="info" %}
This `link` processor needs to be added to any theme that wants to uses this Client-side navigation for embedded links in the content
{% endhint %}

### Image

`<Image />` is a React component that adds `lazy-loading` to the native WordPress images. Combined with [`@html2react/processors`](../features-packages/html2react.md#processors) , you can add this functionality and optimize your images pretty easy.

### Script

`<Script />` is a React component that executes scripts tags found in content.

#### Props

| Name   | Type   | Required | Description                             |
| :----- | :----- | :------- | :-------------------------------------- |
| `src`  | string | no       | `URL` to an external `JavaScript` file. |
| `code` | string | no       | internal `JavaScript` code              |
| `id`   | string | no       | `ID` for script element                 |

#### Usage

External JavaScript file:

```javascript
import Script from "@frontity/components/script";

const MyComponent = () => (
    <Script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js />
);
```

Internal JavaScript code

```javascript
import Script from "@frontity/components/script";

const MyComponent = () => (
  <Script
    code={`
        const body = document.querySelector('body');

        // Triggers anytime anywhere in the body of the page is clicked
        body.addEventListener('click', e => {
            e.preventDefault();
            console.log('Button Works');
        });
    `}
  />
);
```

### Iframe

`<Iframe />` is a React component that implement lazy-load on iframe components. The approach taken in implementing this component is based off the edge cases in the table below.

| Intersection Observer | Native Lazy | Height &gt; 0 | Output                |
| :-------------------- | :---------- | :------------ | :-------------------- |
| true                  | true        | true          | Native Lazy Load      |
| true                  | true        | false         | Intersection Observer |
| true                  | false       | true          | Intersection Observer |
| true                  | false       | false         | Intersection Observer |
| false                 | true        | true          | \(not possible\)      |
| false                 | true        | false         | \(not possible\)      |
| false                 | false       | true          | Normal Load \(eager\) |
| false                 | false       | false         | Normal Load \(eager\) |

{% hint style="info" %}
Native Lazy needs a height attribute. For that reason, we use the Intersection Observer when a height is not provided.
{% endhint %}

#### Props

| Name         | Type   | Required | Description                                               |
| :----------- | :----- | :------- | :-------------------------------------------------------- |
| `title`      | string | yes      | internal `JavaScript` code                                |
| `src`        | string | no       | `URL` to an external `JavaScript` file.                   |
| `width`      | string | no       | width of the iframe component                             |
| `height`     | string | no       | height of the iframe component                            |
| `className`  | string | no       | class name for the component                              |
| `loading`    | string | no       | `"lazy"` \| `"eager"` \| `"auto"` Default value: `"lazy"` |
| `rootMargin` | string | no       | margin around root element                                |

#### Usage

```javascript
import Iframe from "@frontity/components/iframe";

const MyComponent = () => (
  <Iframe
    src="https://frontity.org"
    title="Frontity"
    height="500"
    width="500"
  />
);
```

### Switch

The `<Switch />` renders the first child component that returns `true` as the value of its `when` prop.

The last child component \(which should not have a `when` prop\) will be rendered if no other component matches the condition.

You can use it for routing to different components in your theme:

```javascript
import Switch from "@frontity/components/switch";

const Theme = ({ state }) => {
  const data = state.source.get(state.router.link);

  return (
    <Switch>
      <Loading when={data.isFetching} />
      <Home when={data.isHome} />
      <Archive when={data.isArchive} />
      <Post when={data.isPostType} />
      <ErrorPage /> {/* rendered by default */}
    </Switch>
  );
};
```

But also inside any other component. For example, in a `<Header>` component that has a different menu for the home:

```javascript
import Switch from "@frontity/components/switch";

const Header = ({ state }) => {
  const data = state.source.get(state.router.link);

  return (
    <Switch>
      <MenuHome when={data.isHome} />
      <Menu /> // rendered by default
    </Switch>
  );
};
```

This component is an alternative to applying plain JavaScript logic in React:

```javascript
const Theme = ({ state }) => {
  const data = state.source.get(state.router.link);

  return (
    <>
      {(data.isFetching && <Loading />) ||
        (data.isHome && <Home />) ||
        (data.isArchive && <Archive />) ||
        (data.isPostType && <Post />) || <ErrorPage />}
    </>
  );
};
```

{% hint style="info" %}
Still have questions? Ask [the community](https://community.frontity.org/)! We are here to help 😊
{% endhint %}
