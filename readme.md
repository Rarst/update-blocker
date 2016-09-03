# Update Blocker — for WP repositories

Update Blocker is a lightweight generic blocker of plugin, theme, and core updates from official WordPress repositories.

It was created as shared reusable plugin for the sake of no longer reinventing that particular wheel.

### Goals

 - single main file
 - `mu-plugins`–friendly
 - no hard dependencies

### Not goals

 - interface
 - elaborate API
 - as–library use
 - unreasonable compat

## Installation

### Plugin

1. [Download ZIP](https://github.com/Rarst/update-blocker/archive/master.zip).
2. Unpack files from inside into `wp-content/plugins/update-blocker`.

### MU-plugin

1. [Download `update-blocker.php`](https://raw.githubusercontent.com/Rarst/update-blocker/master/update-blocker.php).
2. Place into `wp-content/mu-plugins`.
3. Edit settings for blocks in the file.

### Composer

Create project in the `wp-content/plugins`:

```
composer create-project rarst/update-blocker:~1.0
```

Or require in `composer.json` of site project:

```json
{
	"require": {
		"rarst/update-blocker": "~1.0"
	}
}
```

Requiring on plugin/theme level is not implemented, use `suggest`:

```json
{
	"suggest": {
		"rarst/update-blocker": "Prevents invalid updates from official repositories"
	}
}
```

## Configuration

Plugin's settings have following structure:

```php
array(
	'all'     => false,
	'files'   => array( '.git', '.svn', '.hg' ),
	'plugins' => array( 'update-blocker/update-blocker.php' ),
	'themes'  => array(),
	'core'    => false,
)
```

 - `all` — boolean, disables updates completely
 - `files` — array of plugin/theme root–relative files to detect for block
 - `plugins` — array of plugin base names (`folder-name/plugin-name.php`) to block
 - `themes` — array of theme slugs (`theme-name`) to block
 - `core` — boolean, disables core updates

Settings pass through `update_blocker_blocked` filter.

Processed data passes through `update_blocker_plugins` and `update_blocker_themes` filters during update checks.

### Plugin opt–in

```php
add_filter( 'update_blocker_blocked', function( $blocked ) {
	$blocked['plugins'][] = plugin_basename( __FILE__ ); // or just folder-name/plugin-name.php string
	return $blocked;
} );
```

### Theme opt–in

```php
add_filter( 'update_blocker_blocked', function( $blocked ) {
	$blocked['themes'][] = 'theme-name';
	return $blocked;
} );
```

### Core opt–in

```php
add_filter( 'update_blocker_blocked', function ( $blocked ) {
	$blocked['core'] = true;
	return $blocked;
} );
```

## License

 - MIT