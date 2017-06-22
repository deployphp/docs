# Installation

Deployer can be installed in two ways: using phar archive and using composer.

### Phar archive

To install Deployer via phar archive, run next commands:

```sh
curl -LO https://deployer.org/deployer.phar
mv deployer.phar /usr/local/bin/dep
chmod +x /usr/local/bin/dep
```

If you need another version of Deployer, you can find them on [download page](https://deployer.org/download).
Later, to upgrade Deployer run the command:

```sh
dep self-update
```

To upgrade to the next major release, if available, use the `--upgrade (-u)` option:

```sh
dep self-update --upgrade
```

### Composer

To install Deployer with Composer, run next command:

```sh
composer require deployer/deployer --dev
```

You can also install it globally:

``` sh
composer global require deployer/deployer
```

More info: https://getcomposer.org/doc/03-cli.md#global

Then to use Deployer, run the following command:

```sh
php vendor/bin/dep
```

> If you have installed Deployer using **both** methods, running `dep` command will prefer composer-installed version. 

### Source

If you want build Deployer from source code, clone the project from GitHub:

```sh
git clone https://github.com/deployphp/deployer.git
```

Then run the following command in the project directory:

```sh
php bin/build
```

This will build the `deployer.phar` phar archive.

### Autocomplete

Deployer comes with autocomplete script for bash/zsh/fish, so you don't need to remember all tasks and options.
To install run following command:

~~~bash
dep autocomplete
~~~

And follow instructions. 

Read [getting started](getting-started.md) next.
