# Debugging

Deployer has a set of predefined options for debugging.

Options can be called  like this:

``` sh
$ dep -option deploy
```

### Option List
* *without option* : *VERBOSITY_NORMAL*
* **-v** : *VERBOSITY_VERBOSE*
* **-vv** : *VERBOSITY_VERY_VERBOSE*
* **-vvv** : *VERBOSITY_DEBUG*
* **-q** : *VERBOSITY_QUIET* (in quiet mode, deployer will use your default answer for ask and askConfirmation function. [More details](https://github.com/deployphp/docs/blob/master/functions.md#functions))