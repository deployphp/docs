# Tasks

You can define your own tasks in the `deploy.php` file.
When you run the `dep` command, Deployer will scan the current directory for a `deploy.php` file and use it.

To define you own tasks, use the `task` function:

``` php
task('my_task', function () {
    // Your tasks code...
});
```

To run your tasks:

``` sh
$ dep my_task
```

To list all available commands:

``` sh
$ dep list
```

You can give your tasks a description with the `desc` method:

``` php
task('my_task', function () {
    // Your tasks code...
})->desc('Doing my stuff');
```

When your task will be running, you will see the description in the output:

``` sh
$ dep my_task
✔ Executing task my_task
```

To get help on a task:

``` sh
$ dep help deploy
```

To run a task only on a specified server:

``` sh
$ dep deploy main
```


### Group tasks

You can combine tasks in groups:

``` php
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
task('test', function () {
    ...
})->desc('Run tests for application.')
  ->onlyOn('test_server');
```

Also you can specify a group on servers to run as arguments: `->onlyOn('server1', 'server2', ...);` or as an array `->onlyOn(['server1', 'server2, ...]);`

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
