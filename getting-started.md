# Getting Started

Deployer will help you deploy your applications to remote servers. First install Deployer, download the `deployer.phar` archive.

[Download deployer.phar](http://deployer.org/deployer.phar)

Then move `deployer.phar` to your bin directory and make it executable:

~~~
mv deployer.phar /usr/local/bin/dep
chmod +x /usr/local/bin/dep
~~~

You can now use Deployer via the `dep` command. Later, you may upgrade Deployer to the latest version, you can run the `dep self-update` command.

Now imagine that we are going to deploy Symfony-like application (other frameworks and apps pretty much same).

Create a `deploy.php` file in your project directory. First start from including the provided Symfony recipe. All Deployer recipes based on `recipe/common.php` recipe. 

~~~ php
require 'recipe/symfony.php';

// Define server for deploy.
// Let's name it "prod" and use 22 port.
server('prod', 'domain.net', 22)
    ->user('name')
    ->forwardAgent() // You can use identity key, ssh config, or username/password to auth on server.
    ->stage('production')
    ->env('deploy_path', '/home/your/project'); // Define base path to deploy you project.
    
// Specify repository from which to download your projects code.
// If you don't using forward aget, server has to be able clone your project from this repository.
set('repository', 'git@github.com:org/app.git');
~~~

Now open a terminal in your project directory and run the following command to deploy your application:

~~~
dep deploy production
~~~

> To list all the available commands, run the `dep` command.

> Note what Deployer try to give write permissions with `sudo` command, so this command has to be running without promt passwords.
> For that connect to you server via ssh, and run next command:
> ```
> sudo visudo
> ```
> And add next line in the end of file:
> ```
> name   ALL=(ALL) NOPASSWD: /usr/bin/setfacl
> ```
> When save file. If you don't want Deployer to take care of writable dirs, override `deploy:writable` task as you wish.

Deployer will create next directory structure on `prod` server:

```
/home/your/project
|--releases
|  |--20150513120631
|--shared
|  |--...
|--current -> /home/elfet/new.xu.su/releases/20150513120631
```

On next deploy will be create new directory in `releases`, and `current` symlink will be pointed to new release only in end. By default Deployer keeps 3 releases in `releases` dir, you can increase this by setting this option: `set('keep_releases', 3)`.

If something goes wrong during deployment procces, or something wrong with your new release, simple rollback to previos succesful deploy:
```
dep rollback prod
```

Also you may want to do some task before/after deploy. It's realy simple, lets reload php-fpm after deploy:

```php
task('reload:php-fpm', function () {
    run('sudo /usr/sbin/service php5-fpm reload');
});

after('deploy', 'reload:php-fpm');
```

> If you want to see what exactly deployer runs on you server, run `dep deploy -vvv`.

Configure you server to serve `currect` directory as root. 

What's all!
