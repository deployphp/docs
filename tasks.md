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

~~~ sh
dep my_task
~~~

To list all available commands:

~~~ sh
dep list
~~~

You can give your tasks a description with the `desc` method:

~~~ php
task('my_task', function () {
    // Your tasks code...
})->desc('Doing my stuff');
~~~

When your task will be running, you will see the description in the output:

~~~ sh
$ dep my_task
✔ Executing task my_task
~~~

To get help on a task:

~~~ sh
dep help deploy
~~~

To run a task only on a specified server:

~~~ sh
dep deploy main
~~~


### Group tasks

You can combine tasks in groups:

~~~ php
task('deploy', [
    'deploy:prepare',
    'deploy:update_code',
    'deploy:vendors',
    'deploy:symlink',
    'deploy:cleanup'
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

### Using input options

You can define additional input options and arguments:

~~~ php
argument('stage', InputArgument::OPTIONAL, 'Run tasks only on this server or group of servers.');

option('tag', null, InputOption::VALUE_OPTIONAL, 'Tag to deploy.');
~~~
