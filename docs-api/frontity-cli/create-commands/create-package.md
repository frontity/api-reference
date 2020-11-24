## `create-package`

The `create-package` command creates a new Frontity package in a project. Launch this command from the root of the Frontity project

```text
npx frontity create-package [package-name] [options]
```

### Arguments

#### **`[package-name]`**

This argument sets the _name_ of your Frontity package. The `create-package` command will create a folder named `[package-name]` under `packages`. It will also add the proper dependency in the `package.json` of your Frontity project

#### **`[options]`**

| Option | Description |
| :---: | :--- |
| `--namespace <value>` | Sets the [namespace](https://docs.frontity.org/learning-frontity/namespaces) for this package |
| [`--no-prompt`](../environment-variables#frontity_name) | Skips prompting the user for options. Related environment variable: [`FRONTITY_NAME`](../environment-variables#frontity_name).|
| `--open` | Output usage information |

### Examples

* Create a custom theme package named `my-custom-project`

```text
>  npx frontity create-package my-custom-theme
? Enter the namespace of the package: theme
✔ Adding package.json.
✔ Adding src/index.js.
✔ Installing package my-custom-theme.

New package "my-custom-theme" created.
```

