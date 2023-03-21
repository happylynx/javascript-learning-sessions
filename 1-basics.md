# Javascript 1

## Lullaby

### History

* created by Brendan Eich at Netscape in 1995 in two weeks
* standardized as ECMAScript https://tc39.es/ecma262/
* "JavaScript" trademark owned by Oracle (previously Sun)
* versions:
  * 3 (1999)
  * 4 (abandoned in 2003)
    * many parts incorporated into Macromedia's Flash scripting language ActionScript
    * features
      * classes
      * modules
      * type annotations & static typing
      * generators & iterators
      * destructuring assignment
      * algebraic data types
  * 5 (2009)
    * strict mode added
    * `JSON` added
    * many Array methods
  * 6 (2015)
    * aka ES2015
    * https://kangax.github.io/compat-table/es6/
  * ... annual updates

* user friendly reference
  * https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference
  * compatibility table
  * specification link
* no reference implementation, multiple environments
  * web browsers
  * server-side
    * node.js
    * deno
    * bun
  * Java
    * https://www.graalvm.org/22.0/reference-manual/js/JavaInteroperability/
  * Oracle DB
    * https://medium.com/graalvm/mle-executing-javascript-in-oracle-database-c545feb1a010#55f2

## Data types

<details>
    <summary>Find primitive data types</summary>

... using `typeof` operator

```javascript
""
'foo ' + 2.0
33
2 ** -12
33n
`foo 
 ${new Date()} bar`
Symbol('foo')
Symbol.for('bar')
new Symbol('baz')
{}
true
{}['foo']
null
!!undefined
[3, 'apes']
new String('foo')
Boolean(null)
NaN
BigInt('-1_024')
```
</details>

### `boolean`

* instantiable wrapper

  ```javascript
  new Boolean(true) !== true // true
  typeof new Boolean(false)
  typeof false
  ```

* autoboxing

  ```javascript
  (true).toString()
  ```
* to primitive value

  ```javascript
  new Boolean(true).valueOf()
  ```
  
* "truthy"
  * all values except for `false`, `0`, `-0`, `0n`, `""`, `null`, `undefined`, and `NaN`
    
    ```javascript
    if ('foo') {
        console.log('bar')
    }
    ```
    
  * explicit conversion `!!`


### `number`

* instantiable wrapper
* 64bit floating point
* bitwise operations are performed on 32 bit integers
* collection of static methods `Math`
* literals
  ```javascript
  -12
  1.602e-19
  0o72 // 58
  0x10 // 16
  0b1101 //13
  ```
* special values
  * Infinity, NaN
  
  ```javascript
  -1/0
  Number.parseInt('foo')
  ```

### `string`

* instantiable wrapper
* literals
  * `''`
  * `""`
  * ```javascript
    const foo = 'bar'
    `<b>
    foo=${foo}
    </b>`
    ```

### `symbol`

* a bit like uniques strings

```javascript
Symbol('a') === Symbol('a') // false
```

* runtime-wide symbols

```javascript
Symbol.for('a') === Symbol.for('a') // true
```

* well known symbols
  * `Symbol.toStringTag`, `Symbol.iterator`, ...
  * key role in customization of object behavior

### `bigint`

* unlimited integer
* in supports basic operations `+`, `*`, `-`, `/`, `**`, `%` and bitwise operators except for `>>>`

```javascript
1234n ** 5678n
```

### `object`

* dynamic, a bit like HashMap, dictionary
* keys can only be strings and symbols

```javascript
{
    const propertyName = 'baz'
    const symbolA = Symbol('a')
    const o = {
        foo: 'hello',
        'bar': 8,
        [propertyName]: [],
        [symbolA]: 9
    }
    console.log(o)
    o[Symbol.toStringTag] = 'My great object'
    delete o.baz
    console.log(o)
}
```

### `undefined`

* "empty" value
  
  ```javascript
  {
      const a = { foo: 'bar' }
      console.log(a.baz) // undefined
  }
  ```
  
* default return value

  ```javascript
  function foo() {
      return
  }
  console.log(foo())
  ```
  
* `void` operator

  ```javascript
  void parseInt('3')
  ```

### Null

* typically used to express empty result

```javascript
typeof null === 'object'
```

## Variables

### Definition

* `var`, `const`, `let`
  * const value, not properties
* hoisting
* use before initialization
* default value `undefined`

```javascript
function a(flag) {
    'use strict'
    if (flag) {
        foo = 'bar'
    }
    var foo
    console.log(foo)
}
```

```javascript
function a(flag) {
  'use strict'
  if (flag) {
    var foo
  }
  foo = 8
  console.log(foo)
}
```

```javascript
function a(flag) {
    'use strict'
    var foo
    var foo
}
```

### Scopes

* local
* enclosing
  * block
  * function
  * module
* global

<details>
<summary>Write function <code>createLogger(loggerPrefix: string): (string) => void</code></summary>
</details>


* global object
    * `globalThis`
      * `window`, `global`
      
      ```javascript
      console.log(globalThis)
      
      {
          let counter = 0
          for (const i in globalThis) {
              counter++
          }
          console.log('enumerable global properties', counter)
      }
      ```
    * full of objects already there
      
      ```javascript
      globalThis.Math === Math // true
      ```
    * ```javascript
      delete Math
      ```

## Flow control

* `if-else`

  ```javascript
  if (true) {
      console.log('true branch')
  } else {
      console.log('branch')
  }
  ```
  
  * ternary operator
  
  ```javascript
  const a = foo !== null ? foo : bar
  ```
  
  * `,` operator
  
  ```javascript
  if (false) console.log('foo'), console.log('bar')
  
  const foo = 3, Math.random()
  
  for (let i = 0, j = 0; i < 3; i++, j+= 2) {
  //            👆 ❌             👆 ✅
      console.log(`i=${i} j${j}`)
  }
  ```
  
  * `&&`, `||`

  ```javascript
  'foo' && 'bar' && 'baz' // 'baz'
  'foo' && 'bar' && null && 'baz' // null
  'foo' || 'bar' || 'baz' // 'foo'
  null || 'foo' || 'bar' || 'baz' // 'foo'
  (console.log('foo'), 'foo') && (console.log('bar'), 'bar') 
  (console.log('foo'), 'foo') || (console.log('bar'), 'bar') 
  ```

* `while`
  * `continue`, `break`
  * labels
    
    ```javascript
    function a() {
        let counter = 0
        outter:
        while (true) {
            while (true) {
                counter++
                console.log('counter', counter)
                if (counter > 3) {
                    break outter
                }
            }
            console.log('never printed')
        }
    }
    a()
    ```
    
    ```javascript
    {
      a: if (true) {
        b: {
          console.log('before break')
          break a
          console.log('after break')
        }
        console.log('after b block')
      }
      console.log('after a labeled if')
    }
    ```
* `do-while`

  ```javascript
  {
    let counter = 0
    do {
      counter++
      console.log(counter)
    } while (counter < 3)
  }
  ```

* `for`

  ```javascript
  for (let i = 0, j = 0; i < 3; i++, j+= 2) {
    console.log(`i=${i} j${j}`)
  }
  ```
  
* `for ... in`
  * enumerable, string-keyed properties

  ```javascript
  for (const key in ['apple', 'orange']) {
    console.log(`key=${key}`)
  }
  
  for (const key in { foo: 'apple', bar: 'orange', [Symbol('bar')]: 'pear'}) {
    console.log(`key=${key}`)
  }

  for (const key in console) {
    console.log(`key=${key}`)
  }
  
  for (const key in Function.prototype) {
    console.log(`key=${key}`)
  }
  ```

* `for ... of`
  * iterable protocol
    * implementing types
      * `Array`
      * `string`
      * `Map` & `Set`
      * `NodeList` (DOM type)
  
  ```javascript
  for (const value of ['apple', 'orange']) {
    console.log('value', value)
  }
  
  for (const value of Object.values({ foo: 'apple', bar: 'orange', [Symbol('bar')]: 'pear'})) {
    console.log('value', value)
  }

  for (const value of new Set(['apple', 'orange'])) {
    console.log('value', value)
  }
  
  for (const value of new Map([['apple', 'red'], ['orange', 'orange']])) {
    console.log('value', value)
  }
  ```

<details>
<summary>TASK</summary>

Create a function that takes single non-negative integer argument and prints two rectangles
of stars `*` like:

```
3:
***
**
*
  *
 **
***

5:
*****
****
***
**
*
    *
   **
  ***
 ****
*****
```

</details>

* `switch`
* `try-catch-finally`
  * `throw`
  * exception types
  * `Error`
  
  ```javascript
  try {
    throw 3
    throw 'foo'
    throw new Error('message')
    console.log('never reached')
  } catch (e) {
    if (typeof e === 'string') {
      console.error('string exception', e)
    } else if (e instanceof Error) {
      console.error('Error exception', e)
    } else {
      throw e
    }
  } finally {
    console.log('finally')
  }
  ```

## Semicolons

* not required (most of the time), automatically inserted
* https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#automatic_semicolon_insertion
* rules of thumb
  * never break line after `return`, `throw`, `yield`
  * always put semicolon before `(`, `[`, `,` `+`, `-`, `/` 

## Comments

```javascript
#!/usr/bin/node
// 👆 likely not yet official

// single line

/*
multi
line
 */

/**
 * JSDoc; not part of the standard
 */
function foo() {
  
}
```

## Debugging

### `console`

* `log`
  * object inspector vs. string
  * multiple arguments
* `debug`, `info`, `warn`, `error`
* `trace`
* `group(name)`, `groupEnd`
* `count(name)`, `resetCount(name)`
* `clear`
* `assert(condition, ...messages)`
* `time(name)`, `timeLog(name)`, `timeEnd(name)`
* `table`

```javascript
console.log(
        'hello %cred world %cregular world %clarge world',
        `color: red; background-color: yellow;`,
        '',
        'font-size: large; font-family: serif; border: lime thick dashed',
        'other messages')

console.table([
    { foo: 1, bar: 2 },
    { foo: 3, bar: 4, baz: 5 }]) 

console.group('foo group')
console.warn('warn in group')
console.error('error in group')
console.groupEnd()

console.time('timer 1')
console.trace('message with stack')
console.timeLog('timer 1', 'time logged')
console.assert('0' === 0, 'assertion failed')
console.timeEnd('timer 1')

console.count('counter 1')
console.count('counter 1')
console.count('counter 1')
```

### `debugger`

* only when debugger is attached
  * demo https://jsfiddle.net/

```javascript
console.log('foo')
debugger
console.log('bar')
```

## Null safety

* "nullish"
  * `null` or `undefined`
* `?.`
* `??`
* `??=`

```javascript
const empty = null
empty?.foo?.bar // null
empty?.[foo] // null
empty?.() // no function invoked

undefined ?? 'foo' // 'foo'
'bar' ?? 'baz' // 'baz'

let a   // a = undefined
a ??= 1 // a = 1
a ??= 2 // a = 1
```

<details>
    <summary>
TASK
    </summary>

Write in as many ways as possible an expression that returns value of `foo.bar().baz`
if the value exists and it is not `null` or `undefined`. Return a random number otherwise.

How is the answer changed if a random number should be returned instead of any "falsy" value
of `foo.bar().baz`?

```javascript
[
  null,
  undefined,
  5,
  {},
  { bar: "bar"},
  { bar: {
      baz: () => '😜'
    }
  },
  {
    bar() {
      return { baz: 'success' }
    }
  },
  {
    bar() {
      return { baz: function success() {return '42'}}
    }
  },
  {
    bar() {
      return { baz: 0}
    }
  },
  {
    bar() {
      return { baz: false}
    }
  },
  {
    bar() {
      const result = new Boolean(false)
      result.baz = 'success'
      return result
    }
  }
].map((foo) => ...)
        .forEach(value => console.log(value))
```

<details>

```javascript
(typeof foo?.bar) === 'function' && foo.bar()?.baz || Math.random()
```

</details>

</details>

## TODO 

* casting vs coercion
  * https://www.w3schools.com/js/js_type_conversion.asp
  * https://flaviocopes.com/javascript-casting/
* equality
  * https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness
* `+`
  * https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Addition