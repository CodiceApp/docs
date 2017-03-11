# Writing a plugin

This section of documentation will introduce to you into writing your first simple plugin,
explain its structure and conventions related to this topic.

For the learning purposes we will follow the path of creating simple Codice plugin,
which will add custom subpage displaying tasks to be done in upcoming days.

<div class="alert alert-info">
It is assumed that you have at least basic skill in Laravel. Codice, while providing
custom Plugin API, is still built upon it and in many places writing a plugin just
boils down to the regular Laravel coding.
</div>

### Plugin identifier
Each plugin has its unique ID based on the name of directory it's located in. However,
your plugin folder must live directly in `/plugins` - more levels of nesting are not
allowed.

This is why you should prefix your directory name with so called *vendor prefix*, let
it be your nickname, organisation name etc. You can choose whatever you like and treat
that just like top vendor namespace in case of PHP (e.g. Composer) packages.

<div class="alert alert-danger">
<code>codice-</code> prefix is restricted for plugins released by Codice team itself.
Please stay away from using it.
</div>

Our training plugin will be named `docs-upcoming` (where `docs` serves as vendor prefix),
filesystem path of it will be `/plugins/docs-upcoming` then.

### `plugin.json`
First mandatory element each plugin must provide is `plugin.json` file in its directory.
It holds plugin metadata such as name, version, author name and website. It can also
define plugin-specific system requirements.

Let's look how such file might look for our plugin. Place this contents in
`/plugins/docs-upcoming/plugin.json` file.

```json
{
  "name": "Upcoming tasks",
  "description": "Provides additional page with tasks for upcoming days",
  "author": "You",
  "homepage": "http://codice.eu/docs",
  "version": "1.0.0",
  "require": {
    "codice": "0.5.*"
  }
}
```

This is almost minimal setup, except for the `require` part which, in this case,
ensures plugin can only be installed only on Codice 0.5 (and any of its patch
releases).

If it resembles you of `composer.json` syntax, it's totally fine. It is done on
purpose and version constraints syntax is the same as well, utilizing Composer's
[semver][semver] compontent.

Apart from the core you can also define requirements for PHP version, PHP extensions,
and other Codice plugins.

```json
{
  [...]

  "require": {
    "php": ">=7.0",
    "codice": "0.5.*",
    "ext-simplexml": "*",
    "plugin-johndoe-core": "^2.0"
  }
```

Just like in `composer.json`, it is recommended to only specify `"*"` for the PHP
extensions. Their versioning is inconsistent and it's not safe to rely upon it.

Now, when we have explained all conventions and configuration, let's get to some
[real coding](plugin-class)!


[semver]: http://semver.org
