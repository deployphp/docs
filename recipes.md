# Recipes

Deployer has a set of predefined tasks called _recipes_.

Recipes can be included to your `deploy.php` file like this:

``` php
require 'recipe/common.php';
```

### Common Recipe

The common recipe is used by all other recipes. It creates the following directory structure:

```
|-- current → /home/www/releases/20140812131123
|-- releases
|   `-- 20140812131123
|   `-- 20140809150234
|   `-- 20140801145678
`-- shared
```

#### deploy:prepare


This task prepare server for deploy, creates a `releases` and a `shared` directory.

* `releases` - contains your project releases.
* `shared` - contains shared/common files and directories between releases (logs, shared data, etc.).

#### deploy:update_code

Update code from the configured repository and puts it into the directory of the upcoming release.

Use the `set` function to specify which repository to use:

``` php
set('repository', 'git@github.com:user/project.git');
```

#### deploy:shared

Creates a symlink to the shared files and directories. Use `set` to define them.

``` php
set('shared_dirs', ['app/logs']);

set('shared_files', ['app/config/parameters.yml']);
```

#### deploy:writable

Creates writable directories.

``` php
set('writable_dirs', ['app/cache', 'app/logs']);
```

Use the `set` function to specify the user who should own the directories.

```php
set('http_user', 'user');
```

#### deploy:vendors

Installs vendors with composer. If `composer` is not found, a new `composer.phar` is downloaded. To specify the path of the composer command:

```php
set('composer_command', '/bin/composer.phar');
```

#### deploy:copy_dirs

Copy directories. Useful for vendors directories. Instead of forcing composer to retrieve the packages one by one from cache/internet 
during composer install, it can simply copy them from your current release. 

```php
set('copy_dirs', ['vendor']);
```

Also, this task has to be called manually as it is not part of the deploy task, and it must be call before composer install runs.

```php
before('deploy:vendors', 'deploy:copy_dirs');
```

#### deploy:symlink

Creates a symlink named `current` which points to the latest release.

#### cleanup

Removes old releases and keeps the last 3. To change the number of kept releases:

``` php
set('keep_releases', 5);
```
#### deploy:clean

Cleaning up files and/or directories.

``` php
set('clear_paths', ['app/cache', 'app/logs']);
```

Allow using `sudo` when cleaning up files and/or directories.

```php
set('clear_use_sudo', true);
```

#### rollback

Rollback to the previous release. If only one release is available, nothing will be done.

### Composer Recipe

``` php
require 'recipe/composer.php';
```

The composer recipe is a simple recipe suitable for simple projects which uses composer.

It consists of the following tasks:

* deploy:prepare
* deploy:release
* deploy:update_code
* deploy:vendors
* deploy:symlink
* cleanup

### Symfony Recipe

``` php
require 'recipe/symfony.php';
```

This recipe is specifically for deploying Symfony2 projects. The default configuration of this recipe is:

``` php
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
```

> For Symfony 3, require the symfony3 recipe:
> ``` php
> require 'recipe/symfony3.php';
> ```
>
> If you installed assetic inside Symfony 3 you can enable the dumping this with:
> ```php
> set('dump_assets', true);
> ```

> To add automatic database migration, you can add something like the following:
> ``` php
> after('deploy:vendors', 'database:migrate');
> ```


### Laravel Recipe

``` php
require 'recipe/laravel.php';
```

This recipe is specifically for deploying Laravel 5 projects. The default configuration of this recipe is:

```php
// Laravel shared dirs
set('shared_dirs', [
    'storage/app',
    'storage/framework/cache',
    'storage/framework/sessions',
    'storage/framework/views',
    'storage/logs',
]);

// Laravel 5 shared file
set('shared_files', ['.env']);

// Laravel writable dirs
set('writable_dirs', ['storage', 'vendor']);
```

### Yii Recipe

#### Yii

``` php
require 'recipe/yii.php';
```

This recipe is specifically for deploying Yii projects. The default configuration of this recipe is:

```php
// Yii shared dirs
set('shared_dirs', ['runtime']);

// Yii writable dirs
set('writable_dirs', ['runtime']);
```

#### Yii 2 Basic

``` php
require 'recipe/yii2-app-basic.php';
```

This recipe is specifically for deploying Yii 2 basic projects. The default configuration of this recipe is:

```php
// Yii 2 Basic Project Template shared dirs
set('shared_dirs', ['runtime']);
```

#### Yii 2 Advanced

``` php
require 'recipe/yii2-app-advanced.php';
```

This recipe is specifically for deploying Yii 2 advanced projects. The default configuration of this recipe is:

```php
// Yii 2 Advanced Project Template shared dirs
set('shared_dirs', [
    'frontend/runtime',
    'backend/runtime',
    'console/runtime',
]);

// Yii 2 Advanced Project Template shared files
set('shared_files', [
    'common/config/main-local.php',
    'common/config/params-local.php',
    'frontend/config/main-local.php',
    'frontend/config/params-local.php',
    'backend/config/main-local.php',
    'backend/config/params-local.php',
    'console/config/main-local.php',
    'console/config/params-local.php',
]);
```

### ZendFramework Recipe

``` php
require 'recipe/zend_framework.php';
```

### WordPress Recipe

``` php
require 'recipe/wordpress.php';
```

### Drupal Recipe

TODO

### Magento Recipe

TODO

### CodeIgniter Recipe

TODO

### CakePHP Recipe

TODO

### FuelPHP Recipe

TODO
