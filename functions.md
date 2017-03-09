# Functions

Deployer provides a lot of helpful functions. Make sure to learn about them!

```php
run(string $command)
```

Run command on remote server.

```php
cd(string $path)
```

Sets the working path for the following `run` functions. 
Every task restores the working path to the base working path at the beginning of the task.

```php
runLocally(string $command, int $timeout = 60)
```

Runs a command on your local machine.
Default timeout: 60 seconds

```php
upload(string $file, string $uploadFile)
```

Upload a file from your machine to your deployment machine
You can use environment in both $file and $uploadFile - meaning you can type something like this

upload('.deploy/parameters.{{env}}.yml', '{{release_path}}/app/config/parameters.yml');

Which will be translated to something like this

upload('.deploy/parameters.dev.yml', '/var/www/release/current/app/config/parameters.yml');

```php
download(string $localFile, string $deploymentFile)
```

Download a file from your deployment machine to your machine


```php
write(string $message)
```

Write message in the console. You can format the message with the tags `<info>...</info>`, `<comment></comment>` or `<error></error>` (see [Symfony Console](http://symfony.com/doc/current/components/console/introduction.html#coloring-the-output)).

```php
writeln(string $message)
```

Same as the `write` function, but also writes a new line.

```php
ask(string $message, mixed $default)
```

Ask the user for input. You need to specify a default value which will be used in quiet mode.

```php
askChoice(string $message, array $availableChoices, mixed $default, bool $multiselect)
```

Ask the user to select from multiple key/value options and return an array. Multiselect enables selection of multiple comma separated choices. Default value will be used in quiet mode otherwise the first available choice will be accepted.

```php
askConfirmation(string $message[, bool $default = false])
```

Ask the user a yes or no question.

```php
askHiddenResponse(string $message)
```

Ask the user for a password.

```php
output()
```

Get the current console output.
