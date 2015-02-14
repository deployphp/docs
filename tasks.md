# Tasks

You can define your own tasks in the `deploy.php` file.
When you run the `dep` command, Deployer will scan the current directory for a `deploy.php` file and use it.

To define you own tasks, use the `task` function:

~~~ php
task('my_task', function () {
    // Your tasks code...
});
~~~

To run your tasks:

~~~
dep my_task
~~~

To list all available commands:

~~~
dep list
~~~

You can give your tasks a description with the `desc` method:

~~~ php
task('my_task', function () {
    // Your tasks code...
})->desc('Doing my stuff');
~~~

When your task will be running, you will see the description in the output:

~~~
$ dep my_task
Doing my stuff..............................✔
~~~

To get help on a task:

~~~
dep help deploy
~~~

To run a task only on a specified server add the `--server` option:

~~~
dep deploy --server=main
~~~

### Group tasks

You can combine tasks in groups:

~~~ php
task('deploy', [
    'deploy:start',
    'deploy:prepare',
    'deploy:update_code',
    'deploy:vendors',
    'deploy:symlink',
    'cleanup',
    'deploy:end'
]);
~~~


### Before and after

You can define tasks to be run before or after some tasks.

~~~ php
task('deploy:done', function () {
    write('Deploy done!');
});

after('deploy', 'deploy:done');
~~~

After the `deploy` task is be called, `deploy:done` will be executed.

You may also use an anonymous function:

~~~ php
before('task', function () {
    // Do your stuff...
});
~~~

### Using input options

You can define additional input options by calling the `option` method on your defined tasks.

~~~ php
// Task->option(name, shortcut = null, description = '', default = null);

task('deploy:upload_code', function (InputInterface $input) {
    $branch = $input->getArgument('stage') !== 'production' ? $input->getOption('branch',get('branch', null)) : get('branch', null);
    ...
})->option('branch', 'b', 'Set the deployed branch', 'develop');


task('deploy', [
    ...
    'deploy:upload_code'
    ...
])->option('branch', 'b', 'Set the deployed branch', 'develop');
~~~

**define the option on the complete chain or else it will not be available to all of the tasks**
