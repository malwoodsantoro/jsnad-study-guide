# Notes for JSNAD prep

Sections from Linux Foundation Node.js Application Development (LFW211) course

- [x] Introduction (completed 12/29)
- [x] Setting Up (completed 12/29)
- [x] Node Binary (completed 12/29)
- [x] Debugging and Diagnostics (completed 1/9)
- [-] Key JavaScript Concepts 
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

## ▶︎ Debugging and diagnostics 

### Inspect Mode 
Inspect mode can be enabled with the --inspect flag:

`node --inspect app.js`

For most cases however, it is better to cause the process to start with an active breakpoint at the very beginning of the program using the --inspect-brk flag:

`node --inspect-brk app.js` --> execution is paused at the first line of executable code (like function call below)

![image](https://github.com/malwoodsantoro/jsnad-study-guide/assets/19801577/5e1ae698-0c76-4b8d-81ef-d7cd046afdf6)

More debugging info: https://nodejs.org/en/guides/debugging-getting-started

### Breaking on Error
The "Pause on exceptions" feature can be used to automatically set a breakpoint at the line where an error is thrown

### Adding a Breakpoint in Devtools
Clicking the blue play button in the right column will cause program execution to resume, the f function will be called and the runtime will pause on line 3:

![image](https://github.com/malwoodsantoro/jsnad-study-guide/assets/19801577/94ebcf26-402d-4118-92e8-de8b8d95caa0)

### Adding a Breakpoint in Code

The debugger statement can be used to explicitly pause on the line that the statement appears when debugging.

```
function f (n = 99) {
  if (n === 0) throw Error()
  debugger
  f(n - 1)
}
f()
```

💭 _When not debugging, these debugger statements are ignored, however due to noise and potential performance impact it is not good practice to leave debugger statements in code._

❔What keyword can be used within the code of a program to cause the process to pause on a specific line when in debug mode? `debugger`<br>
❔In order to set a breakpoint on the first line of execution when entering debug mode, which flag should be used? `--inspect-brk`

## ▶︎ Key Javascript concepts
### Data types

**LOOSELY** typed language - meaning that variable types are not strictly enforced, allowing for dynamic and flexible variable assignments without explicit type declarations. 

JavaScript 7️⃣ primitives are as follows:

- null - deliberate assignment to indicate the absence of a meaningful value
- undefined - any variable initialized without a value 
- Number: 1, 1.5, -1e4, NaN
- BigInt: 1n, 9007199254740993n
- String: 'str', "str", `str ${var}`
- Boolean: true, false
- Symbol: Symbol('description'), Symbol.for('namespace')

Other than that, absolutely everything else in JavaScript is an **object**.

All JavaScript objects have **prototypes**. A prototype is an implicit reference to another object that is queried in property lookups. If an object doesn't have a particular property, the object's prototype is checked for that property. If the object's prototype does not have that property, the object's prototype's prototype is checked and so on.

### Functions 

A function can be assigned to an object:

```
const obj = { id: 999, fn: function () { console.log(this.id) } }
obj.fn() // prints 999
```
When a function is assigned to an object, when the implicit **this** keyword is accessed within that function it will refer to the object on which the function was called. This is why obj.fn() outputs 999. 

Functions have a **call** method that can be used to set their this context:

```
function fn() { console.log(this.id) }
const obj = { id: 999 }
const obj2 = { id: 2 }
fn.call(obj2) // prints 2
fn.call(obj) // prints 999
fn.call({id: ':)'}) // prints :)
```

In this case the fn function wasn't assigned to any of the objects, this was set dynamically via the **call** function.

### Functional prototypal inheritance 

The functional approach to creating prototype chains is to use **Object.create**:

```
const wolf = {
  howl: function () { console.log(this.name + ': awoooooooo') }
}

const dog = Object.create(wolf, {
  woof: { value: function() { console.log(this.name + ': woof') } }
})

const rufus = Object.create(dog, {
  name: {value: 'Rufus the dog'}
})

rufus.woof() // prints "Rufus the dog: woof"
rufus.howl() // prints "Rufus the dog: awoooooooo"
```

To describe the full prototype chain:

    - the prototype of rufus is dog
    - the prototype of dog is wolf
    - the prototype of wolf is Object.prototype.

### Constructor function prototypal inheritance 



## ▶︎ Data Types

## Packages & Dependencies
