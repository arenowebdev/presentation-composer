[CHANGE]

WHAT IS IT?
===========
So what is Composer? The developers of the project describe it as [CHANGE]

a "tool for dependency management in PHP". They go out of their way to say that it is [CHANGE]

NOT a package management tool in that the dependencies it manages for you are on a per-project basis. It may be helpful for Windows developers to note that it is also cross-platform compatible only requiring PHP 5.3.2 or greater.


BENEFITS OF USING IT
====================
Most popular scripting languages that are used for web development have at least one package manager of some sort. [CHANGE]

Ruby has Bundler and Node has npm, to name just two.

If you haven't used any of these tools, you really don't know what you're missing. [CHANGE]

Working with zip files (as used to be the case with PHP libraries) is messy, slows you down and increases risks by not having an easy update mechanism. [CHANGE]

This opens you up for the most annoying kind of security issues [CHANGE]

(ones that have already been patched.)

Let's talk a little about what the major benefits of using Composer are. Firstly it [CHANGE]

    * Decreases development time
        Finding and downloading required libraries from a centralized repository is much simpler than scouring the web for obscure zip files

    [CHANGE]

    * Easy access to libraries leads to more people re-using existing code
        This improves the quality of code in the community both functionality wise but also security wise

    [CHANGE]

    * Automatic dependency management encourages developers to publish new packages while relying on old (DRY)

    * You are also able to specify minimum versions of PHP, the requirement of a particular PHP extension or library. We'll discuss this more soon...

    But most of all:

    [CHANGE]

    * You specify what your project needs are *in writing*

Sounds great, David. But how do I get started? Well, you need to install it:


INSTALLATION
============
Installing Composer is outlined on the web site (getcomposer.org) and, if you're comfortable with the command line) is really dead simple.

    `$ curl -sS http://getcomposer.org/installer | php`

This command downloads some installer code which creates a composer.phar file whereever you issued the command. It is highly recommended that you move this to your /usr/bin/local folder so composer becomes available globally as such:

[CHANGE]

    `$ sudo mv composer.phar /usr/local/bin/composer`

The rest of this talk is going to assume you have composer available globally. If you don't, simply replace `composer` with `php composer.phar` throughout this presentation.


BASICS
------

    `$ composer --version`

Shows you the current version you are running

One of the nicer things about composer is that it tells you when it is more than 30 days out of date...and it has it's own mechanism for updating itself!

[CHANGE]

    `$ composer
Warning: This development build of composer is over 30 days old. It is
recommended to update it by running "composer self-update" to get the
latest version.`

    `$ composer self-update`


COMPOSER.JSON FILE
------------------
The composer.json file is your applications "phone book" of dependencies. This is a very basic example project file which pulls in the Laravel Framework: [CHANGE]

    `{
        "name": "Example Project",
        "require": {
            "laravel/laravel": "v4.0.0"
        },
        "authors": [
            {
                "name": "David Mosher",
                "email": "dmosher@noip.com"
            }
        ]
    }`

This file can be created in two ways. The first is manually, which is never fun, but sometimes fastest. The second way is from the command line. (POP OUT TO COMMAND LINE WITH: `$ composer init`)

As you can see it will run through some questions with you to generate the file as we have seen in the slide. (SHOW COMPLETED COMPOSER.JSON FILE ALSO)

INSTALL ALL THE THINGS
----------------------
Once the composer.json file is created we are able to install our dependencies.

    `$ composer install`

Running that simple command will make your system run out to wherever your dependencies live, download the files and place them in a vendor folder. It also creates a vendor autoload file for you which we'll discuss later. Here's a look at what our project directory looks like now (POP OUT TO COMMAND LINE WITH: `$ ls`):

    `./
        composer.json
        composer.lock
        vendor/
            laravel/
                ...`

Now whenever there is an update to Laravel's version 4.0.0, you can simply call

    `$ composer update`

...and composer will automatically place those updated files in the vendor directory.

FINDING PACKAGES
----------------
The easiest way to locate a package is the companion web app to Composer: [CHANGE]

Packagist (http://packagist.org). The site contains a list of community maintained packages which are available for your use in your applications. The site is also what powers the package search on the command line behind the scenes. (POP OUT TO COMMAND LINE TO SHOW COMPOSER SEARCH [packagename])


CLOSED SOURCE PACKAGES
----------------------
So what about if you have libraries of your own that are closed source? [CHANGE]

Well, Composer allows you to install a package from any version control system that you would have access to from the machine. Your composer.json file would comtain something like this:

    `{
        "repositories": [
            {
                "type": "vcs",
                "url": "https://dmosher@bitbucket.org/dmosher/presentation.git"
            }
        ]
    }`

While you will probably want to put your packages on packagist most of the time, there are some use cases for hosting your own repository such as with private company packages or just to have a separate ecosystem. When this happens your best bet is to utilize Satis.

Satis is a static composer repository generator. It is a bit like an ultra-lightweight, static file-based version of packagist. For more information visit (http://getcomposer.org/doc/articles/handling-private-packages-with-satis.md).


VERSION CONSTRAINTS
-------------------
You have a lot of options when it comes to what version of a package (or platform packages...discussed soon) you would like to depend on. [CHANGE]

    * Exact Version
        `1.0.3` means only version 1.0.3 (and any updates made to that version tag...)

    * Range
        `>=1.0` Anything greater than version 1.0
        `>=1.2,<2.0` Anything greater than version 1.2 and less than version 2.0

    * Wildcard
        `1.0.*` Equivalent to `>=1.0,<1.1`

    * Next Significant Release
        `~1.2` The ~ operator is equivalent to >=1.2,<2.0 [CHANGE]
        `~1.2.3` is equivalent to >=1.2.3,<1.3.

PLATFORM PACKAGES
-----------------
Platform packages (virtual packages) are for things that are installed on the system but are not installable by Composer. There are three major types of platform packages:

    * PHP Version
        `"php": ">=5.3.2"` Makes sure PHP version is >= 5.3.2

    * PHP Extensions
        `"ext-gd": "*"` Makes sure the GD library is available, any version

    * PHP Libraries
        `"lib-curl": ">=7.3.*"` Makes sure the cURL library is at least version 7.3


So what does all of this work get me?

AUTOLOADING
===========
For those unfamiliar, the PHP Framework Interop Group (PHP-FIG http://php-fig.org) has put together a specification on an autoloader which is utilized by Composer. I highly recommend reading up on all thing on the PHP FIG site as well as PHP The Right Way (http://phptherightway.com) for some great information about programming today using PHP.

After you run Composer's install, composer creates an autoload file for you based on the dependencies you had outlined in your composer.json file. Here's all you have to do to include all of those dependencies into your project:

    `<?php
    require 'vendor/autoload.php';`

Now you are able to utilize the packages as they were namespaced from within your own application, and you get the added benefit of being able to simply run `composer update` to get the latest version of your wizz-bang dependencies.

Composer also allows you to manually include additional files in the autoloader. There are three methods to this as well:

    * PSR-0
        `"autoload": {
            "psr0": { "Mosher\\Presentation\\": "src/" }
        }`
        You define a mapping from namespaces to directories. The src directory would be in your project root, on the same level as vendor directory is. An example filename would be src/Acme/Foo.php containing an Acme\Foo class.

    * Classmap
        `"autoload": {
            "classmap": [ "lib/", "ext/library.php" ]
        }`
        The classmap references are all combined, during install/update, into a single key => value array which may be found in the generated file vendor/composer/autoload_classmap.php. This map is built by scanning for classes in all .php and .inc files in the given directories/files.

        You can use the classmap generation support to define autoloading for all libraries that do not follow PSR-0. To configure this you specify all directories or files to search for classes.

    * Files (directories or individually)
        `"autoload": {
            "files": [ "ext/functions.php" ]
        }`
        If you want to require certain files explicitly on every request then you can use the 'files' autoloading mechanism. This is useful if your package includes PHP functions that cannot be autoloaded by PHP.

THANKS!
=======