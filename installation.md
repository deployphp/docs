# Installation

To install Deployer, download the `deployer.phar` archive.

[Download deployer.phar](http://deployer.org/deployer.phar)

Then move `deployer.phar` to your bin directory and make it executable.

``` sh
$ mv deployer.phar /usr/local/bin/dep
$ chmod +x /usr/local/bin/dep
```

To upgrade Deployer, run the command:

``` sh
$ dep self-update
```

To redownload an update if already using the current version, use the `--redo (-r)` option

``` sh
$ dep self-update --redo
```

To allow pre-release updates, use the `--pre (-p)` option

``` sh
$ dep self-update --pre
```

To upgrade to the next major release, if available, use the `--upgrade (-u)` option

``` sh
$ dep self-update --upgrade
```

### Via Composer

You can install Deployer with composer:

``` sh
$ composer require deployer/deployer:~3.0
```

Then to use Deployer you may run the following command:

``` sh
$ php vendor/bin/dep
```

### Source Code

If you want build Deployer from source code, clone the project from GitHub:

``` sh
$ git clone https://github.com/deployphp/deployer.git
```

Then run the following command in the project directory:

``` sh
$ php ./build
```

This will build the `deployer.phar` phar archive.
