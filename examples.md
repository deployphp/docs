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

```yaml
# app/config/servers.yml
dev:
    host:          your-host.com
    user:          release
    identity_file:
        public_key: "~/.ssh/id_release.pub"
        private_key: "~/.ssh/id_release"
        password: "key-password"
    stage:         dev
    deploy_path:   "/var/www"
    branch:        dev
```
