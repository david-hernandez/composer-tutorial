# Using the Init Command

https://getcomposer.org/doc/03-cli.md#init

Everything Composer does it based on a `composer.json` file sitting in the root 
of your project. You can create this file yourself if you want, but the `init` 
command can get you started. It will walk you through creating basic parts of the 
file, like the description and initial requirements.

```$xslt
$ composer init
                                          
Welcome to the Composer config generator  
                                            
This command will guide you through creating your composer.json config.
```

This generator will prompt you for basic information to create your initial 
`composer.json` file.

```
Package name (<vendor>/<name>) [root/www]: david-hernandez/init
```

I used my Github username and a made up project name. This isn't required for
your local project purposes, but is if you want to make your project public and
create an account on Packagist.org.
        
```
Description []: Using the init command to create a new project.
```

Add a helpful description for the project.

```
Author [, n to skip]: David Hernandez <david@example.com>
```

The author isn't required, but helpful. It provides contact information.     

```
Minimum Stability []: dev
```

Minimum stability dictates what versions of packages you consider acceptable. 
Setting to `dev` means it is ok to download development versions of packages. 
Whether a version is considered dev, alpha, stable, etc, is dictated by the package 
Composer retrieves, not you.

https://getcomposer.org/doc/04-schema.md#minimum-stability

```
Package Type (e.g. library, project, metapackage, composer-plugin) []: project
```

Set the package type. For a website or application, `project` is likely the type.

```
License []:
```

If you want to define the open source license your project will use, like GPL, 
put it here. This is optional. 

```Define your dependencies.

Would you like to define your dependencies (require) interactively [yes]? no
```

The generator lets you define the packages your projects requires at this step. These 
can be added later, so it is not necessary to do it now.

```
Would you like to define your dev dependencies (require-dev) interactively [yes]? no
```

Development dependencies are grouped separately. We'll get to this later.


```
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
    "require": {}
}
```

When the generator is done, it will display what the resultant `composer.json` 
file will look like.

```
Do you confirm generation [yes]? yes
```

The file will not be written unless you say `yes` to the 
confirmation prompt.

