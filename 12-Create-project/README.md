# Using the `create-project` Command

https://getcomposer.org/doc/03-cli.md#create-project

Composer has a command for copying another project. Essentially, using it as a template. It will retrieve the project 
and run install using its `composer.json` file.

```
composer create-project
```

The main caveat with `create-project` is it has to be used with an empty directory. Either run it inside
 an empty directory or supply a new directory name. Composer will create the new project directory.
 
### Drupal Composer

https://github.com/drupal-composer/drupal-project

Drupal Composer has a recommended project for starting Drupal 8 projects that are managed with Composer. 
Much of the information in the previous steps of this tutorial is based on the setup of the Drupal Composer project.

If you run the following in an empty directory it will clone the Drupal Composer project, run the install, and fully setup a 
Drupal 8 project (using a directory called `web`, instead of `docroot`.)

```
composer create-project drupal-composer/drupal-project:8.x-dev . --stability dev --no-interaction
```

Let's break down the command.

```
composer create-project
```

The create project command.

```
drupal-composer/drupal-project
```

The vendor and project name.

```
:8.x-dev
```

Retrieve the 8.x-dev version of Drupal Composer project. This is not the Drupal version. If you look at the 
`composer.json` file that comes with the project, you will see the version of Drupal it requires.

```
.
```

Following the command, provide the directory name where the project is created. By adding a period, this specifies the 
current directory. Remember, the directory must be empty. If you want to put it in a new directory, replace the period 
with the directory name.

```
--stability dev
```

Many of the same options used with `init` can be used with `create-project`. This option sets the stability level 
to `dev`.

```
--no-interaction
```

`no-interaction` will prevent prompts that ask the user questions. All defaults will be used.

### Can't I just copy the `composer.json` file instead?

No. There is a major difference between duplicating a projects `composer.json` file and duplicating the whole 
project. The project may contain additional files, like scripts. If you look at the Drupal Composer project, you will 
see it has its own script that performs additional setup tasks, as well as custom drush commands. You could, however, 
git clone the project directly from Github.