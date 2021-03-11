---
description: API reference of `@frontity/wp-comments` package
---

# `@frontity/wp-comments`

Comments package that adds integration for WordPress native comments.

> ## Beta version
>
> Please, bear in mind that this package is currently in beta version. It is not fully documented yet and its implementation may change with the final release.

<!-- toc -->

<!-- tocstop -->

## Installation

Add the `wp-comments` package to your project:

```bash
npm i @frontity/wp-comments
```

## Settings

### In WordPress

In order to use this package, you will need to add a single line of configuration to your Wordpress installation:

```php
add_filter( 'rest_allow_anonymous_comments', '__return_true' );
```

### In Frontity

You can add that directly in your `theme.php` file or use a [Code Snippets](https://wordpress.org/plugins/code-snippets/) plugin.

This package doesn't have any configuration, it should just be added in the `frontity.settings.js` to the `packages` array.

**`frontity.settings.js`**

```js
export default {
  packages: ["@frontity/wp-comments"],
};
```

### Comments handler

This is a `wp-source` handler for fetching comments from a specific post using its ID. For example, to fetch all comments that belong to the post with ID 60 you would do:

```
await actions.source.fetch("@comments/60");
```

This would fetch all comments published in that post and populate a data object inside `state.source.data` with a tree structure of comments and replies, sorted by date (most recent first).

To access the fetched comments you could use something similar to this example:

```
const Comments = connect(({ postId, state }) => {
  // Get comments from state.
  const data = state.source.get(`@comments/${postId}`);

  // Utility to render comments and replies recursively.
  const renderComments = (items) => items.map(({ id, children }) => (
    <Comment id={id}>
      {/* Render replies */}
      {children && renderComments(children)}
    </Comment>
  ))

  // Render comments if data is ready.
  return data.isReady
    ? renderComments(data.items)
    : null;
});
```

### `forms` object

The `wp-comments` package stores in `state.comments.forms` a map of objects by post ID, representing each one a comment form. These objects are intended to be used as the state of React `<form>` components and contain the input values as well as the submission status. They have the following two properties:

- **`fields`**: the following map of fields, representing the current field values that have been input in the form rendered in the given post. The content of this property is updated using the **`updateFields()`** action described later.

  | Name      | Type   | Description                                 |
  | --------- | ------ | ------------------------------------------- |
  | `author`  | string | Author's name                               |
  | `email`   | string | Author's email                              |
  | `comment` | string | Text of the comment                         |
  | `url`     | string | URL of the author's site                    |
  | `parent`  | number | ID of the comment to which this one replies |

- **`submitted`**: object that store field values when the form was submitted along with the submission status. It has the same properties described in **`fields`** (with the values that were sent to WordPress) and also the next ones:

  | Name           | Type    | Description                                                     |
  | -------------- | ------- | --------------------------------------------------------------- |
  | `isPending`    | boolean | The comment hasn't been received by WP yet                      |
  | `isOnHold`     | boolean | The comment has been received but not accepted yet              |
  | `isApproved`   | boolean | The comment has been received and is published                  |
  | `isError`      | boolean | The request has failed                                          |
  | `errorMessage` | string  | Failure reason                                                  |
  | `timestamp`    | number  | Submission timestamp (in milliseconds)                          |
  | `id`           | number  | Comment ID if it has been received (`isOnHold` or `isApproved`) |

### `updateFields()` action

Update the fields of the form specified by `postId`. This action simply updates what is stored in `state.comments.form[postId].fields` with the given values.

If no fields are specified, the form fields are emptied.

```
actions.comments.updateFields(60, {
  comment: "Hello world!"
});
```

### `submit()` action

This asynchronous action publishes a new comment to the post specified by `postId`. It submits the fields stored in the respective form (i.e. `state.comments.form[postId]`) or the fields passed as a second argument. If fields are passed, those replace the current values stored in `state.comments.form[postId].fields`.

After calling this action, you can access `state.comments.forms[postId].submitted` properties (described above) to know the submission status.

```
// Submit the comment to the post with ID 60
// using the values stored in `state.comments.forms[60].fields`.
await actions.comments.submit(60);

// Submit the comment to the post with ID 60
// using the values passed in the second argument.
await actions.comments.submit(60, {
  comment: "This is a comment example. Hi!",
  author: "Frontibotito",
  email: "frontibotito@frontity.com",
});
```
