---
description: API reference of `@frontity/hooks` package
---

# @frontity/hooks

This package is a collection of React hooks that have proven to be pretty useful for a Frontity project.

## Table of Contents

- [Installation](./#installation)
- [How to use](./#how-to-use)
- [Hooks](./#hooks)

## Installation

Add the `@frontity/hooks` package to your project:

```text
npm i @frontity/hooks
```

## How to use

In order to use it, you just have to import the hook you want to use in your theme from `@frontity/hooks` and place it wherever needed. For example, if we want to use the `useInView` hook:

```javascript
import useInView from "@frontity/hooks/use-in-view";
```

## Hooks

These are the hooks available from the package `@frontity/hooks`

- [**Intersection Observer Hooks**](intersection-observer-hooks.md)
  - [`useInView`](intersection-observer-hooks.md#useinview)
- [**Infinite Scroll Hooks**](infinite-scroll-hooks.md)
  - [`useInfiniteScroll`](infinite-scroll-hooks.md#useinfinitescroll)
  - [`useArchiveInfiniteScroll`](infinite-scroll-hooks.md#usearchiveinfinitescroll)
  - [`usePostTypeInfiniteScroll`](infinite-scroll-hooks.md#useposttypeinfinitescroll)
