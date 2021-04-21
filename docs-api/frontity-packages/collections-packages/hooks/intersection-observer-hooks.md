# Intersection Observer Hooks

These hooks helps you to track when an element enters or leaves the viewport.

## Table of Contents

* [`useInView`](intersection-observer-hooks.md#useinview)
  * [Usage](intersection-observer-hooks.md#usage)
  * [Parameters](intersection-observer-hooks.md#parameters)
  * [Return value](intersection-observer-hooks.md#return-value)
  * [Demo](intersection-observer-hooks.md#demo)

## `useInView`

This hook just wraps the [`react-intersection-observer`](https://github.com/thebuilder/react-intersection-observer) library which uses internally the [`IntersectionObserver`](https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserver) API. As some old browsers don't support it, `useInView` also returns a `supported` prop indicating if it's supported or not.

### Usage

```javascript
import useInView from "@frontity/hooks/use-in-view";

const MyLazyElement = ({ children }) => {
  // Get the reference and the visibility status.
  const { ref, inView } = useInView({ triggerOnce: true });

  // Pass the reference to the container and render `children` if
  // the container is visible, or a placeholder otherwise.
  return <div ref={ref}>{inView ? children : <MyPlaceholder />}</div>;
};
```

### Parameters

It accepts a single object with the following props:

| Name | Type | Default | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| **`root`** | Element | `window` | no | The Element that is used as the viewport for checking visibility of the target. Defaults to the browser viewport \(`window`\) if not specified or if null. |
| **`rootMargin`** | string | `"0px"` | no | Margin around the root. Can have values similar to the CSS margin property, e.g. "10px 20px 30px 40px" \(top, right, bottom, left\). |
| **`threshold`** | number or array of numbers | `0` | no | Number between 0 and 1 indicating the percentage that should be visible before triggering. Can also be an array of numbers, to create multiple trigger points. |
| **`triggerOnce`** | boolean | `false` | no | Only trigger this method once |

### Return value

An object with the following properties:

| Name | Type | Description |
| :--- | :--- | :--- |
| **`ref`** | React.RefObject | React reference object pointing to the DOM element. It must be passed to the element you want to track. |
| **`inView`** | boolean | Boolean indicating if the element is visible. The value is always `true` if _`supported`_ is `false`. |
| **`supported`** | boolean | Boolean indicating if [`IntersectionObserver`](https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserver) is supported by the browser. |

{% hint style="info" %}
Still have questions? Ask [the community](https://community.frontity.org/)! We are here to help 😊
{% endhint %}

### Demo

This [demo project](https://github.com/frontity-demos/frontity-examples/blob/master/intersection-observer-hooks/README.md) shows how to use the Intersection Observer Hook `useInView` available in the `@frontity/hooks` package or directly implemented in some components available at `@frontity/components`

