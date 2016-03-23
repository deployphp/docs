# Getting Started

Deployer will help you deploy your applications to remote servers.

To install it, first download the `deployer.phar` archive:

[Download deployer.phar](http://deployer.org/deployer.phar)

Move `deployer.phar` to your bin directory and make it executable:

```
mv deployer.phar /usr/local/bin/dep
chmod +x /usr/local/bin/dep
```

Now you can use Deployer via the `dep` command. Upgrade Deployer to the
latest version at any time, by running `dep self-update`.

To deploy your application, you have to choose the right "recipe" for it.
For example, let's see how a Symfony application can be deployed.

Open up a terminal in your project directory and run the following command:

``` 
dep init
```

This command will create `deploy.php` file in current directory. Open that file and change `server` credentials to your own.
And specify right `repository`. If you're not using a forward agent, then the server has to be able to clone your project from this repository. The server needs to have git installed for Deployer to work.

Now to deploy your application:
``` 
dep deploy
```

> To list all the available commands, run the `dep` command.

> Note what Deployer will try to get write permission with the `sudo` command, so this command has to be running without prompt passwords.
> For that, connect to you server via ssh, and run the following command:
> ```
> sudo visudo
> ```
> Add this line at the end of file:
> ```
> user_name   ALL=(ALL) NOPASSWD: /usr/bin/setfacl
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

``` sh
$ dep rollback
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

> If you want to see exactly what deployer does on you server, then just run it in a more verbose mode: `$ dep deploy -vvv`.

Finally: configure your server to serve your public directory inside `current`.

That's all!
