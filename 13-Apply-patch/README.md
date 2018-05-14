# Apply patches file with Composer

We use the module color field to demonstrate how to patch a module.

### Install module.

Install module with composer:
```
composer require drupal/color_field
```

### Patching the module.

The color field module works pretty well, but there is an issue related to spectrum widget, this issue was reported and fixed in https://www.drupal.org/node/2882634. But this solution wasn't included in the release of color field module.

To patch a package is need to edit put composer.json file to provide the patch instructions as you can see in the following snippet of code.
```
"extra": {
    "installer-paths": {
        "docroot/core": ["type:drupal-core"],
        "docroot/libraries/{$name}": ["type:drupal-library"],
        "docroot/modules/contrib/{$name}": ["type:drupal-module"],
        "docroot/profiles/contrib/{$name}": ["type:drupal-profile"],
        "docroot/themes/contrib/{$name}": ["type:drupal-theme"],
        "drush/contrib/{$name}": ["type:drupal-drush"]
    },
    "patches": {
        "drupal/color_field": {
            "#2882634: Spectrum: palette values is duplicate": "https://www.drupal.org/files/issues/color-spectrum-widget-duplicate-2882634-4.patch"
        }
    }
}
```

### Applying your patches.
The next time do you run `composer install` or `composer update`
