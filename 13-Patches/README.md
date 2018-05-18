# Applying Patches with Composer

https://github.com/cweagans/composer-patches

It is an often occurrence when dealing software that you need to apply a patch to a package you require.

Composer doesn't provide much functionality for this out of the box, but there is a commonly used tool 
for this, created by Cameron Eagans (linked above.) It lets you specify patch files for each dependency, 
including urls to download the patch files. The patch files do not need to be included with your code base.

Using the `create-project` command from step 12 we'll create a new Drupal 8 based project and see how to apply a patch. 

```$xslt
composer create-project drupal-composer/drupal-project:8.x-dev . --stability dev --no-interaction

```

**Note that the `composer.json` file in this directory is the result of the above command. It will look different than the 
files used in other steps in this tutorial. But, the steps below you can apply to your own `composer.json` file. We just 
need a starting point.**

For this example, we will add the Color Field module as a dependency. (https://www.drupal.org/project/color_field)

```
composer require drupal/color_field:2.0-rc2
```

I am specifying the exact version (2.0-rc2,) because I know the patch supplied below applies to this specific version 
of the module.

A particular bug with this module was reported and worked on here - https://www.drupal.org/node/2882634. Our task will 
be to instruct Composer to retrieve and apply one of the patches proposed in that Drupal.org issue.

### Patching the module.

First, we'll need to require the Composer Patches package. (You could also just make this a dev dependency.)

```$xslt
composer require cweagans/composer-patches
```

The `composer.json` file should now list both new requirements.

```$xslt
"require": {
    "composer/installers": "^1.2",
 => "cweagans/composer-patches": "^1.6",
    "drupal-composer/drupal-scaffold": "^2.2",
 => "drupal/color_field": "2.0-rc2",
    ...
},
```

To supply a patch file, add a new section to the `composer.json` file. Patches will go within the `extra` section, because 
this is extra information being supplied to the scripts Composer will run. We don't need to define the scripts. They are part 
of Composer Patches.

```
"extra": {
    ...
    "patches": {
        "drupal/color_field": {
            "Color palette values get duplicated": "https://www.drupal.org/files/issues/color-spectrum-widget-duplicate-2882634-4.patch"
        }
    }
}
```

Let's break this down a little bit.

```$xslt
 "drupal/color_field": {

```
Inside the `patches` array, add an array for each dependency you want to patch. Then, line by line, specify your patch 
files for that dependency. You can have more than one patch for each dependency.

```$xslt
"Color palette values get duplicated":
```

Add a text label for the patch. Make it something easy to identify so someone else looking at this file knows why the 
patch is there.

```$xslt
https://www.drupal.org/files/issues/color-spectrum-widget-duplicate-2882634-4.patch
```
Add the url to the patch file. You can supply a local file system path to a file, so if you are supplying your own patch file, 
provide the path to it here.

### Applying your patches.

When you run `composer update`, or when someone runs `composer install` to setup your project, you should see Composer apply the patch.

```$xslt
Gathering patches for root package.
Gathering patches for dependencies. This might take a minute.
  - Installing drupal/color_field (2.0.0-rc2): Loading from cache
  - Applying patches for drupal/color_field
    https://www.drupal.org/files/issues/color-spectrum-widget-duplicate-2882634-4.patch (Color palette values get duplicated)
```
