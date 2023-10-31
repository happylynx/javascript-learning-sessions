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

```javascript
{
    const lambda = () => {
        'use strict'
        console.log('inside lambda', (function () {
            return !this
        })()) // true
    }

    function f() {
        'use strict'
        console.log('inside function', (function () {
            return !this
        })()) // true
    }

    console.log('oustside function', (function () {
        return !this
    })()) // false
    f()
    lambda()
}
```

```javascript
console.log((function () { return !this})())               // false
console.log((function () { `use strict`; return !this})()) // false
console.log((function () { 'use strict'; return !this})()) // true
```

[modules example](demo/strict-mode/modules.html)

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

### Failure of assigning to object property

```javascript

```

## Enabling strict mode

* for whole evaluation context / file
  * first statement is string `'use strict'`
* for a function
  * first statement of the function is string `'use strict'` of `"use strict"`
  * not allowed for functions with rest (vararg), default and destructed arguments
* Enabled by default in modules
* Enabled by default in classes

## Detection

```javascript
(function () { return !this})()
```

```javascript
(function () {
    var isStrict = true
    eval('var isStrict = false')
    return isStrict
})()
```
