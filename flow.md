# Flow

If your recipe based on *common* recipe or one of frameworks recipe shiped with Deployer, then you are using one of our default flows.
Each flow described as group task of other tasks in `deploy` name space. Common deploy flow may look like this:

```php
task('deploy', [
    'deploy:prepare',
    'deploy:lock',
    'deploy:release',
    'deploy:update_code',
    'deploy:shared',
    'deploy:writable',
    'deploy:vendors',
    'deploy:clear_paths',
    'deploy:symlink',
    'deploy:unlock',
    'cleanup',
    'success'
]);
```

Frameworks recipes may diff in flow, but basic structure is same. You can create your own flow by overriding `deploy` task, but better slution is to using cache. 
For example if you want to run some task before symlink new release:

```php
before('deploy:symlink', 'deploy:build');
```

Or to do some notifications after success deploy:

```php
after('success', 'notify');
```

In next section i will give short overview of each task. 

### deploy:prepare

Preeparation for deployment. Checks if `deploy_path` exists, otherwise create it. Also checks for existing of next paths:

* `releases` – in this dir will be stored releases.
* `shared` – shread files across all releases.
* `.dep` – metadata used by Deployer.

### deploy:lock

Locks deployment. So only one concurent deployment can running. To lock deployment, task check of existing `.dep/deploy.lock` file. In deploy process was canceld by Ctrl+C, run `dep deploy:unlock` t odelete this file. In case if deployment was filed `deploy:unlock` task will be triggered automatically. 
