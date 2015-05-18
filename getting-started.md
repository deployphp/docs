# Getting Started

Deployer will help you deploy your applications to remote servers. First install Deployer, download the `deployer.phar` archive.

[Download deployer.phar](http://deployer.org/deployer.phar)

Then move `deployer.phar` to your bin directory and make it executable:

~~~
mv deployer.phar /usr/local/bin/dep
chmod +x /usr/local/bin/dep
~~~

You can now use Deployer via the `dep` command. Later, you may upgrade Deployer to the latest version, you can run the `dep self-update` command.

Now imagine that we are going to deploy Symfony-like application (other frameworks and apps pretty same).

Create a `deploy.php` file in your project directory. Let's imagine that your project is based on the Symfony2 Framework (other frameworks are described in the docs). First start off by including the provided Symfony recipe.

~~~ php
require 'recipe/symfony.php';

// Define server for deploy.
// Let's name it "main" and use 22 port.
server('main', 'domain.net', 22)
    ->path('/home/your/project') // Define base path to deploy you project.
    ->user('name', 'password');  // Define SSH user name and password.
                                 // You can skip password and it will be asked on deploy.
                                 // Also you can connect to server SSH via public keys and ssh config file.

// Specify repository from which to download your projects code.
// Server has to be able clone your project from this repository.
set('repository', 'git@bitbucket.org:elfchat/elfchat.net.git');
~~~

Now open a terminal in your project directory and run the following command to deploy your application:

~~~
dep deploy
~~~

To list all the available commands, run the `dep` command.
