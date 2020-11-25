# Create commands

These commands will allow you to either create a Frontity project or a Frontity package

* [`create`](create.md)
* [`create-package`](create-package.md)

## Environment Variables

### FRONTITY_NAME

If you pass the `--no-prompt` flag to the [`create`](create.md) or [`create-package`](create-package.md), the CLI will use the name from this `FRONTITY_NAME` environment variable.

If the CLI cannot find a `FRONTITY_NAME` environmental variable, it will prompt for the name of the package

This is the scheme followed by the CLI to get the name of the package

![](../../.gitbook/assets/no-prompt.png)
