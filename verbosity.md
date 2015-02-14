# Verbosity

Deployer has 3 levels of verbosity. To specify it, add one of following options to the `dep` command.

* `--quiet (-q)` Do not output any message.
* `--verbose (-v|vv|vvv)` Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug.

In your tasks, you can check the verbosity level with the following methods:

~~~ php
task('my_task', function () {
    if (output()->isQuiet()) {
        // ...
    }

    if (output()->isVerbose()) {
        // ...
    }

    if (output()->isVeryVerbose()) {
        // ...
    }

    if (output()->isDebug()) {
        // ...
    }
});
~~~
