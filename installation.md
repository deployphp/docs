# Installation

Deployer can be installed in three ways: 
1) using **composer package with phar only** - later called also "Deployer Distribution" version (no dependencies)
2) using **composer package with sources** - later called also "Deployer Source" version (dependencies but good for debug 
   Deployer) 
3) using direct download of **phar archive**

The best option for most cases is to choose  **composer package with phar only**. 


### Composer package with phar only

To install Deployer Distribution version with Composer, run the command:

```sh
composer require deployer/dist --dev
```

Then to use Deployer, run the following command:

```sh
php vendor/bin/dep
```

If you want to run deployer with `dep` only instead of `php vendor/bin/dep` then you need to add 
`export PATH="$PATH:./vendor/bin"` to your ~/.bashrc or ~/.profile file. For security reason mind to add
it on the very end of file so binaries read from project ./vendor/bin folder are the last taken into consideration.
Remember that you need to open new shell after changes in ~/.bashrc or ~/.profile file so the new config is loaded.  


### Composer package with sources

To install Deployer Source version with Composer, run the command:

```sh
composer require deployer/deployer --dev
```

Then to use Deployer, run the following command:

```sh
php vendor/bin/dep
```


### Phar archive

To install Deployer as phar archive, run the following commands:

```sh
curl -LO https://deployer.org/deployer.phar
mv deployer.phar /usr/local/bin/dep
chmod +x /usr/local/bin/dep
```

If you need another version of Deployer, you can find it on the [download page](https://deployer.org/download).
Later, to upgrade Deployer, run the command:

```sh
dep self-update
```

To upgrade to the next major release, if available, use the `--upgrade (-u)` option:

```sh
dep self-update --upgrade
```


### Source

If you want to build Deployer from the source code, clone the project from GitHub:

```sh
git clone https://github.com/deployphp/deployer.git
```

Then run the following command in the project directory:

```sh
php bin/build
```

This will build the `deployer.phar` phar archive.

### Autocomplete

Deployer comes with an autocomplete script for bash/zsh/fish, so you don't need to remember all tasks and options.
To install, run the following command:

~~~bash
dep autocomplete
~~~

And follow the instructions. 

Read [getting started](getting-started.md) next.
