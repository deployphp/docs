# Getting Started

First, let's [install Deployer](installation.md). Run next commands in terminal:

```sh
curl -LO https://deployer.org/deployer.phar
mv deployer.phar /usr/local/bin/dep
chmod +x /usr/local/bin/dep
```

Now you can use Deployer via the `dep` command. 
Open up a terminal in your project directory and run the following command:

```sh
dep init
```

This command will create `deploy.php` file in current directory. It is called *recipe* and contains configuration and tasks for deployment.
By default all recipes extends [common](https://github.com/deployphp/deployer/blob/master/recipe/common.php) recipe. 


Define your task really simple:
 
```php
task('test', function () {
    writeln('Hello world');
});
```

To run that task, run next command:

```sh
dep test
```

And you get next output:

```text
➤ Executing task test
Hello world
✔ Ok
```

Now lets create some task which will run commands on remote server. For that we must configure server. 
Your created `deploy.php` file should contain `server` declaration line this:
 
```php
server('production', 'domain.com')
    ->user('username')
    ->identityFile()
    ->set('deploy_path', '/var/www/domain.com');
```

You can find more about servers configuration [here](servers.md). Now let's define task which will output `pwd` command from remote server:
 
```php
task('pwd', function () {
    $result = run('pwd');
    writeln("Current dir: $result");
});
```

Run it `dep pwd`, and you will get this:

```text
➤ Executing task pwd
Current dir: /var/www/domain.com
✔ Ok
```

Now lets prepare for our first deploy. You need to configure, such params as `repository`, `shared_files` and others:
   
```php
set('repository', 'git@domain.com:username/repository.git');
set('shared_files', [...]);
```

You can get params value in tasks using `get` function. 
Also you can override each config for each server:

```php
server('production', 'domain.com')
    ...
    ->set('shared_files', [...]);
```

More about configuration can be found [here](configuration.md).


Now let's deploy our application:
 
```sh
dep deploy
```

To see what exactly happening you can increase verbosity of output with `--verbose` option: 

* `-v`  for normal output,
* `-vv`  for more verbose output,
* `-vvv`  for debug.
 
On first Deployer will create the following directories on the server:

* `releases`  contains releases dirs,
* `shared` contains shared files and dirs,
* `current` symlink to current release.

Configure your server to serve your public directory from `current`.

> Note what deployer use [ACL](https://en.wikipedia.org/wiki/Access_control_list) by default for setting up permissions.
> You can change this behavior `writable_mode` config.    

By default deployer keeps last 5 releases, but you can increase this number by modifying parameter:
 
```php
set('keep_releases', 10);
```

If something went wrong during the deployment process, or something is wrong with your new release, 
simply run the following command to rollback to the previous working release:

```sh
dep rollback
```

You may want to run some task before/after another tasks. Configuring that is really simple!

Let's reload php-fpm after `deploy` task finished:

```php
task('reload:php-fpm', function () {
    run('sudo /usr/sbin/service php5-fpm reload');
});

after('deploy', 'reload:php-fpm');
```

Read more about [configuring](configuration.md) deploy. 
