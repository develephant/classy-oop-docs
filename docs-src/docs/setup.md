
## Get the Plugin

If you don't already have it, get the __Classy OOP__ plugin from the [Corona Marketplace](https://marketplace.coronalabs.com/plugin/classy-oop).

## Enable the Plugin

Enable the plugin by adding an entry to the __plugins__ table of your projects __build.settings__ file:

```
settings =
{
    plugins =
    {
        ["plugin.classy"] =
        {
            publisherId = "com.develephant"
        },
    },
}
```

## Require the Plugin

In your code file make sure to `require` the plugin.

```lua
local Classy = require("plugin.classy")
```

You're ready to start building using OOP! Move on the [Usage](/usage/class/) section to learn more.