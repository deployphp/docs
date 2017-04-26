# Dealing with IO in parallel mode

If you try to make task which will be asking user, for example about a branch,
but you still want to use parallel deploy, you may notice what it's now working and program doesn't waits for used input.

To workaround this problem you need to create a local task and ask user about branch there:

~~~php
task('what_branch', function () {
    $branch = ask('What branch to deploy?');

    on(roles('app'), function ($host) use ($branch) {
        set('branch', $branch);
    });
})->local();
~~~

And call this task before `deploy` task:

~~~php
before('deploy', 'what_branch');
~~~

Now it should work as expected and user will be asked for branch only once.

~~~sh
$ dep deploy -p
➤ Executing task what_branch
What branch to deploy? master
✔ Ok
✔ Executing task deploy:prepare
✔ Executing task deploy:release
...
~~~
