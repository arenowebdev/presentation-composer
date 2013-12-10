# Introduction To Composer
http://presentations.wsi-services.com/PHPaaSA/Composer-Introduction/#/0/16
## Presentation Outline

What I Want to Cover
--------------------
What is it
    Dependency manager, NOT package manager
    Inspired by Ruby's Bundler and Node's npm
    Cross-platform compatible (requires >= PHP 5.3.2 optionally a VCS)

Why you should use it
    Your project has dependencies
    Those dependencies have dependencies
    Unzipping files sucks
    Updating those dependencies manually sucks
    Manually requiring those dependencies sucks
    You specify your project needs in writing

Installing it
    Locally
        curl -sS http://getcomposer.org/installer | php

    Globally
        curl -sS https://getcomposer.org/installer | php
        mv composer.phar /usr/local/bin/composer

Using it
    Basics
        `composer`
            [screenshot]

        `composer --version`
            [screenshot]

        `composer self-update`
            [screenshot]

    Finding Packages
        packagist.org

    Project Setup
        All composer projects utilize a composer.json file in the root directory of the project. You can write it manually, or utilize the command line to create it for you.
        Command Line
            `composer init`
                [screenshot]
                (walk through each step:)
                * Package name (<vendor>/<name>)
                * Description:
                * Author:
                * Minimum Stability:
                * License:
                * Would you like to define your dependencies (require) interactively?
                * Would you like to define your dev dependencies (requiredev) interactively?

        What you end up with / Manually
            {
                "name": "Mosher/ComposerPresentation",
                "description": "Description of our project",
                "license": "proprietary",
                "authors": [
                    {
                        "name": "David Mosher",
                        "email": "david@dmwc.biz"
                    }
                ],
                "require": {
                    "laravel/framework": "4.0.*"
                },
                "require-dev": {
                    "mockery/mockery": "dev-master@dev"
                }
            }

        Break down require and require dev along with version constraints
            Exact version:
                `"require": {
                    "laravel/framework": "4.0.0"
                }`
            Wildcard version:
                `"require": {
                    "laravel/framework": "4.0.*"
                }`
                (equivalant to >= 4.0, < 4.1)
            Range of versions:
                `"require": {
                    "laravel/framework": ">=4.0.0"
                }`
                (valid values: >, >=, <, <=, !=)
            Special stuff - Tilde:
                `"require": {
                    "laravel/framework": "~4.0.0"
                }`
                (equivalant to >= 4.0.0, < 4.1.0)

        You can also utilize packages hosted in version control...public or private repos!
            {
                "name": "Mosher/ComposerPresentation",
                "require": {
                    "": ""
                },
                "repositories": [
                    {
                        "type": "vcs",
                        "url": "https://dmosher@bitbucket.org/dmosher/composer-presentation.git"
                    }
                ]
            }

        Virtual Packages
            You are also able to make sure components that aren't installable by Composer are present on the system.

            * php - Allows you to apply constraints on the PHP version
                `{
                    "require": {
                        "php": ">=5.3.2"
                    }
                }`
            * ext-<name> - Allows requiring of PHP extensions
                `{
                    "require": {
                        "extcurl": "*"
                    }
                }`
            * lib-<name> - Allows version constraints of PHP libraries
                `{
                    "require": {
                        "libpcre": ">=7.8"
                    }
                }`

        Autoloading
            There are currently three ways to auto-load files in PHP:
                1. PSR-0 (PSR-4...)
                    `"autoload": {
                        "psr0": { "Mosher\\ComposerPresentation\\": "src/" }
                    }`
                2. Classmap
                    `"autoload": {
                        "classmap": [ "lib/", "ext/library.php" ]
                    }`
                3. Files
                    `"autoload": {
                        "files": [ "ext/functions.php" ]
                    }`

        Install & Update
            `composer install`
            `composer update`

            This reads from composer.lock from the current directory if it exists
            else it reads composer.json
            It then resolves all dependencies
            Creates and updates composer.lock with exact version info
            Installs / Updates packages in /vendor
            The vendor directory defaults to /vendor in the root of the project, but you can change this behavior with the vendor-dir option
                `{
                    "vendordir": "packages"
                }`

    Let's Build Something Quickly
        `composer create-project laravel/laravel composer-presentation`


<!-- What Is It?
* A tool for dependency management for PHP, written in PHP
* Is not a package manager. Manages libraries/packages on a per project basis
* Strongly inspired by node's npm and ruby's bundler
* Looks to solve:

    a) You have a project that depends on a number of libraries

    b) Some of those libraries depend on other libraries

    c) You declare the things you depend on

    d) Composer finds out which versions of which packages need to be installed, and installs them (meaning it downloads them into your project)


Installing

Two ways to do this. Recommended global install so you can run with `composer`:
    curl -s http://getcomposer.org/installer | php -- --install-dir=/usr/local/bin

or

    curl -s http://getcomposer.org/installer | php

which just installs to the current directory and you have to run `php composer.phar` each time you wish to use it.


Packagist
Packagist is the one stop shop for finding dependencies to pull into your application.


Declaring Dependencies (manual)
Let's say you are building a websocket application and wish to utilize the Ratchet  websocket library. Simply create a file called `composer.json` in your project root directory with the following contents:

    {
        "require": {
            "cboden/ratchet": "0.3.*"
        }
    }

Once you have outlined your dependencies you simply run `composer install` and composer does the rest, which I will show in a minute...

Declaring Dependencies (almost automatic)
You are able to search for packages directly from the command line as well. Simply run `composer require` and you are walked through requiring a package for your project. It will ask you what to search for. Enter what you would like and search results will be returned to you. Enter the number of the item you wish to use and you are then asked what version constraint you would like to use. Once you've entered those two pieces of information, composer goes to work downloading the dependencies and creating an autoloader for you to use.

The All Mighty Autoloader
 -->
