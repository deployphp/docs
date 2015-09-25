# Installation

To install Deployer, download the `deployer.phar` archive.

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

To upgrade Deployer, run the command:

~~~
dep self-update
~~~

To redownload an update if already using the current version, use the `--redo (-r)` option

~~~
dep self-update --redo
~~~

To allow pre-release updates, use the `--pre (-p)` option

~~~
dep self-update --pre
~~~

To upgrade to the next major release, if available, use the `--upgrade (-u)` option

~~~
dep self-update --upgrade
~~~

### Via Composer

You can install Deployer with composer:

~~~
composer require deployer/deployer:~3.0
~~~

Then to use Deployer you may run the following command:

~~~
php vendor/bin/dep
~~~

### Source Code

If you want build Deployer from source code, clone the project from GitHub:

~~~
git clone https://github.com/deployphp/deployer.git
~~~

Then run the following command in the project directory:

~~~
php ./build
~~~

This will build the `deployer.phar` phar archive.
