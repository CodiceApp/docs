# Other plugin-related APIs
This chapter will describe other APIs to be used by plugins.

### Menu manager
Like in case of our training plugin, sometimes there is a neeed to alter default
menu. Codice provides special class for that, so let's see how it might be used.

Remember that all plugins are in fact service providers? That also means they have
`app` instance injected which we can utilize to resolve menu manager dependency
from the container. Let's finally extend `boot()` method for our plugin.

```php
public function boot()
{
    parent::boot();

    $m = $this->app->make('menu.main');
    $m->add('upcoming', 'Upcoming tasks', 'tasks', 6);
}
```

<div class="alert alert-info">
Codice core creates two instances of menu manager. <code>menu.main</code> is for
the main menu and <code>menu.user</code> is for the dropdown on the right side.
</div>

Apart from calling parent method we are using obtained menu manager instance and
call `add()` method to create new link.

#### *void* `Menu::add`(*string* `$route`, *string* `$name`, *string* `$icon`, *int* `$position` = `null`, *array* `$additionalRoutes` = `[]`)

**`$route`**  
Route name we want link to.

**`$name`**  
Display name for the menu item.

**`$icon`**  
Class for one of the [FontAwesome][fontawesome] icons.

**`$position`**  
Numeric position of the menu item. Priorites for default items are subsequent
multiples of five so additional items can be placed between them. If ommited,
priority will be highest registered priority + 1. Order of items with same
priority is undefined.

**`$additionalRoutes`**  
Optional array of routes which, apart from `$route`, should trigger `.active`
class for the item.

### *Upcoming tasks* plugin completed
And that's all! Our plugin should work just fine. If you performed every step
correctly, you should be able to install the plugin from plugin manager (available
from user menu). For the convenience you can find the complete plugin code in its
[GitHub repository][docs-upcoming].


[fontawesome]: http://fontawesome.io/icons/
[docs-upcoming]: https://github.com/getcodice/plugin-docs-upcoming
