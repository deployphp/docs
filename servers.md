# Servers

Deployer uses ssh2 pecl extension, but if you do not have it installed it on you machine - do not worry,
Deployer will also use [PHPSecLib](https://github.com/phpseclib/phpseclib).

You can define servers with the `server` function. Here is an example of a server definition:

~~~ php
server('main', 'site.com')
    ->path('/home/user/site.com')
    ->user('user')
    ->pubKey();
~~~

This function gets 3 parameters `server(server_name, host, port)` and return a `Deployer\Server\Configuration` object which contains the server configuration.

You then need to specify the base deployment path where your project will be deployed with the `path()` method.

Finally, you need to specify how to connect to server using SSH. There are a few ways:

### With a username and a password

~~~ php
server(...)
  ->user('name', 'password')
~~~

You can provide only the username and you will be prompted for your password on deploy.

### With a public key

~~~ php
server(...)
    ->user('name')
    ->pubKey();
~~~

If your keys were created with a password or are located outside of the `.ssh` directory, you can specify it:

~~~ php
server(...)
    ...
    ->pubKey('~/.ssh/id_rsa.pub', '~/.ssh/id_rsa', 'pass phrase');
~~~

The `~` symbol  will be replaced with your home directory. If you set the pass phrase as `null`,
you will be prompted for it on deploy.

You may prefer to use the following methods (if you are more verbose):

~~~ php
server(...)
    ...
    ->setPublicKey(...)
    ->setPrivateKey(...)
    ->setPassPhrase(...);
~~~

### With a config file

Deployer can use your SSH config file.

~~~ php
server(...)
    ->user('name')
    ->configFile('/path/to/file');
~~~

This can only be used with the ssh2 pecl extension.

### With a pem file

Authentication using a pem file is currently only supported with PhpSecLib.

~~~ php
// Switch to PhpSecLib
set('use_ssh2', false);

server('ec2', 'host.aws.amazon.com')
    ->user('ec2-user')
    ->pemFile('~/.ssh/keys.pem');
~~~

### Upload and download

You can upload a file or directory with the `upload(local, remote)` function.

You can download a file with the `download(local, remote)` function.
