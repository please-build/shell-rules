# Shell rules
Shell rules for the Please build system

## Usage

The easiest way to get set up is to run `plz init plugin shell`. This will install
the plugin and automatically make the shell rules available in every BUILD file. 

Alternatively add the following to `plugins/BUILD`:
```python
plugin_repo(
  name = "shell",
  version = <latest version here>,
)
```

And add the following to your `.plzconfig`:
```
[Plugin "shell"]
Target = //plugins:shell
```

And then use `subinclude("///shell//build_defs:shell")` to make the rules available in 
a BUILD file. 

## sh_cmd()

The `sh_cmd()` rule can be used to run a bash command:
```python
subinclude("///shell//build_defs:shell")

sh_cmd(name="hello_world", cmd="echo Hello, world!")
```

```
$ plz run //:hello_world
Hello, world!
```

## sh_binary()

The `sh_binary()` rules is similar to `sh_cmd()` however it takes in a `.sh` file as a source:
```python
subinclude("///shell//build_defs:shell")

sh_binary(
  name = "hello_world",
  srcs = ["hello.sh"],
)
```

And then add `hello.sh`:
```
#!/bin/bash

echo Hello, world!
```

```
$ plz run //:hello_world
Hello, world!
```

## sh_test()

The `sh_rule()` works the same as sh binary, but can provide a test. The `.sh` file must indicate success or failure
via the exit code:

```python
sh_test(
  name = "test",
  src = "test.sh",
)
```

```
#!/bin/bash

echo "failure"
exit 1
```
