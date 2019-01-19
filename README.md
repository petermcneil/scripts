# Scripts to improve daily life
This is just a collection of useful scripts that I have built up over time (so far 2!). They usually are for automating common tasks such as switching out of a proxy and updating multi-module git projects.

1. update
2. proxy
3. update_old

To install add the script to `/usr/local/bin/`.

## update
**Language**: python
**Tested on**: macOS, git, maven, gradle, sbt

`update` updates git projects with build systems. It looks for known files for the build system (i.e. maven -> pom.xml), updates from the remote repo, and runs the build command.


##  update_old

**Language**: _bash_

**Requirements**: macOS, git, maven _(Optional)_, gradle _(Optional)_


*update_old* is a script that will pull down git changes from the remote and run either maven or gradle or none!

It works by checking the latest commit on the up-stream branch that you are on and comparing it with your latest local commit. If they are different it will perform a git pull.

By default it will change your project to the `master` branch and change you back to your previous branch. However it can stay on the same branch with the flag `-s`. You may also force an update of the git project with `-f`.

If a project doesn't contain a `maven` or `gradle` project, you can update the `avoid` array with the name of the project and the script will not run either in the project.

**Arguments**:

* `-g` - Run Gradle in the project
* `-m` - Run Maven in the project
* `-c` - Toggle off changing back to the branch you were on, i.e. permanently checkout `master`
* `-f` - Force a git update
* `-s` - Stay on the branch you were on

## proxy

**Language**: _Python_

**Requirements**: macOS, _sudo_ 

_proxy_ is an extendable script that changes your `network location` and updates files that are affected by the proxy.

It is based on the network location name. Therefore only works if there are locations with names that change depending on whether you are **on** the proxy or **off** the proxy.

Files are updated by using the dictionary called `files`. In there you specify the location of the file and the on/off text to replace inside of it. For example:

```python
files= {
	'spotify' : {
		'location' : '{}/Library/Application Support/Spotify/prefs'.format(home),
		'on'  : 'network.proxy.mode=2',
		'off' : 'network.proxy.mode=1'
	}
}
```

If you update the file you may also need to reload the program associated with it. In this case you will need to update the `reload_process` array with the name of the program.

Another handy feature of this script is that it will update all running terminals with `source ~/.profile`. As this is where I put all my proxy config, it has proven useful to me as it doesn't require updating each tab etc. Terminals that are supported are _iTerm2_ and _Terminal_ but is easily expandable to other terminals with a new AppleScript.

**Configuration**

* `proxy_name` - The name of the proxy location **without** the changing part.
* `on` - A string containing the **on** state of the proxy.
* `off` - A string containing the **off** state of the proxy.
* `reload_command` - The command send to all open terminals.
* `files` - Add any files that need to be updated on a proxy change.
* `reload_process` - Add the name of the process that needs to be reloaded on proxy change.
* `terminals` - Add a terminal with the **key** as the terminal name and the **value** as an AppleScript to run the reload command.
