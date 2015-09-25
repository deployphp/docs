# Getting Started

Deployer will help you deploy your applications to remote servers.

#### Installation on Linux and Mac OS X

~~~
$ sudo curl -LsS http://deployer.org/deployer.phar -o /usr/local/bin/dep
$ sudo chmod +x /usr/local/bin/dep
~~~

#### Installation on Windows
~~~
c:\> php -r "readfile('http://deployer.org/deployer.phar');" > dep
~~~
Move the downloaded file to your projects directory and execute it as php dep

You can now use Deployer via the `dep` command. You may upgrade Deployer to the
latest version at any time, by running `dep self-update`.

To deploy your application, you have to choose the right "recipe" for it.
For example, let's see how a Symfony application can be deployed!

Create a `deploy.php` file in your project's root directory, containing the
following:

~~~ php
<?php

// All Deployer recipes are based on `recipe/common.php`.
require 'recipe/symfony.php';

// Define a server for deployment.
// Let's name it "prod" and use port 22.
server('prod', 'host', 22)
    ->user('name')
    ->forwardAgent() // You can use identity key, ssh config, or username/password to auth on the server.
    ->stage('production')
    ->env('deploy_path', '/your/project/path'); // Define the base path to deploy your project to.

// Specify the repository from which to download your project's code.
// The server needs to have git installed for this to work.
// If you're not using a forward agent, then the server has to be able to clone
// your project from this repository.
set('repository', 'git@github.com:org/app.git');
~~~

Now open up a terminal in your project directory and run the following command
to deploy your application:

~~~
dep deploy production
~~~

> To list all the available commands, run the `dep` command.

> Note what Deployer will try to get write permission with the `sudo` command, so this command has to be running without prompt passwords.
> For that, connect to you server via ssh, and run the following command:
> ```
> sudo visudo
> ```
> Add this line at the end of file:
> ```
> name   ALL=(ALL) NOPASSWD: /usr/bin/setfacl
> ```
> When saving the file, if you don't want Deployer to take care of writable dirs, override `deploy:writable` task as you wish.

Deployer will create the following directory structure on the `prod` server:

```
/your/project/path
|--releases
|  |--20150513120631
|--shared
|  |--...
|--current -> /your/project/path/releases/20150513120631
```

Every deployment will create a new directory in `releases`. When the deployment
finished a symlink called `current` will be pointed to the new release. By
default Deployer keeps the last 3 releases, but you can increase this number by
modifying the `keep_releases` parameter: `set('keep_releases', 3)`.

If something went wrong during the deployment process, or something is wrong
with your new release, simply run the following command to roll back to the
previous working release:

```
dep rollback prod
```

You may want to run some task before/after another tasks. Configuring that is
really simple!

Let's reload php-fpm after the Symfony `deploy` task:

```php
task('reload:php-fpm', function () {
    run('sudo /usr/sbin/service php5-fpm reload');
});

after('deploy', 'reload:php-fpm');
```

> If you want to see exactly what deployer does on you server, then just run it in a more verbose mode: `dep deploy -vvv`.

Finally: configure you server to serve your public directory inside `current`.

That's all!
