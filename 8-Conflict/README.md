# Managing Conflicts

https://getcomposer.org/doc/04-schema.md#conflict

You may find yourself running into a conflict or two. In general, Composer will manage versions based 
on what you provide in the `composer.json` file and what your dependencies provide in their `composer.json` files. 
That is why specifying ranges is better than exact versions; especially, when more than one of your dependencies 
depends on the same project/library.

Sometimes, however, you need to ensure Composer does not retrieve a specific version of a dependency. Composer 
provides a field for this called `conflict`.

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
        "drupal/drupal": "~8.5.3"
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
    ]
}

```

Add a `conflict` section to the `composer.json` file we've been using in the previous steps and run 
`composer install`. You should get the following error.

```
Loading composer repositories with package information
Updating dependencies (including require-dev)
Your requirements could not be resolved to an installable set of packages.

Problem 1
- drupal/drupal 8.5.3 conflicts with david-hernandez/init[No version set (parsed as 1.0.0)].
- drupal/drupal 8.5.x-dev conflicts with david-hernandez/init[No version set (parsed as 1.0.0)].
- Installation request for david-hernandez/init No version set (parsed as 1.0.0) -> satisfiable by david-hernandez/init[No version set (parsed as 1.0.0)].
- Installation request for drupal/drupal ~8.5.3 -> satisfiable by drupal/drupal[8.5.3, 8.5.x-dev].
```

Composer was not able to setup the project at all, because it respected the conflict we provided. The 
require section tells Composer to retrieve the latest version of Drupal 8.5, but our conflict, which used 
a wildcard (*), blocks the use of any version of Drupal.

Change the Drupal version required, like this:

```$xslt
"require": {
    "drupal/drupal": "~8.5.0"
},
```

and change the conflict to this:

```$xslt
"conflict": {
    "drupal/drupal": "8.5.3"
},
```

This requirement can be satisfied by any version of 8.5 (8.5.0 and up,) and the conflict tells Composer 
8.5.3 is unacceptable. After the change try `composer install` again.

```$xslt
$ composer install
Loading composer repositories with package information
Updating dependencies (including require-dev)
...
Writing lock file
Generating autoload files
Loading composer repositories with package information
Updating dependencies (including require-dev)
Nothing to install or update
Generating autoload files
```

Checking the `composer.lock` file we see Composer retrieved 8.5.2.

```$xslt
"name": "drupal/drupal",
"version": "8.5.2",
```

