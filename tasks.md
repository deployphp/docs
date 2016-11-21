# Tasks

Define you own tasks, use the `task` function. Also you can setup description for task with `desc` function:

```php
desc('My task');
task('my_task', function () {
    run(...);
});
```

To run your tasks:

```sh
dep my_task
```

To list all available commands:

```sh
dep list
```

To run a task only on a specified server or stage:

```sh
dep deploy main
```

If you task contains only `run` calls or just one bash command, you can simplify task definition:

```php
task('build', 'npm build');
```

Or you can use multi line script:
 
```php
task('build', '
    gulp build;
    webpack -p;
    echo "Build done";
');
```

### Task grouping

You can combine tasks in groups:

```php
task('deploy', [
    'deploy:prepare',
    'deploy:update_code',
    'deploy:vendors',
    'deploy:symlink',
    'cleanup'
]);
```

### Before and after

You can define tasks to be run before or after some tasks.

``` php
task('deploy:done', function () {
    write('Deploy done!');
});

after('deploy', 'deploy:done');
```

After the `deploy` task is be called, `deploy:done` will be executed.

### Only on

You can specify on which server to run task with `onlyOn` method:

``` php
desc('Run tests for application.');
task('test', function () {
    ...
})->onlyOn('test_server');
```

Also you can specify a group on servers to run as arguments: `onlyOn('server1', 'server2', ...)` or as an array `onlyOn(['server1', 'server2, ...])`.

To run task only on specified stages use `onlyOnStage`:

```php
task('notify', function () {
    ...
})->onlyOnStage('prod');
```

### Once

Mark task `once` to run it locally and only one time, independent of servers count.

```php
task('notify', function () {
    ...
})->once();
```

> Note what calling `run` inside that task will have same effect as calling `runLocally`. 

### Using input options

You can define additional input options and arguments, before defining tasks:

``` php
use Symfony\Component\Console\Input\InputOption;
use Symfony\Component\Console\Input\InputArgument;

argument('stage', InputArgument::OPTIONAL, 'Run tasks only on this server or group of servers.');
option('tag', null, InputOption::VALUE_OPTIONAL, 'Tag to deploy.');
```

To get the input inside a task this can be used:

``` php
task('foo:bar', function() {
    // For arguments
    $stage = null;
    if (input()->hasArgument('stage')) {
        $stage = input()->getArgument('stage');
    }
    
    // For option
    $tag = null;
    if (input()->hasOption('tag')) {
        $tag = input()->getOption('tag');
    }
}
```

Next: [servers configuration](servers.md).
