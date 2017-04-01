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

To run a task only on a specified host or stage:

```sh
dep deploy main
```

You can specify host via `--hosts` option (comma separated for a few) and roles via `--roles` option:

```sh
dep deploy --hosts domain.com
dep deploy --roles app
```

### Simple tasks

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

### Filtering

You can specify on which hosts/stages/roles to run a task.

### By stage

Filter hosts by stage:

``` php
desc('Run tests for application');
task('test', function () {
    ...
})->onStage('test');
```

### By roles

Filter tasks by roles:

``` php
desc('Migrate database');
task('migrate', function () {
    ...
})->onRoles('db');
```

Also you can specify a few roles: `onRoles('app', 'db', ...)`.

### By hosts

Filter tasks by roles:

``` php
desc('Migrate database');
task('migrate', function () {
    ...
})->onHosts('db.domain.com');
```

Also you can specify a few hosts: `onHosts('db.domain.com', ...)`.

### Local tasks

Mark task `local` to run it locally and only one time, independent of hosts count.

```php
task('build', function () {
    ...
})->local();
```

> Note what calling `run` inside local task will have same effect as calling `runLocally`. 

### Reconfigure

You can reconfigure tasks, e.g. provided by 3rd part recipes by retrieving them by name:

```php
task('notify')->onStage('production');
```

### Using input options

You can define additional input options and arguments, before defining tasks:

``` php
use Symfony\Component\Console\Input\InputOption;
use Symfony\Component\Console\Input\InputArgument;

argument('stage', InputArgument::OPTIONAL, 'Run tasks only on this host or stage.');
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

### Parallel task execution

When deploying to multiple hosts, Deployer will run by one task on each host:

<svg width="600" height="350" viewBox="0 0 600 350" xmlns="http://www.w3.org/2000/svg"><g fill="none" fill-rule="evenodd"><g transform="translate(456 309)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="42" y="24">task 2</tspan></text></g><g transform="translate(306 271)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="42" y="24">task 2</tspan></text></g><g transform="translate(156 233)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="42" y="24">task 2</tspan></text></g><g transform="translate(6 195)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="43" y="24">task 2</tspan></text></g><g transform="translate(456 157)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="41" y="24">task 1</tspan></text></g><g transform="translate(306 119)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="41" y="24">task 1</tspan></text></g><g transform="translate(156 81)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="41" y="24">task 1</tspan></text></g><g transform="translate(6 43)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="41" y="24">task 1</tspan></text></g><path d="M3 35h594.5" stroke="#EBEBEB" stroke-linecap="square" stroke-dasharray="3,5"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="497" y="25">Host 4</tspan></text><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="347" y="25">Host 3</tspan></text><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="197" y="25">Host 2</tspan></text><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="47" y="25">Host 1</tspan></text></g></svg>

To speedup deployment add `--parallel` or `-p` option with will run tasks in parallel on each host. If one of host executing task longer then another, Deployer will wait until all host finish tasks.

<svg width="600" height="153" viewBox="0 0 600 153" xmlns="http://www.w3.org/2000/svg"><g fill="none" fill-rule="evenodd"><g transform="translate(456 91)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="42" y="24">task 2</tspan></text></g><g transform="translate(306 91)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="42" y="24">task 2</tspan></text></g><g transform="translate(156 91)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="42" y="24">task 2</tspan></text></g><g transform="translate(6 91)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="43" y="24">task 2</tspan></text></g><g transform="translate(456 43)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="41" y="24">task 1</tspan></text></g><g transform="translate(306 43)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="41" y="24">task 1</tspan></text></g><g transform="translate(156 43)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="41" y="24">task 1</tspan></text></g><g transform="translate(6 43)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="41" y="24">task 1</tspan></text></g><path d="M3 35h594.5" stroke="#EBEBEB" stroke-linecap="square" stroke-dasharray="3,5"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="497" y="25">Host 4</tspan></text><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="347" y="25">Host 3</tspan></text><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="197" y="25">Host 2</tspan></text><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="47" y="25">Host 1</tspan></text></g></svg>

Limit the number of concurrent tasks by specify a number. By default, up to 10 tasks will be processed concurrently.
  
```sh
dep deploy --parallel 2
```

<svg width="600" height="210" viewBox="0 0 600 210" xmlns="http://www.w3.org/2000/svg"><g fill="none" fill-rule="evenodd"><g transform="translate(456 157)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="42" y="24">task 2</tspan></text></g><g transform="translate(306 157)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="42" y="24">task 2</tspan></text></g><g transform="translate(156 119)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="42" y="24">task 2</tspan></text></g><g transform="translate(6 119)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="43" y="24">task 2</tspan></text></g><g transform="translate(456 81)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="41" y="24">task 1</tspan></text></g><g transform="translate(306 81)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="41" y="24">task 1</tspan></text></g><g transform="translate(156 43)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="41" y="24">task 1</tspan></text></g><g transform="translate(6 43)"><rect fill="#EBEBEB" width="140" height="37.176" rx="8"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="41" y="24">task 1</tspan></text></g><path d="M3 35h594.5" stroke="#EBEBEB" stroke-linecap="square" stroke-dasharray="3,5"/><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="497" y="25">Host 4</tspan></text><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="347" y="25">Host 3</tspan></text><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="197" y="25">Host 2</tspan></text><text font-family="Monaco" font-size="16" fill="#9B9B9B"><tspan x="47" y="25">Host 1</tspan></text></g></svg>

Next: [hosts](hosts.md).
