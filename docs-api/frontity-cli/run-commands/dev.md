# `dev`

Starts a development server.

```text
npx frontity dev [options]
```

## Arguments

### **`[options]`**

|                                   Option                                   | Description                                                                                                                                                                                             |
| :------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
|                  [`--production`](#the-production-option)                  | Builds the project for production. Related environment variable: [`FRONTITY_DEV_PRODUCTION`](#frontity_dev_production)                                                                                  |
|                              `--port <port>`                               | Runs the server on a custom port. Default is 3000. Related environment variable: [`FRONTITY_DEV_PORT`](#frontity_dev_port)                                                                              |
|                                 `--https`                                  | Runs the server using https. Related environment variable: [`FRONTITY_DEV_HTTPS`](#frontity_dev_https)                                                                                                  |
|                           `--dont-open-browser`                            | Don't open a browser window with the localhost. Related environment variable: [`FRONTITY_DEV_DONT_OPEN_BROWSER`](#frontity_dev_dont_open_browser)                                                       |
|    [`--target <target>`](../build-commands/README.md#the-target-option)    | Create bundles with `es5` or `module`. Default target is `module`. Related environment variable: [`FRONTITY_DEV_TARGET`](#frontity_dev_target)                                                          |
| [`--publicPath <path>`](../build-commands/build.md#the-public-path-option) | Set the [public path](https://webpack.js.org/guides/public-path/) for static assets. Default path is `/static/`. Related environment variable: [`FRONTITY_DEV_PUBLIC_PATH`](#frontity_dev_public_path). |
|                                  `--help`                                  | Output usage information                                                                                                                                                                                |

**Examples**

- Starts a server in development mode using https and port 3002

```text
npx frontity dev --https --port 3002
```

- Starts a server in development mode using the folder `assets` as the path for statics

```text
npx frontity dev --public-path="/assets"
```

### The `--production` option

This flag correspond to [webpack’s mode parameter](https://webpack.js.org/configuration/mode/) so it will run webpack in the production mode as described [there](https://webpack.js.org/configuration/mode/) before launching the development server.

So, if you do:

```text
npx frontity dev --production
```

The webpack bundler internally will do things like..

- Enable certain webpack-specific optimizations and minify the code
- Also disable hot-module reloading (HMR)
- Not create source maps
- Append hashes to filenames so for caching purposes

Normally, you would always use the development server in development mode, but sometimes you may want to check that everything works in production mode, or check the bundle analyzer (the files at `/build/analyze`) for the production bundle.

## Environment Variables

### `FRONTITY_DEV_TARGET`

Create bundles with `es5`, `module` or `both`. Default target is `both`.

If detected, and no `--target <target>` option is defined for [`dev`](#dev) Frontity command, this environment variable value will be applied.

_Example:_

```text
FRONTITY_DEV_TARGET=module
FRONTITY_DEV_TARGET=es
```

### `FRONTITY_DEV_PORT`

Runs the server on a custom port. Default is `3000`.

If detected, and no `--port <port>` option is defined for [`dev`](#dev) Frontity command, this environment variable value will be applied.

_Example:_

```text
FRONTITY_DEV_PORT=3002
```

### `FRONTITY_DEV_HTTPS`

Runs the server using https.

_Example:_

```text
FRONTITY_DEV_HTTPS=true
```

### `FRONTITY_DEV_PRODUCTION`

`frontity dev` by default runs the server in "development mode" \(no optimizations, uses the dev build of react, etc.\). Setting this variable makes it run in "production mode".

_Example:_

```text
FRONTITY_DEV_PRODUCTION=true
```

### `FRONTITY_DEV_PUBLIC_PATH`

Set the public path for static assets. Default path is `/static/`.

If detected, and no `--public-path` flag is defined for [`dev`](#dev) Frontity command, this environment variable value will be applied.

_Example:_

```text
FRONTITY_DEV_PUBLIC_PATH=/assets/
```

### `FRONTITY_DEV_DONT_OPEN_BROWSER`

Don't open a browser window after the Frontity server has been started.

_Example:_

```text
FRONTITY_DEV_DONT_OPEN_BROWSER=true
```
