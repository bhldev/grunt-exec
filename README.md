[![build status](https://secure.travis-ci.org/jharding/grunt-exec.png?branch=master)](http://travis-ci.org/jharding/grunt-exec)
grunt-exec
==========

Grunt plugin for executing shell commands.

[![NPM](https://nodei.co/npm/grunt-exec.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/grunt-exec/)
[![NPM](https://nodei.co/npm-dl/grunt-exec.png)](https://nodei.co/npm/grunt-exec/)

Installation
------------

Install grunt-exec using npm:

```
$ npm install grunt-exec --save-dev
```

Then add this line to your project's *Gruntfile.js*:

```javascript
grunt.loadNpmTasks('grunt-exec');
```

Usage
-----

This plugin is a [multi task][types_of_tasks], meaning that grunt will 
automatically iterate over all exec targets if a target is not specified.

If the exit code generated by the specified shell command is greater than 0, 
grunt-exec will assume an error has occurred and will abort grunt immediately.

[types_of_tasks]: http://gruntjs.com/configuring-tasks#task-configuration-and-targets

### Properties

*  __command__ (alias: __cmd__): The shell command to be executed. Must be a 
  string or a function that returns a string.
*  __stdin__: If `true`, stdin will be redirected from the child process to the current process allowing user interactivity (EXPERIMENTAL)
*  __stdout__: If `true`, stdout will be printed. Defaults to `true`.
*  __stderr__: If `true`, stderr will be printed. Defaults to `true`.
*  __cwd__: Current working directory of the shell command. Defaults to the 
  directory containing your Gruntfile.
*  __exitCode__ (alias: __exitCodes__): The expected exit code(s), task will 
  fail if the actual exit code doesn't match. Defaults to `0`. Can be an array 
  for multiple allowed exit codes.
*  __callback__: The callback function passed `child_process.exec`. Defaults to 
  a noop.
* __callbackArgs__: Additional arguments to pass to the callback. Defaults to empty array.
* __sync__: Whether to use `child_process.spawnSync`. Defaults to false.
* __options__: Options to provide to `child_process.exec`. [NodeJS Documentation](http://nodejs.org/api/child_process.html#child_process_child_process_exec_command_options_callback)
  - `cwd` String Current working directory of the child process
  - `env` Object Environment key-value pairs
  - `encoding` String *(Default: 'utf8')*
  - `shell` String Shell to execute the command with *(Default: '/bin/sh' on UNIX, 'cmd.exe' on Windows, The shell should understand the -c switch on UNIX or /s /c on Windows. On Windows, command line parsing should be compatible with cmd.exe.)*
  - `timeout` Number *(Default: 0)*
  - `maxBuffer` Number largest amount of data (in bytes) allowed on stdout or stderr - if exceeded child process is killed **(Default: 200*1024)**
  - `killSignal` String *(Default: 'SIGTERM')*
  - `uid` Number Sets the user identity of the process. (See [setuid(2)](http://man7.org/linux/man-pages/man2/setuid.2.html).)
  - `gid` Number Sets the group identity of the process. (See [setgid(2)](http://man7.org/linux/man-pages/man2/setgid.2.html).)

If the configuration is instead a simple `string`, it will be
interpreted as a full command itself:

```javascript
exec: {
  echo_something: 'echo "This is something"'
}
```

### Command Functions

If you plan on doing advanced stuff with grunt-exec, you'll most likely be using 
functions for the `command` property of your exec targets. This section details 
a couple of helpful tips about command functions that could help make your life 
easier.

#### Passing arguments from the command line

Command functions can be called with arbitrary arguments. Let's say we have the 
following exec target that echoes a formatted name:

```javascript
exec: {
  echo_name: {
    cmd: function(firstName, lastName) {
      var formattedName = [
        lastName.toUpperCase(),
        firstName.toUpperCase()
      ].join(', ');

      return 'echo ' + formattedName;
    }
  }
}
```

In order to get `SIMPSON, HOMER` echoed, you'd run 
`grunt exec:echo_name:homer:simpson` from the command line.

### Accessing `grunt` object

All command functions are called in the context of the `grunt` object that they 
are being ran with. This means you can access the `grunt` object through `this`.

### Example

The following examples are available in grunt-exec's Gruntfile.

```javascript
grunt.initConfig({
  exec: {
    remove_logs: {
      command: 'rm -f *.log',
      stdout: false,
      stderr: false
    },
    list_files: {
      cmd: 'ls -l **'
    },
    list_all_files: 'ls -la',
    echo_grunt_version: {
      cmd: function() { return 'echo ' + this.version; }
    },
    echo_name: {
      cmd: function(firstName, lastName) {
        var formattedName = [
          lastName.toUpperCase(),
          firstName.toUpperCase()
        ].join(', ');

        return 'echo ' + formattedName;
      }
    }
  }
});
```

Testing
-------

```
$ cd grunt-exec
$ npm test
```

Issues
------

Found a bug? Create an issue on GitHub.

https://github.com/jharding/grunt-exec/issues

Versioning
----------

For transparency and insight into the release cycle, releases will be numbered 
with the follow format:

`<major>.<minor>.<patch>`

And constructed with the following guidelines:

* Breaking backwards compatibility bumps the major
* New additions without breaking backwards compatibility bumps the minor
* Bug fixes and misc changes bump the patch

For more information on semantic versioning, please visit http://semver.org/.

License
-------

Original Copyright (c) 2012-2014 [Jake Harding](http://thejakeharding.com)
Copyright (c) 2016 grunt-exec
Licensed under the [MIT License](http://www.opensource.org/licenses/mit-license.php).
