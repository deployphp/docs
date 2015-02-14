# Examples

This is an example of a `deploy.php` script used to reload the `php5-fpm` service after deploying.

~~~ php
require 'recipe/symfony.php';

server('main', 'site.com')
    ->path('/home/user/site.com')
    ->user('user')
    ->pubKey();

set('repository', 'git@github.com:user/site.git');

task('php-fpm:reload', function () {
	run("sudo /usr/sbin/service php5-fpm reload");
})->description('Reloading PHP5-FPM');

after('deploy:end', 'php-fpm:reload');
~~~
