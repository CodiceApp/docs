# Hooking into Laravel

Because Codice is based on the [Laravel][laravel] framework, an important part
of the Plugin API is just to define *a bridge* between your plugin and
underlying framework code. In this chapter we will demonstrate how to extend
Codice with your own routes, controllers, models, views, language files and
even database migrations.

### Theory: namespaces
Apart from PHP namespace convention mentioned in context of defining main plugin
class (`CodicePlugin\{StudlyIdentifier}`), Laravel also has it's own kind of
namespaces for framework-specific assets, like views of language files. Their
purpose is exactly the same – to ensure no collisions between application and
third party code (such as plugins).

Laravel namespace for your plugin will be its raw identifier and the separator is
`::`. Look at the route definitions below to see it in action.

### Routes
If you want your plugin to define any routes, create `routes.php` in its directory.
Any route defined inside this file will automagically be placed within a route group
with `CodicePlugin\{StudlyIdentifier}\Controllers` as the namespace.

Let's create basic route for our *Upcoming tasks* plugin.

```php
<?php
Route::get('upcoming', ['as' => 'upcoming', 'uses' => 'UpcomingController@getIndex']);
```

### Controllers
Autoloading in Codice adheres to [PSR-4][psr-4] standard so our controller must be saved
in `plugins/docs-upcoming/Controllers/UpcomingController.php`.

```php
<?php

namespace CodicePlugin\DocsUpcoming\Controllers;

use Carbon\Carbon;
use Codice\Http\Controllers\Controller;
use Codice\Note;

class UpcomingController extends Controller
{
    public function __construct()
    {
        $this->middleware('auth');
    }

    public function getIndex()
    {
        $notes = Note::mine()
            ->with('labels')
            ->where('expires_at', '>', Carbon::now())
            ->orderBy('expires_at', 'asc')
            ->get();

        return view('docs-upcoming::upcoming', [
            'notes' => $notes,
        ]);
    }
}
```
As you can see, it is a regular Laravel controller. We started with applying `auth`
middleware in the constructor (it can be done in route definition, if you want).

The most important part is the Eloquent query. We fetch notes using custom Codice's
`mine()` scope, which only fetch items for currently logged in user. Then we join
labels table, require expiration time to be greater than now and sort by it ascending.

Check out core models inside `/app` and you should use to it very quickly.

Last thing we do is to pass data into the view. Do note `docs-upcoming::` part in its
name. This is Laravel namespace.

### Views
Views prefixed with `plugin-identifier::` should be placed in `/plugins/plugin-identifier/views`,
so the full path for our view will be `/plugins/docs-upcoming/views/upcoming.blade.php`.

```php
@extends('app')

@section('content')
    <h2 class="page-heading">@lang('docs-upcoming::main.title')</h2>

    @each('note.single', $notes, 'note', 'note.none')
@stop
```

Once again, this view is nothing special. Moreover, we basically reference other views
provided by Codice. `app` being the main template for the authenticated user and views
meant for displaying notes conveniently inside the `@each` directive.

### Language files
As you probably noticed, we referenced translation way using same method as for views.
The core will look for it in `/plugins/docs-upcoming/lang/{LOCALE}/main.php` and then
`title` key.

Place this content in `lang/en/main.php` to support default Codice's language:

```php
<?php

return [
    'title' => 'Upcoming tasks',
];
```

You can of course provide other languages, such as Polish (`lang/pl/main.php`):

```php
<?php

return [
    'title' => 'Nadchodzące zadania',
];
```

### Models
Even though our sample plugin does not provide any models, adding them is pretty
straightforward. Knowing that PSR-4 autoloading is supported within plugin files
as well, you only need to create a model class somewhere inside the plugin directory
and then `use` it. As in the Laravel itself, there is no specific directory for models,
althought you can create one — just remember to reflect that ins the namespace.

### Migrations
Plugins can provide database migrations as well. There is no helper for creating them
yet, so remember that their order is determined by the filename. You probably just want
to prefix them with current datetime, as Laravel does.

Migrations are applied (`up()`) on plugin install and rolled back (`down()`) when it's
uninstalled (*not* disabled) — it's simple as that.

<div class="alert alert-danger">
Remember to always provide <code>down()</code> method, do it well. Do not let your plugin
to leave cluttered (or worse, corrupted) database after uninstallation. Be civilized and
revert any changes you've made.
</div>

The last thing we are going to do is adding our page to the menu. The process has been
described in [Other APIs](plugin-other) chapter.


[laravel]: https://laravel.com
[psr-4]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader.md
