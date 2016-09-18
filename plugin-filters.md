# Filters

<div class="alert alert-info">
Filters are another kind of <em>hookables</em> - just like actions are. They even
share most of their code implementation. Be sure to read previous chapter first.
</div>

*Filters* API is another way plugins can interact with Codice using event-driven approach

There is one key difference betweeen actions and filters - actions are just places where
you can hook your own code to execute and filters are meant to *alter* values (be that
strings, arrays, objects - technically anything we can assign to a PHP variable) at various
stages of execution.

In other words - filter must return a value, while action does not have to.

For example, you can modify Note object right before its saved into the database.

*Somewhere in Codice code:*
```php
class NoteController extends Controller
{
	...
	public function postCreate()
	{
		// Method fired when form for creating new note is send using POST request
		
		$note->content = $request->input('content');
        $note->created_at = time();
        $note->other_field = 'value';
		
        ...
        $note = Filter::call('note.save', $value);
        ...

		$note->save();
	}
}
```

`$note` specified as a second parameter to `Filter::call()` is going to be passed to the
first filter (priority is taken into account), registered for a hook named `note.save` - it
can modify its value and must return it back, so resulting state will be passed to the next
filter. After all filters are called, final value is saved back to the `$note` variable.

Let's take a look at possible usecase for filters - dead simple censorship purely for
educational purposes.

*In your plugin:*
```php
class Plugin extends PluginBase
{
	public function boot()
	{
		Filter::register('note.save', 'docs_sample_plugin_censorship', function ($note) {
            $note->content = str_replace('badword', '***', $value);

            return $note;
        });
	}
}
```

Registering filters is very similiar to registering actions. The only difference is that
first parameter to your callable will always be a value you can modify.

### Public API reference
#### *bool* `Filter::register`(*string* `$hookName`, *string* `$filterName`, *callable* `$callable`, *int* `$priority` = `10`)
Registers new filter for a specified hook.

**`$hookName`**  
String identyfing value filter is registered for.

**`$filterName`**  
Identifier for a registered callable. **Must** be unique within all registered filters.

**`$callable`**  
Valid PHP callable - anonymous function, reference to class method (dynamic or static) or a function.
See [PHP documentation on callable](http://php.net/manual/en/language.types.callable.php) for more details.
It must return a value.

**`$priority`**  
Numeric priority determining order of executing filters registered for a given hook. 
Filters registered with same priority have no defined order of being called.

#### *bool* `Filter::deregister`(*string* `$hookName`, *string* `$filterName`)
Unsets given filter from a specified hook. Deregistration must happen before that
hook is being called. Built-in filters can also be deregistered.

**`$hookName`**  
String identyfing value filter is registered for.

**`$filterName`**  
Identifier for a callable to unregister (dequeue).

#### *mixed* `Filter::call`(*string* `$hookName`, *mixed* `$value`, *array* `$parameters` = `[]`)
Returns `$value` after being processed by all registered callables.

**`$hookName`**  
Unique name identyfing a filter.

**`$parameters`**  
Optional array of parameters - if specified, an array of parameters will be
passed to user-defined callable next to the filtered value.

#### *bool* `Filter::isRegistered`(*string* `$hookName`, *string* `$filterName`)
Checks whether specified filter is registered within a given hook.

**`$hookName`**  
String identyfing value filter is registered for.

**`$filterName`**  
Unique name identyfing registered filter.
