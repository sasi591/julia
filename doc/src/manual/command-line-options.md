# [Command-line Options](@id command-line-options)

## Using arguments inside scripts

When running a script using `julia`, you can pass additional arguments to your script:

```
$ julia script.jl arg1 arg2...
```

These additional command-line arguments are passed in the global constant `ARGS`. The
name of the script itself is passed in as the global `PROGRAM_FILE`. Note that `ARGS` is
also set when a Julia expression is given using the `-e` option on the command line (see the
`julia` help output below) but `PROGRAM_FILE` will be empty. For example, to just print the
arguments given to a script, you could do this:

```
$ julia -e 'println(PROGRAM_FILE); for x in ARGS; println(x); end' foo bar

foo
bar
```

Or you could put that code into a script and run it:

```
$ echo 'println(PROGRAM_FILE); for x in ARGS; println(x); end' > script.jl
$ julia script.jl foo bar
script.jl
foo
bar
```

The `--` delimiter can be used to separate command-line arguments intended for the script file from arguments intended for Julia:

```
$ julia --color=yes -O -- script.jl arg1 arg2..
```

See also [Scripting](@ref man-scripting) for more information on writing Julia scripts.

Julia can be started in parallel mode with either the `-p` or the `--machine-file` options. `-p n`
will launch an additional `n` worker processes, while `--machine-file file` will launch a worker
for each line in file `file`. The machines defined in `file` must be accessible via a password-less
`ssh` login, with Julia installed at the same location as the current host. Each machine definition
takes the form `[count*][user@]host[:port] [bind_addr[:port]]`. `user` defaults to current user,
`port` to the standard ssh port. `count` is the number of workers to spawn on the node, and defaults
to 1. The optional `bind-to bind_addr[:port]` specifies the IP address and port that other workers
should use to connect to this worker.

If you have code that you want executed whenever Julia is run, you can put it in
`~/.julia/config/startup.jl`:

```
$ echo 'println("Greetings! 你好! 안녕하세요?")' > ~/.julia/config/startup.jl
$ julia
Greetings! 你好! 안녕하세요?

...
```

Note that although you should have a `~/.julia` directory once you've run Julia for the
first time, you may need to create the `~/.julia/config` folder and the
`~/.julia/config/startup.jl` file if you use it.

## Command-line switches for Julia

There are various ways to run Julia code and provide options, similar to those available for the
`perl` and `ruby` programs:

```
julia [switches] -- [programfile] [args...]
```

The following is a complete list of command-line switches available when launching julia, e.g.


|Switch                                 |Description|
|:---                                   |:---|
|`-v`, `--version`                      |Display version information|
|`-h`, `--help`                         |Print command-line options (this message).|
|`--project[={<dir>\|@.}]`              |Set `<dir>` as the home project/environment. The default `@.` option will search through parent directories until a `Project.toml` or `JuliaProject.toml` file is found.|
|`-J`, `--sysimage <file>`              |Start up with the given system image file|
|`-H`, `--home <dir>`                   |Set location of `julia` executable|
|`--startup-file={yes\|no}`             |Load `~/.julia/config/startup.jl`|
|`--handle-signals={yes\|no}`           |Enable or disable Julia's default signal handlers|
|`--sysimage-native-code={yes\|no}`     |Use native code from system image if available|
|`--compiled-modules={yes\|no}`         |Enable or disable incremental precompilation of modules|
|`-e`, `--eval <expr>`                  |Evaluate `<expr>`|
|`-E`, `--print <expr>`                 |Evaluate `<expr>` and display the result|
|`-L`, `--load <file>`                  |Load `<file>` immediately on all processors|
|`-t`, `--threads {N\|auto`}            |Enable N threads; `auto` currently sets N to the number of local CPU threads but this might change in the future|
|`-p`, `--procs {N\|auto`}              |Integer value N launches N additional local worker processes; `auto` launches as many workers as the number of local CPU threads (logical cores)|
|`--machine-file <file>`                |Run processes on hosts listed in `<file>`|
|`-i`                                   |Interactive mode; REPL runs and `isinteractive()` is true|
|`-q`, `--quiet`                        |Quiet startup: no banner, suppress REPL warnings|
|`--banner={yes\|no\|auto}`             |Enable or disable startup banner|
|`--color={yes\|no\|auto}`              |Enable or disable color text|
|`--history-file={yes\|no}`             |Load or save history|
|`--depwarn={yes\|no\|error}`           |Enable or disable syntax and method deprecation warnings (`error` turns warnings into errors)|
|`--warn-overwrite={yes\|no}`           |Enable or disable method overwrite warnings|
|`-C`, `--cpu-target <target>`          |Limit usage of CPU features up to `<target>`; set to `help` to see the available options|
|`-O`, `--optimize={0,1,2,3}`           |Set the optimization level (default level is 2 if unspecified or 3 if used without a level)|
|`--min-optlevel={0,1,2,3}`             |Set the lower bound on per-module optimization (default is 0)|
|`-g`, `-g <level>`                     |Enable or set the level of debug info generation (default level is 1 if unspecified or 2 if used without a level)|
|`--inline={yes\|no}`                   |Control whether inlining is permitted, including overriding `@inline` declarations|
|`--check-bounds={yes\|no\|auto}`       |Emit bounds checks always, never, or respect `@inbounds` declarations|
|`--math-mode={ieee,fast}`              |Disallow or enable unsafe floating point optimizations (overrides `@fastmath` declaration)|
|`--code-coverage={none\|user\|all}`    |Count executions of source lines|
|`--code-coverage`                      |equivalent to `--code-coverage=user`|
|`--track-allocation={none\|user\|all}` |Count bytes allocated by each source line|
|`--track-allocation`                   |equivalent to `--track-allocation=user`|

!!! compat "Julia 1.1"
    In Julia 1.0, the default `--project=@.` option did not search up from the root
    directory of a Git repository for the `Project.toml` file. From Julia 1.1 forward, it
    does.
