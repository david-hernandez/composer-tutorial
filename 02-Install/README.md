# Using the Install Command

https://getcomposer.org/doc/03-cli.md#install

The `install` command sets up your project using all of the information 
in the `composer.json` file. This is the power of a package manager. If you 
move the config file to another directory or computer, you can replicate 
all of the setup tasks and dependencies for the project.

In this directory you will find a `composer.json` file that is the result 
of the steps taken in the first step of the tutorial.

```$xslt
composer install
```

Run this command from the same location as the `composer.json` file.

```$xslt
1/11:	http://packagist.org/p/provider-archived$afa373e95aa9fa7678612f8c7a961c6373160760005c1f8f41450f2860d933f9.json
2/11:	http://packagist.org/p/provider-latest$906c970475603cf8253151905fee2dbaad7f2a95e41fb7a4477fae2a0987d9b2.json
3/11:	http://packagist.org/p/provider-2018-04$312ab290f669dc52354b4211b61bda5377c496c9a641f7ec2ba4e0cc29f85984.json
4/11:	http://packagist.org/p/provider-2014$ac705a4d1c9d787f47c7d17837173ffc7df3b4494ea8ca5f23f9cbd52be80457.json
5/11:	http://packagist.org/p/provider-2017-07$37db856f75795d614325ebebc96133ada2848fc20df8d59d8044da590a4be728.json
6/11:	http://packagist.org/p/provider-2013$e8a58721d48f09fca4bf257fac28558dc991cef7b48c19788e27bff3a99281d4.json
7/11:	http://packagist.org/p/provider-2017-10$7ae8967e7d3971c313626809b38683c065775ad5d7020d1001895da729a7881a.json
8/11:	http://packagist.org/p/provider-2015$d8af00cb324d408f74e33f20ec721d08538d1ef8237ab97ff0ec99803c8d581d.json
9/11:	http://packagist.org/p/provider-2017$8ce5fb77f3af8fd8ccf18a1a6190b0ee9a80f958934dac688e5e673bea403ce2.json
10/11:	http://packagist.org/p/provider-2018-01$3fdaed0b8be86588b9fa5499aaf4a494d7376f4b554e91b239913baf4c55e8c7.json
11/11:	http://packagist.org/p/provider-2016$4fdf60a32e59900cc7b4a265f421d347d9c0ce0aee07b6a1567eed7f0c114fa7.json
Finished: success: 11, skipped: 0, failure: 0, total: 11
Loading composer repositories with package information
Updating dependencies (including require-dev)
Nothing to install or update
Generating autoload files
```

Composer will read the json file and get to work. Even with no packages 
required in the json file it still performs certain setup tasks.

In the example above, Composer is downloading lists of providers. It doesn't 
have to do this every time, so you will may not see it again after you run 
it the first time.

```$xslt
$ ls -l
-rw-r--r--  1 davidhernandez  staff  2392 May 11 15:31 README.md
-rw-r--r--  1 davidhernandez  staff   307 May 11 14:36 composer.json
drwxr-xr-x  4 davidhernandez  staff   136 May 11 15:28 vendor
```

We can see here that one of the setup tasks is to create a `vendor` directory.
This is where Composer will put all the packages you will use in your project.

```$xslt
$ ls -l vendor/
-rw-r--r--   1 davidhernandez  staff  178 May 11 15:28 autoload.php
drwxr-xr-x  10 davidhernandez  staff  340 May 11 15:28 composer
```

Without requiring any packages, Composer will still download and setup one thing. 
That is an autoloader. An autoloader is used by a PHP application to automatically 
load classes and interfaces. How that works is beyond the scope of this tutorial 
but it is something a lot of applications will take advantage of.