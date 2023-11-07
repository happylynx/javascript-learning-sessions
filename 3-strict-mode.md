# Strict mode

<!-- TODO remove -->
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode

## What is strict mode

* Set of opt-in language restrictions to fix design flaws 
* Introduced in ECMAScript 5 (2009)
* It aims to
  * Replace silent errors by throwing exceptions
  * Eliminate code that is difficult to optimize for performance
  * Limit syntax likely used in future version of Javascript

## Detection

```javascript
/**
 * An expression returning `true` if strict mode is enable, `false` otherwise
 */
(function () { return !this})()
```

## Scope

* opt-in
  * script files
  * functions (lambdas included)
  * enabled using `"use strict"` or `'use strict'` at the beginning of file / function 
* on by default
  * modules
  * classes

Function scope:

```javascript
{
    function f() {
        'use strict'
        console.log('inside function', (function () {return !this})()) // true
    }

    const lambda = () => {
        'use strict'
        console.log('inside lambda', (function () {return !this})()) // true
    }

    console.log('outside of function', (function () {return !this})()) // false
    f()
    lambda()
}
```

Enabling for functions:

```javascript
console.log((function () { return !this})())               // false
console.log((function () { `use strict`; return !this})()) // false
console.log((function () { 'use strict'; return !this})()) // true
```

[modules example](demo/strict-mode/modules.html)

Class example:

```javascript
{
  console.log('outside', (function () { return !this})()) // false

  class Foo {
    static field = (function () { return !this})()
    static method() {
      console.log('method', (function () { return !this})()) // true
    }
  }
  console.log('field', Foo.field) // true
  Foo.method()
}
```

### Propagation

* not by calling

  ```javascript
  function nonStrict1() {
    console.log('nonStrict1', (function () { return !this})()) // false
    strict1()
  }
  
  function strict1() {
    'use strict'
    console.log('strict1', (function () { return !this})()) // true
    nonStrict2()
  }
  
  function nonStrict2() {
    console.log('nonStrict2', (function () { return !this})()) // false
  }
  
  nonStrict1()
  ```
  
* by definition location

  ```javascript
  function nonStrict1() {
    function strict1() {
      'use strict'
      function nonStrict2() {
        console.log('nonStrict2', (function () { return !this})()) // true
      }
      console.log('strict1', (function () { return !this})()) // true
      nonStrict2()
    }
    console.log('nonStrict1', (function () { return !this})()) // false
    strict1()
  }
  nonStrict1()
  ```

## Changes

### Assigning to undeclared variable

```javascript
foo = 8 // it throws in strict mode
```

### Failing to assign to object properties

* Assigning to
  * non-writable property
    
    ```javascript
    {
      const obj = Object.defineProperty({}, 'p', { value: 'foo', writable: false })
      obj.p = 'bar'
      console.log(obj)
    }
    ```    

  * getter only property

    ```javascript
    {
      const obj = Object.defineProperty({}, 'p', { get() { return 'foo' } })
      obj.p = 'bar'
      console.log(obj)
    }
    ```

* Creating a new property of a non-extensible object

  ```javascript
  {
    const obj = { foo: 'bar' }
    Object.preventExtensions(obj)
    obj.baz = 'hello'
    console.log(obj)
  }
  ```
  
### Deleting undeletable properties, variabled

* non-configurable property

  ```javascript
  console.log(Object.getOwnPropertyDescriptor([], 'length')) // configurable: false
  delete [].length
  ```
  
  ```javascript
  {
    const obj = { foo: 'bar' }
    Object.seal(obj)
    console.log(delete obj.foo) // throws in strict mode
    console.log(obj)
  }
  ```

* local variable

  ```javascript
  {
    let foo = 'bar'
    delete foo // throws in strict mode
    console.log(foo)
  }
  ```
  
### Duplicate parameter names

```javascript
function f(foo, foo) {                                 // it throws in scrict mode
    console.log('foo=', foo, 'arguments=', arguments)
}
f('bar', 'baz')
```

### Legacy octal literals

* numberic

  ```javascript
  console.log(0o10)
  console.log(010)
  ```
  
* character

  ```javascript
  // 'use strict'
  console.log(0o101, 0x41, 65)
  console.log('\u0041')
  console.log('\010') // octal character literal starting with zero allowed even in strict mode
  console.log('\101') // throws in strict mode
  ```

### Setting properties on primitive values

```javascript
true.foo = 'bar' // throws in strict mode
console.log(true)
```

### Duplicate property names

* it used to be forbidden in strict mode, it's not anymore since ES2015 

```javascript
const obj = { foo: "bar", foo: "baz" }
console.log(obj)
```

### `with` prohibited

* it allow to create code that's difficult to optimize

```javascript
const foo = "foo variable"
const bar = "bar variable"
const obj = { foo: "foo property" }
with (obj) { // throws in strict mode
    console.log(foo, bar)
}
```

### Non-leaking eval

Outside of strict mode variables defined using `var` are propagated outside of `eval()` call

```javascript
eval('var foo = "bar"')
console.log(foo) // it throws "ReferenceError: foo is not defined" in strict mode
```

### Block level function declaration

* i.e. functions can be declared conditionally
* It's not not allow outside of strict mode
  * Many runtimes came with unofficial extensions allowing this outside strict mode

```javascript
if (false) {
    function foo() {}
}
console.log(foo) // undefined, with or without strict mode
```

### `eval` and `arguments` can't be assigned or used as variable name

```javascript
{
    const eval = "foo"
    arguments = "foo"
}
```

### No `arguments` - named parameters synchronization

```javascript
{
  function f(a, b) {
      console.log(a, b, arguments)
      a = 'updated a'
      arguments[1] = 'updated second argument'
      console.log(a, b, arguments)
  }
  f('foo', 'bar')
}
```

### No `this` substitution

* `this` is always an object in sloppy mode
* `this` inside a function
  * in sloppy mode it points to globalThis

    ```javascript
    (function () { console.log(this) })()
    ```

* boxing of bound `this`

  ```javascript
  function fn() {
    console.log(this)
  }
  fn.call(null)
  fn.call(2)
  ```
  
### No stack-walking properties

* `f.caller` reference to a function that called the current function,
  a method one stack frame lower with respect to the current function
* `f.argument` same as just `arguments`
* `arguments.callee` - reference to the current function

```javascript
function f() {
    console.log(f.caller)  // it throws in strict mode
    console.log(f.arguments) // it throws in strict mode
    console.log(arguments.callee) // it throws in strict mode
}
f()
```

<details>
<summary>Task</summary>

Write a function `printStack()` that prints stacktrace 
* one line per frame in form `<function-name>` or `<function-instance>` followed by arguments passed into the function
* the latest frame first

Verify using snippet:

```javascript
function g(item, index, array) {
    printStack()
}

function f(p1, p2) {
    [1].forEach(g)
}

f('foo', 'bar')
```

<details>
<summary>Solution</summary>

```javascript
function printStack() {
  let fn = arguments.callee
  while (fn !== null) {
    console.log(fn, fn.arguments)
    fn = fn.caller
  }
}
```

</details>

</details>

### New reserved words

* can't be used as variable names

```javascript
implements
interface
let
package
private
protected
public
static
yield

```

```javascript
let package = 8 // throws in strict mode
```

<details>
<summary>Task</summary>

Pick coupe of described differences between strict mode and sloppy mode. For each difference write an expression
testing if code is running in strict mode. The expression should evaluate to `true` if and only if
it is evaluated in strict mode.

</details>
