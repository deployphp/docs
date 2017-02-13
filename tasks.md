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

Also you can specify a group of servers to run as arguments: `onlyOn('server1', 'server2', ...)` or as an array `onlyOn(['server1', 'server2', ...])`.

To run task only on specified stages use `onlyForStage`:

```php
task('notify', function () {
    ...
})->onlyForStage('prod');
```

Also you can specify a group of stages to run as arguments: `onlyForStage('stage1', 'stage2', ...)` or as an array `onlyForStage(['stage1', 'stage2', ...])`.

### Once

Mark task `once` to run it locally and only one time, independent of servers count.

```php
task('notify', function () {
    ...
})->once();
```

> Note what calling `run` inside that task will have same effect as calling `runLocally`. 

### Reconfigure

You can reconfigure tasks, e.g. provided by 3rd part recipes by retrieving them by name:

```php
task('notify')->onlyOn([
  'firstserver',
  'thirdserver',
]);
```

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

### Parallel task execution

When deploying to multiple server, Deployer will run by one task on each server:

<svg width="600" height="305" viewBox="990 42 600 305" xmlns="http://www.w3.org/2000/svg">
  <g fill="none" fill-rule="evenodd">
    <path d="M996.726 67.258h141.256v275.34H996.726V67.26zM990 63h600v283.857H990V63zm154.71 4.258h141.254v275.34H1144.71V67.26zm147.98 0h141.256v275.34H1292.69V67.26zm148.655 0H1582.6v275.34h-141.255V67.26z" fill="#D8D8D8"/>
    <g transform="translate(1000.762 73.09)">
      <rect fill="#B8E986" width="134.529" height="30.942" rx="8"/>
      <text font-family="HelveticaNeue-Light, Helvetica Neue" font-size="18" font-weight="300" fill="#000">
        <tspan x="41.031" y="22.363">Task 1</tspan>
      </text>
    </g>
    <g transform="translate(1148.744 104.704)">
      <rect fill="#B8E986" width="134.529" height="30.942" rx="8"/>
      <text font-family="HelveticaNeue-Light, Helvetica Neue" font-size="18" font-weight="300" fill="#000">
        <tspan x="41.031" y="22.363">Task 1</tspan>
      </text>
    </g>
    <g transform="translate(1296.726 138.336)">
      <rect fill="#B8E986" width="134.529" height="30.942" rx="8"/>
      <text font-family="HelveticaNeue-Light, Helvetica Neue" font-size="18" font-weight="300" fill="#000">
        <tspan x="41.031" y="22.363">Task 1</tspan>
      </text>
    </g>
    <g transform="translate(1444.71 169.95)">
      <rect fill="#B8E986" width="134.529" height="30.942" rx="8"/>
      <text font-family="HelveticaNeue-Light, Helvetica Neue" font-size="18" font-weight="300" fill="#000">
        <tspan x="41.031" y="22.363">Task 1</tspan>
      </text>
    </g>
    <g>
      <g transform="translate(1000.09 209.637)">
        <rect fill="#B8E986" width="134.529" height="30.942" rx="8"/>
        <text font-family="HelveticaNeue-Light, Helvetica Neue" font-size="18" font-weight="300" fill="#000">
          <tspan x="41.031" y="22.363">Task 2</tspan>
        </text>
      </g>
      <g transform="translate(1148.072 241.25)">
        <rect fill="#B8E986" width="134.529" height="30.942" rx="8"/>
        <text font-family="HelveticaNeue-Light, Helvetica Neue" font-size="18" font-weight="300" fill="#000">
          <tspan x="41.031" y="22.363">Task 2</tspan>
        </text>
      </g>
      <g transform="translate(1296.054 274.883)">
        <rect fill="#B8E986" width="134.529" height="30.942" rx="8"/>
        <text font-family="HelveticaNeue-Light, Helvetica Neue" font-size="18" font-weight="300" fill="#000">
          <tspan x="41.031" y="22.363">Task 2</tspan>
        </text>
      </g>
      <g transform="translate(1444.036 306.498)">
        <rect fill="#B8E986" width="134.529" height="30.942" rx="8"/>
        <text font-family="HelveticaNeue-Light, Helvetica Neue" font-size="18" font-weight="300" fill="#000">
          <tspan x="41.031" y="22.363">Task 2</tspan>
        </text>
      </g>
    </g>
    <path d="M994.73 205.292h588.942" stroke="#979797"/>
    <g font-size="18" font-family="HelveticaNeue-Light, Helvetica Neue" fill="#000" font-weight="300">
      <text transform="translate(1031 42)">
        <tspan x="0" y="17">Server 1</tspan>
      </text>
      <text transform="translate(1031 42)">
        <tspan x="146" y="17">Server 2</tspan>
      </text>
      <text transform="translate(1031 42)">
        <tspan x="304" y="17">Server 3</tspan>
      </text>
      <text transform="translate(1031 42)">
        <tspan x="450" y="17">Server 4</tspan>
      </text>
    </g>
  </g>
</svg>

To speedup deployment add `--parallel` or `-p` option with will run tasks in parallel on each server. If one of server executing task longer then another, Deployer will wait until all servers finish tasks.

<svg width="600" height="305" viewBox="990 418 600 305" xmlns="http://www.w3.org/2000/svg">
  <g fill="none" fill-rule="evenodd">
    <path d="M996.726 443.258h141.256v275.34H996.726V443.26zM990 439h600v283.857H990V439zm154.71 4.258h141.254v275.34H1144.71V443.26zm147.98 0h141.256v275.34H1292.69V443.26zm148.655 0H1582.6v275.34h-141.255V443.26z" fill="#D8D8D8"/>
    <g transform="translate(1000.762 449.09)">
      <rect fill="#B8E986" width="134.529" height="30.942" rx="8"/>
      <text font-family="HelveticaNeue-Light, Helvetica Neue" font-size="18" font-weight="300" fill="#000">
        <tspan x="41.031" y="22.363">Task 1</tspan>
      </text>
    </g>
    <g transform="translate(1148.762 449.09)">
      <rect fill="#B8E986" width="134.529" height="40.923" rx="8"/>
      <text font-family="HelveticaNeue-Light, Helvetica Neue" font-size="18" font-weight="300" fill="#000">
        <tspan x="41.031" y="24.093">Task 1</tspan>
      </text>
    </g>
    <g transform="translate(1296.762 449.09)">
      <rect fill="#B8E986" width="134.529" height="30.942" rx="8"/>
      <text font-family="HelveticaNeue-Light, Helvetica Neue" font-size="18" font-weight="300" fill="#000">
        <tspan x="41.031" y="22.363">Task 1</tspan>
      </text>
    </g>
    <g transform="translate(1444.762 449.09)">
      <rect fill="#B8E986" width="134.529" height="30.942" rx="8"/>
      <text font-family="HelveticaNeue-Light, Helvetica Neue" font-size="18" font-weight="300" fill="#000">
        <tspan x="41.031" y="22.363">Task 1</tspan>
      </text>
    </g>
    <g>
      <g transform="translate(999.09 498.637)">
        <rect fill="#B8E986" width="134.529" height="30.942" rx="8"/>
        <text font-family="HelveticaNeue-Light, Helvetica Neue" font-size="18" font-weight="300" fill="#000">
          <tspan x="41.031" y="22.363">Task 2</tspan>
        </text>
      </g>
      <g transform="translate(1149.09 498.637)">
        <rect fill="#B8E986" width="134.529" height="30.942" rx="8"/>
        <text font-family="HelveticaNeue-Light, Helvetica Neue" font-size="18" font-weight="300" fill="#000">
          <tspan x="41.031" y="22.363">Task 2</tspan>
        </text>
      </g>
      <g transform="translate(1296.09 498.637)">
        <rect fill="#B8E986" width="134.529" height="30.942" rx="8"/>
        <text font-family="HelveticaNeue-Light, Helvetica Neue" font-size="18" font-weight="300" fill="#000">
          <tspan x="41.031" y="22.363">Task 2</tspan>
        </text>
      </g>
      <g transform="translate(1446.09 498.637)">
        <rect fill="#B8E986" width="134.529" height="30.942" rx="8"/>
        <text font-family="HelveticaNeue-Light, Helvetica Neue" font-size="18" font-weight="300" fill="#000">
          <tspan x="41.031" y="22.363">Task 2</tspan>
        </text>
      </g>
    </g>
    <path d="M994.73 494.292h588.942" stroke="#979797"/>
    <g font-size="18" font-family="HelveticaNeue-Light, Helvetica Neue" fill="#000" font-weight="300">
      <text transform="translate(1031 418)">
        <tspan x="0" y="17">Server 1</tspan>
      </text>
      <text transform="translate(1031 418)">
        <tspan x="146" y="17">Server 2</tspan>
      </text>
      <text transform="translate(1031 418)">
        <tspan x="304" y="17">Server 3</tspan>
      </text>
      <text transform="translate(1031 418)">
        <tspan x="450" y="17">Server 4</tspan>
      </text>
    </g>
  </g>
</svg>

Next: [servers configuration](servers.md).
