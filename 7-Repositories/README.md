# Adding Repositories

https://getcomposer.org/doc/04-schema.md#repositories

The packages we've been requiring so far have all come from packagist.org. If 
you look through the Drupal account, you will see Drupal 8, Console, and a few 
other things. What you won't see are all the modules and themes you can download 
from drupal.org. That would be too much to add to Packagist, especially when all 
those individual projects are maintained by different people.

If you try to require a Drupal module using Composer, you'll see it won't work.

Try by attempting to require the Devel module, which is a Drupal module that 
provides handy development features, like Console does.

```$xslt
$ composer require drupal/devel --dev
                                                                                                                                                                                                               
  [InvalidArgumentException]                                                                                                                                                                                   
  Could not find a matching version of package drupal/devel. Check the package spelling, your version constraint and that the package is available in a stability which matches your minimum-stability (dev).
```

This is because Devel is not listed on packagist.org.

To solve this problem, Composer lets you add alternate repositories where packages 
can be retrieved. Github, Bitbucket, etc.

For Drupal, specifically, drupal.org provides a "façade" to its Git infrastructure. 
That's a fancy way of saying an interface to drupal.org that Composer can treat like 
packagist.org. We can add this to our json file with a new `repositories` section.

```$xslt
"repositories": [
    {
        "type": "composer",
        "url": "https://packages.drupal.org/8"
    }
]
```

_This façade was specifically setup for working with Drupal 8, because that is the first 
version of Drupal to start using Composer, hence the `/8` in the url. There is now a `/7` 
url for Drupal 7 based projects. Drupal modules also have a different version scheme than 
Composer expects, so there is some translation going on._

Now, when using the `require` command let's see what happens.

```$xslt
composer require drupal/devel --dev
...
Using version ^1.2 for drupal/devel
./composer.json has been updated
...
Writing lock file
Generating autoload files
1/1:	http://packagist.org/p/provider-latest$734553e1793e72c786c45ee909374c902541b4adc497992d1243a2d7c7e53704.json
Finished: success: 1, skipped: 0, failure: 0, total: 1
Loading composer repositories with package information
Updating dependencies (including require-dev)
Nothing to install or update
Generating autoload files
```

And, Devel has been added to the `composer.json` file.

```$xslt
"require-dev": {
    "drupal/console": "^1.0.0",
    "drupal/devel": "^1.2"
},
```

Two things to note here. One, Devel's version number is only two digits. That is 
because Drupal modules use a different versioning scheme. Its real version is somthing 
like 8.x-1.2. That's why it didn't add `1.2.0` or something similar. Two, Composer 
automatically added the caret (^). That is default behavior. If you don't want it 
to do that, specify your version and constraint when running the `require` command.

**Also, if you are playing along at home, you will see that Composer will build a vendor 
directory, and put Drupal, Symfony, Console, and all that other stuff in it, but it will 
stick Devel in a modules directory. Ignore all that. This is all kinds of broken until 
we can tell Composer how to properly setup a Drupal project.**