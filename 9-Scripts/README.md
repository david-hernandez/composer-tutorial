# Utilizing Scripts

https://getcomposer.org/doc/articles/scripts.md

Composer has the ability to run scripts, and has a collection of triggering events to run those scripts. 
Check the documentation for a full list of these events.

To get Composer to build our Drupal 8 project, we'll take advantage of this functionality.

Luckily, for Drupal 8, there exists a public scaffolding package that can handle all of the setup tasks 
for us.

https://github.com/drupal-composer/drupal-scaffold

To use it, require the `drupal-composer/drupal-scaffold` package.

```$xslt
"require": {
    ...
    "drupal-composer/drupal-scaffold": "^2.4"
},
```

Then, add a `scripts` section to the `composer.json` file.

```$xslt
"scripts": {
    "drupal-scaffold": "DrupalComposer\\DrupalScaffold\\Plugin::scaffold"
}
```

The Drupal scaffold project defines a plugin which adds its own event to Composer's list of events.

If you read the help for Drupal scaffold, you'll see that it does not want you to use the 
`drupal/drupal` package. It is instead designed to work with `drupal/core`. This package is slightly 
different. It contains just the `core` directory of Drupal. For those that don't know anything about 
Drupal, this is a directory inside the root Drupal project directory. Other things, like `index.php` and 
the other files and directories outside of the `core` directory will get setup by the scaffolder.

The json file now looks like this.

```$xslt
{
    "name": "david-hernandez/init",
    "description": "Using the init command to create a new project.",
    "type": "project",
    "authors": [
        {
            "name": "David Hernandez",
            "email": "david@example.com"
        }
    ],
    "minimum-stability": "dev",
    "prefer-stable": true,
    "require": {
        "drupal/core": "~8.5.0",
        "drupal-composer/drupal-scaffold": "^2.4"
    },
    "require-dev": {
        "drupal/console": "^1.0.0"
    },
    "conflict": {
        "drupal/drupal": "*"
    },
    "repositories": [
        {
            "type": "composer",
            "url": "https://packages.drupal.org/8"
        }
    ],
    "scripts": {
        "drupal-scaffold": "DrupalComposer\\DrupalScaffold\\Plugin::scaffold"
    }
}

```

The conflict from the previous step has been changed to a wildcard (*) since we don't want to ever use 
the `drupal/drupal` package now. We only want to use `drupal/core`, so this will prevent Composer from 
even trying to retrieve it.

With this new setup, if you run `composer install` you'll see Composer will no longer put Drupal in the 
vendor directory. Instead, the scaffolder tries to set it up in the root of the project directory. But, 
it's still a bit messy. The scaffolder is treating the project root as the document root of our website. 
This can work, but we ultimately want something different. We want to define an actual document root, and 
contain our files inside it. To do that, we need to add some extras.