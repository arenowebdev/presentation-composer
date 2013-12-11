
Quick installation
First you need to download the composer phar file. The quick way:
$ curl -s https://getcomposer.org/installer | php
This will download and execute an installer code, which will download an executable composer.phar file. Move this file somewhere in your $PATH, eg/usr/local/bin, so you can execute it like this on the terminal:
$ sudo mv composer.phar /usr/local/bin/composer
$ composer --version
If this prints out something like the following, you are good to go (if not, visitthe official installer guide and follow the instructions).
Composer version e2f8098
Example
Let’s install Twig, a template engine for PHP (yes, i believe in the separation of code from view ;)). You could now just download and unpack the twig libraries, but let’s do it right the first time. Create the dependency filecomposer.json in your project folder, containing:
{
    "require": {
        "twig/twig": "1.*"
    }
}
Now go to your project dir and install twig by running
$ cd MyProject
$ composer install
You should see something like this:
Loading composer repositories with package information
Installing dependencies
  - Installing twig/twig (v1.9.2)
    Downloading: 100%

Writing lock file
Generating autoload files
Afterwards, you have a directory vendor which contains some composer files and the the sub directory twig/twig, where twig was installed in. Cause we are on it, let’s also install Doctrine. Use composer to search for it:
$ composer search doctrine | grep orm
...
doctrine/orm: Object-Relational-Mapper for PHP
a2lix/translation-form-bundle: Translation field to use with Translatable Doctrine extension
...
The search is still a bit messy, so i’ve narrowed it down with grep orm… However, you should see doctrine/orm. Now let’s look what versions are there:
$ composer show doctrine/orm
name     : doctrine/orm
descrip. : Object-Relational-Mapper for PHP
keywords : database, orm
versions : dev-master, 2.4.x-dev, 2.3.x-dev, 2.3.0-RC1, 2.3.0-BETA1, 2.2.x-dev, 2.2.3, 2.2.2, 2.2.1, 2.2.0, 2.2.0-RC1, 2.2.0-BETA2, 2.2.0-BETA1, 2.1.x-dev, 2.1.7, 2.1.6, 2.1.5, 2.1.4, 2.1.3, 2.0.x-dev, dev-join-poc, dev-DDC-1766, dev-DDC-1652, dev-DDC-1637, dev-DDC-1544, dev-DDC-1509, dev-DDC-1385, dev-DDC-1383, dev-DDC-720, dev-DDC-551, dev-DDC-217, dev-DCOM-93, dev-DDC-93, dev-Test, dev-ImproveErrorMessages, dev-feature/flush-many-documents
type     : library
...
This will give you a bunch of information, among them which versions are available and what PHP versions is required and so on. Let’s use the latest 2.2 version (the others are currently RC or dev as of this date). Extend thecomposer.json like this:
{
    "require": {
        "twig/twig": "1.*",
        "doctrine/orm": "2.2.*"
    }
}
Now run update (not install). It will download the doctrine/orm as well asdoctrine/dbal and doctrine/common onto which it depends.
$ composer update
Loading composer repositories with package information
Updating dependencies
  - Installing doctrine/common (2.2.2)
    Downloading: 100%

  - Installing doctrine/dbal (2.2.2)
    Downloading: 100%

  - Installing doctrine/orm (2.2.3)
    Downloading: 100%

Writing lock file
Generating autoload files
And that’s it. Now you’ve all you need. You can run $ composer update later on to make sure you have the latest.
You can now include the files in your own boostrap (eg index.php) file:
<?php
require_once __DIR__ . '/vendor/autoload.php';
?>
Hope you are as delighted as we are about Composer and will use it in your future projects!

