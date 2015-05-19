# Environment

Every server has it's own independent environment, which contains information such as deploy path, release path, stages, etc.

To get current the environment in a task, use the `env()` function.

~~~ php
task('my_task', function () {
    env(...);
});
~~~

You can insert your environment variables in your run commands with `{{...}}`:

~~~ php
run("cd {{deploy_path}} && ln -sfn {{release_path}} current");
~~~

To set an environment variable to a new value:

~~~ php
env('key', 'value');
~~~

To get the value of an environment variable:

~~~ php
env('key');
~~~

To set global variables (not enviroment), use `set` and `get`:

~~~ php
set('key', 'value');

get('key');
~~~

To set a default environment variable, you can define it in `deploy.php`:

~~~ php
env('deploy_path', '/home/www');
~~~

You can also setup a callback for an environment variable which will be called when it is first accessed.

~~~ php
/**
 * Return current release path.
 */
env('current', function () {
    return run("readlink {{deploy_path}}/current")->toString();
});
~~~
