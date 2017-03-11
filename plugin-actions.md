# Actions

*Actions* API is designed for simple event-driven interaction with Codice for
plugin developers. Basic idea of actions is dead simple - Codice will call
different actions (identified by *hooks*) in many areas of its core codebase.
You, as a plugin developer, are able to *hook into* them and execute your own
code when specific action happens. Don't worry if it's not clear yet - look at
the example. 

*Somewhere in Codice code:*
```php
class NoteController extends Controller
{
	...
	public function getChangeStatus()
	{
		// Controller's action for changing note status (on GET request)
		
		...
		
		$note->save();
		
		Action::call('note.updateStatus', [
			'newStatus' => $newStatus,
			'note' => $note,
		]);
	}
}
```

In this place in the core, Codice will *call* all actions identified with a hook named
`note.updateStatus` - additionally it will pass new status and whole Note object as
a parameters for all actions registered within this hook.

Let's have a look at the simple action you could register for it. You would
place this code somewhere in your plugin, probably `boot()` method of it.

<div class="alert alert-info">
This example is unrelated to the <em>Upcoming tasks</em> plugin we are creating.
</div>

*In your plugin:*
```php
class Plugin extends PluginBase
{
	public function boot()
	{
		Action::register('note.create', 'docs_sample_plugin_log_note_creation', function ($parameters) {
			if ($parameters['newStatus'] === 1) {
				$string = 'done';
			} else {
				$string = 'undone';
			}
			
			Log::info("Note {$parameters['note']->id} is $string");
		});
	}
}
```

New actions are registered using `Action::register()`. At least three parameters are
required being: hook name, action name and actual callable to run. Detailed description
of each parameter is placed below.

<div class="alert alert-info">
Not only plugins utilize Actions API. Some of the core features also use it - for example,
adding welcome note is nothing more than action registered for <code>user.create</code> hook.
Don't be afraid to unregister it inside a plugin - either disabling functionallity completely
or hooking own implementation instead.
</div>

### Public API reference
#### *bool* `Action::register`(*string* `$hookName`, *string* `$actionName`, *callable* `$callable`, *int* `$priority` = `10`)
Registers new action for a specified hook.

**`$hookName`**  
String identyfing event action is hooked into.

**`$actionName`**  
Identifier for a registered callable. **Must** be unique within all registered actions.

**`$callable`**  
Valid PHP callable - anonymous function, reference to class method (dynamic or static) or a function.
See [PHP documentation on callable](http://php.net/manual/en/language.types.callable.php) for more details.

**`$priority`**  
Numeric priority determining order of executing actions registered for a given hook. 
Actions registered with same priority have no defined order of being called.

#### *bool* `Action::deregister`(*string* `$hookName`, *string* `$actionName`)
Unsets given action from a specified hook. Deregistration must happen before that
hook is being called. Built-in actions can also be deregistered.

**`$hookName`**  
String identyfing event action is hooked into.

**`$actionName`**  
Identifier for a callable to unregister (dequeue).

#### *bool* `Action::call`(*string* `$hookName`, *array* `$parameters` = `[]`)
Fires all user-defined callables registered for a given action.

**`$hookName`**  
Unique name identyfing an action.

**`$parameters`**  
Optional array of parameters - if specified, an array of parameters will be
passed to user-defined callable. Look at the first example in this chapter
for real-life usage of action parameters.

#### *bool* `Action::isRegistered`(*string* `$hookName`, *string* `$actionName`)
Checks whether specified action is registered within a given hook.

**`$hookName`**  
Unique name identyfing an action.

**`$actionName`**  
Unique name identyfing registered action.
