# Create commands

These commands will allow you to either create a Frontity project or a Frontity package

* [`create`](create.md)
* [`create-package`](create-package.md)

{% hint style="info" %}
Have a look at the [environment variables](../environment-variables.md) page to check which ones can be used with these commands
{% endhint %}

## Environment Variables

The Frontity CLI allows to define some options/flags for some of the Frontity commands via environment variables.
If some of these environment variables are detected the proper values will be set for the proper commands

### FRONTITY_NAME

If you pass the [`--no-prompt`](https://docs.frontity.org/frontity-cli/create-commands#the-no-prompt-option) flag to the [`create`](https://docs.frontity.org/frontity-cli/create-commands#create) or [`create-package`](https://docs.frontity.org/frontity-cli/create-commands#create-package), the CLI will use the name from this `FRONTITY_NAME` environment variable.

If the CLI cannot find a [`FRONTITY_NAME`](https://docs.frontity.org/frontity-cli/environment-variables#frontity_name) environmental variable, it will prompt for the name of the package

This is the scheme followed by the CLI to get the name of the package

![](../.gitbook/assets/no-prompt.png)
