# CLI Usage

After [installation](installation.md) on deployer you will have an ability to run `dep` command from terminal.

To get list of all available task run `dep`. You can run it from any subdirectories of you project, 
deployer will automatically find project root dir. 
 
~~~bash
Deployer

Usage:
  command [options] [arguments]

Options:
  -h, --help            Display this help message
  -q, --quiet           Do not output any message
  -V, --version         Display this application version
      --ansi            Force ANSI output
      --no-ansi         Disable ANSI output
  -n, --no-interaction  Do not ask any interactive question
  -f, --file[=FILE]     Specify Deployer file
  -v|vv|vvv, --verbose  Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Available commands:
  help         Displays help for a command
  init         Initialize deployer system in your project
  list         Lists commands
  self-update  Updates deployer.phar to the latest version
  ssh          Connect to host through ssh
~~~

Best way to configure your `deploy.php` to automatically deploy to staging on this command:
 
~~~bash
dep deploy
~~~

So somebody can't accidentally deploy production (for production `dep deploy production` explicitly stage is required).

You get info about available options and usage use `help` command:
 
~~~bash
$ dep help deploy
Usage:
  deploy [options] [--] [<stage>]

Arguments:
  stage                      Stage or hostname

Options:
  -p, --parallel             Run tasks in parallel
  -l, --limit=LIMIT          How many host to run in parallel?
      --no-hooks             Run task without after/before hooks
      --log=LOG              Log to file
      --roles=ROLES          Roles to deploy
      --hosts=HOSTS          Host to deploy, comma separated, supports ranges [:]
  -h, --help                 Display this help message
  -q, --quiet                Do not output any message
  -V, --version              Display this application version
      --ansi                 Force ANSI output
      --no-ansi              Disable ANSI output
  -n, --no-interaction       Do not ask any interactive question
  -f, --file[=FILE]          Specify Deployer file
      --tag[=TAG]            Tag to deploy
      --revision[=REVISION]  Revision to deploy
      --branch[=BRANCH]      Branch to deploy
  -v|vv|vvv, --verbose       Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Help:
  Deploy your project
~~~  
