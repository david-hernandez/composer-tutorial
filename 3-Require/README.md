# Requiring Dependencies

In order for composer to retrieve packages needed for your project, you have 
to declare them as dependencies. Don't think of the packages as something Composer 
installs. Remember that everything Composer does is about building and deploying 
an application. Instead of "installing packages," we "require dependencies."

Starting with the json file from the previous steps in this tutorial, we'll use 
the `require` command to dictate what packages the project requires.

The command uses this syntax.

```$xslt
composer require [vendor]/[package_name]:[version] [additional options]
```

To add Drupal 8 as a dependency run this command.

```$xslt
composer require drupal/drupal
```

The result:

```$xslt
Using version 8.6.x-dev for drupal/drupal
./composer.json has been updated
Loading composer repositories with package information
Updating dependencies (including require-dev)
Package operations: 3 installs, 0 updates, 0 removals
  - Installing wikimedia/composer-merge-plugin (v1.4.1): Downloading (100%)         
  - Installing composer/installers (v1.5.0): Loading from cache
  - Installing drupal/drupal (8.6.x-dev c2585a1): Cloning c2585a1888 from cache
Writing lock file
Generating autoload files
Loading composer repositories with package information
Updating dependencies (including require-dev)
Nothing to install or update
Generating autoload files
```

You can see Composer downloads a few things that it will stick in the `vendor` 
directory, including a copy of Drupal 8. Each package is organized into folders 
based on the vendor name. So Drupal ends up in a `drupal` folder.

```$xslt
$ ls -l vendor/
total 8
drwxr-xr-x   6 davidhernandez  staff  204 May 11 16:41 .
drwxr-xr-x   6 davidhernandez  staff  204 May 11 16:45 ..
-rw-r--r--   1 davidhernandez  staff  178 May 11 16:41 autoload.php
drwxr-xr-x  11 davidhernandez  staff  374 May 11 16:41 composer
drwxr-xr-x   3 davidhernandez  staff  102 May 11 16:40 drupal
drwxr-xr-x   3 davidhernandez  staff  102 May 11 16:40 wikimedia

```

Well, if you know anything about Drupal, you know this is no good. Drupal is a CMS 
and is the basis for our project. It needs to be outside of the vendor directory 
in a web or document root directory. We'll get to that later, and you can see why 
Drupal isn't a straightforward use-case, but let's look at the json file.

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
    "require": {
        "drupal/drupal": "8.6.x-dev"
    }
}
```

Composer has added a new section to the file. The `require` section. This is where dependencies 
get listed.

Composer goes and gets `drupal/drupal` using the public information from the account 
on Packagist.org.

https://packagist.org/packages/drupal/drupal

Since we did not specify the version to get, it grabs the most recent one. Furthermore, 
because we set `minimum-stability` to `dev` we told Composer it is ok to get the dev version.
Thus, it retrieved `8.6.x-dev` which is Drupal 8's development branch.

If we save this `composer.json` file and give it to someone else, when they run 
`composer install` they will get the same result. A vendor directory with Drupal in it, 
the Composer autoloader, and one or two other packages.

Lastly, you'll discover Composer did one more thing. It wrote a lock file.

Here is just a small section of that lock file. The part that concerns Drupal.
```$xslt
"name": "drupal/drupal",
"version": "8.6.x-dev",
"source": {
    "type": "git",
    "url": "https://github.com/drupal/drupal.git",
    "reference": "c2585a1888dba714a61b48dc619c88674084e118"
},
"dist": {
    "type": "zip",
    "url": "https://api.github.com/repos/drupal/drupal/zipball/c2585a1888dba714a61b48dc619c88674084e118",
    "reference": "c2585a1888dba714a61b48dc619c88674084e118",
    "shasum": ""
},
"require": {
    "composer/installers": "^1.0.24",
    "wikimedia/composer-merge-plugin": "^1.4"
},
"replace": {
    "drupal/core": "^8.6"
},
"type": "project",
```

Drupal, being a public project we retrieved though Packagist.org, also has a json 
file. Composer reads it and performs the tasks necessary to also satisfy Drupal's 
dependencies. That is why it also retrieved `composer/installers` and `wikimedia/composer-merge-plugin`. 
This is the whole point of using a package manager. We don't want to manage all 
the dependencies of the projects, libraries, etc, we use. That's a lot to keep track of 
and we're much better off letting Composer do it for us.

We can also see in the lock file the ultimate source of the code, version, reference hash, 
and other detailed information.

The lock file is critically important, because it shows the result of everything Composer 
did. And, when you move your project to another location (like after committing the 
`composer.json` and `composer.lock` file to a Git repo and sharing with a teammate,) 
Composer will replicate what it did in the lock file. It will **not** rely on the json file 
when you run `composer install`.

What is critically important here, is the lock file will specify the **exact** versions 
of the packages it retrieved. No ambiguous wildcards. We need to know exactly 
what software we have.