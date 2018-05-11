# Requiring Development Dependencies

Sometimes there are things you want to use in your project, but only when developing. 
Testing frameworks, debugging tools, etc. Composer has a section for this called `require-dev`.

You can add this section to your `composer.json` file or use the `require` command 
to include these development dependencies. Just add `--dev` to the end of the `require` 
command.

```$xslt
composer require drupal/console --dev
```

Drupal Console is a development tool specifically designed to work with Drupal. It 
is based on Symfony's Console project. It's handy, but something only needed when 
developing, not on a production server.

After running this, let's look at the result.

```$xslt
$ composer require drupal/console --dev
...
Using version dev-master for drupal/console
./composer.json has been updated
Loading composer repositories with package information
Updating dependencies (including require-dev)
1/50:	https://codeload.github.com/php-fig/http-message/legacy.zip/f6561bf28d520154e4b0ec72be95418abe6d9363
2/50:	https://codeload.github.com/alchemy-fr/Zippy/legacy.zip/5ffdc93de0af2770d396bf433d8b2667c77277ea
3/50:	https://codeload.github.com/composer/installers/legacy.zip/049797d727261bf27f2690430d935067710049c2
4/50:	https://codeload.github.com/ralouphie/getallheaders/legacy.zip/5601c8a83fbba7ef674a7369456d12f1e0d0eafa
5/50:	https://codeload.github.com/wikimedia/composer-merge-plugin/legacy.zip/81c6ac72a24a67383419c7eb9aa2b3437f2ab100
...
49/50:	https://codeload.github.com/symfony/filesystem/legacy.zip/d961178b56f0fdacd7d2d1dc13202b7cb36b4a51
50/50:	https://codeload.github.com/drupal/drupal/legacy.zip/c2585a1888dba714a61b48dc619c88674084e118
Finished: success: 50, skipped: 0, failure: 0, total: 50
Package operations: 50 installs, 0 updates, 0 removals
- Installing composer/installers (v1.5.0): Loading from cache
- Installing wikimedia/composer-merge-plugin (v1.4.1): Loading from cache
- Installing symfony/polyfill-ctype (dev-master 7cc359f): Cloning 7cc359f1b7 from cache
- Installing symfony/yaml (3.4.x-dev c5010cc): Cloning c5010cc169 from cache
- Installing symfony/finder (3.4.x-dev bd14efe): Cloning bd14efe8b1 from cache
...
- Installing guzzlehttp/guzzle (dev-master 0773d44): Cloning 0773d442aa from cache
- Installing drupal/console (dev-master f69efe5): Cloning f69efe5ccd from cache
- Installing drupal/drupal (8.6.x-dev c2585a1): Cloning c2585a1888 from cache
symfony/translation suggests installing psr/log-implementation (To use logging capability in translator)
symfony/event-dispatcher suggests installing symfony/http-kernel ()
symfony/dependency-injection suggests installing symfony/expression-language (For using expressions in service container configuration)
symfony/dependency-injection suggests installing symfony/proxy-manager-bridge (Generate service proxies to lazy load them)
symfony/console suggests installing symfony/lock ()
symfony/console suggests installing psr/log-implementation (For using the console logger)
paragonie/random_compat suggests installing ext-libsodium (Provides a modern crypto API that can be used to generate random bytes.)
psy/psysh suggests installing ext-pdo-sqlite (The doc command requires SQLite to work.)
psy/psysh suggests installing hoa/console (A pure PHP readline implementation. You'll want this if your PHP install doesn't already support readline or libedit.)
alchemy/zippy suggests installing guzzle/guzzle (To use the GuzzleTeleporter with Guzzle 3)
drupal/console suggests installing symfony/thanks (Thank your favorite PHP projects on Github using the CLI!)
drupal/console suggests installing vlucas/phpdotenv (Loads environment variables from .env to getenv(), $_ENV and $_SERVER automagically.)
Writing lock file
Generating autoload files
    1/1:	http://packagist.org/p/provider-latest$b3bf2022fdc8e1bbd261e1b0d965488d30cb006a736d01e951739fdfa4ce58c4.json
    Finished: success: 1, skipped: 0, failure: 0, total: 1
Loading composer repositories with package information
Installing dependencies (including require-dev) from lock file
Nothing to install or update
Generating autoload files
```

As you can see, Console requires a whole bunch of stuff, and that stuff requires 
a whole bunch of other stuff. This one package jumps the codebase up a notch in size.

Let's take a look at the `composer.json` file.

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
    },
    "require-dev": {
        "drupal/console": "dev-master"
    }
}
```

Not much has changed, but we do have the new `require-dev` section with 
Drupal Console in it. And, once again, it downloaded the dev branch of the 
package, because we didn't specify a version, and we set `minimum-stability` 
to `dev`.

Take a look at the lock file now. **It is huge!** We went from a three hundred 
line file to a three _thousand_ line file.

## Using `composer install` with Development Dependencies

Here is the gotcha you need to know. When you run `composer install` it 
will retrieve everything in the `require` section **and** the `require-dev` 
section. It defaults to assuming you are running these commands while developing 
your project.

When building for production, or any case when you don't want the development 
dependencies, run `composer install --no-dev`. This will ignore the `require-dev` 
section.

Also note, including `require-dev` when you install is a root-level operation only. 
Composer will not include the development dependencies of the packages it retrieves. 
It assumes I'm developing my project, not Drupal Console or any of the other packages.