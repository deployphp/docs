# Hosts

Host in Deployer is there are you going to deploy your application. It can be remote machine, local machine or Amazon EC2 instances.
Each host contains a hostname, a stage, one or a few roles and configuration parameters. 

You can define hosts with the `host` function in `deploy.php` file. Here is an example of a hosts definition:

~~~php
host('domain.com')
    ->stage('production')
    ->roles('app')
    ->set('deploy_path', '~/app');
~~~

Host *domain.com* has stage `production`, one role `app` and config `deploy_path` = `~/app`.

Same host can be described using yaml syntax. Write in `hosts.yml` file next:

~~~yaml
domain.com:
  stage: production
  roles: app
  deploy_path: ~/app
~~~

Then in `deploy.php`:

~~~php
inventory('hosts.yml');
~~~

Make sure what you `~/.ssh/config` file contains information about your domains and how to connect.
Or you can specify that information in `deploy.php` file itself.

~~~php
host('domain.com')
    ->user('name')
    ->port(22)
    ->configFile('~/.ssh/config')
    ->identityFile('~/.ssh/id_rsa')
    ->forwardAgent(true)
    ->multiplexing(true)
    ->addSshOption('UserKnownHostsFile', '/dev/null')
    ->addSshOption('StrictHostKeyChecking', 'no');
~~~

> **Best practice** is to leave connecting information for hosts in `~/.ssh/config` file.
> That way your allow different users connect in different way.

### Overriding config per host

For example, if you have some global configuration you can override it per host:

~~~php
set('branch', 'master');

host('prod')
    ...
    ->set('branch', 'production');
~~~

You can branch production will be only on `prod` host, on other – master.

### Gathering host info

Inside task, you can get host config with `get` function and host object with `host` function.

~~~php
task('...', function () {
    $deployPath = get('deploy_path');
    
    $host = host('domain.com');
    $port = $host->getPort();
});
~~~

### Multiple hosts

You can pass a few hosts to `host` function:

~~~php
host('110.164.16.59', '110.164.16.34', '110.164.16.50', ...)
    ->stage('production')
    ...
~~~

If your inventory `hosts.yml` file contains a few hosts, you can change configs for all of them in a same way.

~~~php
inventory('hosts.yml')
    ->roles('app')
    ...
~~~

### Host ranges

If you have a lot of hosts following similar patterns you can do this rather than listing each hostname:

~~~php
host('www[01:50].domain.com');
~~~

For numeric patterns, leading zeros can be included or removed, as desired. Ranges are inclusive. 

You can also define alphabetic ranges:

~~~php
host('db[a:f].domain.com');
~~~

### Localhost

If you need to build your release before deploy on local machine, or deploy to localhost instead of remote,
you need to define localhost:

~~~php
localhost()
    ->stage('production')
    ->roles('test', 'build')
    ...
~~~

### Host aliases

If you want to deploy same app to one host, but for example in different directories, you can describe to hosts aliases:

~~~php
host('domain.com/green', 'domain.com/blue')
    ->set('deploy_path', '~/{{hostname}}')
    ...
~~~

For Deployer those hosts are different ones, and after deploy to both on server will be next directory structure:

~~~
~
└── domain.com
    ├── green
    │   └── ...
    └── blue
        └── ...
~~~

### Inventory file

Include hosts defined in inventory files `hosts.yml` by `inventory` function:

~~~php
inventory('hosts.yml');
~~~

Example of an inventory file `hosts.yml` with full set of configuration:

~~~yaml
domain.com:
  hostname: domain.com
  user: name
  port: 22
  configFile: ~/.ssh/config
  identityFile: ~/.ssh/id_rsa
  forwardAgent: true
  multiplexing: true
  sshOptions:
    UserKnownHostsFile: /dev/null
    StrictHostKeyChecking: no
  stage: production
  roles:
    - app
    - db
  deploy_path: ~/app
  extra_param: "foo {{hostname}}"
~~~

> **Note** what same as with `host` function in *deploy.php* file it's better to omit information such as 
> *user*, *port*, *identityFile*, *forwardAgent* and put it to `~/.ssh/config` file instead.

If you inventory file contains many similar hosts definition, you can use YAML extend syntax:

~~~yaml
.base: &base
  roles: app
  deploy_path: ~/app
  ...

www1.domain.com:
  <<: *base
  stage: production
  
beta1.domain.com:
  <<: *base
  stage: beta
    
...
~~~

Hosts what started with `.` (*dot*) called are hidden and does not visible outside that file.
 
To define localhost in inventory files add `local` key:

~~~yaml
localhost:
  local: true
  roles: build
  ...
~~~

### Become

Deployer allows you to ‘become’ another user, different from the user that logged into the machine (remote user).

~~~php
host('domain.com')
    ->become('deployer')
    ...
~~~

Deployer uses `sudo` privilege escalation method by default.

> **Note** what currently become doesn't work with `tty` run option.

Next: [deployment flow](flow.md).
