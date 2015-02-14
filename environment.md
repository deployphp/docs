# Environment

To get current environment in a task, use the `env()` function.

~~~ php
task('my_task', function () {
    env()->get(...);
});
~~~

To set an environment variable:

~~~ php
env()->set('key', 'value');
~~~

To get an environment variable:

~~~ php
env()->get('key');
~~~

To get the current release path:

~~~ php
env()->getReleasePath();
~~~

To get the server configuration:

~~~ php
config();

// Which is the same as

env()->getConfig();
~~~

To set global Deployer variables (shared amongst servers), use `set` and `get`:

~~~ php
set('key', 'value');

get('key');
~~~
