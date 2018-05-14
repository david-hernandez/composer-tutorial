# Defining Stability

https://getcomposer.org/doc/04-schema.md#minimum-stability

In the previous examples/steps of this tutorial we've been getting dev versions
of packages. This is because of the minimum stability question that was presented 
during `composer init`. It added the following to the `composer.json` file:

```$xslt
"minimum-stability": "dev"
```

This permits Composer to retrieve development releases. Most of the time you 
probably don't want to do that. If you leave the original question blank 
Composer won't add anything to the json file and will default to only 
retrieving `stable` versions. We can also set this by changing the value in 
the json file.

```$xslt
"minimum-stability": "stable"
```

The full list of options is: `dev`, `alpha`, `beta`, `RC`, and `stable`.

### But wait there's more...

Perhaps you sometimes want to use dev releases, but most of the time you 
want stable releases. There is a setting for that.

```$xslt
"prefer-stable": true
```

If you leave the minimum stability set to `dev`, that permits Composer to 
retrieve dev versions if it must, but `prefer-stable` tells it to avoid it 
if it can. As you can imagine, this can have unpredictable results.

After adding `prefer-stable` to the `composer.json` file and running `composer install` in this 
new directory, lets see the result in the lock file.

```$xslt
"name": "drupal/drupal",
"version": "8.5.3",
...
"name": "drupal/console",
"version": "1.8.0",
```

Drupal Console stayed the same as in the previous step, but now the development 
version of Drupal did not get retrieved. `~8.5.3` resulted in `8.5.3` since that 
is the latest stable release  of 8.5, as of this writing.

**You aren't limited to just these options. There are other fancy ways you can 
specify retrieving dev releases, like adding it to the `required` section, but 
this covers most use-cases. Check the Composer documentation linked at the top
for more details.**