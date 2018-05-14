# Updating Dependencies

https://getcomposer.org/doc/03-cli.md#update

If you followed the previous steps in this tutorial, you will have a `composer.json` file capable of 
building a fully-functional Drupal 8 website. But how will you handle future updates?

Composer comes with an update command.

```
composer update
```

Let's start with the `composer.json` file from the previous step, and remove the tilde from the requirement 
for `drupal/core`.

```
"require": {
    "drupal/core": "8.5.0"
}
```

When running `composer install` this will setup the project with Drupal 8.5.0. Verify this in the lock file.

```
"name": "drupal/core",
"version": "8.5.0",
```

Return to the `composer.json` file and put the tilde back.

```
"require": {
    "drupal/core": "~8.5.0"
}
```

Using the update command we can get Composer to retreive the newer version.

```
$ composer update
 ...
Loading composer repositories with package information
Updating dependencies (including require-dev)
    1/1:	https://codeload.github.com/drupal/core/legacy.zip/b012f0ae51504880e920f2c6efdbdf03b6fe2129
    Finished: success: 1, skipped: 0, failure: 0, total: 1
Package operations: 0 installs, 1 update, 0 removals
  - Updating drupal/core (8.5.0 => 8.5.3): Loading from cache
Writing lock file
Generating autoload files

```

Verfiy in the lock file.

```
"name": "drupal/core",
"version": "8.5.3",
```

### Specifiying what to update

You don't have to update everything with one command. You can pick and chose packages using the 
same syntax as the `require` command.

One package:

```
composer update drupal/core
```

Multiple packages:

```
composer update drupal/core drupal/devel
```

**It is highly recommended you read through the documentation for the update command (linked above.) 
There are lots of options, and updating is a big part of the difficulty people have using Composer. Just 
remember that Composer is constantly trying to make sense of not just the packages you ask for, but also all 
the various dependencies those packages have.**