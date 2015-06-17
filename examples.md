# Examples

This is an example of a `deploy.php` script used to reload the `php5-fpm` service after deploying.

``` php
<?php
require 'recipe/symfony.php';

serverList('app/config/servers.yml');

set('repository', 'https://github.com/user/app.git');

task('reload:php-fpm', function () {
    run('sudo /usr/sbin/service php5-fpm reload');
});

after('deploy', 'reload:php-fpm');
after('rollback', 'reload:php-fpm');
```
