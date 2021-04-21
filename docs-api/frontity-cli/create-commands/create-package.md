# create-package

The `create-package` command creates a new Frontity package in a project. Launch this command from the root of the Frontity project

```text
npx frontity create-package [package-name] [options]
```

## Arguments

### **`[package-name]`**

This argument sets the _name_ of your Frontity package. The `create-package` command will create a folder named `[package-name]` under `packages`. It will also add the proper dependency in the `package.json` of your Frontity project

### **`[options]`**

| Option | Description |
| :---: | :--- |
| `--namespace <value>` | Sets the [namespace](https://docs.frontity.org/learning-frontity/namespaces) for this package |
| `--no-prompt` | Skips prompting the user for options. Related environment variable: [`FRONTITY_CREATE_PACKAGE_NAME`](create-package.md#frontity_create_package_name). |
| `--open` | Output usage information |

## Examples

* Create a custom theme package named `my-custom-project`

```text
>  npx frontity create-package my-custom-theme
? Enter the namespace of the package: theme
✔ Adding package.json.
✔ Adding src/index.js.
✔ Installing package my-custom-theme.

New package "my-custom-theme" created.
```

## Environment Variables

### `FRONTITY_CREATE_PACKAGE_NAME`

If you pass the [`--no-prompt`](../#no-prompt) flag to the [`create-package`](create-package.md), the CLI will use the name from this `FRONTITY_CREATE_PACKAGE_NAME` environment variable.

If the CLI cannot find a `FRONTITY_CREATE_PACKAGE_NAME` environmental variable, it will prompt for the name of the package

_Example:_

```text
FRONTITY_CREATE_NAME=test-project
```

{% hint style="info" %}
You can see a scheme of the whole workflow of using this `FRONTITY_CREATE_PACKAGE_NAME` environment variable in the [`--no-prompt`](../#no-prompt) section
{% endhint %}

