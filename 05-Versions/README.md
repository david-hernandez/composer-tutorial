# Working with Package Versions

https://getcomposer.org/doc/articles/versions.md

When working with Composer, you won't generally specify exact versions. 
Instead, you'll use version constraints, like wildcards. This is important 
when dealing with updates to packages, and the part of Composer that causes 
the most confusion.

**The most important thing to note is that Composer does a lot of work to 
figure out what package to retrieve. That doesn't always result in the newest version.**

#### Exact version

```$xslt
"drupal/drupal": "8.5.1"
```

This is an exact version. If you specify a dependency this way, Composer will 
retrieve this exact version, and never update it. It will always use `8.5.1`.

#### Wildcard

```$xslt
"drupal/drupal": "8.5.*"
```

This will retrieve any version of 8.5 that meets Composer's requirements. 
`8.5.0`, `8.5.1`, `8.5.2`, etc, but not `8.6.x`.

#### Range

```$xslt
"drupal/drupal": ">=8.5 <8.6"
```

This is equivalent to the wildcard above. `8.5.0` and up, but not `8.6.x`.

#### Tilde

```$xslt
"drupal/drupal": "~8.5.0"
```

The tilde character will tell Composer to only increment the last digit when 
retrieving the package or updating. If `8.5.0` is the only one that works, that 
is what you get. If `8.5.9` is available and satisfies dependencies, you'll get 
that one. But, never `8.6.x`.

```$xslt
"drupal/drupal": "~8.5"
```

Leaving off the third digit reduces the number of significant digits, so 
Composer can increment the 5. It can retrieve any version of `8.5.x`, `8.6.x`, 
`8.7.x`, etc, but not `9.x`.

#### Caret

```$xslt
"drupal/drupal": "^8.5.0"
```

The caret is less restrictive than the tilde. It can increment the 0 and 
the 5, but not the 8. So, `8.5.0`, `8.5.9`, `8.6.2`, `8.9.5`, etc, but not `9.x`.

#### Branches

```$xslt
"drupal/drupal": "8.6.x-dev"
```

These versions ultimately trace back to software releases in version control 
systems. Most of the public projects are coming from Git repos on github.com. You 
may want to retrieve a specific branch, like the current development branch. This 
is why our Drupal example in the previous steps has `-dev` attached. This, 
as written, will retrieve Drupal's 8.6 development branch.

https://packagist.org/packages/drupal/drupal#8.6.x-dev

## Using version constraints

Let's update the requirement sections of the `composer.json` file to now use 
version constrains. Remember, editing the file is no different than using the 
`require` command.

```$xslt
"require": {
    "drupal/drupal": "~8.5.3"
},
"require-dev": {
    "drupal/console": "^1.0.0"
}
```

This `composer.json` file is in a new directory, with no vendor directory. 
We are treating this like a new project, with a prebuilt json file. So, after 
changing the json file, run `composer install`. (Just use the `composer.json` 
file included in this directory of the tutorial.)

```$xslt
$ composer install
1/11:	http://packagist.org/p/provider-archived$afa373e95aa9fa7678612f8c7a961c6373160760005c1f8f41450f2860d933f9.json
2/11:	http://packagist.org/p/provider-2013$e8a58721d48f09fca4bf257fac28558dc991cef7b48c19788e27bff3a99281d4.json
...
Finished: success: 11, skipped: 0, failure: 0, total: 11
Loading composer repositories with package information
Updating dependencies (including require-dev)
1/50:	https://codeload.github.com/dflydev/dflydev-placeholder-resolver/legacy.zip/c498d0cae91b1bb36cc7d60906dab8e62bb7c356
2/50:	https://codeload.github.com/alchemy-fr/Zippy/legacy.zip/5ffdc93de0af2770d396bf433d8b2667c77277ea
...
50/50:	https://codeload.github.com/drupal/drupal/legacy.zip/be1f0d7d06c9bc744f9b54f2aad25297ba78eb2d
Finished: success: 50, skipped: 0, failure: 0, total: 50
Package operations: 50 installs, 0 updates, 0 removals
- Installing wikimedia/composer-merge-plugin (v1.4.1): Loading from cache
- Installing composer/installers (v1.5.0): Loading from cache
...
Writing lock file
Generating autoload files
Loading composer repositories with package information
Updating dependencies (including require-dev)
Nothing to install or update
Generating autoload files
```

What was the result? If we look at `composer.lock`, we see the following:

```$xslt
"name": "drupal/drupal",
"version": "8.5.x-dev",
...
"name": "drupal/console",
"version": "1.8.0",
```

Composer retrieved the latest of Drupal `8.5.x`, which is the dev release, 
since we still have `"minimum-stability": "dev"` in our file. But, it did not 
go up to `8.6`. For Console, it retrieved `1.8.0`, which is the most current 
1.x release as of this writing. It blew past `1.0.0`, and `1.0.1`, and 
`1.1.0` and `1.2.5`, etc.

This is why the lock file is so important. We need to know the exact result, because 
we are often dealing with version ranges.