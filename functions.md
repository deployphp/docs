# Functions

Deployer provides a lot of helpful functions. Make sure to learn about them!

~~~ php
run(string $command)
~~~

Runs a command on a remote server in the working path (`server(...)->env('deploy_path', '/home/path')`).

~~~ php
cd(string $path)
~~~

Sets the working path for the following `run` functions. Every task restores the working path to the base working path at the beginning of the task.

~~~ php
runLocally(string $command)
~~~

Runs a command on your local machine.

~~~ php
 write(string $message)
~~~

Write message in the console. You can format the message with the tags `<info>...</info>`, `<comment></comment>` or `<error></error>` (see [Symfony Console](http://symfony.com/doc/current/components/console/introduction.html#coloring-the-output).

~~~ php
 writeln(string $message)
~~~

Same as the `write` function, but also writes a new line.

~~~ php
ask(string $message, mixed $default)
~~~

Ask the user for input. You need to specify a default value which will be used in quiet mode.

~~~ php
askConfirmation(string $message[, bool $default = false])
~~~

Ask the user a yes or no question.

~~~ php
askHiddenResponse(string $message)
~~~

Ask the user for a password.

~~~ php
output()
~~~

Get the current console output.
