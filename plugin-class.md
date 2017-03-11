# Plugin class

The foundation for each Codice plugin is a single class extending `Codice\Plugins\Plugin`.
It is a kind of *entry point* which is called to hook your plugin into the core. The class
itself must be named `Plugin` and you can see the simplest example of it below:

```php
<?php

namespace CodicePlugin\DocsUpcoming;

use Codice\Plugins\Plugin as PluginBase;

class Plugin extends PluginBase
{
    // Code of your plugin goes here
}
```

This file should be saved as `/plugins/docs-upcoming/Plugin.php`.

Please note the convention for the plugin namespace. Like in this case, it will always be 
`CodicePlugin\{StudlyCasedIdentifier}`. Autoloading is supported within plugin namespace,
so you don't need to include files manually. This will be demonstrated in next chapters.

### Basic plugin methods
#### Plugin::install()
This method can be implemented for running any custom code on plugin installation. Code will be
executed right after running plugin database migrations (if there are any) and only if defined
plugin-specific requirements are met.

```php
public function install()
{
    Log::info('Installed "Upcoming tasks" plugin successfully');
}
```

#### Plugin::uninstall()
Same as above method, but for uninstalling a plugin. This is probably the right place to
revert any changes done by the plugin, clean the filesystem, database etc. Note this
method is called before rolling back defined migrations.

#### Plugin::register()
This call happens on registration of the plugin and usually contains most of plugin's code,
such as registering actions, filters etc.

#### Plugin::boot()
This call happens on booting of the plugin. Each `Plugin` class internally extends Laravel's
`ServiceProvider` so this is what defines a difference in their behaviour. Consult the
[Laravel documentation][laravel-providers] on service providers for more details.


[laravel-providers]: https://laravel.com/docs/5.4/providers
