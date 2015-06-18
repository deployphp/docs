# Environment

Every server has it's own independent environment, which contains information
such as deploy path, release path, stages, etc.

To get the current environment in a task, use the `env()` function.

``` php
task('my_task', function () {
    env(...);
});
```

You can insert your environment variables in your run commands with `{{...}}`:

``` php
run("cd {{deploy_path}} && ln -sfn {{release_path}} current");
```

To set an environment variable to a new value:

``` php
env('key', 'value');
```

To get the value of an environment variable:

``` php
env('key');
```

Adding a server by the `server` function automatically sets some environment
variables. These variables are protected from future changes, so that every
task can safely rely on them.

```php
// Gets the server array
env('server');

// Using the dot notation we can access arrays in a simpler way
env('server.name');
env('server.host');
env('server.port');
```

To handle global variables (not environment), use `set`, `get` and `has`:

``` php
set('key', 'value');
get('key');
has('key');
```

To set a default environment variable, you can define it in `deploy.php`:

``` php
env('deploy_path', '/home/www');
```

You can also setup a callback for an environment variable which will be called
when it's first accessed.

``` php
/**
 * Return current release path.
 */
env('current', function () {
    return run("readlink {{deploy_path}}/current")->toString();
});
```
