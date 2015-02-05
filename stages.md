# Stages

You can define stages with `stage` function. Here is example of stage definition:

~~~ php
// stage(string name, array serverlist, array options = array(), bool default = true)
stage('development', array('development-server'), array('branch'=>'develop'), true);
stage('production', array('production-primary', 'production-secondary'), array('branch'=>'master'));
~~~

The first argument is the name of the stage, the second argument is an array of server names, the third argument is an array of options. Using the fourth (optional) argument you can define the default stage.

### Default stage

An alternative way of defining the default stage is with the `multistage` function:

~~~ php
multistage('develop');
~~~

### Deploying using stages

You can now pass the stage name as an extra argument:

```ssh
dep deploy production
```

If not provided, the default stage is used:

```ssh
# Use the default stage ("development" in this case)
dep deploy
```

### Options

Besides passing the option through the helper method, it is also possible to add them afterwards.

~~~ php
stage('production', array('production-server'))->options(array('branch'=>'master'));
~~~

It is also possible to set a specific option.

~~~ php
stage('production', array('production-server'))->set('branch','master');
~~~

The options will overwrite the ones set in your deploy.php and just like other options you can retrieve them by calling `get`.
