# Writing a plugin

This section of documentation will introduce to you into writing your first simple plugin,
explain its structure and conventions related to this topic.

<div class="alert alert-info">
It is assumed that you have at least basic skill in Laravel. Codice, while providing
custom Plugin API, is still built upon it and in many places writing a plugin just
comes down to the regular Laravel coding.
</div>

### Plugin identifier
Each plugin has its unique ID based on the name of directory it's located in. However,
your plugin folder must live directly in `/plugins` - more levels of nesting are not
allowed.

This is why you should prefix your directory name with so called *vendor prefix*, let
it be your nickname, organisation name etc. You can choose whatever you like and treat
that just like  top vendor namespace in case of PHP (e.g. Packagist) packages.

While learning about plugins in Codice we are also going to create one - name it just a
`sample-plugin`. Combine it with the simplests prefix we can think of and result will be
`docs-sample-plugin`. Filesystem path of it will be `/plugins/docs-sample-plugin` then.

<div class="alert alert-danger">
Plugins released as oficially related to the Codice itself use <code>codice-</code>
prefix. Please do not use it.
</div>

### Defining a plugin class
In Codice all plugins are starting from a single class extending `Codice\Plugins\Plugin`
(most frequently aliased as `PluginBase` to avoid naming collision) as seen in the example
below:

```php
<?php

namespace CodicePlugin\DocsSamplePlugin;

use Codice\Plugins\Plugin as PluginBase;

class Plugin extends PluginBase
{
    // Code of your plugin goes here
}
```

This file should be saved as `/plugins/docs-sample-plugin/Plugin.php` and naming your
base class as `Plugin` is another convention you are obliged to follow.

<div class="alert alert-info">
Plugins extend Laravel's <code>ServiceProvider</code>. Being aware of that is important
in understanding how they work, especially when it comes to difference between 
<code>register()</code> and <code>boot()</code> methods.
</div>

### Basic plugin methods
#### Plugin::pluginDetails()
This method is marked as abstract, which means you must implement it explicicly. `pluginDetails()`
is meant to return basic informations about the plugin, which are used mainly by the plugin manager.

This method must return an array containing following keys: `name`, `description`, `author` and
`version`. The names are rather self-explainatory so let's go straight to the example (`docs-sample-plugin`
is still assumed):

```php
public function pluginDetails()
{
    return [
        'name' => 'Sample docs plugin',
        'description' => 'This is a sample plugin for the purpose of official Codice documentation',
        'author' => 'Codice Team',
        'version' => '1.0',
    ];
}
```

#### Plugin::install()
Implement this method if you want to register any code getting executed on plugin's installation.

```php
public function install()
{
    Log::info('Installed docs-sample-plugin successfully');
}
```

#### Plugin::uninstall()
Same as above method, but for uninstalling a plugin. This is probably the right place to
revert any changes done by the plugin, clean the filesystem, database etc.

#### Plugin::register()
This call happens on registration of the plugin.

#### Plugin::boot()
This call happens on booting of the plugin. The key difference between register and boot
is that `register()` does not guarantee that any other plugin is already available. Booting
is done after every plugin gets registered, so this method is a right place for interaction
with any other plugin.

In fact, this is where most code of your plugin should go - registering actions, filters,
language files, configs and so on. You will learn more in next chapters.
