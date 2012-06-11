Phark
=====

Phark is a package management system for PHP code. It provides dependencies, 
versioning and installation for packages either system-wide or for an individual project. 

**Note: Phark is no longer under active development, I'd recommend [Composer](http://getcomposer.org/)**

**Read the [STATUS document](https://github.com/lox/phark/blob/master/STATUS.md) for more details**

Installing Phark
----------------

```bash
$ curl http://pharkphp.org/install | sh
```

Using Phark
-----------

Phark packages can be installed manually with the command-line tool, 
[phark](https://github.com/lox/phark/wiki/Using-the-commandline-tools).

```bash
$ cd projects/myproject
$ phark install pheasant
```

You can then proceed to reference that code in your application.

```php
<?php

require_once 'pheasant/Pheasant.php';
```

Declaring dependencies in a project
------------------------------------

A project can declare the packages it depends on with a `Pharkdeps` in the top
level of the project:

```php
<?php

$deps
	->dependency( 'pheasant', array('git'=>'https://lox@github.com/lox/pheasant.git') )
	->dependency( 'yaml', '~>1.0.0' )
	->dependency( 'simpletest', '2.0.0beta1', array('group'=>'dev') )
	;
```
Then the following should be executed at the top level of the project:

```bash
$ phark deps
$ phark lock-deps
```

This will result in the above dependencies installed, and a `Pharkdeps.lock` file 
generated with the exact versions installed. This can be committed to your SCM
to make sure other contributors get the same dependency versions.


How does Phark work?
--------------------

Unfortunately, a little bit of magic is required. PHP include and require work
relative to PHP's include_path. In other languages it's possible to influence
this via environmental variables, but in PHP it's either set via Apache or via
php.ini.

To work around this, Phark uses the [auto_prepend_file](http://php.net/manual/en/ini.core.php#ini.auto-prepend-file) directive in php.ini.
Each time a php script is run, the Phark stub is executed first to set up the 
[include_path](http://php.net/manual/en/ini.core.php#ini.include_path) environment. 

See [alternate installation methods](https://github.com/lox/phark/wiki/Alternate-Installation-Methods) 
if you use this directive for something else.


What do the Packages look like?
-------------------------------

Check out the wiki page on the [anatomy of a phark package](https://github.com/lox/phark/wiki/Anatomy-of-a-Phark-Package).


Why the name?
--------------

Phark was born out of my frustration with trying to write and distribute
re-usable code in PHP. Phark packages use the PHP Archive format, 
[Phar](http://www.php.net/manual/en/book.phar.php).

Read the [original proposal on github](https://gist.github.com/711221).

Why not PEAR?
-------------

[PEAR][1] is a complicated beast, with a lot of legacy. Aside from the rough port to
PHP5.3 and bulky codebase, PEAR still doesn't allow for multiple versions of a package to be
installed side-by-side, or for per-project installations.

What's worse is how hard it is to contribute a PEAR package. Phark allows anyone to submit packages for 
instant consumption by other developers. If you really want you can even consume PEAR packages via this mechanism.

References and Reading
----------------------

Lots of other languages have great package management tools. Any great software
is built on the shoulders of giants, in Pharks case, that would be the
following:

* [RubyGems][2] (ruby)
* [Bundler][3] (ruby)
* [RIP][4] (ruby)
* [PIP][5] (python)
* [VirtualEnv][6] (python)
* [NPM][7] (nodejs)
* [Homebrew][8] (ruby)

[1]: http://pear.php.net/manual/
[2]: http://docs.rubygems.org/read/book/7
[3]: http://gembundler.com/
[4]: https://github.com/defunkt/rip
[5]: http://www.pip-installer.org/en/latest/
[6]: http://pypi.python.org/pypi/virtualenv
[7]: http://npmjs.org/
[8]: http://mxcl.github.com/homebrew/

Some notes on each of these are available [on the wiki](https://github.com/lox/phark/wiki/Package-Managers-in-Other-Languages). 

