# Composer Tutorial
A walk through of various Composer tasks.

[Composer](//getcomposer.org/) is a dependency manager for PHP. It downloads
public PHP packages (projects, libraries, etc) from Packagist.org.

Each step in this tutorial goes over basic Composer usage. The 
`composer.json` files in each directory should be usable, and each directory 
will have a README file explaining the concepts and commands for that step.

If you find any mistakes, please open an issue or create a pull request. Don't 
assume I know what I'm doing.&trade;

## Background
The steps in this tutorial are based on building a [Drupal 8](//drupal.org) project with
Composer, since that was the original motivation for the tutorial. However,
most of what is explained is specific to Composer, not Drupal, so the
information should still be relevant to any PHP project. And since the Drupal 8
use-case is more complex than simple "Hello, World!" functionality, it shows
some of Composer's possibilities.

## Before Starting
You need to have Composer installed. This will vary based on operating system.
See the Composer docs for instructions.

https://getcomposer.org/doc/00-intro.md

It is important to note that the PHP version you use can matter. Composer uses 
whichever version your command line uses. Run `php -v` to see which version 
you have. And, be consistent with your project. If your application and web 
server will use PHP 7, upgrade your command line to use 7, as well.

## Using the `composer` Command

In this tutorial, all commands begin with the `composer` command. If this does 
not work for you, you many have to manually setup an alias or move Composer's 
`.phar` file. Composer's installation instructions, linked above, walk you 
through that process.

## Terminology

There are two terms that cause some confusion and sometimes are used 
interchangeably - **package** and **project**. In this guide, **package** 
refers to anything Composer downloads. A PHP library, project, etc. **Project** 
refers to the project you are building.