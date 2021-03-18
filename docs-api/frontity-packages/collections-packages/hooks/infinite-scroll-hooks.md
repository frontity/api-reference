# Infinite Scroll Hooks

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
- [More things added](#more-things-added)
  - [`actions.source.updateState`](#actions-source-updatestate)
    - [Parameters](#parameters)

* [Out of Scope](#out-of-scope)
* [API changes](#api-changes)
  - [Backward compatible changes](#backward-compatible-changes)
  - [Breaking changes](#breaking-changes)
  - [Deprecated APIs](#deprecated-apis)
* [Technical explanation](#technical-explanation)

<!-- tocstop -->

## `useArchiveInfiniteScroll`

This hook implements the logic needed to include infinite scroll in archives (i.e. categories, tags, the posts archive, etc.).

The hook receives options to set a limit of pages shown automatically, to disable it, and also settings for the intersection observers that are passed to the `useInfiniteScroll` hooks used internally.

`useArchiveInfiniteScroll` is designed to be used inside an `Archive` component. That component would render all the archive pages from the `pages` returned by the hook.

Apart from that list, it returns a set of boolean values to know if the next pages is being fetched, if the limit has been reached or if the next page returned an error, and a function to fetch the next page manually.

### Parameters

It accepts an optional object with the following props:

| Name                     | Type                | Default    | Required | Description                                                                                                                                                                                                              |
| :----------------------- | :------------------ | :--------- | :------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`active`**             | boolean             | `true`     | no       | A boolean indicating if this hook should be active or not. It can be useful in situations where users want to share the same component for different types of Archives, but avoid doing infinite scroll in some of them. |
| **`limit`**              | number              | `infinite` | no       | The number of pages that the hook should load automatically before switching to manual fetching.                                                                                                                         |
| **`fetchInViewOptions`** | IntersectionOptions | -          | no       | The intersection observer options for fetching.                                                                                                                                                                          |
| **`routeInViewOptions`** | IntersectionOptions | -          | no       | The intersection observer options for routing.                                                                                                                                                                           |

> NOTE: The IntersectionOptions type refers to the type of the the parameters
> received by the [`useInView` hook](https://api.frontity.org/frontity-packages/collections-packages/hooks#parameters).

### Return value

An object with the following properties:

| Name             | Type                | Description                                                                                                                                                                                                                              |
| :--------------- | :------------------ | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`pages`**      | Array of page props | An array of the existing pages. Users should iterate over this array in their own layout. The content of each element of this array is explained below.                                                                                  |
| **`isFetching`** | boolean             | If it's fetching the next page. Useful to add a loader.                                                                                                                                                                                  |
| **`isLimit`**    | boolean             | If it has reached the limit of pages and it should switch to manual mode.                                                                                                                                                                |
| **`isError`**    | boolean             | If the next page returned an error. Useful to try again.                                                                                                                                                                                 |
| **`fetchNext`**  | function            | A function that fetches the next page. Useful when the limit has been reached (`isLimit === true`) and the user pushes a button to get the next page or when there has been an error fetching the last page and the user wants to retry. |

Each element of the `pages` array has the following structure:

| Name          | Type     | Description                                                                                                  |
| :------------ | :------- | :----------------------------------------------------------------------------------------------------------- |
| **`key`**     | string   | A unique key to be used in the iteration.                                                                    |
| **`link`**    | string   | The link of this page.                                                                                       |
| **`isLast`**  | boolean  | If this page is the last page. Useful to add separators between pages, but avoid adding it for the last one. |
| **`Wrapper`** | React.FC | The Wrapper component that should wrap the real `Archive` component.                                         |

### Usage

````javascript
import { connect, useConnect } from "frontity";
import { useArchiveInfiniteScroll } from "@frontity/hooks";
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

This hook is more complex than the previous one, as it works getting the post type entities from the specified archive and thus it doesn't fetch the next post but the next page of posts.

It recevies an `archive` and a `fallback` prop ―both links―, to specify the source of the post entities. If none of them is specified, `state.source.postsPage` is used. When the penultimate post of the first page is
rendered, the next page of the archive is fetched. A list of the fetched pages is stored in the browser history state along with the list of posts.

The `limit` prop in this case stands for the number of posts being shown, not the number of fetched pages. In the same way, the `fetchNext` shows the next post, and only fetches the next page of posts if needed.

### Parameters

It accepts an optional object with the following props:

| Name                     | Type                | Default    | Required | Description                                                                                                                                                                                                              |
| :----------------------- | :------------------ | :--------- | :------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`active`**             | boolean             | `true`     | no       | A boolean indicating if this hook should be active or not. It can be useful in situations where users want to share the same component for different types of Archives, but avoid doing infinite scroll in some of them. |
| **`limit`**              | number              | `infinite` | no       | The number of pages that the hook should load automatically before switching to manual fetching.                                                                                                                         |
| **`archive`**            | string              | -          | no       | The archive that should be used to get the next posts. If none is present, the previous link is used. If the previous link is not an archive, the homepage is used.                                                      |
| **`fallback`**           | string              | -          | no       | The archive that should be used if the `archive` option is not present and the previous link is not an archive.                                                                                                          |
| **`fetchInViewOptions`** | IntersectionOptions | -          | no       | The intersection observer options for fetching.                                                                                                                                                                          |
| **`routeInViewOptions`** | IntersectionOptions | -          | no       | The intersection observer options for routing.                                                                                                                                                                           |

> NOTE: The IntersectionOptions type refers to the type of the the parameters
> received by the [`useInView` hook](https://api.frontity.org/frontity-packages/collections-packages/hooks#parameters).

### Return value

The output of these hooks is pretty similar to the previous one's:

| Name             | Type                | Description                                                                                                                                                                                                                              |
| :--------------- | :------------------ | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`posts`**      | Array of post props | An array of the existing posts. Users should iterate over this array in their own layout. The content of each element of this array is explained below.                                                                                  |
| **`isFetching`** | boolean             | If it's fetching the next post. Useful to add a loader.                                                                                                                                                                                  |
| **`isLimit`**    | boolean             | If it has reached the limit of posts and it should switch to manual mode.                                                                                                                                                                |
| **`isError`**    | boolean             | If the next page returned an error. Useful to try again.                                                                                                                                                                                 |
| **`fetchNext`**  | function            | A function that fetches the next post. Useful when the limit has been reached (`isLimit === true`) and the user pushes a button to get the next post or when there has been an error fetching the last post and the user wants to retry. |

Each element of the `posts` array has the following structure:

| Name          | Type     | Description                                                                                                  |
| :------------ | :------- | :----------------------------------------------------------------------------------------------------------- |
| **`key`**     | string   | A unique key to be used in the iteration.                                                                    |
| **`link`**    | string   | The link of this page.                                                                                       |
| **`isLast`**  | boolean  | If this post is the last post. Useful to add separators between posts, but avoid adding it for the last one. |
| **`Wrapper`** | React.FC | The Wrapper component that should wrap the real `Post` component.                                            |

### Usage

````js
import { connect, useConnect } from "frontity";
import { usePostTypeInfiniteScroll } from "@frontity/hooks";
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

---

## More things added

### `actions.source.updateState`

Action that replaces the value of `state.router.state` with the give object. The
same object is stored in the browser history state using the
[`history.replaceState()`](https://developer.mozilla.org/en-US/docs/Web/API/History/replaceState)
function.

#### Parameters

| Name               | Type   | Default | Required | Description                             |
| :----------------- | :----- | :------ | :------- | :-------------------------------------- |
| **`historyState`** | object | -       | yes      | The object to set as the history state. |

# Out of Scope

I was out of the scope of this PR to implement a way to let developers to change
the logic that `usePostTypeInfiniteScroll` uses to get the next post.

Right now, that logic is the following:

- If the post is the first post rendered and it's not included in the first page
  of the archive, the next post is the first post of the archive.
- For any other case, get the index where the post appears in the fetched pages.
  The next post will be the one with index + 1.

# API changes

### Backward compatible changes

Instead of having to import each hook from its module, hooks can be imported now
from the package root:

```javascript
import { useInView, useInfiniteScroll } from "@frontity/hooks";
```

They can still be imported directly from each module:

```javascript
import useInView from "@frontity/hooks/use-in-view";
import useInfiniteScroll from "@frontity/hooks/use-infnite-scroll";
```

### Breaking changes

No breaking changes.

### Deprecated APIs

No deprecated APIs.

# Technical explanation

Work in progress.

{% hint style="info" %}
Still have questions? Ask [the community](https://community.frontity.org/)! We are here to help 😊
{% endhint %}
