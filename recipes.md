# Recipes

Deployer has a set of predefined tasks called _recipes_.

Recipes can be included to your `deploy.php` file like this:

~~~ php
require 'recipe/common.php';
~~~

### Common Recipe

The common recipe is used by all other recipes. It creates the following directory structure:

~~~
|-- current → /home/www/releases/20140812131123
|-- releases
|   `-- 20140812131123
|   `-- 20140809150234
|   `-- 20140801145678
`-- shared
~~~

#### deploy:prepare


This task prepare server for deploy, creates a `releases` and a `shared` directory.

* `releases` - contains your project releases.
* `shared` - contains shared/common files and directories between releases (logs, shared data, etc.).

#### deploy:update_code

Update code from the configured repository and puts it into the directory of the upcoming release.

Use the `set` function to specify which repository to use:

~~~ php
set('repository', 'git@github.com:user/project.git');
~~~

#### deploy:shared

Creates a symlink to the shared files and directories. Use `set` to define them.

~~~ php
set('shared_dirs', ['app/logs']);

set('shared_files', ['app/config/parameters.yml']);
~~~

#### deploy:writable_dirs

Creates writable directories.

~~~ php
set('writable_dirs', ['app/cache', 'app/logs']);
~~~

#### deploy:vendors

Installs vendors with composer.

#### deploy:symlink

Creates a symlink named `current` which points to the latest release.

#### cleanup

Removes old releases and keeps the last 3. To change the number of kept releases:

~~~ php
set('keep_releases', 5);
~~~

#### rollback

Rollback to the previous release. If only one release is available, nothing will be done.

### Composer Recipe

~~~ php
require 'recipe/composer.php';
~~~

The composer recipe is a simple recipe suitable for simple projects which uses composer.

It consists of the following tasks:

* deploy:prepare
* deploy:release
* deploy:update_code
* deploy:vendors
* deploy:symlink
* cleanup

### Symfony Recipe

~~~ php
require 'recipe/symfony.php';
~~~

This recipe is specifically for deploying Symfony2 projects. Default Symfony configuration of this recipe are:

~~~ php
// Symfony shared dirs
set('shared_dirs', ['app/logs']);

// Symfony shared files
set('shared_files', ['app/config/parameters.yml']);

// Symfony writable dirs
set('writable_dirs', ['app/cache', 'app/logs']);

// Assets
set('assets', ['web/css', 'web/images', 'web/js']);

// Environment vars
env('env_vars', 'SYMFONY_ENV=prod');
env('env', 'prod');
~~~

> Switching to the Symfony3 dir structure:
> ~~~ php
> set('shared_dirs', ['var/logs']);
> set('writable_dirs', ['var/cache', 'var/logs']);
> set('bin_dir', 'bin');
> set('var_dir', 'var');
> ~~~

> To add automatic database migration, you can add something like the following:
> ~~~ php
> after('deploy:vendors', 'database:migrate');
> ~~~


### Laravel Recipe

TODO

### Yii Recipe

TODO

### ZendFramework Recipe

TODO

### WordPress Recipe

TODO
