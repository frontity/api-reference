---
description: API reference of Frontity CLI commands.
---

# 💻 CLI Commands

The Frontity command-line tool \(CLI\) is the main entry point for getting up and running with a Frontity application. It provides functionality like running a development server or building your Frontity application for deployment.

## How to use Frontity CLI

The Frontity CLI \(Frontity commands\) is available via [npm](https://www.npmjs.com/package/frontity). You can run any Frontity command by doing `npx frontity <frontity-command>`

Run `npx frontity --help` for full help.

## Commands

The `frontity` commands you have available are:

### [Create commands](create-commands/)

These commands allow you to either create a Frontity project or a Frontity package:

- [`create`](create-commands/create.md)
- [`create-package`](create-commands/create-package.md)

### [Run commands](run-commands/)

These commands allow you to run a Frontity project in development or production mode:

- [`dev`](run-commands/dev.md)
- [`serve`](run-commands/serve.md)

### [Build commands](build-commands/)

These commands allow you to generate the code that can be used to run or analyze a Frontity project:

- [`build`](build-commands/build.md)

### [Extra commands](extra-commands.md)

- [`subscribe`](extra-commands.md#subscribe)
- [`info`](extra-commands.md#info)

{% hint style="info" %}
You can also use `--help` with each of these commands to get more information about them: `npx frontity dev --help`
{% endhint %}

{% hint style="info" %}
Have a look at [this video](https://www.youtube.com/watch?v=3d7b-cy5cFY) to learn more about what Frontity does internally when these commands are executed.
{% endhint %}

## Arguments & Environment Variables

The Frontity CLI allows parametrization via arguments or environment variables to customize their execution.

If some of these arguments or environment variables are detected the proper values will be set and applied in the execution of the command.

![](https://frontity.org/wp-content/uploads/2021/04/cli-environment-variables.png)

### `--no-prompt`

There's a **`--no-prompt`** option that can be used along with environment variables to avoid any questions from the CLI.

**Example**

If you pass the `--no-prompt` flag to the [`create`](create-commands/create.md) or [`create-package`](create-commands/create-package.md), the CLI will use the name from either [`FRONTITY_CREATE_NAME`](create-commands/create.md#frontity_create_name) or [`FRONTITY_CREATE_PACKAGE_NAME`](create-commands/create-package.md#frontity_create_package_name) environment variables.

If the CLI cannot find any of these environmental variables, it will prompt for the name of the package.

This is the scheme followed by the CLI to get the name of the package:

![](https://frontity.org/wp-content/uploads/2021/04/no-prompt.png)

## A typical workflow with Frontity commands

### In development

1. Create a Frontity project: `npx frontity create my-cool-project`
2. Add a custom theme \(package\): `npx frontity create-package my-custom-theme`
3. Launch a development server: `npx frontity dev`

### In production

1. Generate a build of the project: `npx frontity build`
2. Launch the project in production using the build generated before: `npx frontity serve`
