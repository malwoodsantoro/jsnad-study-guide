# jsnad-study-guide

Sections from Linux Foundation Node.js Application Development (LFW211) course

- [x] Introduction (completed 12/29)
- [x] Setting Up (completed 12/29)
- [x] Node Binary (completed 12/29)
- [ ] Debugging and Diagnostics 
- [ ] Key JavaScript Concepts
- [ ] Packages & Dependencies
- [ ] Node's Module Systems
- [ ] Asynchronous Control Flow
- [ ] Node's Event System
- [ ] Handling Errors
- [ ] Using Buffers
- [ ] Working with Streams
- [ ] Interacting with the File System
- [ ] Process & Operating System
- [ ] Creating Child Processes
- [ ] Writing Unit Tests

## ▶︎ Setting Up 

### Downloading Node 
❌ Downloading Node with package managers can lag behind and cause compatibility problems (i.e. Brew for MacOS)<br>
❌ Another significant issue with installing Node.js via an OS package manager is that installing global modules with Node's module installer (npm) tends to require the use of `sudo` (a command which grants root privileges)<br>
✅ Use Node version manager (nvm) `nvm install 20`

❔ Aside from the Node binary, what else does a Node installation provide?
 A module package manager 

## ▶︎ Node Binary 

### Command options
To see all Node command line flags for any version of Node, execute `node --help` and view the output. Beyond the Node command line flags there are additional flags for modifying the JavaScript runtime engine: V8. To view these flags run `node --v8-options`.

Check syntax using `--check or -c flag` (i.e. `node --check app.js`)

### Evaluate node in shell
Node can directly evaluate code from the shell. The `-p` or `--print` flag evaluates an expression and prints the result, the `-e` or `--eval` flag evaluates without printing the result of the expression.

The following will print 2:

`node --print "1+1"`

The following will not print anything because the expression is evaluated but not printed:

`node --eval "1+1"`

### Accessing modules
Usually a module would be required, like so: `require('fs')`, however all Node core modules can be accessed by their namespaces within the code evaluation context.

For example, the following would print all the files with a .js extension in the current working directory in which the command is run:

node -p "fs.readdirSync('.').filter((f) => /.js$/.test(f))"

### Stack Trace Limit
Set stack trace limit like `node --stack-trace-limit=99999 app.js`

Generally, the stack trace limit should stay at the default in production scenarios due to the overhead involved with retaining long stacks. It can nevertheless be useful for development purposes.

❔ Which flag allows a CommonsJS module to be preloaded? `-r` or `--require`
