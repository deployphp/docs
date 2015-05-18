# Environment

Every server has it's own independet enviroment, which contains information such as deploy path, release path, stages, etc.

To get current environment in a task, use the `env()` function.

~~~ php
task('my_task', function () {
    env(...);
});
~~~

You can you environment variable in your run commands with `{{...}}`:

~~~ php
run("cd {{deploy_path}} && ln -sfn {{release_path}} current");
~~~

To set an environment variable:

~~~ php
env('key', 'value');
~~~

To get an environment variable:

~~~ php
env('key');
~~~

To set global variables (not enviroment), use `set` and `get`:

~~~ php
set('key', 'value');

get('key');
~~~

To set default enviroment variable, you can define it in `deploy.php`:

~~~ php
env('deploy_path', '/home/www');
~~~

You can also setup a callback for enviroment variable which will be called of first access to it.

~~~ php
/**
 * Return current release path.
 */
env('current', function () {
    return run("readlink {{deploy_path}}/current")->toString();
});
~~~
