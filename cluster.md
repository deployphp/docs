# Cluster

You can define clusters with the `cluster` function. Here is an example of a cluster definition:

~~~ php
cluster('my_istanbul_dc', ['192.168.99.1', '192.168.99.2', '192.168.99.3', '192.168.99.4'])
    ->user('user')
    ->password('pass')
    ->env('deploy_path', '/home/www')
    ->stage('production');
    
cluster('my_konya_dc', ['10.0.1.1', '10.0.1.2', '10.0.1.9', '10.0.1.41'])
    ->user('user')
    ->password('pass')
    ->env('deploy_path', '/home/www')
    ->env('extra_stuff', '...')
    ->stage('production');
~~~

This function takes 3 parameters, like this: `cluster(cluster_name, [hosts], port)`. It returns a `Deployer\Cluster\ClusterBuilder` object.

To specify how to connect to server using SSH, there are a few ways:

### With a username and a password

~~~ php
cluster(...)
  ->user('name')
  ->password('pass')
~~~

### With a username and a typed password

~~~ php
cluster(...)
  ->user('name')
  ->password(null)
~~~

Set password to *null* and it will be asked.

### With an identity file

~~~ php
cluster(...)
    ->user('name')
    ->identityFile();
~~~

If your keys were created with a password or if they are located outside of the `.ssh` directory, you can specify the location by providing the full path:

~~~ php
cluster(...)
    ...
    ->identityFile('~/.ssh/id_rsa.pub', '~/.ssh/id_rsa', 'pass phrase');
~~~

The `~` symbol  will be replaced with your home directory. 

### With a config file

Deployer can use your SSH config file.

~~~ php
cluster(...)
    ->user('name')
    ->configFile('/path/to/file');
~~~

### With a pem file

~~~ php
cluster('ec2', ['ist1.example.com', 'ist2.example.com', 'ist3.example.com'])
    ->user('ec2-user')
    ->pemFile('~/.ssh/keys.pem');
~~~

> Authentication using a pem file is currently only supported with PhpSecLib.

### Using PHP SSH2 extension

Package `herzult/php-ssh` is required for this to work but is not included inside `deployer.phar` so you should install `deployer/deployer` and `herzult/php-ssh` using composer:

```
composer require deployer/deployer herzult/php-ssh
```
Example `deploy.php` file:

```php
require 'vendor/autoload.php';
require 'vendor/deployer/deployer/recipe/symfony.php';

set('ssh_type', 'ext-ssh2');
//...
```

## Cluster list

You can define clusters in YAML file as follows, the only difference between defining a server and cluster is the `nodes` and `cluster` entries. Instead of host, you need to put your cluster nodes as array and set cluster as true. Whatever you use for server defination, you can use same keys for cluster as well. But, it's obvious that you can't use 'local' keyword for instance. Please see server help page for full explanation.

~~~ yml
istanbul_dc_cluster:
  nodes: [172.18.23.1, 172.18.23.3, 172.18.48.12]
  user: test
  password: qwerty
  stage: production
  deploy_path: /home/www/
  run_test: true
  cluster: true

konya_dc_cluster:
  nodes: [213.30.92.1, 213.30.92.4, 213.30.92.72]
  user: test
  password: qwerty
  stage: production
  deploy_path: /home/www/
  run_test: true
  cluster: true

~~~

And then in `deploy.php`:

~~~ php
serverList('servers.yml');
~~~

