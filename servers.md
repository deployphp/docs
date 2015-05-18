# Servers

Define servers with the `server` function. Here is an example of a server definition:

~~~ php
server('prod_1', 'domain.com')
    ->user('user', 'password')
    ->env('deploy_path', '/home/www')
    ->stage('production');
    
server('prod_2', 'domain.com')
    ->user('user', 'password')
    ->env('deploy_path', '/home/www')
    ->env('extra_stuff', '...')
    ->stage('production');
~~~

This function gets 3 parameters `server(server_name, host, port)` and return a `Deployer\Server\Builder` object.

To specify how to connect to server using SSH, there are a few ways:

### With a username and a password

~~~ php
server(...)
  ->user('name', 'password')
~~~

### With a identity file

~~~ php
server(...)
    ->user('name')
    ->identityFile();
~~~

If your keys were created with a password or are located outside of the `.ssh` directory, you can specify it:

~~~ php
server(...)
    ...
    ->identityFile('~/.ssh/id_rsa.pub', '~/.ssh/id_rsa', 'pass phrase');
~~~

The `~` symbol  will be replaced with your home directory. 

### With a config file

Deployer can use your SSH config file.

~~~ php
server(...)
    ->user('name')
    ->configFile('/path/to/file');
~~~

> This can only be used with the ssh2 pecl extension.
> ~~~ php
> // Switch to ext-ssh2
> set('use_ssh2', true);
> ~~~

### With a pem file

~~~ php
server('ec2', 'host.aws.amazon.com')
    ->user('ec2-user')
    ->pemFile('~/.ssh/keys.pem');
~~~

> Authentication using a pem file is currently only supported with PhpSecLib.

### Upload and download

You can upload a file or directory with the `upload(local, remote)` function.

You can download a file with the `download(local, remote)` function.
