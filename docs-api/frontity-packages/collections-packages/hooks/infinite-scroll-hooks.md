# Infinite Scroll Hooks

The recommended hooks to use for a Infinite Scroll behaviour are:

- `useArchiveInfiniteScroll`
- `usePostTypeInfiniteScroll`

There's also another one available for implementing custom infinite scroll hooks (used internally by the previous two hooks):

- `useInfiniteScroll`

{% hint style="danger" %}
`useInfiniteScroll` is not intented to be used directly by theme developers unless they are creating their own infinite scroll logic. Use `useArchiveInfiniteScroll` or `usePostTypeInfiniteScroll` instead.
{% endhint %}

The main idea behind these hooks is that they return a list of `Wrapper` components, one for each entity listed while scrolling, that handle both the route updating and fetching of the next entity.

## Table of Contents

<!-- toc -->


- [`useArchiveInfiniteScroll`](#usearchiveinfinitescroll)
  - [Parameters](#parameters)
  - [Return value](#return-value)
  - [Usage](#usage)
- [`usePostTypeInfiniteScroll`](#useposttypeinfinitescroll)
  - [Parameters](#parameters)
  - [Return value](#return-value)
  - [Usage](#usage)
- [Demo](#demo)
- [`useInfiniteScroll`](#useinfinitescroll)
  - [Parameters](#parameters)
  - [Return value](#return-value)
  - [Usage](#usage)

<!-- tocstop -->

## `useArchiveInfiniteScroll`

This hook implements the logic needed to include infinite scroll in archives (i.e. categories, tags, the posts archive, etc.).

The hook receives options to set a limit of pages shown automatically, to disable it, and also settings for the intersection observers that are passed to the `useInfiniteScroll` hooks used internally.

`useArchiveInfiniteScroll` is designed to be used inside an `Archive` component. That component would render all the archive pages from the `pages` returned by the hook.

In addition to the above, the hook returns a set of boolean values that indicate if the next page is being fetched, if the limit has been reached, or if the next page returned an error, and a function that allows the next page to be fetched manually.

### Parameters

It accepts an optional object with the following props:

| Name                     | Type                                                                                                      | Default                                                                                                 | Required | Description                                                                                                                                                                                                              |
| :----------------------- | :-------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------ | :------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`active`**             | `boolean`                                                                                                 | `true`                                                                                                  | no       | A boolean indicating if this hook should be active or not. It can be useful in situations where users want to share the same component for different types of Archives, but avoid doing infinite scroll in some of them. |
| **`limit`**              | `number`                                                                                                  | [`Infinity`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Infinity) | no       | The number of pages that the hook should load automatically before switching to manual fetching.                                                                                                                         |
| **`fetchInViewOptions`** | [`IntersectionOptions`](https://api.frontity.org/frontity-packages/collections-packages/hooks#parameters) | -                                                                                                       | no       | The intersection observer options for fetching.                                                                                                                                                                          |
| **`routeInViewOptions`** | [`IntersectionOptions`](https://api.frontity.org/frontity-packages/collections-packages/hooks#parameters) | -                                                                                                       | no       | The intersection observer options for routing.                                                                                                                                                                           |

{% hint style="info" %}
The IntersectionOptions type refers to the type of the the parameters received by the [`useInView` hook](https://api.frontity.org/frontity-packages/collections-packages/hooks#parameters).
{% endhint %}

### Return value

An object with the following properties:

| Name             | Type                  | Description                                                                                                                                                                                                                              |
| :--------------- | :-------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`pages`**      | `Array` of page props | An array of the existing pages. Users should iterate over this array in their own layout. The content of each element of this array is explained below.                                                                                  |
| **`isFetching`** | `boolean`             | If it's fetching the next page. Useful to add a loader.                                                                                                                                                                                  |
| **`isLimit`**    | `boolean`             | If it has reached the limit of pages and it should switch to manual mode.                                                                                                                                                                |
| **`isError`**    | `boolean`             | If the next page returned an error. Useful to provide functionality (e.g. a button) to enable the user to try again.                                                                                                                     |
| **`fetchNext`**  | `function`            | A function that fetches the next page. Useful when the limit has been reached (`isLimit === true`) and the user pushes a button to get the next page or when there has been an error fetching the last page and the user wants to retry. |

Each element of the `pages` array has the following structure:

| Name          | Type       | Description                                                                                                  |
| :------------ | :--------- | :----------------------------------------------------------------------------------------------------------- |
| **`key`**     | `string`   | A unique key to be used in the iteration.                                                                    |
| **`link`**    | `string`   | The link of this page.                                                                                       |
| **`isLast`**  | `boolean`  | If this page is the last page. Useful to add separators between pages, but avoid adding it for the last one. |
| **`Wrapper`** | `React.FC` | The Wrapper component that should wrap the real `Archive` component.                                         |

### Usage

````javascript
import { connect } from "frontity";
import useArchiveInfiniteScroll from "@frontity/hooks/use-archive-infinite-scroll";
import ArchivePage from "./archive-page";

/**
 * Simple component showing the usage of the `useArchiveInfiniteScroll` hook.
 *
 * @example
 * ```
 * // In the Theme component:
 * <Switch>
 *   {...}
 *   <Archive when={data.isArchive} />
 * </Switch>
 * ```
 */
const Archive = () => {
  // Get the list of pages from the hook.
  const {
    pages,
    isFetching,
    isLimit,
    isError,
    fetchNext,
  } = useArchiveInfiniteScroll({ limit: 3 });

  return (
    <>
      {pages.map(({ Wrapper, key, link, isLast }) => (
        <Wrapper key={key}>
          <ArchivePage link={link} />
          {!isLast && <PageSeparator />}
        </Wrapper>
      ))}

      {isFetching && <div>Loading more...</div>}

      {(isLimit || isError) && (
        <button onClick={fetchNext}>
          {isError ? "Something failed - Retry" : "Load More"}
        </button>
      )}
    </>
  );
};

export default connect(Archive);
````

## `usePostTypeInfiniteScroll`

Hook that implements the logic needed to include infinite scroll in a post type view (i.e. posts, pages, galleries, etc.).

This hook is more complex than the previous one, as it works by getting the post type entities from the specified archive and thus it doesn't fetch the next post but the next page of posts.

It recevies an `archive` and a `fallback` prop ―both links―, to specify the source of the post entities. If none of them is specified, `state.source.postsPage` is used. When the penultimate post of the first page is
rendered, the next page of the archive is fetched. A list of the fetched pages is stored in the browser history state along with the list of posts.

The `limit` prop in this case stands for the number of posts being shown, not the number of fetched pages (as in the case of `useArchiveInfiniteScroll`). In the same way, the `fetchNext` shows the next post, and only fetches the next page of posts if needed.

### Parameters

It accepts an optional object with the following props:

| Name                     | Type                                                                                                      | Default                                                                                                 | Required | Description                                                                                                                                                                                                              |
| :----------------------- | :-------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------ | :------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`active`**             | `boolean`                                                                                                 | `true`                                                                                                  | no       | A boolean indicating if this hook should be active or not. It can be useful in situations where users want to share the same component for different types of Archives, but avoid doing infinite scroll in some of them. |
| **`limit`**              | `number`                                                                                                  | [`Infinity`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Infinity) | no       | The number of posts that are rendered until the user interacts (e.g. clicking a button) in order to show the next post.                                                                                                  |
| **`archive`**            | `string`                                                                                                  | -                                                                                                       | no       | The archive that should be used to get the next posts. If none is present, the previous link is used. If the previous link is not an archive, the homepage is used.                                                      |
| **`fallback`**           | `string`                                                                                                  | -                                                                                                       | no       | The archive that should be used if the `archive` option is not present and the previous link is not an archive.                                                                                                          |
| **`fetchInViewOptions`** | [`IntersectionOptions`](https://api.frontity.org/frontity-packages/collections-packages/hooks#parameters) | -                                                                                                       | no       | The intersection observer options for fetching.                                                                                                                                                                          |
| **`routeInViewOptions`** | [`IntersectionOptions`](https://api.frontity.org/frontity-packages/collections-packages/hooks#parameters) | -                                                                                                       | no       | The intersection observer options for routing.                                                                                                                                                                           |

{% hint style="info" %}
The IntersectionOptions type refers to the type of the the parameters received by the [`useInView` hook](https://api.frontity.org/frontity-packages/collections-packages/hooks#parameters).
{% endhint %}

### Return value

The output of these hooks is pretty similar to the previous one's:

| Name             | Type                  | Description                                                                                                                                                                                                                              |
| :--------------- | :-------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`posts`**      | `Array` of post props | An array of the existing posts. Users should iterate over this array in their own layout. The content of each element of this array is explained below.                                                                                  |
| **`isFetching`** | `boolean`             | If it's fetching the next post. Useful to add a loader.                                                                                                                                                                                  |
| **`isLimit`**    | `boolean`             | If it has reached the limit of posts and it should switch to manual mode.                                                                                                                                                                |
| **`isError`**    | `boolean`             | If the next post fetched returned an error. Useful to provide functionality (e.g. a button) to enable the user to try again.                                                                                                             |
| **`fetchNext`**  | `function`            | A function that fetches the next post. Useful when the limit has been reached (`isLimit === true`) and the user pushes a button to get the next post or when there has been an error fetching the last post and the user wants to retry. |

Each element of the `posts` array has the following structure:

| Name          | Type       | Description                                                                                                  |
| :------------ | :--------- | :----------------------------------------------------------------------------------------------------------- |
| **`key`**     | `string`   | A unique key to be used in the iteration.                                                                    |
| **`link`**    | `string`   | The link of this page.                                                                                       |
| **`isLast`**  | `boolean`  | If this post is the last post. Useful to add separators between posts, but avoid adding it for the last one. |
| **`Wrapper`** | `React.FC` | The Wrapper component that should wrap the real `Post` component.                                            |

### Usage

````js
import { connect } from "frontity";
import usePostTypeInfiniteScroll from "@frontity/hooks/use-post-type-infinite-scroll";
import PostTypeEntity from "./post-type-entity";

/**
 * Simple component showing the usage of the `usePostTypeInfiniteScroll` hook.
 *
 * @example
 * ```
 * // In the Theme component:
 * <Switch>
 *   {...}
 *   <PostType when={data.isPostType} />
 * </Switch>
 * ```
 */
const PostType = () => {
  // Get the list of posts from the hook.
  const {
    posts,
    isFetching,
    isLimit,
    isError,
    fetchNext,
  } = usePostTypeInfiniteScroll({ limit: 5 });

  return (
    <>
      {posts.map(({ Wrapper, key, link, isLast }) => (
        <Wrapper key={key}>
          <PostTypeEntity link={link} />
          {!isLast && <PostSeparator />}
        </Wrapper>
      ))}

      {isFetching && <div>Loading more...</div>}

      {(isLimit || isError) && (
        <button onClick={fetchNext}>
          {isError ? "Something failed - Retry" : "Load More"}
        </button>
      )}
    </>
  );
};

export default connect(PostType);
````

## Demo

This short video demonstrates the usage of the Infinite Scroll Hooks avalable at the `@frontity/hooks` package.

{% embed url="https://www.youtube.com/watch?v=30E3lG3-onU" %}

The project used in the video is available [here](https://github.com/frontity-demos/frontity-examples/tree/master/infinite-scroll-hooks).

## `useInfiniteScroll`


{% hint style="danger" %}
`useInfiniteScroll` is not intented to be used directly by theme developers unless they are creating their own infinite scroll logic. Use `useArchiveInfiniteScroll` or `usePostTypeInfiniteScroll` instead.
{% endhint %}

This is the core hook with the basic logic to build an infinite scroll hook.

It basically receives two links, `currentLink` and `nextLink`, and returns two React refs that should be attached to react elements. The hook uses `useInView` internally to track the visibility of those elements and trigger an `actions.router.set` to update the current link or an `actions.source.fetch` to fetch the next entity. You can pass options for these `useInView` hooks as well, using the `fetchInViewOptions` and the `routeInViewOptions` params.

`useInfiniteScroll` also keeps a record of the fetched & ready entities in the browser history state, in order to restore the list when you go back and forward while navigating. That record is accessible from the browser history state under the `infiniteScroll.links` array.

{% hint style="info" %}
Note: the history state is also accessible from the Frontity state, in `state.router.state`.
{% endhint %}

It was designed to be used inside a `Wrapper` component that would wrap the
entity pointed by `currentLink`.

### Parameters

It requires an object with the following props:

| Name                     | Type                                                                                                      | Default | Required | Description                                                                 |
| :----------------------- | :-------------------------------------------------------------------------------------------------------- | :------ | :------- | :-------------------------------------------------------------------------- |
| **`currentLink`**        | `string`                                                                                                  | -       | yes      | The current link that should be used to start the infinite scroll.          |
| **`nextLink`**           | `string`                                                                                                  | -       | no       | The next link that should be fetched and loaded once the user scrolls down. |
| **`fetchInViewOptions`** | [`IntersectionOptions`](https://api.frontity.org/frontity-packages/collections-packages/hooks#parameters) | -       | no       | The intersection observer options for fetching.                             |
| **`routeInViewOptions`** | [`IntersectionOptions`](https://api.frontity.org/frontity-packages/collections-packages/hooks#parameters) | -       | no       | The intersection observer options for routing.                              |

{% hint style="info" %}
The IntersectionOptions type refers to the type of the the parameters received by the [`useInView` hook](https://api.frontity.org/frontity-packages/collections-packages/hooks#parameters).
{% endhint %}

### Return value

| Name              | Type        | Description                                                                                                                                                                  |
| :---------------- | :---------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`supported`**   | `boolean`   | Boolean indicating if the Intersection Observer is supported or not by the browser. In the case `supported` is `false`, all the other returned properties will be undefined. |
| **`routeRef`**    | `React.Ref` | The ref that should be attached to the element used to trigger `actions.router.set`.                                                                                         |
| **`fetchRef`**    | `React.Ref` | The ref that should be attached to the element used to trigger `actions.source.fetch`.                                                                                       |
| **`routeInView`** | `boolean`   | Boolean that indicates when the element used to trigger `actions.router.set` is in the screen.                                                                               |
| **`fetchInView`** | `boolean`   | Boolean that indicates when the element used to trigger `actions.source.fetch` is in the screen.                                                                             |

### Usage

{% hint style="info" %}
Note: this is just an example to illustrate how the `useInfiniteScroll` works. For better examples, see the `useArchiveInfiniteScroll` and the `usePostTypeInfiniteScroll` implementation.
{% endhint %}

```javascript
import { useConnect, connect, css } from "frontity";
import useInfiniteScroll from "@frontity/hooks/use-infinite-scroll";
import { isArchive, isError } from "@frontity/source";

export const wrapperGenerator = ({
  link,
  fetchInViewOptions,
  routeInViewOptions,
}) => {
  const Wrapper = ({ children }) => {
    const { state } = useConnect();

    const current = state.source.get(link);
    const next =
      isArchive(current) && current.next
        ? state.source.get(current.next)
        : null;

    const { supported, fetchRef, routeRef } = useInfiniteScroll({
      currentLink: link,
      nextLink: next?.link,
      fetchInViewOptions,
      routeInViewOptions,
    });

    if (!current.isReady || isError(current)) return null;
    if (!supported) return children;

    const container = css`
      position: relative;
    `;

    const fetcher = css`
      position: absolute;
      width: 100%;
      bottom: 0;
    `;

    return (
      <div css={container} ref={routeRef}>
        {children}
        {<div css={fetcher} ref={fetchRef} />}
      </div>
    );
  };

  return connect(Wrapper);
};
```

{% hint style="info" %}
Still have questions? Ask [the community](https://community.frontity.org/)! We are here to help 😊
{% endhint %}
