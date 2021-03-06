# Composer Preload

Preload your sweet sweet code to opcache with a composer command, making your code faster to run

Composer Preload is a composer plugin aiming to provide and complement PHP opcache warming.
This plugin introduces a new `composer preload` command that can generate a `vendor/preload.php` file (following `vendor/autoload.php` pattern) that contains calls to warm up the opcache cache.

Please note that this plugin is currently in a very rudimentary state, and it is highly recommend to not use this in any production system.

# How it works

At the moment, this plugin scans for `.php` files in the given paths recursively, and create a file that calls `opcache_compile_file` function.
When you want to warm up the cache, you can either call `php vendor/preload.php` in command line, or when PHP 7.4 hits the shelves, configure PHP to automatically load this file.

# Installation

Just the way you'd install a normal composer package, you can install this plugin aswell:
```
composer require ayesh/composer-preload
```
If you'd rather install this globally:
```
composer g require ayesh/composer-preload
```

# Configuration

Before you can run the `composer preload` command, you **must** define the directories to preload.

Modify your `composer.json` file, and create a section called `extra` if it's not there already.
```
{
  "extra": {
	"preload": {
	    "paths": [
		    "src"
	    ],
        "exclude": [
          "src/tests"
        ]
        "no-status-check": false,
    }
  }
}
```
The `extra.preload` directive contains all the configuration options for this plugin. The `paths` directive must be an array of directories relative to the `composer.json` file. These directories will be scanned recursively for `.php` files, converted to absolute paths, and appended to the `vendor/preload.php` file.

## Configuration options

### `extra.preload.paths` : _Required_

An array of directory paths to look for `.php` files in. This setting
is required as of now. The directories must exist at the time
`composer preload` command is run.

### `extra.preload.exclude` : _Optional_

An array of directory paths to exclude from the `preload.php`. This list
must be relative to the composer.json file, similar to the `paths`
directive. The ideal use case limiting the scope of the `paths` directive.

### `extra.preload.no-status-check`: _Optional_, Default: _`false`_

If this setting is set to `true` (you can also pass command line option
`--no-status-check`), make the generated `preload.php` file not contain
additional checks to make sure the opcache is enabled. This setting is
disabled by default, and the generated `preload.php` file will contain
a small snippet on the top that makes it quit if opcache is not enabled.

# Roadmap

 - ☐ Extend `extras.preload` section to configure the packages that should be preloaded instead of setting the individual paths.
 - ✓ Feature to set an exclude pattern (v0.0.3)
 - ☐ Progress bar to show the file generation progress
 - ☐ Flag to generate the file _and_ run it, so the cache is immediately warmed up.
 - ☐ Fancier progress bar.
 - ☐ Full test coverage.
 - ☐ Even more fancier progress bar with opcache memory usage display, etc.
 - ☐ Get many Github stars
