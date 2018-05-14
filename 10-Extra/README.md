# Extras

https://getcomposer.org/doc/04-schema.md#extra

Composer supports adding an `extra` section for supplying arbitrary information scrips might need. We'll 
use this to supply information that the Drupal scaffolder will use.

When we first setup a project, one of the questions asked us for a `type`. The answer we gave was `project`, since 
that is what most projects use. Drupal has a list of its own types, which comes in handy when using Composer. 
If you look at the `composer.json` in the `core` directory or any of the module directories from the 
previous steps, you'll see their types declared.

Drupal 8 declares itself as `drupal-core`, modules declare themselves as `drupal-module`, themes as `drupal-theme`, 
and so forth. Using this information, we can tell the scaffolder where to put these files when retrieving 
core, modules, theme, and everything else. This goes for the original setup and when later requiring new dependencies.

```$xslt
"extra": {
    "installer-paths": {
        "docroot/core": ["type:drupal-core"],
        "docroot/libraries/{$name}": ["type:drupal-library"],
        "docroot/modules/contrib/{$name}": ["type:drupal-module"],
        "docroot/profiles/contrib/{$name}": ["type:drupal-profile"],
        "docroot/themes/contrib/{$name}": ["type:drupal-theme"],
        "drush/contrib/{$name}": ["type:drupal-drush"]
    }
  }
```

As you can see, the scaffolder will now put the Drupal-related things in a `docroot` directory. (You can define 
any path here you want.) Drush, is a command-line tool, so we want to leave it outside the document root (which will 
be publicly accessible.) The vendor directory, which defaults to the root of the project directory, will also be outside of the 
document root. But we don't define the vendor directory here, because Composer takes care of that itself.

Now, when running `composer install` we get a much more structured project.

```$xslt
$ ls -l
-rw-r--r--   1 davidhernandez  staff    1222 May 13 20:18 composer.json
-rw-r--r--   1 davidhernandez  staff  156593 May 13 20:20 composer.lock
drwxr-xr-x  16 davidhernandez  staff     544 May 13 20:20 docroot
drwxr-xr-x  29 davidhernandez  staff     986 May 13 20:20 vendor
```
Drupal 8 (core) is nicely built inside the docroot.

```$xslt
$ ls -l docroot/
-rw-r--r--   1 davidhernandez  staff  1025 May 13 20:20 .csslintrc
-rw-r--r--   1 davidhernandez  staff   357 May 13 20:20 .editorconfig
-rw-r--r--   1 davidhernandez  staff   151 May 13 20:20 .eslintignore
-rw-r--r--   1 davidhernandez  staff    41 May 13 20:20 .eslintrc.json
-rw-r--r--   1 davidhernandez  staff  3858 May 13 20:20 .gitattributes
-rw-r--r--   1 davidhernandez  staff  2306 May 13 20:20 .ht.router.php
-rw-r--r--   1 davidhernandez  staff  7866 May 13 20:20 .htaccess
-rw-rw-rw-   1 davidhernandez  staff   385 May 13 20:20 autoload.php
drwxr-xr-x  40 davidhernandez  staff  1360 May 13 20:20 core
-rw-r--r--   1 davidhernandez  staff   549 May 13 20:20 index.php
-rw-r--r--   1 davidhernandez  staff  1596 May 13 20:20 robots.txt
drwxr-xr-x   6 davidhernandez  staff   204 May 13 20:20 sites
-rw-r--r--   1 davidhernandez  staff   848 May 13 20:20 update.php
-rw-r--r--   1 davidhernandez  staff  4555 May 13 20:20 web.config
```

And, when we require a new Drupal module as a dependency, Composer will put it in the right place. 
Thanks to the scaffolder and our `extra` info.

```$xslt
$ composer require drupal/devel
 
Using version ^1.2 for drupal/devel
./composer.json has been updated
Loading composer repositories with package information
Updating dependencies (including require-dev)
    1/1:	https://ftp.drupal.org/files/projects/devel-8.x-1.2.zip
    Finished: success: 1, skipped: 0, failure: 0, total: 1
Package operations: 1 install, 0 updates, 0 removals
  - Installing drupal/devel (1.2.0): Loading from cache
Writing lock file
Generating autoload files
 
 
$ ls -l docroot/modules/contrib/
 
drwxr-xr-x  26 davidhernandez  staff   884B May 13 20:35 devel
```

**For Drupal folks, note that this DOES NOT install the module. It only downloads it and places it 
in the right directory. You still need to install the module in one of the usual Drupal ways, like 
through the web interface.**